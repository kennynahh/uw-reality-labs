# Reality from Scratch

<img src="https://github.com/kennynahh/reality-from-scratch/assets/86166209/60a159f4-3cd3-422a-b64f-7fdb6bef2cae" width="100" height="100">

## About

Reality from Scratch is a newly founded design team at the University of Waterloo, which offers students the opportunity to engage directly with virtual reality (VR) technologies. This initiative focuses on providing practical experience in VR, covering a comprehensive range of aspects including hardware, software, drivers, and ergonomics, among others. This repository is primarily maintained by me - please reach out if you have any questions.

## Overview

Based off of open-source guides, the team has been building our very own, fully custom VR headset. So far, we have soldered an inertial measurement unit (IMU) and microcontroller unit (MCU) together, and gotten real-time motion vector data translated into SteamVR with drivers forked from the OpenVR SDK. We then routed the SteamVR output to our VR displays, which will soon have accompanying fresnel lenses and a 3D-printed housing.

### Basic HMD Hardware

| **Components** | **Our Choice** | **Count** |
| --- | --- | --- |
| IMU | MPU-9250 | 1 |
| MCU | Arduino Pro Micro | 1 |
| Display | 1440x1440 90Hz LCDs | 2 |
| Housing | 3D-printed | *varies* |
| Lenses | $⌀=40mm,$ $f = 50mm$ fresnels | 2 |

The Ardiuno Pro Micro is a good choice since it supports USB HID. The USB HID class is generally more optimized for smaller, more frequent packets of data vs. serial, which is better for our use since we'll be sending small but very frequent data from our IMU. In terms of IMU choice, any that supports [FastIMU](https://github.com/LiquidCGS/FastIMU) will work. Any display will work as well - there are a plethora of options on Aliexpress. We talk more about lenses further below.

For a comphrehensive guide on how to build a basic HMD, check out project docs from other DIY VR projects, like [HadesVR's](https://github.com/HadesVR/HadesVR/blob/main/docs/DocsIndex.md).

### Controllers (coming soon!)

Custom PCBs and other various electrical components have been ordered for 2 DIY Vive Wand-like controllers, which will be based off of [this](https://github.com/HadesVR/Wand-Controller) WIP open-source guide by LiquidCGS (creator of HadesVR). Each controller will have an IMU, a rechargeable battery, RF transceivers, tactile buttons, triggers, and joysticks. The HMD's microcontroller will also be upgraded and moved onto a central PCB.

### Drivers

SteamVR is the only universal platform with accessible driver SDKs. It is an easy choice to pick for an open-source project. While we hope in the future to build our own drivers from the OpenVR SDK, for now (in our very early stage), we're going to borrow drivers from [HadesVR](https://github.com/HadesVR/HadesVR), an existing project on DIY VR. These drivers can be found [here](https://github.com/kennynahh/reality-from-scratch/tree/main/drivers). They should be placed in the drivers folder in the SteamVR file path.

## Ideation

### Tracking

While the accelerometer in IMUs can technically be used for positional tracking, it is just not possible for it to be accurate enough on its own. A positional tracking demo found on [YouTube](https://youtu.be/_q_8d0E3tDk?si=je-FXEluvx5F-icd) that used the IMU of the Oculus DK1 showed that since acceleration data needs to be integrated twice in order to become displacement (positional) data, there is a significant amount of error (quadratic) introduced into the tracking over time (in the order of meters per second), which causes drift. Therefore, reference points need to be set up in the surrounding space of the user, through either inside-out ([visual-intertial odometry](https://en.wikipedia.org/wiki/Visual_odometry)) or outside-in (base-station/lighthouse) tracking.

Knowing this helps us understand how modern standalone and PC VR headsets have positional tracking. On Oculus's "Cresent Bay" prototype and (consequently) the Rift CV1, for example, the IMU was almost purely used for positionally tracking fast movements, but the external camera would track the "constellations" (IR LEDs in array) on the HMD and Touch controllers and continously correct the IMU's positional error that, without the external camera, would quadratically increase over time.

We hope to achieve a full 6DoF positional tracking system with our headset using either [PSMoveServiceEx](https://github.com/psmoveservice/PSMoveService), or by making use of existing open-source SLAM research projects that can create '[point clouds](https://en.wikipedia.org/wiki/Point_cloud)' (or similar) of the surrounding space and use this information as reference points. Many of these projects/papers (like [Basalt](https://github.com/VladyslavUsenko/basalt-mirror)) were made on Linux, which could possibly complicate things when we get to this point in our project. While ambitious, we hope to have a version in the future that uses a Raspberry Pi 5 (or similar board) with cameras and depth sensors integrated into the HMD design that can handle all of the tracking computation, much like Apple's R1 processor.

### Designing around lenses

The interactions between the lenses and the user can often make or break the experience of a lot of consumer VR headsets. Popular consumer VR products like the [Valve Index](https://www.valvesoftware.com/en/index/deep-dive/) have fully adjustable lens interpupilary distance (IPD), as well as adjustable eye relief (distance of the lenses from the user's eyes). This makes this product much more accessible to a wide range of users, and serves to be more comfortable for individuals sensitiive to such changes. However, the further the lenses are from the user's eyes, the smaller the field of view (FoV) is, (given the lenses stay the same size) - and this is why it is especially important that the VR headset sits at a reasonable length from the user's face.

We picked [fresnel](https://xinreality.com/wiki/Fresnel_lens) lenses for now, since they are readily available for very low prices on platforms like Amazon and Aliexpress, and are thin and lightweight. A lot of these lenses can be purchased at various focal lengths; we hope to test multiple. Depending on the focal length, the length of the headset's housing will vary, so there may be some merit in seeing the difference between shorter and longer housings (in terms of image quality, FOV, perceived weight of the HMD and how this affects comfort, etc.) Creating the mechanical design for the housing of the VR headset will be challenging, given that we'd like to include user-addressable IPD, as well as test different housing for different lenses. However, we are excited to begin testing this once our lenses arrive.

## Resources

Reality from Scratch draws inspiration from and utilizes resources and knowledge from existing open-source projects, such as Relativty and HadesVR. We really appreciate the members of the DIY VR community who have put in their best efforts to maintain the open-source community.

A large portion of our research comes from helpful articles and sources from companies like Valve, and from initiatives like XinReality. These and other useful resources are cited throughout this document, and repeated below; we encourage you to read them.

[XinReality - Frensel Lenses](https://xinreality.com/wiki/Fresnel_lens)

[Doc-Ok.org - Hacking the Oculus DK2](http://doc-ok.org/?p=1095)

[Visual odometry - Wikipedia](https://en.wikipedia.org/wiki/Visual_odometry)

[Valve Software - Valve Index Deep Dive](https://www.valvesoftware.com/en/index/deep-dive/)

[LiquidCGS - HadesVR](https://github.com/HadesVR/HadesVR)

[WalkerDev (Hackaday.io) - Easy "Pancake Lenses"](https://hackaday.io/project/187343-easy-pancake-lenses)

[RoNIN: Robust Neural Inertial Navigation](https://ronin.cs.sfu.ca/)

Visual-Inertial Mapping with Non-Linear Factor Recovery, V. Usenko, N. Demmel, D. Schubert, J. Stückler, D. Cremers, In IEEE Robotics and Automation Letters (RA-L) [DOI:10.1109/LRA.2019.2961227] [arXiv:1904.06504].

*Originally created November 23, 2023.*