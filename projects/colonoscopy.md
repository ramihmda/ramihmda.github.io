---
title: Colonoscopy Robot Development
permalink: /projects/colonoscopy/
author_profile: true
layout: single
classes: wide
---

## Overview
I worked on the software side of a robotic colonoscopy system designed to reduce operator fatigue during manual endoscope procedures. The robot has four motorized degrees of freedom and is controlled through teleoperation using a game controller.

My contributions included teleoperation control, sensor integration, and the operator interface. I set up Xbox controller input mapping, integrated an NDI Aurora magnetic tracker for tip localization, and streamed video from a miniature camera at the end of the scope.

<figure class="align-center">
  <img src="/assets/images/colon_setup.png"
       alt="Experimental colonoscopy robot setup"
       style="max-width: 800px; display: block; margin: 0 auto;">
  <figcaption style="max-width: 770px; margin: 0 auto;">
  Experimental setup showing the robotic actuation mechanism, a curved test tube with internal markers, the NDI Aurora magnetic tracker, and the end-effector camera used for live visualization.
  </figcaption>
</figure>

## Control & Teleoperation
The control system runs on a Jetson Nano and uses a wired Xbox 360 controller for input. Each joystick axis maps to one of the robot's four degrees of freedom, giving the operator smooth, continuous control during navigation.

Motor commands go through the Dynamixel SDK, with ROS handling communication between control, sensing, and visualization. For remote testing, I set up a Tailscale connection and ran the system over long distance to check for latency issues. Delay was low enough that control still felt responsive.

## Sensing & Localization
I integrated an NDI Aurora magnetic tracker to estimate the position and orientation of the colonoscope tip in real time. The tracking data runs through NDI's API and gets logged alongside video during experiments, which helps with reviewing maneuvers and understanding how the scope moved through the test environment.

## Vision Integration
Video from the end-effector camera streams into the operator interface through ROS so the operator can see what the scope sees during navigation.

A collaborating researcher trained a tumor detection model and connected it to the video pipeline. I set up the camera stream and interface so detection results display alongside the live feed, keeping everything in one view during navigation.
