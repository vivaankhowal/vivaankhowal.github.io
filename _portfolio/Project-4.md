---
layout: project
title: "A.R.T.I.S"
thumbnail: /images/artis-logo.png
collection: Portfolio
---

## About 
ARTIS (Automated Robotic Task and Interaction System) is my ongoing project to build a fully functional 5-axis robotic arm from scratch. The goal of this project is to create a compact and modular robot arm capable of performing small tasks like picking, placing, or interacting with different tools. It’s a hands-on way for me to explore robotics, combining mechanical design, electronics, and programming into one complete system.

The first version of ARTIS used magnets for the swappable tool head, allowing the arm to quickly switch between different attachments. WThe concept didn't work as well as I wanted, especially when lifting heavier tools. Still, that version performed well enough to win second place at a regional tech competition in the robotics category, which gave me the confidence to keep improving it. I’m now working on version two, which is still in the brainstorming and CAD design phase. This new version focuses on improving stability, range of motion, and the tool-changing system, with the goal of making ARTIS both stronger and more accurate than before.
<p></p>
<img src='/images/12.jpeg' width='300' height='auto' >
<img src='/images/15.jpg' width='400' height='auto' >


## CAD Design
For the first version, I used Onshape to model and assemble every part of the arm. Not all of the components were originally designed by me — I found a YouTube tutorial that featured an excellent base model for a basic robotic arm and provided the design files for it. I used that design as a foundation and made several small modifications to fit my own components, such as changing mount sizes, adjusting hole placements, and modifying the end effector to support my magnetic tool system. This helped me understand how complex mechanical assemblies are structured while still allowing room for customization and learning through hands-on design.
<p></p>
<img src='/images/17.png' width='400' height='auto' >
<img src='/images/18.png' width='300' height='auto' >
<img src='/images/19.png' width='300' height='auto' >
<p></p>
Here is a link to the youtube video:
<a href="https://www.youtube.com/watch?v=5toNqaGsGYs">Click here</a>
<p></p>
Here is a link to the full Onshape file:   
<a href="https://cad.onshape.com/documents/480c6564d0a9bb7166641c2c/w/67f09156e144a437a962df56/e/65f4a082516fa305956dbb93?renderMode=0&uiState=67c1055c314dbd1db302e618">Click here</a>
<p></p>
For version 2, The goal is to design every single part on my own. So far this is the progress that I have made and am still tweaking it.
<p></p>
<img src='/images/artis-logo.png' width='400' height='auto' >


## Hardware and Electronics
The first version of ARTIS was built almost entirely from 3D printed parts using white PLA filament, joined together with various screws and bolts. The base and joints were designed in CAD and printed to house four 20 kg high-torque servo motors, which provided motion for the main axes of the arm. These servos were connected to a PCA9685 PWM Servo Driver Board, controlled by an ESP32 microcontroller that handled the movement logic and sequencing. For the modular tool head, I experimented with neodymium magnets as both a physical connector and a way to transfer data and power. Each magnet had wires soldered to the back in a specific formation to align the connections correctly. While this idea worked in theory, in practice it was inconsistent, as the magnets needed perfectly flat contact to function properly and lost some strength during soldering. The electronics also included a buzzer for system feedback, an L293N motor driver, and a level shifter to allow communication between components operating at different voltages. Everything was neatly soldered to a perfboard, with wires running cleanly underneath and along the sides. Power came from a USB cable plugged into an outlet, with a simple switch to cut power when needed.

For version two, I’m taking a different approach by switching from servos to stepper motors for more precise control and smoother motion. To make the build more challenging and resourceful, I’m reusing components from an old Ender 3 Pro 3D printer, including the stepper motors, aluminum extrusions, and possibly some of the mechanical mounts. This version will also include a camera module for basic vision tracking, allowing the arm to identify and interact with objects in its workspace. I’m still in the process of designing a new tool swapping mechanism, aiming for a system that’s stronger, more reliable, and easier to align than the magnetic connectors from version one. Overall, V2 will be a major upgrade, combining recycled hardware with new features to make ARTIS smarter, faster, and more capable than before.
<p></p>
V1 Circuit:
<p></p>
<img src='/images/16.jpeg' width='450' height='auto' >
<p></p>
Current progress on the V2 circuit:
<p></p>
<img src='/images/v2circuit.jpeg' width='450' height='auto' >


## Design Flaws
The idea behind the first version of ARTIS was solid, but the overall design had several flaws that limited its performance. Some of the arm’s joints and brackets didn’t fit perfectly flush when assembled, which caused small alignment issues and made movements less precise. The servo motors also struggled to lift the arm consistently because the design was heavier than expected, especially near the upper joints.

The magnetic connection system I used for transferring power and data to the modular tool heads seemed promising at first but didn’t work as well in practice. The magnets required perfectly flat contact surfaces to function properly, and even slight misalignment caused weak or unstable connections. I also learned that soldering wires directly to the magnets reduced their magnetic strength, something I hadn’t realized could happen. I plan to address all of these problems in the next version by improving the joint alignment, rethinking the weight balance, and designing a more reliable tool connection system.

