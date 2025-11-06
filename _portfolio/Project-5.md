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
<p></p>
<img src='/images/clockwiring.png' width='500' height='auto'>
<p></p>
In case you would like to see the full CAD by yourself, here is the link to the CAD file: 
<a href="https://cad.onshape.com/documents/c05fe77fce817ccd58ccb914/w/9b64295ff5e8a64c2e3bee27/e/1dd29dc51b94e47abc77f13b">Click here</a>

## Software
In all of my projects, this is the one with the most complicated software. The software for the Arc Reactor Clock is written in Arduino C++ and runs on the ESP32-C3 Mini. It controls the clock display, LED animations, and alarm behavior while allowing wireless configuration through Bluetooth Low Energy (BLE). When powered on, the ESP32 initializes the TM1637 display to show the current time and starts the NeoPixel animations based on the selected mode, such as a solid color or a smooth rainbow effect. The code keeps track of time internally using the ESP32’s built-in clock and updates the display every second. The alarm system is handled in software by comparing the current time to a stored alarm time, triggering the buzzer and LED flashing pattern when they match.
<p></p>
The BLE logic allows the user to control and customize the clock using their phone or computer. Once connected via Bluetooth, the ESP32 listens for text-based commands that let the user adjust settings in real time. For example, sending "settime YYYY-MM-DD HH:MM:SS" updates the current time, "setcolor R,G,B" changes the LED color, and "setmode rainbow" activates the rainbow animation. There are also commands to set or stop the alarm. When the alarm is active, the LEDs flash green while the buzzer pulses, and pressing the physical button or sending a Bluetooth command stops the alarm and restores the previous lighting mode. This setup makes the Arc Reactor Clock fully interactive, combining the look of an Iron Man reactor with the flexibility of smart Bluetooth control.
<p></p>

Here is the full code:
<a href="/images/ArcReactorClockCode copy.ino" download>
  Click Here

