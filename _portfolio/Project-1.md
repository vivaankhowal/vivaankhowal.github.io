---
layout: project
title: "Neopixel Lightsaber"
thumbnail: /images/lightsaber-logo.jpg
---
## About
This is a real working Neopixel Lightsaber. This project is a culmination of my passion for engineering, design, and science fiction. Inspired by Star Wars, I set out to build a fully functional, movie-accurate lightsaber from scratch by combining 3D printing, embedded electronics, and programming to bring a legendary piece of technology to life. Over several versions, I refined every aspect of the build, from mechanical design and internal wiring to software configuration and blade effects, learning how to merge aesthetics with real engineering functionality along the way. This is the project that kickstarted my journey as an engineer.

The lightsaber has motion reactive sound, different ignition techniques like sharp-twisting the hilt, a button ignition, a swing ignition, or a stab ignition. It also has 14 different colors and modes, with some of them as basic colors and others as characterm modes based off of real Star Wars characters like Luke Skywalker and Obi Wan Kenobi.

## CAD Design
I designed all the CAD files in Onshape, the 3D modeling software I use for all my projects. Each component of the lightsaber was printed on my Prusa Mk4 using a mix of PETG and PLA filaments in various colors. The hilt was carefully engineered to strike a balance between sleekness and strength—light and thin enough for comfortable swings, yet durable enough to handle light dueling and solid impacts without compromising its structure. The parts of the hilt are threaded for easy assembly/disassembly, and the electronics were put inside a custom chassis for easy removable and problem diagnosis.

The hilt was designed entirely from scratch and refined through several iterations to achieve the perfect balance of form and function. The internal chassis, however, was based on an open-source CAD model that I found online, which I then modified extensively to accommodate my specific components and wiring layout. I don’t take credit for the original chassis design. Here’s the link to the source: 
<a href="https://www.printables.com/model/216477-universal-1in-lightsaber-chassis/files">Click here</a>
<p></p>

In case you would like to see the full CAD by yourself, here is the link to the CAD file: 
<a href="https://cad.onshape.com/documents/8b1e48b3216e106fd44b4235/w/fd17460cb4c97ed501f7c7bf/e/7a5b61b84dcedab8b72c5028?renderMode=0&uiState=67bb879c9945893e9448000c">Click here</a>

## Hardware and Electronics
For the mechanical design, I wanted the hilt to feel authentic. Something that looked and handled like a real Star Wars lightsaber. I modeled and 3D-printed the entire body from scratch, securing each part using built-in threads for strength and adding a screw on cap at the base to protect the speaker while keeping the design clean and functional.
<p></p>
Inside, the real magic happens. Instead of a simple Arduino, I used a Proffieboard, a custom soundboard built specifically for Neopixel lightsabers. With its built-in gyroscope and accelerometer, every swing and clash comes to life with motion-reactive effects. The blade itself uses two WS2812B LED strips inside a 40-inch polycarbonate tube, wrapped in diffusive film and foam to spread the light evenly and create that bright, movie-accurate glow.
<p></p>
The hilt features one control button with various types of inputs triggering various actions. Power comes from a 3.7 V 18650 Li-ion battery with a 3500 mAh capacity and despite drawing around 2.5 A, the saber can run for just over an hour on a single charge. Finally, a 2 W 8 Ω speaker delivers the signature hum and clash sounds, all within a 3D printed chassis to keep everything protected.
<p></p>
<img src='/images/lightsaberwiring.png' width='600' height='auto'> 

## Software
<p></p>
This part of the build was by far the most challenging, and the most rewarding. The Proffieboard runs on a customized version of C++, and getting it to work perfectly with my specific setup required a lot of careful configuration. Unlike plug-and-play boards, the Proffieboard demands precision: every LED, button, and sound effect must be defined in code before it all comes together.
<p></p>
The process began by flashing the board’s custom operating system using a special config file. After that, I uploaded the actual configuration file for the saber through the Arduino IDE, defining every feature and behavior. Finally, I loaded all the sound fonts and effects onto a micro-SD card, which the board reads during runtime.
<p></p>
Each Proffie config file is broken into four key sections. The first defines the hardware and features: everything from motion sensors to volume. The next defines color presets, specifying the exact color, sound font, and visual effect for each blade style. The third section outlines blade presets, telling the board what type of LED strips I used, how many LEDs are on each, and which pins they connect to. The last part defines button behavior, mapping out what each input does.
<p></p>
After hours of trial, tweaking, and re-uploads, seeing the blade ignite exactly how I imagined: perfect color transitions, synchronized sound, and responsive motion made it all worth it.

Click here to see the full config file:
<a href="/images/Proffie Config.rtf" download>
  Full Config File
</a>

## Version 1
The very first version of the lightsaber was more of an experiment than a finished product. Built entirely out of cardboard and foam, it was held together with duct tape and powered by an Arduino Uno connected through a maze of jumper wires and a breadboard.
<p></p>
It barely held itself together—connections came loose constantly, and the hilt couldn’t even be swung without something falling apart. But despite its rough edges, it was the project that started it all. This early prototype taught me the basics of mechanical and electrical engineering, and it also taught me just how much work goes into making even a simple circuit function reliably.
<p></p>
<img src='/images///Lightsaber V1.jpeg' width='300' height='auto'> 
<img src='/v1nl.png' width='600' height='auto'> 
<p></p>
Here is the Arduino Code for V1:
<p></p>
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

## Version 2
The second version was where things finally started to feel real. The hilt was now 3D-printed, giving the saber a solid structure compared to the cardboard and foam mess of Version 1. It was still held together with duct tape, but it was undeniably stronger and far more functional.
<p></p>
This time, the Arduino Uno was swapped out for a Nano, the code was cleaner, and the entire circuit was soldered instead of being precariously wired through a breadboard. The new design even featured three blades in a Kylo Ren-style layout, giving it a dramatic flair and a real sense of power when lit.
<p></p>
But while the improvements were massive, it was far from perfect. The saber was incredibly heavy, with terrible weight distribution that made it awkward to hold. The hilt had no aesthetic design—just a rough, functional shell meant to test what was possible. Still, this version proved that I could take an idea from prototype to something that actually worked, even if it wasn’t yet beautiful.
<p></p>
<img src='/images//IMG_5417.jpg' width='300' height='auto'> 
<img src='/IMG_0195.JPEG' width='300' height='auto'>

## Takeaways
If there’s one thing this project taught me, it’s that every build comes with failure—and that’s okay. You don’t always get it right on the first try, and sometimes your best lessons come from watching something fall apart. Version after version, I learned that a failure doesn’t define the outcome of a project or the skill of its creator. What truly matters is how you respond to it, what you learn from it, and how you use that experience to build something stronger, smarter, and better the next time.
