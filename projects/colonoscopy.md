---
title: Colonoscopy Robot Development
permalink: /projects/colonoscopy/
author_profile: true
layout: single
classes: wide
---

## Overview
This project focused on developing a software-controlled framework for robotic colonoscopy aimed at reducing operator fatigue during manual endoscope procedures. The system enables teleoperated control of a colonoscope through a robotic platform with four motorized degrees of freedom.

My contributions were on the software side, including teleoperation control, sensing integration, and the operator interface. I implemented game-controller-based control, integrated live video from a miniature end-effector camera, added magnetic localization using an NDI Aurora tracker, and supported the integration of a pre-trained vision model to assist with tumor detection during navigation.

<figure class="align-center">
  <img src="/assets/images/colon_setup.png"
       alt="Experimental colonoscopy robot setup"
       style="max-width: 800px; display: block; margin: 0 auto;">
  <figcaption style="max-width: 750px; margin: 0 auto;">
  Experimental colonoscopy robot setup used for system testing. The setup includes a robotic actuation mechanism for the colonoscope, a curved test tube with internal markers, an NDI Aurora magnetic
  tracking system for tip sensing, and an end-effector camera providing live visualization during experiments.
  </figcaption>
</figure>

## Control & Teleoperation
I implemented a 4-DOF control system in Python running on a Jetson Nano, using a wired Xbox 360 controller for teleoperation. Controller inputs were mapped to the robot’s degrees of freedom to provide smooth, continuous control during navigation.

Motor commands were issued through the Dynamixel SDK, with ROS used as middleware to connect control, sensing, and visualization components. For remote testing, the system was connected using Tailscale and validated through long-distance operation with minimal perceived delay.

## Sensing & Localization
I integrated NDI Aurora magnetic tracking using NDI’s API to estimate the position and orientation of the colonoscope tip during navigation. Tracking data was acquired in real time and recorded during maneuvering, then used alongside the live video feed to provide spatial context during experiments.

## Vision Integration
Live video from the end-effector camera was streamed into the operator interface using ROS to support navigation and situational awareness during teleoperation.

A pre-trained vision model developed by a collaborating researcher was connected to the video pipeline, allowing tumor detection results to be displayed alongside the live feed. My contribution focused on integrating the camera stream and visualization so detection outputs could be viewed during navigation without switching interfaces.


