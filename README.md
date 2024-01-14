# UW Reality Labs

<p align="middle">
<img src="images/uw-reality-labs/UW Reality Labs Banner.png" width="100%" height="auto" alt="UW Reality Labs Banner">
</p>

## About

UW Reality Labs is a newly founded design team at the University of Waterloo, which offers students the opportunity to engage directly with virtual reality technologies. This initiative focuses on providing practical experience in the inner workings of consumer VR technology, covering a comprehensive range of aspects including hardware, software, drivers, and ergonomics, among others. While we are a design "team", our main focus is learning and research.

This repository is primarily maintained by me - please reach out if you have any questions, or join our [Discord](https://discord.gg/Uv7We55DrX).

## Overview

Based off of open-source guides, we are building our very own, fully custom VR headset. We call it **Reality from Scratch**.

We first soldered an inertial measurement unit (IMU) and microcontroller unit (MCU) together, and got real-time motion vector data translated into SteamVR with drivers forked from the OpenVR SDK. Then, the VR Compositor output was routed to our VR displays, which comes with accompanying lenses and a custom 3D-printed housing. In addition to the HMD, we are building Vive Wand-style controllers, which we'll dive into deeper below.

From this headset, we plan to build other systems, such as an inside-out 6DoF tracking solution using visual-inertial odometry, or a varifocal optical stack using eye tracking and motors or voice coils.

<div align="middle">
<img src="images/HMD_Enclosure_1.jpg" alt="HMD Enclosure for the HMD" style="width: 80%; height: auto;"> </div>

### Basic HMD Hardware

| **Components** | **Our Choice** | **Count** |
| --- | --- | --- |
| IMU | MPU-9250 | 1 |
| MCU | Arduino Pro Micro | 1 |
| Display | 1440x1440 90Hz LCDs | 2 |
| Housing | 3D-printed | *varies* |
| Lenses | $⌀=50mm,$ $f = 50mm$, biconvex, glass | 2 |

### 6DoF Tracking Hardware

| **Components** | **Count** |
| --- | --- |
| PS3 Eye Camera | min 1, max 7 |
| HadesVR/PS3 Move Controllers | 2 |
| USB Hubs | *varies* |

Here's a simple flow chart of how the different components of the VR device interact with each other:

<div align="middle">
<img src="images/End-to-End PC VR Interface.jpg" alt="End-to-End PC VR Interface" style="width: 50%; height: auto;"> </div>

### Controllers (coming very soon!)

Our team has received custom PCBs and other various electrical components for the build of 2 DIY Vive Wand-like controllers, which will be based off of [this](https://github.com/HadesVR/Wand-Controller) WIP open-source guide by LiquidCGS (creator of HadesVR). Each controller will have an IMU, a rechargeable battery, RF transceivers, tactile buttons, triggers, and joysticks. The HMD's microcontroller will also be upgraded and moved onto a central PCB. We are almost done with soldering.

<div align="middle">
<img src="images/controllers_unfinished.jpg" alt="Controllers! (unfinished)" style="width: 50%; height: auto;"> </div>

### Drivers

SteamVR is the only universal platform with accessible driver SDKs (from Valve's OpenVR). It is an easy choice for an open-source VR project. The drivers for Reality from Scratch can be found [here](https://github.com/kennynahh/reality-from-scratch/tree/main/drivers). We use them in conjunction with the FastIMU Arduino library.

### Lenses

We recommend [fresnel](https://xinreality.com/wiki/Fresnel_lens) lenses for any DIY VR build, since they are readily available for very low prices on platforms like Amazon and Aliexpress, and are thin and lightweight. Traditional biconvex lenses are wider, heavier (when built with glass), and usually cost more, but they may have increased visual clarity and no god rays. This is due to the design of fresnels and their fine concentric lines, which can introduce god rays and other distracting artifacts. Below is a visual comparision that should help explain the design of fresnel lenses further.

<p align="middle">
  <img src="images/fresnel_plano_convex.jpg" width="200" height="300" alt="Fresnel and equivalent plano-convex lens." style="background-color:white;"/>
  <img src="images/fresnel_lens_collapse.jpg" width="200" height="300" alt="Collapsing a conventional lens into an equivalent power Fresnel lens." style="background-color:white;"/>
  <br>
  <caption><em>Figure 1 (left): Fresnel lens (left) and equivalent power plano-convex lens (right).</em></caption>
    <br>
  <caption><em>Figure 2 (right): Collapsing a conventional lens into an equivalent power Fresnel lens.</em></caption>
</p>

Outside of biconvex and fresnel lenses, there are not many options, though there are some advanced [DIY stacked lens](https://hackaday.io/project/187343-easy-pancake-lenses) solutions out in the wild.

A lot of these simpler lenses can be purchased at a wide assortment of focal lengths; we hope to test multiple. Depending on the focal length, the length of the headset's housing will vary, so there may be some merit in seeing the difference between shorter and longer housings (in terms of image quality, FOV, perceived weight of the HMD and how this affects comfort, etc.) Creating the mechanical design for the housing of the VR headset will be challenging, given that we'd like to include user-addressable IPD, as well as test different housing for different lenses. However, we are very excited to begin testing this once we finish modelling the 3D prints for our headset.

## Building your own Reality from Scratch

For a comphrehensive guide on how to build your own Reality from Scratch - our open-source, DIY HMD, check out the guide [here](HMD-Guide.md), or join the UW Reality Labs team if you're a student at the University of Waterloo!

<div align="middle">
<img src="images/HadesVR_HMD_unfinished.jpg" alt="unfinished HMD module built on HadesVR PCB" style="width: 50%; height: auto;">
<br>
<caption><em>work-in-progress HMD module built on HadesVR PCB.</em></caption>
    </div>
    <br>

## Research

This section dives into the various technological barriers when desinging comfortable and user-friendly VR products, and how these obstacles can be overcome. We talk about the limitations of IMUs when considering 6DoF tracking and the importance of lenses and optical technologies in VR.

### Tracking

While the accelerometer in IMUs can technically be used for positional tracking, it is just not possible for it to be accurate enough on its own. A positional tracking demo found on [YouTube](https://youtu.be/_q_8d0E3tDk?si=je-FXEluvx5F-icd) that used the IMU of the Oculus DK1 showed that since acceleration data needs to be integrated twice in order to become displacement (positional) data, there is a significant amount of error (quadratic) introduced into the tracking over time (in the order of meters per second), which causes drift. Therefore, reference points need to be set up in the surrounding space of the user, through either inside-out ([visual-intertial odometry](https://en.wikipedia.org/wiki/Visual_odometry)) or outside-in (base-station/lighthouse) tracking.

Knowing this helps us understand how modern standalone and PC VR headsets handle positional tracking. On Oculus's "Cresent Bay" prototype and (consequently) the Rift CV1, for example, the IMU was almost purely used for positionally tracking fast movements, but the external camera would track the "constellations" (IR LEDs) on the HMD and Touch controllers and continously correct the IMU's positional error that, without the external camera, would quadratically increase over time.

<p align="middle">
  <img src="https://duet-cdn.vox-cdn.com/thumbor/0x0:4001x2240/1200x800/filters:focal(2000x1120:2001x1121):format(webp)/cdn.vox-cdn.com/uploads/chorus_asset/file/15295653/Crescent-Bay-Front-Pers-on-Light.0.0.1426288038.jpg" width="500" height="auto" alt="Fresnel and equivalent plano-convex lens." style="background-color:white;"/>
  <br>
  <caption>The Oculus Cresent Bay prototype, with its IR LEDs.</caption>
</p>

We hope to achieve a full 6DoF positional tracking system with our headset by initially using [PSMoveServiceEx](https://github.com/psmoveservice/PSMoveService). This is similar to outside-in tracking using IR LEDs, except that there is only one 'giant' LED, which emits colour on the visible spectrum of wavelength. Next, we want to work on implementing existing open-source SLAM research projects (like [Basalt](https://github.com/VladyslavUsenko/basalt-mirror)) that can create '[point clouds](https://en.wikipedia.org/wiki/Point_cloud)' (or similar) of the surrounding space and use this information as reference points for inside-out postitional tracking.

### Optics and comfort

The interactions between the lenses and the user can often make or break the experience of a lot of consumer VR headsets. Popular consumer VR products like the [Valve Index](https://www.valvesoftware.com/en/index/deep-dive/) have fully adjustable lens interpupilary distance (IPD), as well as adjustable eye relief (distance of the lenses from the user's eyes). This makes this product much more accessible to a wide range of users, and serves to be more comfortable for individuals sensitiive to such changes. However, the further the lenses are from the user's eyes, the smaller the field of view (FoV) is, (given the lenses stay the same size) - and this is why it is especially important that the VR headset sits at a reasonable length from the user's face. The Valve Index also has canted (tilted) displays, to further conform around the face of the user. This is beneficial for many reasons (including FoV), but also introduces new types of optical distortions that are difficult to correct.

The issues regarding comfort go further. The [vergence-accommodation conflict](https://en.wikipedia.org/wiki/Vergence-accommodation_conflict) is one phenomenon that VR researchers have been working to solve for many years. It occurs when a user perceives an object in VR to be a certain distance away, but the user's eyes are focused at a a different distance, due to the lenses in the VR headset only being able to show one fixed focal distance (the distance between the user and the virtual object of focus). This focal distance is said to be at around 2m for most consumer VR headets today, meaning that any object closer than or further than 2m away in a virtual scene will look blurry and be unable to focus on naturally. This can cause eye strain and motion sickness, which is likely a deal-breaker for many would-be new VR users.

<p align="middle">
  <img src="images/Vergence-Accommodation_Conflict_Diagram.jpg" width="500" height="auto" alt="Vergence-accommodation conflict diagram." style="background-color:white;"/>
  <br>
  <caption>Vergence-accommodation conflict example. [1]</caption>
</p>

Meta Reality Labs revealed that they were working on a varifocal VR headset prototype as early as 2018 (called Half-Dome). The idea of the first Half-Dome prototype was to use eye tracking built in to the headset to figure out where the user was looking and which in-game object they were attempting to focus on. Then, through Oculus's API, the focal distance to that object could be found and then stepper motors within the headset would move the lenses accordingly, to set the lenses to that focal distance.

This Half-Dome prototype is exactly the type of concept and the level of fidelity that the team wishes to implement into our headset at some point, whether or not it be univerally compatible with SteamVR. This will begin with implementing eye-tracking hardware into Reality from Scratch.

## Acknowledgements

The Reality from Scratch HMD draws inspiration from and utilizes resources and knowledge from existing open-source projects, such as Relativty and HadesVR. We really appreciate the members of the DIY VR community who have put in their best efforts to maintain this open-source knowledge.

In addition, the UW Reality Labs design team draws obvious inspiration from Meta's own Reality Labs.

## Resources

A large portion of our research comes from helpful articles and sources from companies like Valve, and from initiatives like XinReality. These and other useful resources are cited throughout this document, and repeated below; we encourage you to read them.

[Hands-On with Meta's New VR Headset Prototypes! - Tested (YouTube)](https://www.youtube.com/watch?v=x6AOwDttBsc)
<em>This one we really recommend watching.</em>

[XinReality - Frensel Lenses](https://xinreality.com/wiki/Fresnel_lens)

[Vergence-accommodation conflict - Wikipedia](https://en.wikipedia.org/wiki/Vergence-accommodation_conflict)

[Doc-Ok.org - Hacking the Oculus DK2](http://doc-ok.org/?p=1095)

[Visual odometry - Wikipedia](https://en.wikipedia.org/wiki/Visual_odometry)

[Valve Software - Valve Index Deep Dive](https://www.valvesoftware.com/en/index/deep-dive/)

[LiquidCGS - HadesVR](https://github.com/HadesVR/HadesVR)

[WalkerDev (Hackaday.io) - Easy "Pancake Lenses"](https://hackaday.io/project/187343-easy-pancake-lenses)

[RoNIN: Robust Neural Inertial Navigation](https://ronin.cs.sfu.ca/)

[Aspheric Lens - Wikipedia](https://en.wikipedia.org/wiki/Aspheric_lens)

[Dispersion (optics) - Wikipedia](https://en.wikipedia.org/wiki/Dispersion_(optics))

[Augmented and Mixed Reality Optical See-Through Combiners Based on Plastic Optics](https://sid.onlinelibrary.wiley.com/doi/10.1002/msid.1226)

[Achromatic lens - Wikipedia](https://en.wikipedia.org/wiki/Achromatic_lens)

[How Varjo delivers human eye resolution - Varjo](https://varjo.com/blog/introducing-bionic-display-how-varjo-delivers-human-eye-resolution/)

[Hypervision | XR240.Gen2](https://www.hypervision.ai/configure-xr240)

[Project Caliper - Prototyping VR Controllers](https://skarredghost.com/2021/12/08/project-caliper-prototyping-controllers-vr/)

[EyeTrackVR Docs](https://docs.eyetrackvr.dev)

[1] By Rosedaler - Own work, CC BY-SA 4.0, <https://commons.wikimedia.org/w/index.php?curid=123242579>

*Repository originally created on November 23, 2023.*
