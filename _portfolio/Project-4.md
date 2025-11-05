---
layout: default
title: "A.R.T.I.S"
thumbnail: /images/10.jpeg
excerpt: "This was the hardest project I'd ever attempted, but also the coolest. I've always wanted a robot arm in my room but I could always use a ton of hands for different tasks, so why not build a robot that does it all?
<br/>
<br/>
<img src='/images/12.jpeg' width='400' height='auto' >"

collection: Portfolio
---

## About 
Although this is the hardest project I've ever attempted, it also won me 2nd place at the Lanier Regional Tech Competition! It's a multipurpose robot arm where the head of the claw can be swapped out with these pre-made modules to complete different tasks. This is also the first project that could have some real-world potential in the industry. Imagine having a robot arm that can do multiple tasks instead of a robot for every task. It could save a lot of money and time! 
<p></p>
<img src='/images/12.jpeg' width='300' height='auto' >
<img src='/images/15.jpg' width='400' height='auto' >


## CAD Design
For the 3D model, I used Onshape again to model every part, but this time not all the parts were designed by me. I found this youtube video that had a great design for a robot arm, and it provided the files for it as well. I used this design and slightly tweaked it to fit my needs.
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

## Hardware and Electronics
For the hardware, I used all 3D printed parts using white PLA filament and various screws. For the module atatchment and data transfer, I used Neodymium magnets with wires soldered to the backs of them in a specific formation for the correct data transfer. For the electronics, I used 4 high torque servo motors connected to a PCA9685 PWM Servo Driver Board and is controlled by an ESP-32 for code. The additional components are a buzzer for sounds, a L293N Motor driver, a Level shifter to interface components with different operating voltages. They are all soldered on a perfboard with wires running under it and the sides. It's powered with a USB cable to plug in to an outlet, and a switch for power cut off.
<p></p>
<img src='/images/16.jpeg' width='450' height='auto' >

## Design Flaws
The idea of the robot was good, but the overall design wasn't great. Parts of the arm weren't flush when screwed together and the servo motors would have a hard time of lifting it. The magnets I used to transmit the data to the module components didn't work as well because the surface would need to be perfectly flat to work properly. They also lost some of their strength when I soldered wires to it.

