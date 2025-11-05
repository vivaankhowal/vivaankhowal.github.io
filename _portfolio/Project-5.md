---
layout: default
title: "Clocks"
thumbnail: /images/10.jpeg
excerpt: "I've always wanted to build a clock for my room, but I built not one but two clocks. Here are the 2 clocks I built!
<br/>
<br/>
<img src='/images/25.jpeg' width='400' height='auto'>"
collection: Portfolio
---
## About
I've built 2 clocks: one of those was mostly electrical and mechanical, while the other was more of a CAD Design challenge. The one on the right is a Hollow Clock since the actual clock itself is hollow, and the one on the left is a Brake Disk clock: a clock modeled to look like the brake disk of a sports car.
<p></p>
<img src='/images/25.jpeg' width='400' height='auto'>

## Brake Disk Clock
This was almost entirely a CAD Design Challenge instead of an electrical or mechanical project. I saw this idea online where other people made theirs by welding metal or using carbon fiber, but I made mine using 3D-printed parts made from various colors of PLA. For the movement, I used a standard quartz clock module powered by a single 1.5V AA Battery.
<p></p>
<img src='/images/24.jpeg' width='300' height='auto'>
<img src='/images/20.png' width='300' height='auto'>
<img src='/images/21.png' width='300' height='auto'>
<img src='/images/22.png' width='300' height='auto'>

## Hollow Clock
This was mostly mechanical and electrical instead of CAD like the other clock, which is why the CAD files I used for the clock weren't designed by me. I saw this idea online as well and found an instructable page on how to build one, but I made some changes to the design to fit the electrical components that I used and edited the mechanical design to make it quieter since it was too loud at first.
<p></p>
For the electrical parts, I used an Arduino Nano as the microcontroller and a 28BYJ-48 Stepper Motor connected to a ULN2003 Stepper Driver Module. For the mechanical aspect, the way it works is there is a worm gear connected to a spur gear in a 90-degree angle in a 1:12 gear ratio. Inside the main clock, there are 2 big gear rotors: one connected to the worm gear for the hours, and the other connected to the spur gear for the minutes. In the code for the clock, the stepper motor is set to move a certain number of steps every minute. The hour hand also levitates because there are Neodymium magnets inside the hand and in the rotor.
<p></p>
(Close up picture of the levitating hour hand)
<p></p>
<img src='/images/27.jpeg' width='300' height='auto'>
<p></p>
Here is the link to the Instructables page:
<a href="https://www.instructables.com/Hollow-Clock-V/">Click here</a>
<p></p>
<img src='/images/23.jpeg' width='300' height='auto'>
<img src='/images/26.jpeg' width='300' height='auto'>
<p></p>
Here is the code:
<p></p>
```cpp
#include <Stepper.h>
#define STEPS_PER_REV 2048 
#define MOTOR_SPEED 10      

Stepper stepper(STEPS_PER_REV, 8, 10, 9, 11);

unsigned long previousMillis = 0;
const unsigned long interval = 60000;  // 1 minute in milliseconds
int currentMinute = 0;
void setup() {
    Serial.begin(9600);
    stepper.setSpeed(MOTOR_SPEED);
}
void loop() {
    unsigned long currentMillis = millis();
    if (currentMillis - previousMillis >= interval) {
        previousMillis = currentMillis;
        int stepsPerMinute = STEPS_PER_REV / 60;  // 60 minutes = full rotation
        stepper.step(stepsPerMinute);
        currentMinute++;
        if (currentMinute >= 60) {
            currentMinute = 0; // Reset at 60 minutes
        }
        Serial.print("Minute: ");
        Serial.println(currentMinute);
    }
}
```


