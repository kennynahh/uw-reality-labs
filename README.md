# reality from scratch
<img src="https://github.com/kennynahh/reality-from-scratch/assets/86166209/60a159f4-3cd3-422a-b64f-7fdb6bef2cae" width="150" height="150">

## about

Reality from Scratch is a DIY VR project by a small team of 1A Systems Design Engineering students at the University of Waterloo.

## planning

Based off of the open-source guide 'Relativty', a few of my classmates and I have been building my very own, fully custom VR headset. So far, we have soldered an IMU and MCU together, and gotten real-time motion vector data translated into SteamVR with drivers forked from OpenVR. We then routed the SteamVR output to displays, which will soon have accompanying fresnel lenses and a 3D-printed housing.

PCBs and other various electrical components have been ordered for 2 DIY Wand-like controllers, which will be based off of the open-source guide 'HadesVR'. Each controller will have an IMU, a rechargeable battery, RF transceivers, tactile buttons, triggers, and joysticks. The HMD's microcontroller will also be upgraded.

## learnings, math

While the accelerometer in IMUs can technically be used for positional tracking, it is just not possible for it to be accurate enough on its own. A positional tracking demo found on YouTube that used the IMU of the Oculus DK1 showed that since acceleration data needs to be integrated twice in order to become displacement (positional) data, there is a significant amount of error (quadratic) introduced into the tracking, which causes drift. Therefore, reference points need to be set up in the surrounding space of the user, through either inside-out or outside-in (base station) tracking.

We hope to achieve a full 6DoF positional tracking system with our headset using either PSMoveServiceEx, or by making use of existing open-source SLAM research projects. Many of these projects were made on Linux, which could possibly complicate things when we get to this point in our project. While ambitious, I hope to have a version in the future that uses a Raspberry Pi 5 (or similar board) integrated into the HMD design that can handle all of the tracking computation, much like Apple's R1 processor in the soon-to-release Vision Pro.

## other

Reality from Scratch was heavily inspired by, and uses resources and knowledge from existing open-source projects, like Relativty and HadesVR.
