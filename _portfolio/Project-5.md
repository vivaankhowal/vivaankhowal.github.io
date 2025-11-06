---
layout: project
title: "Arc Reactor Clock"
thumbnail: /images/reactorclock-logo.jpg
collection: Portfolio
---
## About
I’ve always wanted a clock for my room, but none of the ones I found online ever felt right. They were either too complicated, too expensive, or just didn’t look good. So, as the saying goes, if you want something done right, do it yourself. I decided to build my own clock instead. Inspired by my love for Iron Man, I designed one that truly feels like it belongs to an engineer, simple, sleek, and entirely my own creation.

The idea for this clock is something I saw online, but I wanted to modify it and add my own features. Everything I used in this project was designed by me from scratch, but just the idea I saw online. 

Here is the source: 
<a href="https://www.instructables.com/Iron-Man-Arc-Reactor-Clock/">Click here</a>

## Hardware
The Arc Reactor Clock runs on an ESP32-C3 Mini, a small but powerful microcontroller with built-in Bluetooth Low Energy. It handles all the logic for displaying time, managing the lights, and running the alarm. Bluetooth allows the user to set the time, color, or alarm directly from their phone using simple commands. The time is shown on a TM1637 four-digit LED display, which connects to the ESP32 with only two wires, keeping the wiring clean and easy to manage. The brightness of the display can also be adjusted through code, allowing it to match the lighting effects of the reactor.

At the center of the design is a 35-LED NeoPixel ring that creates the glowing Arc Reactor effect. Each LED can be controlled individually, which makes it possible to create colorful effects like rainbow animations or smooth pulsing lights. The entire circuit, including the ESP32, display, NeoPixel ring, and buzzer, is powered through a standard 5 V USB cable. The buzzer produces the alarm tone, while a simple push button lets the user stop or snooze the alarm. All the components are neatly arranged inside a custom 3D-printed shell that spreads the light evenly and hides the wiring, giving the clock a clean and futuristic look worthy of Tony Stark.

Its capable of 8 different colors and an extra rainbow mode:
<p>
<img src='/images/blue.jpeg' width='200' height='auto'>
<img src='/images/red.jpeg' width='200' height='auto'>
<img src='/images/orange.jpeg' width='200' height='auto'>
<img src='/images/yellow.jpeg' width='200' height='auto'>
<img src='/images/green.jpeg' width='200' height='auto'>
<img src='/images/purple.jpeg' width='200' height='auto'>
<img src='/images/pink.jpeg' width='200' height='auto'>
<img src='/images/white.jpeg' width='200' height='auto'>
<p></p>
Wiring Diagram:
<p>
<img src='/images/clockwiring.png' width='500' height='auto'>

<p></p>
In all of my projects, this is the one with the most complicated software. The software for the Arc Reactor Clock is written in Arduino C++ and runs on the ESP32-C3 Mini. It controls the clock display, LED animations, and alarm behavior while allowing wireless configuration through Bluetooth Low Energy (BLE). When powered on, the ESP32 initializes the TM1637 display to show the current time and starts the NeoPixel animations based on the selected mode, such as a solid color or a smooth rainbow effect. The code keeps track of time internally using the ESP32’s built-in clock and updates the display every second. The alarm system is handled in software by comparing the current time to a stored alarm time, triggering the buzzer and LED flashing pattern when they match.
<p></p>
The BLE logic allows the user to control and customize the clock using their phone or computer. Once connected via Bluetooth, the ESP32 listens for text-based commands that let the user adjust settings in real time. For example, sending "settime YYYY-MM-DD HH:MM:SS" updates the current time, "setcolor R,G,B" changes the LED color, and "setmode rainbow" activates the rainbow animation. There are also commands to set or stop the alarm. When the alarm is active, the LEDs flash green while the buzzer pulses, and pressing the physical button or sending a Bluetooth command stops the alarm and restores the previous lighting mode. This setup makes the Arc Reactor Clock fully interactive, combining the look of an Iron Man reactor with the flexibility of smart Bluetooth control.
<p></p>

Here is the full code:

```cpp
#include <BLEDevice.h>
#include <BLEServer.h>
#include <BLEUtils.h>
#include <BLE2902.h>
#include <Adafruit_NeoPixel.h>
#include <TM1637Display.h>
#include <time.h>
#include <sys/time.h>

#define SERVICE_UUID        "12345678-1234-1234-1234-123456789abc"
#define CHARACTERISTIC_UUID "abcd1234-5678-90ab-cdef-1234567890ab"

#define NEOPIXEL_PIN 4
#define NUM_PIXELS   35
#define TM1637_CLK   2
#define TM1637_DIO   3
#define BUZZER_PIN   0
#define BUTTON_PIN   5

Adafruit_NeoPixel pixels(NUM_PIXELS, NEOPIXEL_PIN, NEO_GRB + NEO_KHZ800);
TM1637Display display(TM1637_CLK, TM1637_DIO);

BLECharacteristic *pCharacteristic;
BLEAdvertising *pAdvertising;

bool alarmTriggered = false;
bool alarmActive   = false;
unsigned long snoozeUntil = 0;
unsigned long lastRainbowUpdate = 0;
int  rainbowHue = 0;
bool rainbowMode = false;
int  brightnessLevel = 50;  
int  weekdayAlarmH = 7, weekdayAlarmM = 0;
bool weekdayAlarmEnabled = true;
int  weekendAlarmH = 8, weekendAlarmM = 0;
bool weekendAlarmEnabled = false;
uint8_t lastR = 0, lastG = 255, lastB = 255;
uint8_t prevR = 0, prevG = 0, prevB = 0;
bool prevRainbowMode = false;
bool alarmBlinkState = false;
unsigned long lastBlinkTime = 0;
const uint16_t BEEP_DURATIONS_MS[] = {100, 100, 100, 100, 100, 1000};
const uint8_t  BEEP_LEVELS[]       = {1, 0, 1, 0, 1, 0};
const uint8_t  BEEP_STEPS = sizeof(BEEP_DURATIONS_MS) / sizeof(BEEP_DURATIONS_MS[0]);
uint8_t beepStep = 0;
unsigned long stepDeadline = 0;


inline void startBeepPattern() {
  beepStep = 0;
  digitalWrite(BUZZER_PIN, BEEP_LEVELS[0] ? HIGH : LOW);
  stepDeadline = millis() + BEEP_DURATIONS_MS[0];
}

inline void updateBeepPattern() {
  if (!alarmActive) return;
  unsigned long now = millis();
  if (now >= stepDeadline) {
    beepStep++;
    if (beepStep >= BEEP_STEPS) beepStep = 0;
    digitalWrite(BUZZER_PIN, BEEP_LEVELS[beepStep] ? HIGH : LOW);
    stepDeadline = now + BEEP_DURATIONS_MS[beepStep];
  }
}

// ================== HELPERS ==================
void setAllPixels(uint8_t r, uint8_t g, uint8_t b) {
  lastR = r; lastG = g; lastB = b;
  if (!rainbowMode) {
    for (int i = 0; i < NUM_PIXELS; i++) pixels.setPixelColor(i, r, g, b);
    pixels.show();
  }
}

inline void pushAllPixels(uint8_t r, uint8_t g, uint8_t b) {
  for (int i = 0; i < NUM_PIXELS; i++) pixels.setPixelColor(i, r, g, b);
  pixels.show();
}

void updateRainbowEffect() {
  if (millis() - lastRainbowUpdate > 50) {
    lastRainbowUpdate = millis();
    rainbowHue = (rainbowHue + 3) % 360;

    uint8_t scaledV = map(brightnessLevel, 0, 255, 20, 255); 
    uint32_t color = pixels.ColorHSV(rainbowHue * 182, 255, scaledV);
    color = pixels.gamma32(color);

    for (int i = 0; i < NUM_PIXELS; i++) pixels.setPixelColor(i, color);
    pixels.show();
  }
}

void playTripleBeep() {
  for (int i = 0; i < 3; i++) {
    digitalWrite(BUZZER_PIN, HIGH); delay(100);
    digitalWrite(BUZZER_PIN, LOW);  delay(100);
  }
}

// ================== TIME ==================
bool parseTimeHHMM(const String& s, int &h, int &m) {
  int sep = s.indexOf(':'); if (sep < 0) return false;
  h = s.substring(0, sep).toInt();
  m = s.substring(sep + 1).toInt();
  return !(h < 0 || h > 23 || m < 0 || m > 59);
}

bool parseDateYYYYMMDD(const String& s, int &y, int &mo, int &d) {
  if (s.length() < 10) return false;
  y  = s.substring(0,4).toInt();
  mo = s.substring(5,7).toInt();
  d  = s.substring(8,10).toInt();
  return !(y < 2000 || mo < 1 || mo > 12 || d < 1 || d > 31);
}

void setSystemTime(int y, int mo, int d, int h, int m, int s) {
  struct tm t;
  t.tm_year = y - 1900; t.tm_mon = mo - 1; t.tm_mday = d;
  t.tm_hour = h; t.tm_min = m; t.tm_sec = s;
  time_t epoch = mktime(&t);
  struct timeval now = { .tv_sec = epoch };
  settimeofday(&now, NULL);
  Serial.printf("System time set: %04d-%02d-%02d %02d:%02d:%02d\n", y, mo, d, h, m, s);
}

void getCurrentTime(int &y, int &mo, int &d, int &dow, int &h, int &m, int &s) {
  time_t now = time(nullptr);
  struct tm *t = localtime(&now);
  y = t->tm_year + 1900;
  mo = t->tm_mon + 1;
  d = t->tm_mday;
  dow = t->tm_wday;
  h = t->tm_hour;
  m = t->tm_min;
  s = t->tm_sec;
}

// ================== ALARM ==================
void triggerAlarm() {
  Serial.println("ALARM TRIGGERED!");

  prevR = lastR;
  prevG = lastG;
  prevB = lastB;
  prevRainbowMode = rainbowMode;

  alarmActive = true;
  alarmBlinkState = false;
  lastBlinkTime = 0;
  startBeepPattern();
}

void stopAlarm() {
  alarmActive = false;
  digitalWrite(BUZZER_PIN, LOW);

  rainbowMode = prevRainbowMode;
  if (rainbowMode) {
    rainbowHue = 0;
    lastRainbowUpdate = millis();
    updateRainbowEffect();
  } else {
    setAllPixels(prevR, prevG, prevB);
  }
}

void updateAlarmVisuals() {
  if (!alarmActive) return;

  unsigned long now = millis();
  if (now - lastBlinkTime >= 500) {
    lastBlinkTime = now;
    alarmBlinkState = !alarmBlinkState;

    if (alarmBlinkState)
      pushAllPixels(0, 255, 0);   
    else
      pushAllPixels(0, 0, 0);   
  }
  updateBeepPattern();
}

// ================== BLE CALLBACK ==================
class MyCallbacks : public BLECharacteristicCallbacks {
  void onWrite(BLECharacteristic* pCharacteristic) override {
    String received = String((char*)pCharacteristic->getData(), pCharacteristic->getValue().length());
    received.trim(); received.toLowerCase();
    Serial.print("Received: "); Serial.println(received);

    if      (received == "red")        { rainbowMode = false; setAllPixels(128, 0, 0); }
    else if (received == "green")      { rainbowMode = false; setAllPixels(0, 128, 0); }
    else if (received == "blue")       { rainbowMode = false; setAllPixels(0, 0, 128); }
    else if (received == "cyan")       { rainbowMode = false; setAllPixels(0, 128, 128); }
    else if (received == "white")      { rainbowMode = false; setAllPixels(128, 128, 128); }
    else if (received == "yellow")     { rainbowMode = false; setAllPixels(128, 128, 0); }
    else if (received == "purple")     { rainbowMode = false; setAllPixels(128, 0, 128); }
    else if (received == "orange")     { rainbowMode = false; setAllPixels(128, 30, 0); }
    else if (received == "pink")       { rainbowMode = false; setAllPixels(126, 48, 64); }
    else if (received == "off")        { rainbowMode = false; setAllPixels(0, 0, 0); }
    else if (received == "alarm")      { triggerAlarm(); }
    else if (received == "snooze") {
      if (alarmActive) {
        snoozeUntil = millis() + 5UL * 60UL * 1000UL;
        stopAlarm();
        Serial.println("Alarm snoozed 5 mins");
      }
    } else if (received == "stopalarm") {
      if (alarmActive || alarmTriggered) {
        stopAlarm();
        alarmTriggered = true;
        Serial.println("Alarm stopped");
      }
    } else if (received == "rainbow") {
      rainbowMode = true; rainbowHue = 0; brightnessLevel = 180;
      pixels.setBrightness(brightnessLevel);
      lastRainbowUpdate = millis();
      Serial.println("Rainbow mode ON");
    } else if (received.startsWith("alarm ")) {
      String t = received.substring(6); t.trim();
      if (t == "off") weekdayAlarmEnabled = false;
      else {
        int h, m;
        if (parseTimeHHMM(t, h, m)) {
          weekdayAlarmH = h; weekdayAlarmM = m; weekdayAlarmEnabled = true;
          Serial.printf("Weekday alarm set %02d:%02d\n", h, m);
        }
      }
    } else if (received.startsWith("weekendalarm ")) {
      String t = received.substring(13); t.trim();
      if (t == "off") weekendAlarmEnabled = false;
      else {
        int h, m;
        if (parseTimeHHMM(t, h, m)) {
          weekendAlarmH = h; weekendAlarmM = m; weekendAlarmEnabled = true;
          Serial.printf("Weekend alarm set %02d:%02d\n", h, m);
        }
      }
    } else if (received.startsWith("settime ")) {
      String tstr = received.substring(8); tstr.trim();
      time_t now = time(nullptr);
      struct tm *t = localtime(&now);
      int y = t->tm_year + 1900, mo = t->tm_mon + 1, d = t->tm_mday;
      int h, m, s = 0;
      int sep = tstr.indexOf(':'), sep2 = (sep >= 0) ? tstr.indexOf(':', sep + 1) : -1;
      if (sep < 0) return;
      h = tstr.substring(0, sep).toInt();
      if (sep2 < 0) m = tstr.substring(sep + 1).toInt();
      else { m = tstr.substring(sep + 1, sep2).toInt(); s = tstr.substring(sep2 + 1).toInt(); }
      setSystemTime(y, mo, d, h, m, s);
    } else if (received.startsWith("setdate ")) {
      String dstr = received.substring(8); dstr.trim();
      int y, mo, d;
      if (parseDateYYYYMMDD(dstr, y, mo, d)) {
        time_t now = time(nullptr);
        struct tm *t = localtime(&now);
        setSystemTime(y, mo, d, t->tm_hour, t->tm_min, t->tm_sec);
      }
    } else if (received.startsWith("brightness ")) {
      String level = received.substring(11); level.trim();
      if      (level == "low")    brightnessLevel = 30;
      else if (level == "medium") brightnessLevel = 80;
      else if (level == "high")   brightnessLevel = 180;
      else if (level == "max")    brightnessLevel = 255;
      pixels.setBrightness(brightnessLevel);
      if (!rainbowMode) setAllPixels(lastR, lastG, lastB);
      Serial.print("Brightness "); Serial.println(level);
    }
  }
};

// ================== SETUP ==================
void setup() {
  Serial.begin(115200);
  pinMode(BUZZER_PIN, OUTPUT);
  pinMode(BUTTON_PIN, INPUT_PULLUP);
  display.setBrightness(0x03);
  display.clear();

  BLEDevice::init("ESP32-BLE-Clock");
  BLEServer *pServer = BLEDevice::createServer();
  BLEService *pService = pServer->createService(SERVICE_UUID);
  pCharacteristic = pService->createCharacteristic(CHARACTERISTIC_UUID, BLECharacteristic::PROPERTY_WRITE);
  pCharacteristic->setCallbacks(new MyCallbacks());
  pService->start();
  pAdvertising = BLEDevice::getAdvertising();
  pAdvertising->addServiceUUID(SERVICE_UUID);
  pAdvertising->start();
  Serial.println("BLE server started.");

  pixels.begin();
  pixels.setBrightness(brightnessLevel);
  setAllPixels(0, 255, 255);

  struct tm t;
  strptime(__DATE__ " " __TIME__, "%b %d %Y %H:%M:%S", &t);
  time_t compileTime = mktime(&t);
  struct timeval now = { .tv_sec = compileTime };
  settimeofday(&now, NULL);
}

// ================== LOOP ==================
void loop() {
  int y, mo, d, dow, h, m, s;
  getCurrentTime(y, mo, d, dow, h, m, s);

  bool isWeekend = (dow == 0 || dow == 6);
  bool alarmEnabledToday = isWeekend ? weekendAlarmEnabled : weekdayAlarmEnabled;
  int ah = isWeekend ? weekendAlarmH : weekdayAlarmH;
  int am = isWeekend ? weekendAlarmM : weekdayAlarmM;

  if (alarmEnabledToday && h == ah && m == am && !alarmTriggered) {
    triggerAlarm();
    alarmTriggered = true;
  }
  static int lastMinute = -1;
  if (m != lastMinute) {
    alarmTriggered = false;
    lastMinute = m;
  }

  int displayHour = h % 12; if (displayHour == 0) displayHour = 12;
  int timeVal = displayHour * 100 + m;
  display.showNumberDecEx(timeVal, 0b01000000, true);

  if (alarmActive && millis() > snoozeUntil) {
    updateAlarmVisuals(); 
  } else {
    if (rainbowMode) updateRainbowEffect();
  }

  static bool lastButton = HIGH;
  bool btn = digitalRead(BUTTON_PIN);
  if (lastButton == HIGH && btn == LOW) {
    if (alarmActive) {
      stopAlarm();
      alarmTriggered = true;
    } else {
      playTripleBeep();
      pAdvertising->start();
    }
  }
  lastButton = btn;
  delay(10);
}
```


