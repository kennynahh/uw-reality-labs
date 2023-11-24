# reality from scratch
<img src="https://github.com/kennynahh/reality-from-scratch/assets/86166209/60a159f4-3cd3-422a-b64f-7fdb6bef2cae" width="100" height="100">

Reality from Scratch is a DIY VR project by Systems Design Engineering students at the University of Waterloo.

VR and AR has been a passion of mine for many years, and it's only now that I feel I have the financial means to experience it. But as a learner and tinkerer at heart, I wasn't just going to purchase a Quest.

## planning

Based off of the open-source guide 'Relativty', my friends and I have been building my very own, fully custom VR headset. So far, we have soldered an IMU and MCU together, and gotten real-time motion vector data translated into SteamVR with drivers courtesy of the Relativty project. We then routed the SteamVR output to displays, which will soon have fresnel lenses and a 3D-printed housing.

PCBs and other various electrical components have been ordered for 2 DIY Wand-like controllers, which will be based off of the open-source guide 'HadesVR'. Each controller will have an IMU, a rechargeable battery, RF transceivers, tactile buttons, triggers, and joysticks. The HMD's microcontroller will also be upgraded.

## learnings, math

While the accelerometer in IMUs can technically be used for positional tracking, it is just not possible for it to be accurate enough on its own. A positional tracking demo found on YouTube that used the IMU of the Oculus DK1 showed that since acceleration data needs to be integrated twice in order to become displacement (positional) data, there is a significant amount of error (quadratic) introduced into the tracking. Therefore, reference points need to be set up in the 3D space, through either inside-out or outside-in (base station) tracking.

We hope to achieve a full 6DoF with our headset using either PSMoveServiceEx, or by making use of existing open-source SLAM research projects. Many of these projects were made on Linux, which could possibly complicate things when we get to this point in our project. While ambitious, I hope to have a version in the future that uses a Raspberry Pi 5 (or similar) on board that can handle all of the tracking computation, much like Apple's R1 processor.
