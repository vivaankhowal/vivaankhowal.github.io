---
layout: project
title: "F1 Wheel Clock"
thumbnail: /images/10.jpeg
collection: Portfolio
---
## About
This is the first electrical project I attempted and also my favorite. It's a Neopixel Lightsaber with many colors and effects like the ones on the big screen. This originally started as a group project, but I loved the idea and wanted to keep improving it. This was the first version that our group built for the project. It features a cardboard hilt with Neopixel LED strips in the blade and is controlled by an Arduino Uno R3.
<p></p>
<img src='/images/Lightsaber V1.jpeg' width='300' height='auto'>

## Design Flaws
Although it did technically work as a lightsaber, there were a lot of problems with it. Since everything was held with glue and tape, it wasn't nearly strong enough for any dueling and the cardboard hilt looked very makeshift and scrappy. None of the components were bolted down properly so you couldn't properly swing it without the fear of everything falling out. It was powered using a 9V Battery and since the LED strips were 5V, there was a constant risk of overheating and circuit damage (the saber would stay on for a minute max before the Arduino would heat up). The components were also too big to properly fit in the lightsaber hilt which also made it very bulky. There were a lot of problems with this version, but the important thing is there is a lot of room for improvement.

## Hardware and Electronics
For the mechanical parts, it featured a 32" polycarbonate blade with two 5V WS2812b LED strips placed back to back and packing foam on the inside to diffuse the light. The hilt was made entirely out of cardboard and duct tape and used double-sided tape and velcro straps to secure everything in place. For electronics, all the code runs through an Arduino Uno R3 hooked up to a breadboard with jumper wires. For control, there is a tactile button for ignition connected to a 10K Ω resistor. For power, there is a 9V battery connected to a power switch (The switch doesn't show up in the diagram because that component wasn't available in the software I used for the circuit diagram). 
<p></p>
<img src='/v1nl.png' width='500' height='auto'>

## Software
For the software, I used the Arduino IDE to upload the code. It's relatively simple, just detects the inputs from the button and turns on the saber accordingly. Here is the code:
```cpp
#include <Adafruit_NeoPixel.h>

#define PIN 3    // input pin Neopixel is attached to
#define NUMPIXELS 5 // number of neopixels in strip

Adafruit_NeoPixel pixels = Adafruit_NeoPixel(NUMPIXELS, PIN, NEO_GRB + NEO_KHZ800);

int delayVal = 5; // timing delay in milliseconds
int ledState = 0;
int buttonStateNew;
int buttonPin = 2;
int buttonStateOld = 1;

void setup() {  
  pixels.begin();
  pinMode(buttonPin, INPUT_PULLUP); 
}

void loop() {
  buttonStateNew = digitalRead(buttonPin);
  
  if (buttonStateOld == 0 && buttonStateNew == 1) {  // Button press detected
    if (ledState == 0) {
      for (int i = 0; i < NUMPIXELS; i++) {
        pixels.setPixelColor(i, pixels.Color(255, 0, 0));
        pixels.show();
        delay(1);
      }
      ledState = 1;
    } 
    else { 
      for (int i = NUMPIXELS - 1; i >= 0; i--) {
        pixels.setPixelColor(i, pixels.Color(0, 0, 0));
        pixels.show();
        delay(1);
      }
      ledState = 0;
    }
  }

  buttonStateOld = buttonStateNew; // Fix: Always update button state
}
```


