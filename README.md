# Reality from Scratch

<img src="https://github.com/kennynahh/reality-from-scratch/assets/86166209/60a159f4-3cd3-422a-b64f-7fdb6bef2cae" width="100" height="100">

## About

Reality from Scratch is a newly founded design team at the University of Waterloo, which offers students the opportunity to engage directly with virtual reality technologies. This initiative focuses on providing practical experience in the inner workings of consumer VR technology, covering a comprehensive range of aspects including hardware, software, drivers, and ergonomics, among others. While we are a design "team", our main focus is learning and research.

This repository is primarily maintained by me - please reach out if you have any questions.

## Overview

Based off of open-source guides, we are building our very own, fully custom VR headset. We first soldered an inertial measurement unit (IMU) and microcontroller unit (MCU) together, and got real-time motion vector data translated into SteamVR with drivers forked from the OpenVR SDK. We then routed the VR compositor output to our VR displays, which will soon have accompanying lenses and a custom 3D-printed housing. In addition to the HMD, we are building Vive Wand-style controllers, which we'll dive into deeper below.

### Basic Head-Mounted Display (HMD) Hardware

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

Here's a simple flow chart of the how the different components of the VR device interact with each other:

<div>
<img src="images/End-to-End PC VR Interface.jpg" alt="End-to-End PC VR Interface" style="width: 50%; height: auto;"> </div>

### Controllers (coming very soon!)

Our team has received custom PCBs and other various electrical components for the build of 2 DIY Vive Wand-like controllers, which will be based off of [this](https://github.com/HadesVR/Wand-Controller) WIP open-source guide by LiquidCGS (creator of HadesVR). Each controller will have an IMU, a rechargeable battery, RF transceivers, tactile buttons, triggers, and joysticks. The HMD's microcontroller will also be upgraded and moved onto a central PCB. We are almost done with soldering.

<div>
<img src="images/controllers_unfinished.jpg" alt="Controllers! (unfinished)" style="width: 60%; height: auto;"> </div>

### Drivers

SteamVR is the only universal platform with accessible driver SDKs. It is an easy choice to pick for an open-source project. The drivers for Reality from Scratch can be found [here](https://github.com/kennynahh/reality-from-scratch/tree/main/drivers).

## Building your own Reality from Scratch
For a comphrehensive guide on how to build your own Reality from Scratch, check out the guide [here](/docs/Guide.md), or join the Reality from Scratch team if you're a student at the University of Waterloo!

<div>
<img src="images/HadesVR_HMD_unfinished.jpg" alt="unfinished HMD module built on HadesVR PCB" style="width: 60%; height: auto;">
<br>
<caption><em>work-in-progress HMD module built on HadesVR PCB.</em></caption>
    </div>
    <br>

## Research

### Tracking

While the accelerometer in IMUs can technically be used for positional tracking, it is just not possible for it to be accurate enough on its own. A positional tracking demo found on [YouTube](https://youtu.be/_q_8d0E3tDk?si=je-FXEluvx5F-icd) that used the IMU of the Oculus DK1 showed that since acceleration data needs to be integrated twice in order to become displacement (positional) data, there is a significant amount of error (quadratic) introduced into the tracking over time (in the order of meters per second), which causes drift. Therefore, reference points need to be set up in the surrounding space of the user, through either inside-out ([visual-intertial odometry](https://en.wikipedia.org/wiki/Visual_odometry)) or outside-in (base-station/lighthouse) tracking.

Knowing this helps us understand how modern standalone and PC VR headsets handle positional tracking. On Oculus's "Cresent Bay" prototype and (consequently) the Rift CV1, for example, the IMU was almost purely used for positionally tracking fast movements, but the external camera would track the "constellations" (IR LEDs) on the HMD and Touch controllers and continously correct the IMU's positional error that, without the external camera, would quadratically increase over time.

<p align="middle">
  <img src="https://duet-cdn.vox-cdn.com/thumbor/0x0:4001x2240/1200x800/filters:focal(2000x1120:2001x1121):format(webp)/cdn.vox-cdn.com/uploads/chorus_asset/file/15295653/Crescent-Bay-Front-Pers-on-Light.0.0.1426288038.jpg" width="50%" height="auto" alt="Fresnel and equivalent plano-convex lens." style="background-color:white;"/>
  <br>
  <caption>The Oculus Cresent Bay prototype, with its IR LEDs.</caption>
</p>

We hope to achieve a full 6DoF positional tracking system with our headset by initially using [PSMoveServiceEx](https://github.com/psmoveservice/PSMoveService). This is similar to outside-in tracking using IR LEDs, except that there is only one 'giant' LED, which emits colour on the visible spectrum of wavelength. Next, we want to work on implementing existing open-source SLAM research projects (like [Basalt](https://github.com/VladyslavUsenko/basalt-mirror)) that can create '[point clouds](https://en.wikipedia.org/wiki/Point_cloud)' (or similar) of the surrounding space and use this information as reference points for inside-out postitional tracking.

### Designing around lenses

The interactions between the lenses and the user can often make or break the experience of a lot of consumer VR headsets. Popular consumer VR products like the [Valve Index](https://www.valvesoftware.com/en/index/deep-dive/) have fully adjustable lens interpupilary distance (IPD), as well as adjustable eye relief (distance of the lenses from the user's eyes). This makes this product much more accessible to a wide range of users, and serves to be more comfortable for individuals sensitiive to such changes. However, the further the lenses are from the user's eyes, the smaller the field of view (FoV) is, (given the lenses stay the same size) - and this is why it is especially important that the VR headset sits at a reasonable length from the user's face.

<video align="middle" width="50%" height="auto">
  <source src="https://cdn.cloudflare.steamstatic.com/valvesoftware/images/index/videos/IndexEyeRelief.webm" type="video/webm">
  Your browser does not support the video format.
</video>

We recommend [fresnel](https://xinreality.com/wiki/Fresnel_lens) lenses for now, since they are readily available for very low prices on platforms like Amazon and Aliexpress, and are thin and lightweight. Traditional biconvex lenses are wider, heavier (when built with glass), and usually cost more, but they may have increased visual clarity and no god rays. (due to the design of fresnels and their fine concentric lines, they can introduce god rays and other distracting artifacts.) 

<p align="middle">
  <img src="images/fresnel_plano_convex.jpg" width="200" height="auto" alt="Fresnel and equivalent plano-convex lens." style="background-color:white;"/>
  <img src="images/fresnel_lens_collapse.jpg" width="200" height="auto" alt="Collapsing a conventional lens into an equivalent power Fresnel lens." style="background-color:white;"/> 
  <br>
  <caption><em>Figure 1 (left): Fresnel lens (left) and equivalent power plano-convex lens (right).</em></caption>
    <br>
  <caption><em>Figure 2 (right): Collapsing a conventional lens into an equivalent power Fresnel lens.</em></caption>
</p>

Outside of biconvex and fresnel lenses, there are not many options, though there are some advanced [DIY stacked lens](https://hackaday.io/project/187343-easy-pancake-lenses) solutions out in the wild.

A lot of these simpler lenses can be purchased at a wide assortment of focal lengths; we hope to test multiple. Depending on the focal length, the length of the headset's housing will vary, so there may be some merit in seeing the difference between shorter and longer housings (in terms of image quality, FOV, perceived weight of the HMD and how this affects comfort, etc.) Creating the mechanical design for the housing of the VR headset will be challenging, given that we'd like to include user-addressable IPD, as well as test different housing for different lenses. However, we are very excited to begin testing this once we finish modelling the 3D prints for our headset.

## Acknowledgements

Reality from Scratch draws inspiration from and utilizes resources and knowledge from existing open-source projects, such as Relativty and HadesVR. We really appreciate the members of the DIY VR community who have put in their best efforts to maintain the open-source community.

## Resources

A large portion of our research comes from helpful articles and sources from companies like Valve, and from initiatives like XinReality. These and other useful resources are cited throughout this document, and repeated below; we encourage you to read them.

[XinReality - Frensel Lenses](https://xinreality.com/wiki/Fresnel_lens)

[Doc-Ok.org - Hacking the Oculus DK2](http://doc-ok.org/?p=1095)

[Visual odometry - Wikipedia](https://en.wikipedia.org/wiki/Visual_odometry)

[Valve Software - Valve Index Deep Dive](https://www.valvesoftware.com/en/index/deep-dive/)

[LiquidCGS - HadesVR](https://github.com/HadesVR/HadesVR)

[WalkerDev (Hackaday.io) - Easy "Pancake Lenses"](https://hackaday.io/project/187343-easy-pancake-lenses)

[RoNIN: Robust Neural Inertial Navigation](https://ronin.cs.sfu.ca/)

[Aspheric Lens - Wikipedia](https://en.wikipedia.org/wiki/Aspheric_lens)

[How Varjo delivers human eye resolution - Varjo](https://varjo.com/blog/introducing-bionic-display-how-varjo-delivers-human-eye-resolution/)

Visual-Inertial Mapping with Non-Linear Factor Recovery, V. Usenko, N. Demmel, D. Schubert, J. Stückler, D. Cremers, In IEEE Robotics and Automation Letters (RA-L) [DOI:10.1109/LRA.2019.2961227] [arXiv:1904.06504].

*Repository originally created on November 23, 2023.*