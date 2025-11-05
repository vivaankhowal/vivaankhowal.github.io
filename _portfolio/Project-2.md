---
layout: default
title: "Neopixel Lightsaber V2"
thumbnail: images/10.jpeg
excerpt: "This is version two of the Neopixel Lightsaber that I built all on my own this time. It's the second step on the mission to bring this tech to life!
<br/>
<br/>
<img src='/images/IMG_5416.jpg' width='350' height='auto'>"
collection: Portfolio
---
## About
This is the second version I attempted all on my own. I took every aspect of the previous lightsaber and upgraded it in terms of durability, power, and looks. This one is modeled after Kylo Ren's lightsaber and also has a ripple effect built into the code. Although it was a huge upgrade and was much tougher, it still wasn't as good as I wanted it to be and had a bunch of issues like being extremely heavy.
<p></p>
<img src='/images/IMG_5416.jpg' width='300' height='auto'>
<img src='/images/IMG_5417.jpg' width='300' height='auto'>
<img src='/images/IMG_0196.JPEG' width='300' height='auto'>
<img src='/images/Lightsaber in Light.jpeg' width='300' height='auto'>

## Improvements
The hilt was way stronger than the previous version and could handle light hits and dueling. This time I used actual soldering instead of a breadboard with jumper wires for a much stronger connection. The button was a huge upgrade since the new one works a lot better and looks a lot better with the LED ring.

## Design Flaws
Although it was a lot sturdier than the previous version, there were many problems with it. The electronics inside were just stuffed together instead of having some organized structure, the different parts of the 3D printed hilt were held together with duct tape, and the biggest flaw was the saber was very heavy. Since there were 3 blades instead of one and it was powered by a 5V power bank, which is a lot heavier than a battery, the weight wasn't evenly distributed and it was extremely top-heavy.

## Hardware and Electronics
For the mechanical parts, there is a 3D-printed hilt and each part of the hilt is held together with duct tape. For the blades, there are 3 different-sized blades: a 40" Polycarbonate blade and 2 smaller blades for the sides. For the electronics, there are 3 Neopixel LED strips instead of 2: 2 for the main blade, and 1 for the smaller blades. The code runs off an Arduino Nano soldered to a wire with a USB connecter on the other end to plug into a 5V power bank with 5000 mAh, and since this saber has a current draw of about 3.2 Amps, the saber runs for about 1.5 hours before it needs to be recharged. For the button, I used a 5V Red LED Latching button to add to the style and functionality.

## Software
For the software, I used the Arduino IDE again to upload all my code. I used the Adafruit Neopixel library to control the neopixels. Since the big button is a latching button, I had to edit the code to detect the state of the button to turn the saber on and off. I used 2 functions for the ripple effect in the LEDs. The functions are designed to pick 4 LEDs to randomly turn yellow and transition up the blade using the same logic as the ignition, and a random number is generated from 2 to 134 which would define the position in which the yellow effect began so it looks realistic as opposed to constantly pulsing from the same spot every time. Here is the full code:
```cpp
//Library inclusions
#include <Adafruit_NeoPixel.h>
#include <SoftwareSerial.h>
//--------------------------------------------------------------------------------------------------------------------------------------------------------
//Important Values
#define MAIN 3	 
#define CROSSGUARD 5
#define NUMMAINPIXELS  282
#define NUMCROSSGUARDPIXELS 120
//--------------------------------------------------------------------------------------------------------------------------------------------------------
//Item creation based on libraries
Adafruit_NeoPixel mainPixels = Adafruit_NeoPixel(NUMMAINPIXELS, MAIN, NEO_GRB + NEO_KHZ800);
Adafruit_NeoPixel crossPixels = Adafruit_NeoPixel(NUMCROSSGUARDPIXELS, CROSSGUARD, NEO_GRB + NEO_KHZ800);

//--------------------------------------------------------------------------------------------------------------------------------------------------------
//Global Variables
const int mainNeoPixelPin = 3;
const int crossNeoPixelPin = 5;
int mainFlicker;
int crossFlicker;
int ledState = 0;
const int buttonPin = 7;
int buttonControl = 13;
int buttonState = 1;
int lastButtonState = LOW;      // Variable to store the last button state
unsigned long lastDebounceTime = 0;  // Stores the last time the button state changed
unsigned long debounceDelay = 50; 
bool isEffectActive = false;
//--------------------------------------------------------------------------------------------------------------------------------------------------------
void setup() {
  Serial.begin(9600);  
  mainPixels.begin();
  crossPixels.begin();
  pinMode(mainNeoPixelPin,OUTPUT);
  pinMode(crossNeoPixelPin, OUTPUT);
  pinMode(buttonPin, INPUT_PULLUP);
  pinMode(buttonControl, OUTPUT);
  mainPixels.setBrightness(255);
  crossPixels.setBrightness(255);

  for (int i = 141; i >= 0; ) {
    mainPixels.setPixelColor(i, mainPixels.Color(0, 0, 0));
    mainPixels.setPixelColor(281 - i, mainPixels.Color(0, 0, 0));
    mainPixels.setPixelColor(i + 1, mainPixels.Color(0, 0, 0));
    mainPixels.setPixelColor(280 - i, mainPixels.Color(0, 0, 0));
    mainPixels.setPixelColor(i + 2, mainPixels.Color(0, 0, 0));
    mainPixels.setPixelColor(279 - i, mainPixels.Color(0, 0, 0));
    i -= 3;
  }
  
  for (int i= 30; i >= 0 ; i--) {
    crossPixels.setPixelColor(i, crossPixels.Color(0, 0, 0));
    crossPixels.setPixelColor(60 - i, crossPixels.Color(0, 0, 0));
    crossPixels.setPixelColor(120 - i, crossPixels.Color(0, 0, 0));
    crossPixels.setPixelColor(60 + i, crossPixels.Color(0, 0, 0));
  }
  mainPixels.show();
  crossPixels.show();
}
//--------------------------------------------------------------------------------------------------------------------------------------------------------
void loop() {
  static int lastButtonState = HIGH;
  int buttonState = digitalRead(buttonPin);

  if (buttonState == LOW) {
    if (!isEffectActive) {
      activate();
      isEffectActive = true;
    }
  } else {
    if (isEffectActive && lastButtonState == LOW) {
      deactivate();
      isEffectActive = false;
    }
  }
  lastButtonState = buttonState;
  if (isEffectActive) {
    mainBladeEffect(120);
    crossBladeEffect(120);
  }
}
//--------------------------------------------------------------------------------------------------------------------------------------------------------
void activate() {
  for (int i=0; i < 141 ;) {
    mainPixels.setPixelColor(i, mainPixels.Color(255, 0, 0));
    mainPixels.setPixelColor(281 - i, mainPixels.Color(255, 0, 0));
    i += 9;
  }
  Serial.println("Main Blade Activated");
  for (int i = 0; i < 30 ; i++) {
    crossPixels.setPixelColor(i, crossPixels.Color(255, 0, 0));
    crossPixels.setPixelColor(59 - i, crossPixels.Color(255, 0, 0));
    crossPixels.setPixelColor(119 - i, crossPixels.Color(255, 0, 0));
    crossPixels.setPixelColor(60 + i, crossPixels.Color(255, 0, 0));
    crossPixels.show();
  }
  Serial.println("Crossguards Activated");
}
//--------------------------------------------------------------------------------------------------------------------------------------------------------
void deactivate() {
  for (int i = 141; i >= 0; ) {
    mainPixels.setPixelColor(i, mainPixels.Color(0, 0, 0));
    mainPixels.setPixelColor(281 - i, mainPixels.Color(0, 0, 0));
    i -= 3;
  }
  for (int i= 30; i >= 0 ; i--) {
    crossPixels.setPixelColor(i, crossPixels.Color(0, 0, 0));
    crossPixels.setPixelColor(60 - i, crossPixels.Color(0, 0, 0));
    crossPixels.setPixelColor(120 - i, crossPixels.Color(0, 0, 0));
    crossPixels.setPixelColor(60 + i, crossPixels.Color(0, 0, 0));
    crossPixels.show();
  }
}
//Code for the unstable effect on main blade
void mainBladeEffect(int val1) {
  mainFlicker = random(2, 134);
  for (int i = 0; mainFlicker < 141; i++) {
    //1st LED on
    mainPixels.setPixelColor(mainFlicker, mainPixels.Color(255, val1, 0));
    mainPixels.setPixelColor(282 - mainFlicker , mainPixels.Color(255, val1, 0));
    //2nd LED on
    mainPixels.setPixelColor(mainFlicker + 1, mainPixels.Color(255, val1, 0));
    mainPixels.setPixelColor(281 - mainFlicker , mainPixels.Color(255, val1, 0));
    //3rd LED on
    mainPixels.setPixelColor(mainFlicker + 2, mainPixels.Color(255, val1, 0));
    mainPixels.setPixelColor(280 - mainFlicker , mainPixels.Color(255, val1, 0));
    //4th LED on
    mainPixels.setPixelColor(mainFlicker + 3, mainPixels.Color(255, val1, 0));
    mainPixels.setPixelColor(279 - mainFlicker , mainPixels.Color(255, val1, 0));
    //5th LED on
    mainPixels.setPixelColor(mainFlicker + 4, mainPixels.Color(255, val1, 0));
    mainPixels.setPixelColor(278 - mainFlicker , mainPixels.Color(255, val1, 0));
    //6th LED on
    mainPixels.setPixelColor(mainFlicker + 5, mainPixels.Color(255, val1, 0));
    mainPixels.setPixelColor(277 - mainFlicker , mainPixels.Color(255, val1, 0));
    //7th LED on
    mainPixels.setPixelColor(mainFlicker + 6, mainPixels.Color(255, val1, 0));
    mainPixels.setPixelColor(276 - mainFlicker , mainPixels.Color(255, val1, 0));
    //8th LED on
    mainPixels.setPixelColor(mainFlicker + 7, mainPixels.Color(255, val1, 0));
    mainPixels.setPixelColor(275 - mainFlicker , mainPixels.Color(255, val1, 0));
    //---------------------------------------------------------------------------------
    mainPixels.show();
    //---------------------------------------------------------------------------------
    //1st LED off
    mainPixels.setPixelColor(mainFlicker, mainPixels.Color(255, 0, 0));
    mainPixels.setPixelColor(282 - mainFlicker , mainPixels.Color(255, 0, 0));
    //2nd LED off
    mainPixels.setPixelColor(mainFlicker + 1, mainPixels.Color(255, 0, 0));
    mainPixels.setPixelColor(281 - mainFlicker , mainPixels.Color(255, 0, 0));
    //3rd LED off
    mainPixels.setPixelColor(mainFlicker + 2, mainPixels.Color(255, 0, 0));
    mainPixels.setPixelColor(280 - mainFlicker , mainPixels.Color(255, 0, 0));
    //4th LED off
    mainPixels.setPixelColor(mainFlicker + 3, mainPixels.Color(255, 0, 0));
    mainPixels.setPixelColor(279 - mainFlicker , mainPixels.Color(255, 0, 0));
    //5th LED off
    mainPixels.setPixelColor(mainFlicker + 4, mainPixels.Color(255, 0, 0));
    mainPixels.setPixelColor(278 - mainFlicker , mainPixels.Color(255, 0, 0));
    //6th LED off
    mainPixels.setPixelColor(mainFlicker + 5, mainPixels.Color(255, 0, 0));
    mainPixels.setPixelColor(277 - mainFlicker , mainPixels.Color(255, 0, 0));
    //7th LED off
    mainPixels.setPixelColor(mainFlicker + 6, mainPixels.Color(255, 0, 0));
    mainPixels.setPixelColor(276 - mainFlicker , mainPixels.Color(255, 0, 0));
    //8th LED off
    mainPixels.setPixelColor(mainFlicker + 7, mainPixels.Color(255, 0, 0));
    mainPixels.setPixelColor(275 - mainFlicker , mainPixels.Color(255, 0, 0));
    //---------------------------------------------------------------------------------
    mainPixels.show();
    mainFlicker += 15;
  }
}
//----------------------------------------------------------------------------------------------------------------------------------------------
//Code for the unstable effect on crossguards
void crossBladeEffect(int val2) {
  crossFlicker = random(0, 120);
  for (int i = 0; crossFlicker < 30; i++) {
    //1st LED on
    crossPixels.setPixelColor(crossFlicker, crossPixels.Color(255, val2, 0));
    crossPixels.setPixelColor(59 - crossFlicker, crossPixels.Color(255, val2, 0));
    crossPixels.setPixelColor(119 - crossFlicker, crossPixels.Color(255, val2, 0));
    crossPixels.setPixelColor(60 + crossFlicker, crossPixels.Color(255, val2, 0));
    //2nd LED on
    crossPixels.setPixelColor(crossFlicker + 1, crossPixels.Color(255, val2, 0));
    crossPixels.setPixelColor(58 - crossFlicker, crossPixels.Color(255, val2, 0));
    crossPixels.setPixelColor(118 - crossFlicker, crossPixels.Color(255, val2, 0));
    crossPixels.setPixelColor(59 + crossFlicker, crossPixels.Color(255, val2, 0));
    //3rd LED on
    crossPixels.setPixelColor(crossFlicker + 2, crossPixels.Color(255, val2, 0));
    crossPixels.setPixelColor(57 - crossFlicker, crossPixels.Color(255, val2, 0));
    crossPixels.setPixelColor(117 - crossFlicker, crossPixels.Color(255, val2, 0));
    crossPixels.setPixelColor(58 + crossFlicker, crossPixels.Color(255, val2, 0));
    //---------------------------------------------------------------------------------
    crossPixels.show();
    //---------------------------------------------------------------------------------
    //1st LED off
    crossPixels.setPixelColor(crossFlicker, crossPixels.Color(255, 0, 0));
    crossPixels.setPixelColor(59 - crossFlicker, crossPixels.Color(255, 0, 0));
    crossPixels.setPixelColor(119 - crossFlicker, crossPixels.Color(255, 0, 0));
    crossPixels.setPixelColor(60 + crossFlicker, crossPixels.Color(255, 0, 0));
    //2nd LED off
    crossPixels.setPixelColor(crossFlicker + 1, crossPixels.Color(255, 0, 0));
    crossPixels.setPixelColor(58 - crossFlicker, crossPixels.Color(255, 0, 0));
    crossPixels.setPixelColor(118 - crossFlicker, crossPixels.Color(255, 0, 0));
    crossPixels.setPixelColor(59 + crossFlicker, crossPixels.Color(255, 0, 0));
    //3rd LED off
    crossPixels.setPixelColor(crossFlicker + 2, crossPixels.Color(255, 0, 0));
    crossPixels.setPixelColor(57 - crossFlicker, crossPixels.Color(255, 0, 0));
    crossPixels.setPixelColor(117 - crossFlicker, crossPixels.Color(255, 0, 0));
    crossPixels.setPixelColor(58 + crossFlicker, crossPixels.Color(255, 0, 0));
    //---------------------------------------------------------------------------------
    crossPixels.show();
    crossFlicker += 3;
  }
}
```


