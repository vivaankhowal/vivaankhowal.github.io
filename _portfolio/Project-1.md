---
layout: project
title: "Neopixel Lightsaber"
thumbnail: /images/lightsaber-logo.jpg
---
## About
This is a real working Neopixel Lightsaber. This project is a culmination of my passion for engineering, design, and science fiction. Inspired by Star Wars, I set out to build a fully functional, movie-accurate lightsaber from scratch by combining 3D printing, embedded electronics, and programming to bring a legendary piece of technology to life. Over several versions, I refined every aspect of the build, from mechanical design and internal wiring to software configuration and blade effects, learning how to merge aesthetics with real engineering functionality along the way. This is the project that kickstarted my journey as an engineer.

## CAD Design
I designed all the CAD files in Onshape, the 3D modeling software I use for all my projects. Each component of the lightsaber was printed on my Prusa Mk4 using a mix of PETG and PLA filaments in various colors. The hilt was carefully engineered to strike a balance between sleekness and strength—light and thin enough for comfortable swings, yet durable enough to handle light dueling and solid impacts without compromising its structure. The parts of the hilt are threaded for easy assembly/disassembly, and the electronics were put inside a custom chassis for easy removable and problem diagnosis.

The hilt was designed entirely from scratch and refined through several iterations to achieve the perfect balance of form and function. The internal chassis, however, was based on an open-source CAD model that I found online, which I then modified extensively to accommodate my specific components and wiring layout. I don’t take credit for the original chassis design. Here’s the link to the source:
<a href="https://www.printables.com/model/216477-universal-1in-lightsaber-chassis/files">Click here</a>
<p></p>

In case you would like to see the full CAD by yourself, here is the link to the CAD file:
<a href="https://cad.onshape.com/documents/8b1e48b3216e106fd44b4235/w/fd17460cb4c97ed501f7c7bf/e/7a5b61b84dcedab8b72c5028?renderMode=0&uiState=67bb879c9945893e9448000c">Click here</a>

## Hardware and Electronics
For the mechanical parts, I used a 3D-printed hilt modeled after an actual lightsaber of a star wars character and uses screws to hold together each part. The cap at the bottom is a friction-fit cap to cover the speaker. For the electronics, instead of using an Arduino, I used a custom soundboard with a built-in gyroscope and accelerometer called a Proffieboard made specifically for Neopixel Lightsabers. It uses the same 2 WS2812b LED strips for the lights and a 40-inch polycarbonate tube with special diffusive film and packing foam to diffuse the light. It uses 2 momentary buttons instead of 1: one button for activating the saber, and the other for changing colors or blaster effects. For power, it uses a 3.7V 18650 Lithium Ion battery with a capacity of 3500mAh connected to a High Amp kill switch to cut the power if needed, and with the saber having a current draw of around 2.5A, the saber runs for a little more than 1 hour before the battery runs out. For sound, it uses a 3W 4Ω Speaker and the board has a built-in resistor to avoid short circuits. Here is the wiring diagram:
<p></p>
<img src='/images/6.png' width='600' height='auto'>
<p></p>

## Software
<p></p>
This was by far the hardest part of this project. The Proffieboard is coded using an edited version of C++, and the biggest downside about using a Proffieboard is it takes a lot of setup to code it to your specific components. To code the board, you first have to upload its custom OS using a specific file that is specific to each version of the board. In my case, it was this file, "proffieboard_v3_config.h". Then you have to upload the actual config file for the saber which is shown below through the Arduino IDE. Finally, you need to upload all the sound files through an SD Card which is inserted on the board. Proffieboard config files are just the actual code you upload to the board and they all have 4 parts: The top which is where you define all the hardware and the features you want your blade to have, the color presets which is where you define the exact color of your blade; the sound effect it should use, and any effects it should have like a ripple or a flashing effect; the blade presets which is where you define the exact model of LED strips you used, the number of LEDs in each strip, and what pin on the board the LED strip is wired to; and the button presets which is where you define all the buttons your saber has, what pin they are connected to, and what each button should do. Here is the full code for the Proffieboard:

## Improvements
The new hilt has a lot better design in terms of style and functionality and uses actual screws to hold it together instead of tape. The electronics use a custom chassis to stay in one spot instead of being stuffed in the hilt and prone to tangled wires or even short circuits. There is only one blade this time instead of 3 and there is a single battery instead of a power bank, which is a huge reduction in weight from the previous version. There is also a lot more versatility due to the new custom soundboard instead of an Arduino Nano which leads to having many new features and multiple colors.

<p></p>
```cpp
//This is where I define all the features, the number of blades, buttons, the volume, and how many LEDs I used
#ifdef CONFIG_TOP
#include "proffieboard_v3_config.h"
#define NUM_BLADES 2
#define NUM_BUTTONS 2
#define VOLUME 1000
const unsigned int maxLedsPerStrip = 288;
#define CLASH_THRESHOLD_G 1.0
#define ENABLE_AUDIO
#define ENABLE_MOTION
#define ENABLE_WS2811
#define ENABLE_SD
#define yellow Rgb<160, 90, 0>
#define ORANGE Rgb<255, 40, 0>
#define MOTION_TIMEOUT 60 * 10 * 1000
#define FETT263_HOLD_BUTTON_LOCKUP
#define IDLE_OFF_TIME 60 * 10 * 1000
#define DISABLE_BASIC_PARSER_STYLES
#define DISABLE_DIAGNOSTIC_COMMANDS
#define ENABLE_ALL_EDIT_OPTIONS
#define SAVE_PRESET
#define NO_REPEAT_RANDOM
#define COLOR_CHANGE_DIRECT
#define FETT263_EDIT_MODE_MENU
#define FETT263_MULTI_PHASE
#define FETT263_SAY_BATTERY_PERCENT
#define FETT263_LOCKUP_DELAY 200
#define FETT263_BM_CLASH_DETECT 6
#define FETT263_SWING_ON_SPEED 300
#define FETT263_SWING_ON
#define FETT263_SWING_ON_NO_BM
#define FETT263_TWIST_ON_PREON
#define FETT263_TWIST_ON_NO_BM
#define FETT263_THRUST_ON
#define FETT263_THRUST_ON_NO_BM
#define FETT263_STAB_ON
#define FETT263_STAB_ON_NO_BM
#define FETT263_TWIST_OFF
#define FETT263_FORCE_PUSH_ALWAYS_ON
#define FETT263_FORCE_PUSH_LENGTH 5
#define ORIENTATION ORIENTATION_USB_TOWARDS_BLADE
#endif

#ifdef CONFIG_PROP
#include "../props/saber_fett263_buttons.h"
#endif

//This is where I define which colors I want, which sound effects it should play, and what cool effects I want like a fire effect or a ripple
#ifdef CONFIG_PRESETS
Preset presets[] = {
   { "Blue", "tracks/KyloRenTROS.wav",
    StylePtr<InOutSparkTip<EASYBLADE(BLUE, WHITE), 150, 400> >(),
    StylePtr<InOutSparkTip<EASYBLADE(BLUE, WHITE), 150, 400> >(), "blue"},

   { "Green", "tracks/mars.wav",
    StylePtr<InOutHelper<EASYBLADE(GREEN, WHITE), 150, 400> >(),
    StylePtr<InOutHelper<EASYBLADE(GREEN, WHITE), 150, 400> >(), "green"},

   { "Purple", "tracks/KyloRenTROS.wav",
    StylePtr<InOutSparkTip<EASYBLADE(MAGENTA, WHITE), 150, 400> >(),
    StylePtr<InOutSparkTip<EASYBLADE(MAGENTA, WHITE), 150, 400> >(), "purple"},

   { "Orange", "tracks/KyloRenTROS.wav",
    StylePtr<InOutSparkTip<EASYBLADE(ORANGE, WHITE), 150, 400> >(),
    StylePtr<InOutSparkTip<EASYBLADE(ORANGE, WHITE), 150, 400> >(), "orange"},

   { "Yellow", "tracks/KyloRenTROS.wav",
    StylePtr<InOutSparkTip<EASYBLADE(yellow, WHITE), 150, 400> >(),
    StylePtr<InOutSparkTip<EASYBLADE(yellow, WHITE), 150, 400> >(), "yellow"},

   { "Cyan", "tracks/KyloRenTROS.wav",
    StylePtr<InOutSparkTip<EASYBLADE(CYAN, WHITE), 150, 400> >(),
    StylePtr<InOutSparkTip<EASYBLADE(CYAN, WHITE), 150, 400> >(), "cyan"},

   { "Red", "tracks/KyloRenTROS.wav",
    StylePtr<InOutSparkTip<EASYBLADE(RED, WHITE), 150, 400> >(),
    StylePtr<InOutSparkTip<EASYBLADE(RED, WHITE), 150, 400> >(), "red"},

   { "White", "tracks/KyloRenTROS.wav",
    StylePtr<InOutSparkTip<EASYBLADE(WHITE, RED), 150, 400> >(),
    StylePtr<InOutSparkTip<EASYBLADE(WHITE, RED), 150, 400> >(), "white"},

  { "ObiWan 2", "tracks/ObiWan 2.wav",
StylePtr<InOutHelper<OnSpark<Blast<LocalizedClash<Lockup<AudioFlicker<Blue,DodgerBlue>,Pulsing<Gradient<AudioFlicker<Blue,DodgerBlue>,AudioFlicker<Blue,DodgerBlue>,HumpFlicker<OrangeRed,Pink,50>,AudioFlicker<Blue,DodgerBlue>,AudioFlicker<Blue,DodgerBlue>>,Gradient<AudioFlicker<Blue,DodgerBlue>,AudioFlicker<Blue,DodgerBlue>,HumpFlicker<OrangeRed,Pink,50>,AudioFlicker<Blue,DodgerBlue>,AudioFlicker<Blue,DodgerBlue>,AudioFlicker<Blue,DodgerBlue>>,3500>,Gradient<AudioFlicker<Blue,DodgerBlue>,AudioFlicker<Blue,DodgerBlue>,HumpFlicker<Pink,OrangeRed,50>>>,Pink>,Pink>,Pink,400>,200,500>>(),  
StylePtr<InOutHelper<OnSpark<Blast<LocalizedClash<Lockup<AudioFlicker<Blue,DodgerBlue>,Pulsing<Gradient<AudioFlicker<Blue,DodgerBlue>,AudioFlicker<Blue,DodgerBlue>,HumpFlicker<OrangeRed,Pink,50>,AudioFlicker<Blue,DodgerBlue>,AudioFlicker<Blue,DodgerBlue>>,Gradient<AudioFlicker<Blue,DodgerBlue>,AudioFlicker<Blue,DodgerBlue>,HumpFlicker<OrangeRed,Pink,50>,AudioFlicker<Blue,DodgerBlue>,AudioFlicker<Blue,DodgerBlue>,AudioFlicker<Blue,DodgerBlue>>,3500>,Gradient<AudioFlicker<Blue,DodgerBlue>,AudioFlicker<Blue,DodgerBlue>,HumpFlicker<Pink,OrangeRed,50>>>,Pink>,Pink>,Pink,400>,200,500>>(), "0"},  

{ "Luke", "tracks/Luke.wav",
StylePtr<InOutHelper<OnSpark<Blast<LocalizedClash<Lockup<AudioFlicker<Green,Rgb16<0,38402,0>>,Pulsing<Gradient<AudioFlicker<Green,Rgb16<0,38402,0>>,AudioFlicker<Green,Rgb16<0,38402,0>>,HumpFlicker<OrangeRed,Pink,50>,AudioFlicker<Green,Rgb16<0,38402,0>>,AudioFlicker<Green,Rgb16<0,38402,0>>>,Gradient<AudioFlicker<Green,Rgb16<0,38402,0>>,AudioFlicker<Green,Rgb16<0,38402,0>>,HumpFlicker<OrangeRed,Pink,50>,AudioFlicker<Green,Rgb16<0,38402,0>>,AudioFlicker<Green,Rgb16<0,38402,0>>,AudioFlicker<Green,Rgb16<0,38402,0>>>,3500>,Gradient<AudioFlicker<Cyan,DeepSkyBlue>,AudioFlicker<Cyan,DeepSkyBlue>,HumpFlicker<Pink,OrangeRed,50>>>,Pink>,Pink>,Pink,400>,200,500>>(),  
StylePtr<InOutHelper<OnSpark<Blast<LocalizedClash<Lockup<AudioFlicker<Green,Rgb16<0,38402,0>>,Pulsing<Gradient<AudioFlicker<Green,Rgb16<0,38402,0>>,AudioFlicker<Green,Rgb16<0,38402,0>>,HumpFlicker<OrangeRed,Pink,50>,AudioFlicker<Green,Rgb16<0,38402,0>>,AudioFlicker<Green,Rgb16<0,38402,0>>>,Gradient<AudioFlicker<Green,Rgb16<0,38402,0>>,AudioFlicker<Green,Rgb16<0,38402,0>>,HumpFlicker<OrangeRed,Pink,50>,AudioFlicker<Green,Rgb16<0,38402,0>>,AudioFlicker<Green,Rgb16<0,38402,0>>,AudioFlicker<Green,Rgb16<0,38402,0>>>,3500>,Gradient<AudioFlicker<Cyan,DeepSkyBlue>,AudioFlicker<Cyan,DeepSkyBlue>,HumpFlicker<Pink,OrangeRed,50>>>,Pink>,Pink>,Pink,400>,200,500>>(), "0" },

{ "Windu", "tracks/Windu.wav",
StylePtr<Layers<Stripes<16000,-1000,RotateColorsX<Variation,Magenta>,Pulsing<RotateColorsX<Variation,Rgb<128,0,128>>,RotateColorsX<Variation,Magenta>,800>,RotateColorsX<Variation,Magenta>>,TransitionEffectL<TrConcat<TrFade<600>,RandomFlicker<RotateColorsX<Variation,Magenta>,RotateColorsX<Variation,Rgb<128,0,128>>>,TrDelay<30000>,RotateColorsX<Variation,Magenta>,TrFade<800>>,EFFECT_FORCE>,AlphaL<StrobeL<Black,Int<20>,Int<1>>,Scale<IsLessThan<SwingSpeed<600>,Int<13600>>,Scale<SwingSpeed<600>,Int<-19300>,Int<32768>>,Int<0>>>,LockupTrL<Layers<AlphaL<AudioFlickerL<White>,Bump<Scale<BladeAngle<>,Scale<BladeAngle<0,16000>,Int<10000>,Int<30000>>,Int<10000>>,Scale<SwingSpeed<100>,Int<14000>,Int<18000>>>>,AlphaL<White,Bump<Scale<BladeAngle<>,Scale<BladeAngle<0,16000>,Int<10000>,Int<30000>>,Int<10000>>,Int<10000>>>>,TrConcat<TrInstant,White,TrFade<400>>,TrConcat<TrInstant,White,TrFade<400>>,SaberBase::LOCKUP_NORMAL>,ResponsiveLightningBlockL<Strobe<White,AudioFlicker<White,Blue>,50,1>,TrConcat<TrInstant,AlphaL<White,Bump<Int<12000>,Int<18000>>>,TrFade<200>>,TrConcat<TrInstant,HumpFlickerL<AlphaL<White,Int<16000>>,30>,TrSmoothFade<600>>>,ResponsiveStabL<Red,TrWipeIn<600>,TrWipe<600>>,ResponsiveBlastL<White,Int<400>,Scale<SwingSpeed<200>,Int<100>,Int<400>>,Int<400>>,SimpleClashL<White>,LockupTrL<AlphaL<BrownNoiseFlickerL<White,Int<300>>,SmoothStep<Int<30000>,Int<5000>>>,TrWipeIn<400>,TrFade<300>,SaberBase::LOCKUP_DRAG>,LockupTrL<AlphaL<Mix<TwistAngle<>,Red,Orange>,SmoothStep<Int<28000>,Int<5000>>>,TrWipeIn<600>,TrFade<300>,SaberBase::LOCKUP_MELT>,InOutTrL<TrWipe<300>,TrWipeIn<500>,Black>>>(), 
StylePtr<Layers<Stripes<16000,-1000,RotateColorsX<Variation,Magenta>,Pulsing<RotateColorsX<Variation,Rgb<128,0,128>>,RotateColorsX<Variation,Magenta>,800>,RotateColorsX<Variation,Magenta>>,TransitionEffectL<TrConcat<TrFade<600>,RandomFlicker<RotateColorsX<Variation,Magenta>,RotateColorsX<Variation,Rgb<128,0,128>>>,TrDelay<30000>,RotateColorsX<Variation,Magenta>,TrFade<800>>,EFFECT_FORCE>,AlphaL<StrobeL<Black,Int<20>,Int<1>>,Scale<IsLessThan<SwingSpeed<600>,Int<13600>>,Scale<SwingSpeed<600>,Int<-19300>,Int<32768>>,Int<0>>>,LockupTrL<Layers<AlphaL<AudioFlickerL<White>,Bump<Scale<BladeAngle<>,Scale<BladeAngle<0,16000>,Int<10000>,Int<30000>>,Int<10000>>,Scale<SwingSpeed<100>,Int<14000>,Int<18000>>>>,AlphaL<White,Bump<Scale<BladeAngle<>,Scale<BladeAngle<0,16000>,Int<10000>,Int<30000>>,Int<10000>>,Int<10000>>>>,TrConcat<TrInstant,White,TrFade<400>>,TrConcat<TrInstant,White,TrFade<400>>,SaberBase::LOCKUP_NORMAL>,ResponsiveLightningBlockL<Strobe<White,AudioFlicker<White,Blue>,50,1>,TrConcat<TrInstant,AlphaL<White,Bump<Int<12000>,Int<18000>>>,TrFade<200>>,TrConcat<TrInstant,HumpFlickerL<AlphaL<White,Int<16000>>,30>,TrSmoothFade<600>>>,ResponsiveStabL<Red,TrWipeIn<600>,TrWipe<600>>,ResponsiveBlastL<White,Int<400>,Scale<SwingSpeed<200>,Int<100>,Int<400>>,Int<400>>,SimpleClashL<White>,LockupTrL<AlphaL<BrownNoiseFlickerL<White,Int<300>>,SmoothStep<Int<30000>,Int<5000>>>,TrWipeIn<400>,TrFade<300>,SaberBase::LOCKUP_DRAG>,LockupTrL<AlphaL<Mix<TwistAngle<>,Red,Orange>,SmoothStep<Int<28000>,Int<5000>>>,TrWipeIn<600>,TrFade<300>,SaberBase::LOCKUP_MELT>,InOutTrL<TrWipe<300>,TrWipeIn<500>,Black>>>(), "0"},

{ "KyloRenTROS", "tracks/KyloRenTROS.wav",
StylePtr<InOutHelper<OnSpark<Blast<LocalizedClash<Lockup<HumpFlicker<DarkOrange,BrownNoiseFlicker<Red,Black,50>,15>,Gradient<HumpFlicker<DarkOrange,BrownNoiseFlicker<Red,Black,50>,15>,HumpFlicker<DarkOrange,BrownNoiseFlicker<Red,Black,50>,15>,HumpFlicker<Yellow,Green,50>,HumpFlicker<DarkOrange,BrownNoiseFlicker<Red,Black,50>,15>,HumpFlicker<DarkOrange,BrownNoiseFlicker<Red,Black,50>,15>>,Gradient<HumpFlicker<DarkOrange,BrownNoiseFlicker<Red,Black,50>,15>,HumpFlicker<DarkOrange,BrownNoiseFlicker<Red,Black,50>,15>,HumpFlicker<DarkOrange,BrownNoiseFlicker<Red,Black,50>,15>,HumpFlicker<Yellow,Green,50>>>,Pink>,Pink>,Pink,400>,200,500>>(), 
StylePtr<InOutHelper<OnSpark<Blast<LocalizedClash<Lockup<HumpFlicker<DarkOrange,BrownNoiseFlicker<Red,Black,50>,15>,Gradient<HumpFlicker<DarkOrange,BrownNoiseFlicker<Red,Black,50>,15>,HumpFlicker<DarkOrange,BrownNoiseFlicker<Red,Black,50>,15>,HumpFlicker<Yellow,Green,50>,HumpFlicker<DarkOrange,BrownNoiseFlicker<Red,Black,50>,15>,HumpFlicker<DarkOrange,BrownNoiseFlicker<Red,Black,50>,15>>,Gradient<HumpFlicker<DarkOrange,BrownNoiseFlicker<Red,Black,50>,15>,HumpFlicker<DarkOrange,BrownNoiseFlicker<Red,Black,50>,15>,HumpFlicker<DarkOrange,BrownNoiseFlicker<Red,Black,50>,15>,HumpFlicker<Yellow,Green,50>>>,Pink>,Pink>,Pink,400>,200,500>>(), "0"},

  { "Darth Vader 3", "tracks/Darth Vader 3.wav",
StylePtr<InOutHelper<OnSpark<Blast<LocalizedClash<Lockup<AudioFlicker<Red,Rgb16<38402,0,0>>,Gradient<AudioFlicker<Red,Rgb16<38402,0,0>>,AudioFlicker<Red,Rgb16<38402,0,0>>,HumpFlicker<Blue,Cyan,50>,AudioFlicker<Red,Rgb16<38402,0,0>>,AudioFlicker<Red,Rgb16<38402,0,0>>>,Gradient<AudioFlicker<Red,Rgb16<38402,0,0>>,AudioFlicker<Red,Rgb16<38402,0,0>>,HumpFlicker<Blue,Cyan,50>>>,Pink>,Pink>,Pink,400>,200,500>>(),  
StylePtr<InOutHelper<OnSpark<Blast<LocalizedClash<Lockup<AudioFlicker<Red,Rgb16<38402,0,0>>,Gradient<AudioFlicker<Red,Rgb16<38402,0,0>>,AudioFlicker<Red,Rgb16<38402,0,0>>,HumpFlicker<Blue,Cyan,50>,AudioFlicker<Red,Rgb16<38402,0,0>>,AudioFlicker<Red,Rgb16<38402,0,0>>>,Gradient<AudioFlicker<Red,Rgb16<38402,0,0>>,AudioFlicker<Red,Rgb16<38402,0,0>>,HumpFlicker<Blue,Cyan,50>>>,Pink>,Pink>,Pink,400>,200,500>>(), "0"},  

    { "Ashoka Tano", "extras/maytheforcebewithyou.wav",
StylePtr<Layers<Black,ColorChange<TrSelect<Ifon<Int<1>,Int<0>>,TrInstant,TrFadeX<Int<1000>>>,Mix<HoldPeakF<SwingSpeed<250>,Scale<SwingAcceleration<100>,Int<50>,Int<500>>,Scale<SwingAcceleration<>,Int<20000>,Int<10000>>>,RandomFlicker<StripesX<Int<15000>,Scale<HoldPeakF<SwingSpeed<200>,Scale<SwingAcceleration<100>,Int<50>,Int<300>>,Scale<SwingAcceleration<100>,Int<24000>,Int<16000>>>,Int<-3200>,Int<-200>>,RgbArg<BASE_COLOR_ARG,Rgb<255,255,255>>,RgbArg<BASE_COLOR_ARG,Rgb<255,255,255>>,Mix<Int<7710>,Black,RgbArg<BASE_COLOR_ARG,Rgb<255,255,255>>>,RgbArg<BASE_COLOR_ARG,Rgb<255,255,255>>,Mix<Int<19276>,Black,RgbArg<BASE_COLOR_ARG,Rgb<255,255,255>>>>,RgbArg<BASE_COLOR_ARG,Rgb<255,255,255>>>,RgbArg<BASE_COLOR_ARG,Rgb<255,255,255>>>,Stripes<3000,-3500,RgbArg<ALT_COLOR_ARG,Rgb<20,119,225>>,RandomPerLEDFlicker<Mix<Int<7710>,Black,RgbArg<ALT_COLOR_ARG,Rgb<20,119,225>>>,Black>,BrownNoiseFlicker<RgbArg<ALT_COLOR_ARG,Rgb<20,119,225>>,Mix<Int<3855>,Black,RgbArg<ALT_COLOR_ARG,Rgb<20,119,225>>>,200>,RandomPerLEDFlicker<Mix<Int<10280>,Black,RgbArg<ALT_COLOR_ARG,Rgb<20,119,225>>>,Mix<Int<3855>,Black,RgbArg<ALT_COLOR_ARG,Rgb<20,119,225>>>>>>,TransitionEffectL<TrWaveX<RgbArg<BLAST_COLOR_ARG,Rgb<255,255,255>>,Scale<EffectRandomF<EFFECT_BLAST>,Int<100>,Int<400>>,Int<100>,Scale<EffectPosition<EFFECT_BLAST>,Int<100>,Int<400>>,Scale<EffectPosition<EFFECT_BLAST>,Int<28000>,Int<8000>>>,EFFECT_BLAST>,Mix<IsLessThan<ClashImpactF<>,Int<26000>>,TransitionEffectL<TrConcat<TrInstant,AlphaL<RgbArg<CLASH_COLOR_ARG,Rgb<255,255,255>>,Bump<Scale<BladeAngle<>,Scale<BladeAngle<0,16000>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<-12000>>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<10000>>>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<-10000>>>,Scale<ClashImpactF<>,Int<12000>,Int<60000>>>>,TrFadeX<Scale<ClashImpactF<>,Int<200>,Int<400>>>>,EFFECT_CLASH>,TransitionEffectL<TrWaveX<RgbArg<CLASH_COLOR_ARG,Rgb<255,255,255>>,Scale<ClashImpactF<>,Int<100>,Int<400>>,Int<100>,Scale<ClashImpactF<>,Int<100>,Int<400>>,Scale<BladeAngle<>,Scale<BladeAngle<0,16000>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<-12000>>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<10000>>>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<-10000>>>>,EFFECT_CLASH>>,LockupTrL<TransitionEffect<AlphaL<AlphaMixL<Bump<Scale<BladeAngle<>,Scale<BladeAngle<0,16000>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<-12000>>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<10000>>>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<-10000>>>,Scale<SwingSpeed<100>,Int<14000>,Int<22000>>>,AudioFlicker<RgbArg<LOCKUP_COLOR_ARG,Rgb<255,255,255>>,Mix<Int<12000>,Black,RgbArg<LOCKUP_COLOR_ARG,Rgb<255,255,255>>>>,BrownNoiseFlicker<RgbArg<LOCKUP_COLOR_ARG,Rgb<255,255,255>>,Mix<Int<12000>,Black,RgbArg<LOCKUP_COLOR_ARG,Rgb<255,255,255>>>,300>>,Bump<Scale<BladeAngle<>,Scale<BladeAngle<0,16000>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<-12000>>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<10000>>>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<-10000>>>,Scale<SwingSpeed<100>,Int<14000>,Int<22000>>>>,AlphaL<AudioFlicker<RgbArg<LOCKUP_COLOR_ARG,Rgb<255,255,255>>,Mix<Int<20000>,Black,RgbArg<LOCKUP_COLOR_ARG,Rgb<255,255,255>>>>,Bump<Scale<BladeAngle<>,Scale<BladeAngle<0,16000>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<-12000>>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<10000>>>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<-10000>>>,Scale<SwingSpeed<100>,Int<14000>,Int<18000>>>>,TrExtend<5000,TrInstant>,TrFade<5000>,EFFECT_LOCKUP_BEGIN>,TrConcat<TrJoin<TrDelay<50>,TrInstant>,Mix<IsLessThan<ClashImpactF<>,Int<26000>>,RgbArg<LOCKUP_COLOR_ARG,Rgb<255,255,255>>,AlphaL<RgbArg<LOCKUP_COLOR_ARG,Rgb<255,255,255>>,Bump<Scale<BladeAngle<>,Scale<BladeAngle<0,16000>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<-12000>>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<10000>>>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<-10000>>>,Scale<ClashImpactF<>,Int<20000>,Int<60000>>>>>,TrFade<300>>,TrConcat<TrInstant,RgbArg<LOCKUP_COLOR_ARG,Rgb<255,255,255>>,TrFade<400>>,SaberBase::LOCKUP_NORMAL,Int<1>>,ResponsiveLightningBlockL<Strobe<RgbArg<LB_COLOR_ARG,Rgb<75,10,255>>,AudioFlicker<RgbArg<LB_COLOR_ARG,Rgb<75,10,255>>,Blue>,50,1>,TrConcat<TrExtend<200,TrInstant>,AlphaL<RgbArg<LB_COLOR_ARG,Rgb<75,10,255>>,Bump<Scale<BladeAngle<>,Int<10000>,Int<21000>>,Int<10000>>>,TrFade<200>>,TrConcat<TrInstant,RgbArg<LB_COLOR_ARG,Rgb<75,10,255>>,TrFade<400>>,Int<1>>,LockupTrL<AlphaL<TransitionEffect<RandomPerLEDFlickerL<RgbArg<DRAG_COLOR_ARG,Rgb<255,255,255>>>,BrownNoiseFlickerL<RgbArg<DRAG_COLOR_ARG,Rgb<255,255,255>>,Int<300>>,TrExtend<4000,TrInstant>,TrFade<4000>,EFFECT_DRAG_BEGIN>,SmoothStep<Scale<TwistAngle<>,IntArg<DRAG_SIZE_ARG,28000>,Int<30000>>,Int<3000>>>,TrWipeIn<200>,TrWipe<200>,SaberBase::LOCKUP_DRAG,Int<1>>,LockupTrL<AlphaL<Stripes<2000,4000,Mix<TwistAngle<>,RgbArg<STAB_COLOR_ARG,Rgb<255,136,0>>,RotateColorsX<Int<3000>,RgbArg<STAB_COLOR_ARG,Rgb<255,136,0>>>>,Mix<Sin<Int<50>>,Black,Mix<TwistAngle<>,RgbArg<STAB_COLOR_ARG,Rgb<255,136,0>>,RotateColorsX<Int<3000>,RgbArg<STAB_COLOR_ARG,Rgb<255,136,0>>>>>,Mix<Int<4096>,Black,Mix<TwistAngle<>,RgbArg<STAB_COLOR_ARG,Rgb<255,136,0>>,RotateColorsX<Int<3000>,RgbArg<STAB_COLOR_ARG,Rgb<255,136,0>>>>>>,SmoothStep<Scale<TwistAngle<>,IntArg<MELT_SIZE_ARG,28000>,Int<30000>>,Int<3000>>>,TrConcat<TrExtend<4000,TrWipeIn<200>>,AlphaL<HumpFlicker<Mix<TwistAngle<>,RgbArg<STAB_COLOR_ARG,Rgb<255,136,0>>,RotateColorsX<Int<3000>,RgbArg<STAB_COLOR_ARG,Rgb<255,136,0>>>>,RotateColorsX<Int<3000>,Mix<TwistAngle<>,RgbArg<STAB_COLOR_ARG,Rgb<255,136,0>>,RotateColorsX<Int<3000>,RgbArg<STAB_COLOR_ARG,Rgb<255,136,0>>>>>,100>,SmoothStep<Scale<TwistAngle<>,IntArg<MELT_SIZE_ARG,28000>,Int<30000>>,Int<3000>>>,TrFade<4000>>,TrWipe<200>,SaberBase::LOCKUP_MELT,Int<1>>,InOutTrL<TrWipeX<BendTimePowInvX<IgnitionTime<150>,Mult<IntArg<IGNITION_OPTION2_ARG,10992>,Int<98304>>>>,TrWipeInX<BendTimePowX<RetractionTime<0>,Mult<IntArg<RETRACTION_OPTION2_ARG,10992>,Int<98304>>>>,Black>>>(),
StylePtr<Layers<Black,ColorChange<TrSelect<Ifon<Int<1>,Int<0>>,TrInstant,TrFadeX<Int<1000>>>,Mix<HoldPeakF<SwingSpeed<250>,Scale<SwingAcceleration<100>,Int<50>,Int<500>>,Scale<SwingAcceleration<>,Int<20000>,Int<10000>>>,RandomFlicker<StripesX<Int<15000>,Scale<HoldPeakF<SwingSpeed<200>,Scale<SwingAcceleration<100>,Int<50>,Int<300>>,Scale<SwingAcceleration<100>,Int<24000>,Int<16000>>>,Int<-3200>,Int<-200>>,RgbArg<BASE_COLOR_ARG,Rgb<255,255,255>>,RgbArg<BASE_COLOR_ARG,Rgb<255,255,255>>,Mix<Int<7710>,Black,RgbArg<BASE_COLOR_ARG,Rgb<255,255,255>>>,RgbArg<BASE_COLOR_ARG,Rgb<255,255,255>>,Mix<Int<19276>,Black,RgbArg<BASE_COLOR_ARG,Rgb<255,255,255>>>>,RgbArg<BASE_COLOR_ARG,Rgb<255,255,255>>>,RgbArg<BASE_COLOR_ARG,Rgb<255,255,255>>>,Stripes<3000,-3500,RgbArg<ALT_COLOR_ARG,Rgb<20,119,225>>,RandomPerLEDFlicker<Mix<Int<7710>,Black,RgbArg<ALT_COLOR_ARG,Rgb<20,119,225>>>,Black>,BrownNoiseFlicker<RgbArg<ALT_COLOR_ARG,Rgb<20,119,225>>,Mix<Int<3855>,Black,RgbArg<ALT_COLOR_ARG,Rgb<20,119,225>>>,200>,RandomPerLEDFlicker<Mix<Int<10280>,Black,RgbArg<ALT_COLOR_ARG,Rgb<20,119,225>>>,Mix<Int<3855>,Black,RgbArg<ALT_COLOR_ARG,Rgb<20,119,225>>>>>>,TransitionEffectL<TrWaveX<RgbArg<BLAST_COLOR_ARG,Rgb<255,255,255>>,Scale<EffectRandomF<EFFECT_BLAST>,Int<100>,Int<400>>,Int<100>,Scale<EffectPosition<EFFECT_BLAST>,Int<100>,Int<400>>,Scale<EffectPosition<EFFECT_BLAST>,Int<28000>,Int<8000>>>,EFFECT_BLAST>,Mix<IsLessThan<ClashImpactF<>,Int<26000>>,TransitionEffectL<TrConcat<TrInstant,AlphaL<RgbArg<CLASH_COLOR_ARG,Rgb<255,255,255>>,Bump<Scale<BladeAngle<>,Scale<BladeAngle<0,16000>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<-12000>>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<10000>>>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<-10000>>>,Scale<ClashImpactF<>,Int<12000>,Int<60000>>>>,TrFadeX<Scale<ClashImpactF<>,Int<200>,Int<400>>>>,EFFECT_CLASH>,TransitionEffectL<TrWaveX<RgbArg<CLASH_COLOR_ARG,Rgb<255,255,255>>,Scale<ClashImpactF<>,Int<100>,Int<400>>,Int<100>,Scale<ClashImpactF<>,Int<100>,Int<400>>,Scale<BladeAngle<>,Scale<BladeAngle<0,16000>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<-12000>>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<10000>>>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<-10000>>>>,EFFECT_CLASH>>,LockupTrL<TransitionEffect<AlphaL<AlphaMixL<Bump<Scale<BladeAngle<>,Scale<BladeAngle<0,16000>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<-12000>>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<10000>>>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<-10000>>>,Scale<SwingSpeed<100>,Int<14000>,Int<22000>>>,AudioFlicker<RgbArg<LOCKUP_COLOR_ARG,Rgb<255,255,255>>,Mix<Int<12000>,Black,RgbArg<LOCKUP_COLOR_ARG,Rgb<255,255,255>>>>,BrownNoiseFlicker<RgbArg<LOCKUP_COLOR_ARG,Rgb<255,255,255>>,Mix<Int<12000>,Black,RgbArg<LOCKUP_COLOR_ARG,Rgb<255,255,255>>>,300>>,Bump<Scale<BladeAngle<>,Scale<BladeAngle<0,16000>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<-12000>>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<10000>>>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<-10000>>>,Scale<SwingSpeed<100>,Int<14000>,Int<22000>>>>,AlphaL<AudioFlicker<RgbArg<LOCKUP_COLOR_ARG,Rgb<255,255,255>>,Mix<Int<20000>,Black,RgbArg<LOCKUP_COLOR_ARG,Rgb<255,255,255>>>>,Bump<Scale<BladeAngle<>,Scale<BladeAngle<0,16000>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<-12000>>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<10000>>>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<-10000>>>,Scale<SwingSpeed<100>,Int<14000>,Int<18000>>>>,TrExtend<5000,TrInstant>,TrFade<5000>,EFFECT_LOCKUP_BEGIN>,TrConcat<TrJoin<TrDelay<50>,TrInstant>,Mix<IsLessThan<ClashImpactF<>,Int<26000>>,RgbArg<LOCKUP_COLOR_ARG,Rgb<255,255,255>>,AlphaL<RgbArg<LOCKUP_COLOR_ARG,Rgb<255,255,255>>,Bump<Scale<BladeAngle<>,Scale<BladeAngle<0,16000>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<-12000>>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<10000>>>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<-10000>>>,Scale<ClashImpactF<>,Int<20000>,Int<60000>>>>>,TrFade<300>>,TrConcat<TrInstant,RgbArg<LOCKUP_COLOR_ARG,Rgb<255,255,255>>,TrFade<400>>,SaberBase::LOCKUP_NORMAL,Int<1>>,ResponsiveLightningBlockL<Strobe<RgbArg<LB_COLOR_ARG,Rgb<75,10,255>>,AudioFlicker<RgbArg<LB_COLOR_ARG,Rgb<75,10,255>>,Blue>,50,1>,TrConcat<TrExtend<200,TrInstant>,AlphaL<RgbArg<LB_COLOR_ARG,Rgb<75,10,255>>,Bump<Scale<BladeAngle<>,Int<10000>,Int<21000>>,Int<10000>>>,TrFade<200>>,TrConcat<TrInstant,RgbArg<LB_COLOR_ARG,Rgb<75,10,255>>,TrFade<400>>,Int<1>>,LockupTrL<AlphaL<TransitionEffect<RandomPerLEDFlickerL<RgbArg<DRAG_COLOR_ARG,Rgb<255,255,255>>>,BrownNoiseFlickerL<RgbArg<DRAG_COLOR_ARG,Rgb<255,255,255>>,Int<300>>,TrExtend<4000,TrInstant>,TrFade<4000>,EFFECT_DRAG_BEGIN>,SmoothStep<Scale<TwistAngle<>,IntArg<DRAG_SIZE_ARG,28000>,Int<30000>>,Int<3000>>>,TrWipeIn<200>,TrWipe<200>,SaberBase::LOCKUP_DRAG,Int<1>>,LockupTrL<AlphaL<Stripes<2000,4000,Mix<TwistAngle<>,RgbArg<STAB_COLOR_ARG,Rgb<255,136,0>>,RotateColorsX<Int<3000>,RgbArg<STAB_COLOR_ARG,Rgb<255,136,0>>>>,Mix<Sin<Int<50>>,Black,Mix<TwistAngle<>,RgbArg<STAB_COLOR_ARG,Rgb<255,136,0>>,RotateColorsX<Int<3000>,RgbArg<STAB_COLOR_ARG,Rgb<255,136,0>>>>>,Mix<Int<4096>,Black,Mix<TwistAngle<>,RgbArg<STAB_COLOR_ARG,Rgb<255,136,0>>,RotateColorsX<Int<3000>,RgbArg<STAB_COLOR_ARG,Rgb<255,136,0>>>>>>,SmoothStep<Scale<TwistAngle<>,IntArg<MELT_SIZE_ARG,28000>,Int<30000>>,Int<3000>>>,TrConcat<TrExtend<4000,TrWipeIn<200>>,AlphaL<HumpFlicker<Mix<TwistAngle<>,RgbArg<STAB_COLOR_ARG,Rgb<255,136,0>>,RotateColorsX<Int<3000>,RgbArg<STAB_COLOR_ARG,Rgb<255,136,0>>>>,RotateColorsX<Int<3000>,Mix<TwistAngle<>,RgbArg<STAB_COLOR_ARG,Rgb<255,136,0>>,RotateColorsX<Int<3000>,RgbArg<STAB_COLOR_ARG,Rgb<255,136,0>>>>>,100>,SmoothStep<Scale<TwistAngle<>,IntArg<MELT_SIZE_ARG,28000>,Int<30000>>,Int<3000>>>,TrFade<4000>>,TrWipe<200>,SaberBase::LOCKUP_MELT,Int<1>>,InOutTrL<TrWipeX<BendTimePowInvX<IgnitionTime<150>,Mult<IntArg<IGNITION_OPTION2_ARG,10992>,Int<98304>>>>,TrWipeInX<BendTimePowX<RetractionTime<0>,Mult<IntArg<RETRACTION_OPTION2_ARG,10992>,Int<98304>>>>,Black>>>(), "0" },

{ "Darth Maul", "tracks/Darth Maul.wav",
StylePtr<InOutHelper<OnSpark<Blast<LocalizedClash<Lockup<AudioFlicker<Red,Rgb16<38402,0,0>>,Gradient<AudioFlicker<Red,Rgb16<38402,0,0>>,AudioFlicker<Red,Rgb16<38402,0,0>>,HumpFlicker<Blue,Cyan,50>,AudioFlicker<Red,Rgb16<38402,0,0>>,AudioFlicker<Red,Rgb16<38402,0,0>>>,Gradient<AudioFlicker<Red,Rgb16<38402,0,0>>,AudioFlicker<Red,Rgb16<38402,0,0>>,HumpFlicker<Blue,Cyan,50>>>,Pink>,Pink>,Pink,400>,200,500>>(), 
StylePtr<InOutHelper<OnSpark<Blast<LocalizedClash<Lockup<AudioFlicker<Red,Rgb16<38402,0,0>>,Gradient<AudioFlicker<Red,Rgb16<38402,0,0>>,AudioFlicker<Red,Rgb16<38402,0,0>>,HumpFlicker<Blue,Cyan,50>,AudioFlicker<Red,Rgb16<38402,0,0>>,AudioFlicker<Red,Rgb16<38402,0,0>>>,Gradient<AudioFlicker<Red,Rgb16<38402,0,0>>,AudioFlicker<Red,Rgb16<38402,0,0>>,HumpFlicker<Blue,Cyan,50>>>,Pink>,Pink>,Pink,400>,200,500>>(), "0"},

    { "Mercenary", "tracks/Darth Vader 3.wav",
	StylePtr<Layers<

  //Base Fett263 Smoke Blade style
  StripesX<Sin<Int<12>,Int<3000>,Int<7000>>,Scale<SwingSpeed<100>,Int<75>,Int<125>>,StripesX<Sin<Int<10>,Int<1000>,Int<3000>>,Scale<SwingSpeed<100>,Int<75>,Int<100>>,Pulsing<RotateColorsX<Variation,Yellow>,RotateColorsX<Variation,Rgb<15,14,0>>,1200>,Mix<SwingSpeed<200>,RotateColorsX<Variation,Rgb<90,87,0>>,Black>>,RotateColorsX<Variation,Rgb<40,40,0>>,Pulsing<RotateColorsX<Variation,Rgb<36,33,0>>,StripesX<Sin<Int<10>,Int<2000>,Int<3000>>,Sin<Int<10>,Int<75>,Int<100>>,RotateColorsX<Variation,Yellow>,RotateColorsX<Variation,Rgb<60,58,0>>>,2000>,Pulsing<RotateColorsX<Variation,Rgb<90,88,0>>,RotateColorsX<Variation,Rgb<5,5,0>>,3000>>,

  //Underlying Fett263 Smoke Blade Fire layer
  AlphaL<StyleFire<RotateColorsX<Variation,Yellow>,RotateColorsX<Variation,Rgb<2,2,0>>,0,1,FireConfig<10,2000,2>,FireConfig<10,2000,2>,FireConfig<10,2000,2>,FireConfig<0,0,25>>,Int<10000>>,
  
  //Fett263 Ripple swing effect
  AlphaL<Stripes<2500,-3000,RotateColorsX<Variation,Yellow>,RotateColorsX<Variation,Rgb<44,42,0>>,Pulsing<RotateColorsX<Variation,Rgb<22,20,0>>,Black,800>>,SwingSpeed<375>>,
  
  //Fett263 Bright hard swing effect
  AlphaL<RotateColorsX<Variation,LemonChiffon>,Scale<IsLessThan<SwingSpeed<675>,Int<13600>>,Scale<SwingSpeed<750>,Int<-17300>,Int<32768>>,Int<0>>>,
  
  //Fett263 Responsive Intensity Lockup
  LockupTrL<AlphaMixL<Bump<Scale<BladeAngle<>,Scale<BladeAngle<0,16000>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<-12000>>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<10000>>>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<-10000>>>,Scale<SwingSpeed<100>,Int<14000>,Int<18000>>>,BrownNoiseFlickerL<RgbArg<LOCKUP_COLOR_ARG,White>,Int<200>>,StripesX<Int<1800>,Scale<NoisySoundLevel,Int<-3500>,Int<-5000>>,Mix<Int<6425>,Black,RgbArg<LOCKUP_COLOR_ARG,White>>,RgbArg<LOCKUP_COLOR_ARG,White>,Mix<Int<12850>,Black,RgbArg<LOCKUP_COLOR_ARG,White>>>>,
  TrConcat<TrExtend<50,TrInstant>,Mix<IsLessThan<ClashImpactF<>,Int<26000>>,RgbArg<LOCKUP_COLOR_ARG,White>,AlphaL<RgbArg<LOCKUP_COLOR_ARG,White>,Bump<Scale<BladeAngle<>,Scale<BladeAngle<0,16000>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<-12000>>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<10000>>>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<-10000>>>,Scale<ClashImpactF<>,Int<20000>,Int<60000>>>>>,TrExtend<3000,TrFade<300>>,AlphaL<AudioFlicker<RgbArg<LOCKUP_COLOR_ARG,White>,Mix<Int<10280>,Black,RgbArg<LOCKUP_COLOR_ARG,White>>>,Bump<Scale<BladeAngle<>,Scale<BladeAngle<0,16000>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<-12000>>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<10000>>>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<-10000>>>,Int<13000>>>,TrFade<3000>>,
  TrConcat<TrInstant,AlphaL<RgbArg<LOCKUP_COLOR_ARG,White>,Int<0>>,TrWaveX<RgbArg<LOCKUP_COLOR_ARG,White>,Int<300>,Int<100>,Int<400>,Scale<BladeAngle<>,Scale<BladeAngle<0,16000>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<-12000>>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<10000>>>,Scale<SwingSpeed<100>,Int<14000>,Int<18000>>>>>,SaberBase::LOCKUP_NORMAL>,
  
  //Fett263 Responsive Lightning Block
  ResponsiveLightningBlockL<Strobe<RgbArg<LB_COLOR_ARG,White>,AudioFlicker<RgbArg<LB_COLOR_ARG,White>,Blue>,50,1>,TrConcat<TrInstant,AlphaL<RgbArg<LB_COLOR_ARG,White>,Bump<Int<12000>,Int<18000>>>,TrFade<200>>,TrConcat<TrInstant,HumpFlickerL<AlphaL<RgbArg<LB_COLOR_ARG,White>,Int<16000>>,30>,TrSmoothFade<600>>>,
  
  //Fett263 Sparking Emitter Flare
  AlphaL<RotateColorsX<Variation,LemonChiffon>,SmoothStep<Scale<SlowNoise<Int<2750>>,Int<1750>,Int<3750>>,Int<-4000>>>,
  
  //Responsive Stab
  ResponsiveStabL<AudioFlickerL<RgbArg<STAB_COLOR_ARG,DeepPink>>,TrWipeInX<Percentage<WavLen<EFFECT_STAB>,50>>,TrFadeX<Percentage<WavLen<EFFECT_STAB>,50>>>,
  
  //Fett263 Multi-blast, blaster reflect cycles through different responsive effects
  EffectSequence<EFFECT_BLAST,ResponsiveBlastL<RgbArg<BLAST_COLOR_ARG,White>,Int<400>,Scale<SwingSpeed<200>,Int<100>,Int<400>>,Int<400>>,LocalizedClashL<RgbArg<BLAST_COLOR_ARG,White>,80,30,EFFECT_BLAST>,ResponsiveBlastWaveL<RgbArg<BLAST_COLOR_ARG,White>,Scale<SwingSpeed<400>,Int<500>,Int<200>>,Scale<SwingSpeed<400>,Int<100>,Int<400>>>,BlastL<RgbArg<BLAST_COLOR_ARG,White>,200,200>,ResponsiveBlastFadeL<RgbArg<BLAST_COLOR_ARG,White>,Scale<SwingSpeed<400>,Int<6000>,Int<12000>>,Scale<SwingSpeed<400>,Int<400>,Int<100>>>,ResponsiveBlastL<RgbArg<BLAST_COLOR_ARG,White>,Scale<SwingSpeed<400>,Int<400>,Int<100>>,Scale<SwingSpeed<400>,Int<200>,Int<100>>,Scale<SwingSpeed<400>,Int<400>,Int<200>>>>,
  
  //Fett263 Real Clash
  Mix<IsLessThan<ClashImpactF<>,Int<26000>>,TransitionEffectL<TrConcat<TrInstant,AlphaL<RgbArg<CLASH_COLOR_ARG,White>,Bump<Scale<BladeAngle<>,Scale<BladeAngle<0,16000>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<-12000>>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<10000>>>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<-10000>>>,Scale<ClashImpactF<>,Int<12000>,Int<60000>>>>,TrFadeX<Scale<ClashImpactF<>,Int<200>,Int<400>>>>,EFFECT_CLASH>,TransitionEffectL<TrWaveX<RgbArg<CLASH_COLOR_ARG,White>,Scale<ClashImpactF<>,Int<100>,Int<400>>,Int<100>,Scale<ClashImpactF<>,Int<100>,Int<400>>,Scale<BladeAngle<>,Scale<BladeAngle<0,16000>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<-12000>>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<10000>>>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<-10000>>>>,EFFECT_CLASH>>,
  
  //Fett263 Unstable bright ignition effect
  TransitionEffectL<TrConcat<TrInstant,Stripes<3000,-3500,RotateColorsX<Variation,Rgb<100,100,150>>,RandomPerLEDFlicker<RotateColorsX<Variation,Rgb<60,60,80>>,Black>,BrownNoiseFlicker<RotateColorsX<Variation,Rgb<110,115,140>>,RotateColorsX<Variation,Rgb<60,60,80>>,200>,RandomPerLEDFlicker<RotateColorsX<Variation,Rgb<128,128,128>>,RotateColorsX<Variation,Rgb<60,60,80>>>>,TrFadeX<Percentage<WavLen<>,65>>>,EFFECT_IGNITION>,
  
  //Fett263 Bright Humpflicker retraction effect
  TransitionEffectL<TrConcat<TrInstant,HumpFlickerL<White,40>,TrFadeX<Percentage<WavLen<>,125>>>,EFFECT_RETRACTION>,

  //Fett263 Intensity Drag
  LockupTrL<AlphaL<RandomPerLEDFlickerL<RgbArg<DRAG_COLOR_ARG,White>>,SmoothStep<IntArg<DRAG_SIZE_ARG,27500>,Int<5000>>>,TrConcat<TrExtend<4000,TrWipeIn<200>>,AlphaL<BrownNoiseFlickerL<RgbArg<DRAG_COLOR_ARG,White>,Int<300>>,SmoothStep<IntArg<DRAG_SIZE_ARG,29250>,Int<5000>>>,TrFade<4000>>,TrFade<300>,SaberBase::LOCKUP_DRAG>,
  
  //Fett263 Responsive Intensity Melt
  LockupTrL<AlphaL<Remap<Scale<RampF,Int<65536>,Int<0>>,StaticFire<Mix<TwistAngle<>,OrangeRed,DarkOrange>,Mix<TwistAngle<>,OrangeRed,Orange>,0,3,5,3000,10>>,SmoothStep<IntArg<MELT_SIZE_ARG,26000>,Int<6000>>>,TrConcat<TrWipeIn<100>,AlphaL<Red,SmoothStep<Int<29000>,Int<8000>>>,TrExtend<2000,TrFade<300>>,AlphaL<Mix<TwistAngle<>,Red,Orange>,SmoothStep<Int<29000>,Int<8000>>>,TrFade<3000>>,TrFade<250>,SaberBase::LOCKUP_MELT>,
  
  //Fett263 Power Save, if using Fett263's prop file hold AUX and click PWR while ON (pointing up) to dim blade in 25% increments.
  EffectSequence<EFFECT_POWERSAVE,AlphaL<Black,Int<8192>>,AlphaL<Black,Int<16384>>,AlphaL<Black,Int<24576>>,AlphaL<Black,Int<0>>>,

  //Fett263 White Spark Tip ignition and Color Cycle retraction
  InOutTrL<TrJoin<TrWipeX<Percentage<WavLen<EFFECT_IGNITION>,8>>,TrWaveX<White,Percentage<WavLen<EFFECT_IGNITION>,25>,Int<300>,Percentage<WavLen<EFFECT_IGNITION>,8>,Int<0>>>,TrColorCycle<950,7500>>,
  
  //Fett263 optional/alternate Passive Battery Monitor: on boot (1st line) or font change (2nd line) you will get a visual indicator at the emitter of your current battery level. This also works without a blade if you have a lit emitter or blade plug. Green is Full, Red is Low (the color will blend from Green to Red as the battery is depleted), the indicator will fade out after 3000 ms and not display again until powered down and back up or fonts change.
  //TransitionEffectL<TrConcat<TrDelay<1500>,Black,TrFade<1000>,AlphaL<Mix<BatteryLevel,Red,Green>,Bump<Int<0>,Int<6000>>>,TrFade<3000>>,EFFECT_BOOT>,
  //TransitionEffectL<TrConcat<TrInstant,AlphaL<Mix<BatteryLevel,Red,Green>,Bump<Int<0>,Int<6000>>>,TrFade<3000>>,EFFECT_NEWFONT>,
  
  //Fett263 On-Demand Battery Level: if using Fett263's prop file Hold AUX and click PWR while OFF, the battery level is represented by the location on the blade; tip = full, hilt = low and color; green = full, yellow = half, red = low
  TransitionEffectL<TrConcat<TrInstant,AlphaL<Mix<BatteryLevel,Red,Green>,Bump<BatteryLevel,Int<10000>>>,TrDelay<2000>,AlphaL<Mix<BatteryLevel,Red,Green>,Bump<BatteryLevel,Int<10000>>>,TrFade<1000>>,EFFECT_BATTERY_LEVEL>,
  
  //Absorby charge-up preon
  TransitionEffectL<TrConcat<TrDelayX<Percentage<WavLen<>,33>>,TransitionLoopL<TrJoin<TrWipeIn<142>,TrSparkX<RotateColorsX<Variation,LemonChiffon>,Int<425>,Int<142>,Int<32768>>>>,TrDelayX<Percentage<WavLen<>,67>>>,EFFECT_PREON>,
  TransitionEffectL<TrConcat<TrInstant,AlphaL<Mix<Trigger<EFFECT_PREON,Percentage<WavLen<>,33>,Percentage<WavLen<>,67>,Int<0>>,BrownNoiseFlicker<Black,RotateColorsX<Variation,HotPink>,100>,RandomPerLEDFlicker<RotateColorsX<Variation,LightPink>,RotateColorsX<Variation,LemonChiffon>>,BrownNoiseFlicker<Mix<NoisySoundLevel,RotateColorsX<Variation,DeepPink>,RotateColorsX<Int<4000>,RotateColorsX<Variation,Yellow>>>,RotateColorsX<Variation,LemonChiffon>,50>>,SmoothStep<Scale<NoisySoundLevel,Int<-350>,Int<17500>>,Int<-4000>>>,TrDelayX<WavLen<>>>,EFFECT_PREON>,
  TransitionEffectL<TrConcat<TrInstant,AlphaL<Black,SmoothStep<Sin<Int<10>,Int<16500>,Int<14500>>,Sin<Int<7>,Int<10500>,Int<9500>>>>,TrDelayX<WavLen<>>>,EFFECT_PREON>
  >>(), 

StylePtr<Layers<

  //Base Fett263 Smoke Blade style
  StripesX<Sin<Int<12>,Int<3000>,Int<7000>>,Scale<SwingSpeed<100>,Int<75>,Int<125>>,StripesX<Sin<Int<10>,Int<1000>,Int<3000>>,Scale<SwingSpeed<100>,Int<75>,Int<100>>,Pulsing<RotateColorsX<Variation,Yellow>,RotateColorsX<Variation,Rgb<15,14,0>>,1200>,Mix<SwingSpeed<200>,RotateColorsX<Variation,Rgb<90,87,0>>,Black>>,RotateColorsX<Variation,Rgb<40,40,0>>,Pulsing<RotateColorsX<Variation,Rgb<36,33,0>>,StripesX<Sin<Int<10>,Int<2000>,Int<3000>>,Sin<Int<10>,Int<75>,Int<100>>,RotateColorsX<Variation,Yellow>,RotateColorsX<Variation,Rgb<60,58,0>>>,2000>,Pulsing<RotateColorsX<Variation,Rgb<90,88,0>>,RotateColorsX<Variation,Rgb<5,5,0>>,3000>>,

  //Underlying Fett263 Smoke Blade Fire layer
  AlphaL<StyleFire<RotateColorsX<Variation,Yellow>,RotateColorsX<Variation,Rgb<2,2,0>>,0,1,FireConfig<10,2000,2>,FireConfig<10,2000,2>,FireConfig<10,2000,2>,FireConfig<0,0,25>>,Int<10000>>,
  
  //Fett263 Ripple swing effect
  AlphaL<Stripes<2500,-3000,RotateColorsX<Variation,Yellow>,RotateColorsX<Variation,Rgb<44,42,0>>,Pulsing<RotateColorsX<Variation,Rgb<22,20,0>>,Black,800>>,SwingSpeed<375>>,
  
  //Fett263 Bright hard swing effect
  AlphaL<RotateColorsX<Variation,LemonChiffon>,Scale<IsLessThan<SwingSpeed<675>,Int<13600>>,Scale<SwingSpeed<750>,Int<-17300>,Int<32768>>,Int<0>>>,
  
  //Fett263 Responsive Intensity Lockup
  LockupTrL<AlphaMixL<Bump<Scale<BladeAngle<>,Scale<BladeAngle<0,16000>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<-12000>>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<10000>>>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<-10000>>>,Scale<SwingSpeed<100>,Int<14000>,Int<18000>>>,BrownNoiseFlickerL<RgbArg<LOCKUP_COLOR_ARG,White>,Int<200>>,StripesX<Int<1800>,Scale<NoisySoundLevel,Int<-3500>,Int<-5000>>,Mix<Int<6425>,Black,RgbArg<LOCKUP_COLOR_ARG,White>>,RgbArg<LOCKUP_COLOR_ARG,White>,Mix<Int<12850>,Black,RgbArg<LOCKUP_COLOR_ARG,White>>>>,
  TrConcat<TrExtend<50,TrInstant>,Mix<IsLessThan<ClashImpactF<>,Int<26000>>,RgbArg<LOCKUP_COLOR_ARG,White>,AlphaL<RgbArg<LOCKUP_COLOR_ARG,White>,Bump<Scale<BladeAngle<>,Scale<BladeAngle<0,16000>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<-12000>>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<10000>>>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<-10000>>>,Scale<ClashImpactF<>,Int<20000>,Int<60000>>>>>,TrExtend<3000,TrFade<300>>,AlphaL<AudioFlicker<RgbArg<LOCKUP_COLOR_ARG,White>,Mix<Int<10280>,Black,RgbArg<LOCKUP_COLOR_ARG,White>>>,Bump<Scale<BladeAngle<>,Scale<BladeAngle<0,16000>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<-12000>>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<10000>>>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<-10000>>>,Int<13000>>>,TrFade<3000>>,
  TrConcat<TrInstant,AlphaL<RgbArg<LOCKUP_COLOR_ARG,White>,Int<0>>,TrWaveX<RgbArg<LOCKUP_COLOR_ARG,White>,Int<300>,Int<100>,Int<400>,Scale<BladeAngle<>,Scale<BladeAngle<0,16000>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<-12000>>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<10000>>>,Scale<SwingSpeed<100>,Int<14000>,Int<18000>>>>>,SaberBase::LOCKUP_NORMAL>,
  
  //Fett263 Responsive Lightning Block
  ResponsiveLightningBlockL<Strobe<RgbArg<LB_COLOR_ARG,White>,AudioFlicker<RgbArg<LB_COLOR_ARG,White>,Blue>,50,1>,TrConcat<TrInstant,AlphaL<RgbArg<LB_COLOR_ARG,White>,Bump<Int<12000>,Int<18000>>>,TrFade<200>>,TrConcat<TrInstant,HumpFlickerL<AlphaL<RgbArg<LB_COLOR_ARG,White>,Int<16000>>,30>,TrSmoothFade<600>>>,
  
  //Fett263 Sparking Emitter Flare
  AlphaL<RotateColorsX<Variation,LemonChiffon>,SmoothStep<Scale<SlowNoise<Int<2750>>,Int<1750>,Int<3750>>,Int<-4000>>>,
  
  //Responsive Stab
  ResponsiveStabL<AudioFlickerL<RgbArg<STAB_COLOR_ARG,DeepPink>>,TrWipeInX<Percentage<WavLen<EFFECT_STAB>,50>>,TrFadeX<Percentage<WavLen<EFFECT_STAB>,50>>>,
  
  //Fett263 Multi-blast, blaster reflect cycles through different responsive effects
  EffectSequence<EFFECT_BLAST,ResponsiveBlastL<RgbArg<BLAST_COLOR_ARG,White>,Int<400>,Scale<SwingSpeed<200>,Int<100>,Int<400>>,Int<400>>,LocalizedClashL<RgbArg<BLAST_COLOR_ARG,White>,80,30,EFFECT_BLAST>,ResponsiveBlastWaveL<RgbArg<BLAST_COLOR_ARG,White>,Scale<SwingSpeed<400>,Int<500>,Int<200>>,Scale<SwingSpeed<400>,Int<100>,Int<400>>>,BlastL<RgbArg<BLAST_COLOR_ARG,White>,200,200>,ResponsiveBlastFadeL<RgbArg<BLAST_COLOR_ARG,White>,Scale<SwingSpeed<400>,Int<6000>,Int<12000>>,Scale<SwingSpeed<400>,Int<400>,Int<100>>>,ResponsiveBlastL<RgbArg<BLAST_COLOR_ARG,White>,Scale<SwingSpeed<400>,Int<400>,Int<100>>,Scale<SwingSpeed<400>,Int<200>,Int<100>>,Scale<SwingSpeed<400>,Int<400>,Int<200>>>>,
  
  //Fett263 Real Clash
  Mix<IsLessThan<ClashImpactF<>,Int<26000>>,TransitionEffectL<TrConcat<TrInstant,AlphaL<RgbArg<CLASH_COLOR_ARG,White>,Bump<Scale<BladeAngle<>,Scale<BladeAngle<0,16000>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<-12000>>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<10000>>>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<-10000>>>,Scale<ClashImpactF<>,Int<12000>,Int<60000>>>>,TrFadeX<Scale<ClashImpactF<>,Int<200>,Int<400>>>>,EFFECT_CLASH>,TransitionEffectL<TrWaveX<RgbArg<CLASH_COLOR_ARG,White>,Scale<ClashImpactF<>,Int<100>,Int<400>>,Int<100>,Scale<ClashImpactF<>,Int<100>,Int<400>>,Scale<BladeAngle<>,Scale<BladeAngle<0,16000>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<-12000>>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<10000>>>,Sum<IntArg<LOCKUP_POSITION_ARG,16000>,Int<-10000>>>>,EFFECT_CLASH>>,
  
  //Fett263 Unstable bright ignition effect
  TransitionEffectL<TrConcat<TrInstant,Stripes<3000,-3500,RotateColorsX<Variation,Rgb<100,100,150>>,RandomPerLEDFlicker<RotateColorsX<Variation,Rgb<60,60,80>>,Black>,BrownNoiseFlicker<RotateColorsX<Variation,Rgb<110,115,140>>,RotateColorsX<Variation,Rgb<60,60,80>>,200>,RandomPerLEDFlicker<RotateColorsX<Variation,Rgb<128,128,128>>,RotateColorsX<Variation,Rgb<60,60,80>>>>,TrFadeX<Percentage<WavLen<>,65>>>,EFFECT_IGNITION>,
  
  //Fett263 Bright Humpflicker retraction effect
  TransitionEffectL<TrConcat<TrInstant,HumpFlickerL<White,40>,TrFadeX<Percentage<WavLen<>,125>>>,EFFECT_RETRACTION>,

  //Fett263 Intensity Drag
  LockupTrL<AlphaL<RandomPerLEDFlickerL<RgbArg<DRAG_COLOR_ARG,White>>,SmoothStep<IntArg<DRAG_SIZE_ARG,27500>,Int<5000>>>,TrConcat<TrExtend<4000,TrWipeIn<200>>,AlphaL<BrownNoiseFlickerL<RgbArg<DRAG_COLOR_ARG,White>,Int<300>>,SmoothStep<IntArg<DRAG_SIZE_ARG,29250>,Int<5000>>>,TrFade<4000>>,TrFade<300>,SaberBase::LOCKUP_DRAG>,
  
  //Fett263 Responsive Intensity Melt
  LockupTrL<AlphaL<Remap<Scale<RampF,Int<65536>,Int<0>>,StaticFire<Mix<TwistAngle<>,OrangeRed,DarkOrange>,Mix<TwistAngle<>,OrangeRed,Orange>,0,3,5,3000,10>>,SmoothStep<IntArg<MELT_SIZE_ARG,26000>,Int<6000>>>,TrConcat<TrWipeIn<100>,AlphaL<Red,SmoothStep<Int<29000>,Int<8000>>>,TrExtend<2000,TrFade<300>>,AlphaL<Mix<TwistAngle<>,Red,Orange>,SmoothStep<Int<29000>,Int<8000>>>,TrFade<3000>>,TrFade<250>,SaberBase::LOCKUP_MELT>,
  
  //Fett263 Power Save, if using Fett263's prop file hold AUX and click PWR while ON (pointing up) to dim blade in 25% increments.
  EffectSequence<EFFECT_POWERSAVE,AlphaL<Black,Int<8192>>,AlphaL<Black,Int<16384>>,AlphaL<Black,Int<24576>>,AlphaL<Black,Int<0>>>,

  //Fett263 White Spark Tip ignition and Color Cycle retraction
  InOutTrL<TrJoin<TrWipeX<Percentage<WavLen<EFFECT_IGNITION>,8>>,TrWaveX<White,Percentage<WavLen<EFFECT_IGNITION>,25>,Int<300>,Percentage<WavLen<EFFECT_IGNITION>,8>,Int<0>>>,TrColorCycle<950,7500>>,
  
  //Fett263 optional/alternate Passive Battery Monitor: on boot (1st line) or font change (2nd line) you will get a visual indicator at the emitter of your current battery level. This also works without a blade if you have a lit emitter or blade plug. Green is Full, Red is Low (the color will blend from Green to Red as the battery is depleted), the indicator will fade out after 3000 ms and not display again until powered down and back up or fonts change.

TransitionEffectL<TrConcat<TrDelay<1500>,Black,TrFade<1000>,AlphaL<Mix<BatteryLevel,Red,Green>,Bump<Int<0>,Int<6000>>>,TrFade<3000>>,EFFECT_BOOT>,TransitionEffectL<TrConcat<TrInstant,AlphaL<Mix<BatteryLevel,Red,Green>,Bump<Int<0>,Int<6000>>>,TrFade<3000>>,EFFECT_NEWFONT>,
  
  //Fett263 On-Demand Battery Level: if using Fett263's prop file Hold AUX and click PWR while OFF, the battery level is represented by the location on the blade; tip = full, hilt = low and color; green = full, yellow = half, red = low
  TransitionEffectL<TrConcat<TrInstant,AlphaL<Mix<BatteryLevel,Red,Green>,Bump<BatteryLevel,Int<10000>>>,TrDelay<2000>,AlphaL<Mix<BatteryLevel,Red,Green>,Bump<BatteryLevel,Int<10000>>>,TrFade<1000>>,EFFECT_BATTERY_LEVEL>,TransitionEffectL<TrConcat<TrDelayX<Percentage<WavLen<>,33>>,TransitionLoopL<TrJoin<TrWipeIn<142>,TrSparkX<RotateColorsX<Variation,LemonChiffon>,Int<425>,Int<142>,Int<32768>>>>,TrDelayX<Percentage<WavLen<>,67>>>,EFFECT_PREON>,TransitionEffectL<TrConcat<TrInstant,AlphaL<Mix<Trigger<EFFECT_PREON,Percentage<WavLen<>,33>,Percentage<WavLen<>,67>,Int<0>>,BrownNoiseFlicker<Black,RotateColorsX<Variation,HotPink>,100>,RandomPerLEDFlicker<RotateColorsX<Variation,LightPink>,RotateColorsX<Variation,LemonChiffon>>,BrownNoiseFlicker<Mix<NoisySoundLevel,RotateColorsX<Variation,DeepPink>,RotateColorsX<Int<4000>,RotateColorsX<Variation,Yellow>>>,RotateColorsX<Variation,LemonChiffon>,50>>,SmoothStep<Scale<NoisySoundLevel,Int<-350>,Int<17500>>,Int<-4000>>>,TrDelayX<WavLen<>>>,EFFECT_PREON>,TransitionEffectL<TrConcat<TrInstant,AlphaL<Black,SmoothStep<Sin<Int<10>,Int<16500>,Int<14500>>,Sin<Int<7>,Int<10500>,Int<9500>>>>,TrDelayX<WavLen<>>>,EFFECT_PREON>
  >>(),"mercenary"},

  { "Power", "tracks/Power.wav",
    &style_charging,
    &style_charging, "strobe"},

};

//This is where I define the exact LED strips I used, what pin it is wired to, and how many LEDs are on each strip
BladeConfig blades[] = {
 { 0, SubBlade(0, 143, WS281XBladePtr<288, bladePin, Color8::GRB, PowerPINS<bladePowerPin2, bladePowerPin3> >()),
    SubBladeReverse(143, 287, NULL)
  , CONFIGARRAY(presets) },
};
#endif

//This is where I define how many buttons I used and what pin each is connected to
#ifdef CONFIG_BUTTONS
Button PowerButton(BUTTON_POWER, auxPin, "pow");
Button AuxButton(BUTTON_AUX, powerButtonPin, "aux");
#endif
```


