# reality from scratch

<img src="https://github.com/kennynahh/reality-from-scratch/assets/86166209/60a159f4-3cd3-422a-b64f-7fdb6bef2cae" width="100" height="100">

## about

Reality from Scratch is a DIY VR project by a small team of 1A Systems Design Engineering students at the University of Waterloo. This project aims to encompass all components of a consumer PC VR headset - the hardware, the drivers, the housing, and so on. This repository is primarily maintained by myself. Please reach out to me if you have any questions.

## overview

Based off of open-source guides, we have been building our very own, fully custom VR headset. So far, we have soldered an inertial measurement unit (IMU) and microcontroller unit (MCU) together, and gotten real-time motion vector data translated into SteamVR with drivers forked from the OpenVR SDK. We then routed the SteamVR output to displays, which will soon have accompanying fresnel lenses and a 3D-printed housing.

### basic HMD hardware

| **part type** | **our pick** | **qty.** |
| --- | --- | --- |
| IMU | MPU-6050 | 1 |
| MCU | arduino pro micro | 1 |
| display | 1440x1440 90Hz LCDs | 2 (or 1 long) |
| housing | 3D printed | ~ |
| lenses | ⌀40mm/f:50mm fresnels | 2 |

The Ardiuno Pro Micro is a good choice since it supports USB HID. HID is generally more optimized for smaller, more frequent packets of data vs. serial, which is better for our use since we'll be sending small but very frequent data from our IMU. In terms of IMU choice, any that supports [FastIMU](https://github.com/LiquidCGS/FastIMU) will work. Any display will work as well - there are a plethora of options on Aliexpress. We talk more about lenses further below.

For a comphrehensive guide on how to build a basic HMD, check out project docs from other DIY VR projects, like [HadesVR's](https://github.com/HadesVR/HadesVR/blob/main/docs/DocsIndex.md).

### controllers (coming soon!)

Custom PCBs and other various electrical components have been ordered for 2 DIY Vive Wand-like controllers, which will be based off of [this](https://github.com/HadesVR/Wand-Controller) WIP open-source guide by LiquidCGS (creator of HadesVR). Each controller will have an IMU, a rechargeable battery, RF transceivers, tactile buttons, triggers, and joysticks. The HMD's microcontroller will also be upgraded and moved onto a central PCB.

### drivers

SteamVR is the only universal platform with accessible driver SDKs. It is an easy choice to pick for an open-source project. While we hope in the future to build our own drivers from the OpenVR SDK, for now (in our very early stage), we're going to borrow drivers from [HadesVR](https://github.com/HadesVR/HadesVR), an existing project on DIY VR. These drivers can be found [here](https://github.com/kennynahh/reality-from-scratch/tree/main/drivers). They should be placed in the drivers folder in the SteamVR file path.

## ideation

### tracking

While the accelerometer in IMUs can technically be used for positional tracking, it is just not possible for it to be accurate enough on its own. A positional tracking demo found on [YouTube](https://youtu.be/_q_8d0E3tDk?si=je-FXEluvx5F-icd) that used the IMU of the Oculus DK1 showed that since acceleration data needs to be integrated twice in order to become displacement (positional) data, there is a significant amount of error (quadratic) introduced into the tracking, which causes drift. Therefore, reference points need to be set up in the surrounding space of the user, through either inside-out ([visual-intertial odometry](https://en.wikipedia.org/wiki/Visual_odometry)) or outside-in (base-station/lighthouse) tracking.

We hope to achieve a full 6DoF positional tracking system with our headset using either [PSMoveServiceEx](https://github.com/psmoveservice/PSMoveService), or by making use of existing open-source SLAM research projects that can create '[point clouds](https://en.wikipedia.org/wiki/Point_cloud)' (or similar) of the surrounding space and use this information as reference points. Many of these projects/papers (like [Basalt](https://github.com/VladyslavUsenko/basalt-mirror)) were made on Linux, which could possibly complicate things when we get to this point in our project. While ambitious, I hope to have a version in the future that uses a Raspberry Pi 5 (or similar board) with cameras and depth sensors integrated into the HMD design that can handle all of the tracking computation, much like Apple's R1 processor.

### designing around lenses

The interactions between the lenses and the user can often make or break the experience of a lot of consumer VR headsets. Popular consumer VR products like the [Valve Index](https://www.valvesoftware.com/en/index/deep-dive/) have fully adjustable lens interpupilary distance (IPD), as well as adjustable eye relief (distance of the lenses from the user's eyes). This makes this product much more accessible to a wide range of users, and serves to be more comfortable for individuals sensitiive to such changes. However, the further the lenses are from the user's eyes, the smaller the field of view (FoV) is, (given the lenses stay the same size) - and this is why it is especially important that the VR headset sits at a reasonable length from the user's face.

We picked [fresnel](https://xinreality.com/wiki/Fresnel_lens) lenses for now, since they are readily available for very low prices on platforms like Amazon and Aliexpress, and are thin and lightweight. A lot of these lenses can be purchased at various focal lengths; we hope to test multiple. Depending on the focal length, the length of the headset's housing will vary, so there may be some merit in seeing the difference between shorter and longer housings (in terms of image quality, FOV, perceived weight of the HMD and how this affects comfort, etc.) Creating the mechanical design for the housing of the VR headset will be challenging, given that we'd like to include user-addressable IPD, as well as test different housing for different lenses. However, we are excited to begin testing this once our lenses arrive.

## resources

Reality from Scratch draws inspiration from and utilizes resources and knowledge from existing open-source projects, such as Relativty and HadesVR. We really appreciate the members of the DIY VR community who continue to update their repos. 

A large portion of our research comes from helpful articles and sources from companies like Valve, and from initiatives like XinReality. These useful resources are cited throughout this doc.
