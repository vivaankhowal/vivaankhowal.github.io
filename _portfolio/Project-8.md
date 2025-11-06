---
layout: project
title: "Arcade Cabinet"
thumbnail: /images/arcadecabinet-logo.png
collection: Portfolio
---

## About
The Arcade Cabinet project is a work in progress that combines my love for classic games with my passion for building things from scratch. I’ve always admired the style and atmosphere of old school arcades. This project is my attempt to bring that experience back, but with a modern twist using my own custom design and electronics.

The cabinet is powered by a Raspberry Pi running RetroPie, capable of emulating hundreds of classic arcade and console titles. Every part of the system, from the control layout to the wiring and display, is being designed and assembled by hand. The cabinet will feature dual joysticks, illuminated arcade buttons, a high quality display, and built in speakers for a full authentic experience. Even though it’s still under construction, the current setup already runs games smoothly and delivers that nostalgic arcade feel. The final version will include a fully enclosed 3D printed or wooden housing, custom LED lighting, and detailed artwork to complete the retro look. I'm still currently working on this project, and I'll definitely update this portfolio when its done.
<p></p>
<img src='/IMG_2657.jpeg' width='400' height='auto'>


## Hardware
The arcade cabinet is built around a Raspberry Pi, which acts as the main computer and runs RetroPie to play classic arcade and console games. The Pi connects to a 22 inch monitor through HDMI, giving a bright and sharp display for all the games. Power for the monitor and Raspberry Pi comes from a power strip plugged into a wall outlet, keeping everything simple and easy to manage. A separate 5 V power supply is used just for the LED lights, so the lighting system doesn’t affect the rest of the electronics.

For the controls, the cabinet has two joystick setups, one blue and one red, each with six large arcade buttons for playing games, plus smaller buttons for start and menu functions. All of these are wired to a USB encoder, which lets the Raspberry Pi recognize them as normal game controllers. The buttons are LED lit, which makes the control panel glow just like a real arcade machine.

Sound comes from a pair of large speakers powered by a small amplifier board, giving the games strong, full audio. The cabinet also includes LED strips for accent lighting, making it stand out even more when powered on. Every wire and connection is arranged neatly to keep the inside organized and easy to work on. Altogether, the hardware brings together the feel of a classic arcade machine with a simple, modern setup that’s fun to build and play.
<p></p>
<img src="/IMG_2651.jpeg" width="400" style="transform: rotate(-90deg);">
<p></p>
In case you would like to see the full CAD by yourself, here is the link to the CAD file: 
<a href="https://cad.onshape.com/documents/01c772af6ef6c2be564f568c/w/ae22e4b72d2e69db0891c8ea/e/ea388e1eb770b64d4539973f?renderMode=0&uiState=690d2c4c2636fc5039e0416d">Click here</a>

## Software
The arcade cabinet runs on RetroPie, a lightweight Linux-based operating system designed for retro gaming. Installed on the Raspberry Pi, RetroPie allows seamless emulation of classic arcade and console systems such as NES, SNES, Sega Genesis, Atari, and more. The software automatically detects the USB encoder boards connected to the joysticks and buttons, allowing them to function as standard game controllers without any extra configuration. Through the EmulationStation interface, the system provides an easy-to-navigate menu where each game can be browsed and launched with a single press.

All of the games and emulators are stored on a USB stick connected to the Raspberry Pi, and additional ROMs can be added via USB or Wi-Fi transfer. The button layout and joystick mappings were customized through RetroPie’s input configuration menu, ensuring that both players have accurate and responsive controls. Audio is handled through the Pi’s 3.5 mm output and amplified by a small stereo module that powers the two built-in speakers. The software setup also includes startup scripts to automatically launch the RetroPie interface at boot, creating a plug-and-play experience. Once the physical cabinet is complete, the software will be expanded with custom themes, startup animations, and LED lighting effects synchronized with gameplay to give it that true arcade feel.

In case you would like to see Retropie or learn more, here is the link: 
<a href="https://retropie.org.uk/">Click here</a>

