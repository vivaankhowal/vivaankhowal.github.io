---
title: "Neopixel Lightsaber V2"
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
For the mechanical parts, there is a 3D-printed hilt and each part of the hilt is held together with duct tape. For the blades, there are 3 different-sized blades: a 40" Polycarbonate blade and 2 smaller blades for the sides. For the electronics, there are 3 Neopixel LED strips instead of 2: 2 for the main blade, and 1 for the smaller blades. The code runs off an Arduino Nano soldered to a wire with a USB connecter on the other end to plug into a 5V power bank with 5000 mAh, and since this saber has a current draw of about 3.2 Amps, the saber runs for about 1.5 hours before it needs to be recharged.

## Software
For the software, I used the Arduino IDE to upload all my code. 
```cpp
#include <Adafruit_NeoPixel.h>
#include <SoftwareSerial.h>
```


