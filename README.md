# reality from scratch <img src="https://github.com/kennynahh/reality-from-scratch/assets/86166209/60a159f4-3cd3-422a-b64f-7fdb6bef2cae" width="100" height="100">

## about

Reality from Scratch is a DIY VR project by a small team of 1A Systems Design Engineering students at the University of Waterloo. This project aims to encompass all components of a VR headset - the hardware, the drivers, the housing, and so on. This repository is primarily maintained by myself. Please reach out to me (Kenny) if you have any questions.

## overview

Based off of open-source guides, we have been building our very own, fully custom VR headset. So far, we have soldered an IMU and MCU together, and gotten real-time motion vector data translated into SteamVR with drivers forked from the OpenVR SDK. We then routed the SteamVR output to displays, which will soon have accompanying fresnel lenses and a 3D-printed housing.

PCBs and other various electrical components have been ordered for 2 DIY Wand-like controllers, which will be based off of the open-source guide 'HadesVR'. Each controller will have an IMU, a rechargeable battery, RF transceivers, tactile buttons, triggers, and joysticks. The HMD's microcontroller will also be upgraded.

## ideation

### tracking

While the accelerometer in IMUs can technically be used for positional tracking, it is just not possible for it to be accurate enough on its own. A positional tracking demo found on [YouTube](https://youtu.be/_q_8d0E3tDk?si=je-FXEluvx5F-icd) that used the IMU of the Oculus DK1 showed that since acceleration data needs to be integrated twice in order to become displacement (positional) data, there is a significant amount of error (quadratic) introduced into the tracking, which causes drift. Therefore, reference points need to be set up in the surrounding space of the user, through either inside-out or outside-in (base station) tracking.

We hope to achieve a full 6DoF positional tracking system with our headset using either PSMoveServiceEx, or by making use of existing open-source SLAM research projects. Many of these projects were made on Linux, which could possibly complicate things when we get to this point in our project. While ambitious, I hope to have a version in the future that uses a Raspberry Pi 5 (or similar board) integrated into the HMD design that can handle all of the tracking computation, much like Apple's R1 processor.

### designing around lenses

The interactions between the lenses and the user can often make or break the experience of a lot of consumer VR headsets. Popular consumer VR products like the [Valve Index](https://www.valvesoftware.com/en/index/deep-dive/) have fully adjustable lens interpupilary distance (IPD), as well as adjustable eye relief (distance of the lenses from the user's eyes). This makes this product much more accessible to a wide range of users, and serves to be more comfortable for individuals sensitiive to such changes. However, the further the lenses are from the user's eyes, the smaller the field of view (FoV) is, (given the lenses stay the same size) - and this is why it is especially important that the VR headset sits at a reasonable length from the user.

We picked [fresnel](https://xinreality.com/wiki/Fresnel_lens) lenses for now, since they are readily available for very low prices on platforms like Amazon and Aliexpress, and are thin and lightweight. A lot of these lenses can be purchased at various focal lengths; we hope to test multiple. Depending on the focal length, the length of the headset's housing will vary, so there may be some merit in seeing the difference between shorter and longer housings (in terms of image quality, FOV, perceived weight of the HMD and how this affects comfort, etc.) Creating the mechanical design for the housing of the VR headset will be challenging, given that we'd like to include user-addressable IPD, as well as test different housing for different lenses.

### drivers

SteamVR is the only universal platform with easily accessible driver SDKs from the OpenVR repository. It is an easy choice to pick for an open-source project. While we hope in the future to build our own drivers, for now (in our early stages), we're going to borrow the drivers from [HadesVR](https://github.com/HadesVR/HadesVR), an existing project on DIY VR.

## resources

Reality from Scratch draws inspiration from and utilizes resources and knowledge from existing open-source projects, such as Relativty and HadesVR. A large portion of our research comes from helpful articles and sources from companies like Valve, and from initiatives like XinReality. These useful resources will be cited throughout this doc.
