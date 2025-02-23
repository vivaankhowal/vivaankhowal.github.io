---
title: "Neopixel Lightsaber"
excerpt: "This is my first and my favorite electrical engineering project. It's a Neopixel Lightsaber with LED strips in the blade and a bunch of cool features that bring one of the most iconic pieces of tech in sci-fi to life!
<br/>
<br/>
<img src='/images/Lightsaber in Dark.jpeg' width='400' height='auto'>"
collection: Portfolio
---
## About
This is the first electrical project I attempted and also my favorite. It's a Neopixel Lightsaber with many colors and effects like the ones on the big screen. This originally started as a group project but after building the first version for the project, it didn't live up to the vision I had for this project. So I took it upon myself to rebuild it again and again so it matches my vision almost perfectly.
<p></p>
This was the first version that our group built for the project. It featured 2 WS2812b LED strips connected to an Arduino Nano via a breadboard with a momentary button, a 10,000 Ohm Resistor for control, and a 9V battery for power hooked up to a switch. For the hilt, everything was held with glue and tape, and a cardboard hilt as a base, Although it did light up, it was extremely delicate, and almost impossible to do certain things like swing it around or duel.
<p></p>
<img src='/images/Lightsaber V1.jpeg' width='200' height='auto'> <img src='/v1nl.png' width='500' height='auto'>
<p></p>
This is the second version I attempted all on my own. I took every aspect of the previous lightsaber and upgraded it in terms of durability, power, and looks. The electronics for this one feature an Arduino Nano instead of an Uno hooked up to 3 LED strips instead of 2 (One strip is for the side blades). For power, there is a 5V power bank with a much higher battery capacity and the matching voltage for the other electronics. It also features a custom LED button for ignition and a switch to turn the power bank on and off and actual soldering instead of heat shrinks. It also features a custom 3D-printed hilt instead of a cardboard one. Although it was a huge upgrade and was much tougher, it still wasn't as good as I wanted it to be and had a bunch of issues like being extremely heavy.
<p></p>
<img src='/images/IMG_5416.jpg' width='300' height='auto'>
<img src='/images/IMG_5417.jpg' width='300' height='auto'>
<img src='/images/IMG_0196.JPEG' width='300' height='auto'>
<p></p>
For the third and final version, I didn't hold anything back. I poured a ton of effort into research for the components, the features, the power, and even the actual design of the hilt. This version also features a 3D-printed hilt but it is modeled after an actual Star Wars character. This one features a ton of features as it does in the movie like different colors, swinging sound effects, cool ignition and deactivation effects, and even tip drag and flash on clash. There were still a few minor adjustments required so I made a more refined version later by making it more durable and fixing the soldering on the wires, but I kept the same hilt design. This was the version that finally satisfied my vision and did exactly what I wanted it to do.
<p></p>
<img src='/images/IMG_5491.jpg' width='300' height='auto'>
<p></p>
This is the absolute final refined version I talked about previously:

## CAD Design
For the CAD files, I used a 3D modeling software called Onshape to design everything. It's the software I use to design all my projects. Every part of the lightsaber is printed using different colors of PLA filament, printed on a Prusa Mk4 3D printer I have at home. I've designed the hilt to be sleek and thin to avoid being too heavy and easy to swing but also strong enough to withstand heavy hits or dueling. Here are some photos of the 3D Models:
<p></p>
<img src='/images/1.png' width='150' height='auto'>
<img src='/images/2.png' width='200' height='auto'>
<img src='/images/3.png' width='200' height='auto'>
<img src='/images/4.png' width='200' height='auto'>
<img src='/images/5.png' width='200' height='auto'>

In case you would like to see the full model by yourself, here is the link to the CAD file:
<a href="https://cad.onshape.com/documents/8b1e48b3216e106fd44b4235/w/fd17460cb4c97ed501f7c7bf/e/7a5b61b84dcedab8b72c5028?renderMode=0&uiState=67bb879c9945893e9448000c">Click here</a>

## Hardware and Electronics
For the mechanical parts, I used a 3D-printed hilt modeled after an actual lightsaber of a star wars character and uses screws to hold together each part. The cap at the bottom is a friction-fit cap to cover the speaker. For the electronics, instead of using an Arduino, I used a custom soundboard with a built-in gyroscope and accelerometer called a Proffieboard made specifically for Neopixel Lightsabers. It uses the same 2 WS2812b LED strips for the lights and a 40-inch polycarbonate tube with special diffusive film and packing foam to diffuse the light. It uses 2 momentary buttons instead of 1: one button for activating the saber, and the other for changing colors or blaster effects. For power it uses a 3.7V 18650 Lithium Ion battery with a capacity of 3500mAh connected to a 5A kill switch to cut the power if needed, and with the saber having a current draw of around 2.5A, the saber runs for a little more than 1 hour before the battery runs out. For sound, it uses a 3W 4Ω Speaker and the board has a built-in resistor to avoid short circuits. Here is the wiring diagram:
<p></p>
<img src='/images/6.png' width='600' height='auto'>
<p></p>

## Software
<p></p>
This was easily the hardest part of this project. The biggest downside about using a Proffieboard is it takes a lot of setup  to code it to your specific components.
<p></p>
```cpp
#include <iostream>

int main() {
    std::cout << "Hello, World!" << std::endl;
    return 0;
}
```




