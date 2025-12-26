---
title: Colonoscopy Robot Development
permalink: /projects/colonoscopy/
author_profile: true
layout: single
classes: wide
---

## Overview
This project focused on developing the software and control systems for a teleoperated colonoscopy robot designed for precise navigation and real-time sensing. The goal was to enable responsive human-in-the-loop control while integrating multiple sensing modalities into a single operator interface.

I was responsible for the control software, sensing integration, and teleoperation framework. This included implementing a multi-degree-of-freedom control system, integrating magnetic tracking and vision-based sensing, and building a real-time GUI for navigation and feedback.

<figure class="align-center">
  <img src="/assets/images/colonoscopy_system.png" alt="Colonoscopy robot system overview"
       style="max-width: 900px; display: block; margin: 0 auto;">
</figure>

## Control System Architecture
I implemented a **4-DOF teleoperation control system** running on an NVIDIA Jetson Nano. The control stack maps operator inputs to robot motion while maintaining responsive, stable behavior suitable for real-time navigation.

An Xbox controller was used as the primary input device. Custom input mappings were designed to provide intuitive control over articulation and insertion, allowing the operator to smoothly command multiple degrees of freedom without mode switching.

- **Embedded Controller:** Jetson Nano running the control stack and device interfaces  
- **Input Mapping:** Low-latency Xbox controller input translated into continuous robot motion commands  
- **Actuation Control:** Real-time motor command generation for multi-axis control  

<figure class="align-center">
  <img src="/assets/images/control_architecture.png" alt="Control system architecture"
       style="max-width: 800px; display: block; margin: 0 auto;">
  <figcaption style="max-width: 800px; margin: 0 auto;">
    Embedded control architecture showing operator input mapped to real-time robot actuation.
  </figcaption>
</figure>

## Teleoperation & Remote Connectivity
To support remote operation, I implemented a **secure, low-latency teleoperation framework** using Tailscale. This allowed the robot to be safely controlled across networks without exposing open ports or requiring complex firewall configuration.

The system was tested under global network conditions to validate responsiveness and stability. Control latency remained low enough to support real-time teleoperation, enabling the operator to interact with the robot as if it were locally connected.

- Encrypted peer-to-peer communication  
- Stable control under wide-area network conditions  
- No reliance on public IP exposure  

## Sensing & Localization
The robot integrates **NDI Aurora magnetic tracking** to provide real-time localization of the end-effector within the body. I handled the full software integration of the tracking system, including data acquisition, coordinate handling, and visualization.

Tracking data was streamed into the control interface and used to provide continuous 3D position feedback to the operator. This enabled spatial awareness during navigation, even when visual cues were limited.

- Real-time magnetic tracking acquisition  
- 3D position estimation and visualization  
- Synchronization with control and video streams  

<figure class="align-center">
  <img src="/assets/images/magnetic_tracking.png" alt="Magnetic tracking visualization"
       style="max-width: 800px; display: block; margin: 0 auto;">
</figure>

## Vision Integration & Tumor Detection
In addition to localization, I integrated a trained vision model into the navigation interface for **real-time tumor detection**. Live video from the onboard camera was processed and displayed alongside tracking and control information.

Detection outputs were overlaid directly on the video stream, allowing the operator to identify regions of interest without switching interfaces. This integration demonstrates how perception models can be embedded directly into interactive robotic systems.

- Live video streaming within the GUI  
- Real-time inference using a trained detection model  
- Visual overlays integrated into the operator interface  

<figure class="align-center">
  <img src="/assets/images/vision_gui.png" alt="Navigation GUI with tumor detection overlay"
       style="max-width: 900px; display: block; margin: 0 auto;">
</figure>

## Operator Interface
I developed a unified GUI that combines:
- Live camera feed  
- Magnetic tracking visualization  
- Robot state and control feedback  

The interface was designed to minimize operator cognitive load by presenting all relevant information in a single view. This allowed seamless switching between navigation, sensing, and decision-making during teleoperation.

## Impact
This project demonstrates an end-to-end teleoperated robotic system combining real-time control, secure remote operation, and multi-modal sensing. The resulting platform enables responsive navigation, spatial awareness, and vision-assisted decision-making in a medical robotics context.
