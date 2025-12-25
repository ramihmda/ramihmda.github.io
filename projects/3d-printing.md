---
title: Liquid Metal Sensor Fabrication
permalink: /projects/3d-printing/
author_profile: true
---

## Overview
I converted a Prusa i3 MK3S+ into a liquid metal printing system by removing the stock filament extruder and replacing it with a syringe-based toolhead connected to a Nordson Ultimus V high-precision air dispenser. This allowed controlled extrusion of gallium-based conductive material instead of thermoplastic filament.

System coordination was handled using ROS2, with separate nodes managing the printer, dispenser, camera, and a custom GUI. This structure enabled high-speed G-code streaming while independently controlling extrusion pressure and tuning parameters in real time.

The platform was built for research-scale sensor fabrication, including printing within custom molds and maintaining stable deposition under nonstandard material behavior.

![System architecture overview](../assets/images/system_diagram.png)

---

## Motivation
Liquid metal strain sensors require:
- Precise control of extrusion pressure and motion timing
- Careful thermal management
- Accurate spatial alignment when printing into molds

Commercial 3D printers and slicers are not designed for these constraints. This project addressed those gaps by tightly integrating hardware, firmware, and software into a unified fabrication pipeline.

---

## System Architecture
The system consists of four tightly coupled subsystems:

- **Modified Cartesian 3D printer** (motion + positioning)
- **Precision air dispenser** for liquid metal extrusion
- **Vision system** for monitoring and registration
- **Custom control software** for coordination and parameter tuning

![System architecture overview](../assets/images/3d-printing/system_diagram.png)

---

## Hardware Integration
A desktop Prusa-style printer was mechanically adapted to carry a precision air dispensing toolhead. Custom mounts were designed to ensure stable extrusion while maintaining access to the printer’s full workspace.

![Integrated printer and dispenser](../assets/images/3d-printing/printer_setup.jpg)

The air dispenser allows fine-grained control over extrusion pressure, which is critical for liquid metal deposition consistency.

---

## Software & Control
A **modular ROS2 architecture** was implemented to separate concerns and enable parallel execution:

- **GUI node** – real-time parameter tuning and system control
- **Printer node** – high-speed G-code streaming and motion control
- **Dispenser node** – pressure and timing control
- **Camera node** – visual feedback and monitoring

This separation enabled sustained high-speed operation without dropped commands or timing jitter.

---

## Custom Firmware Modifications
The printer firmware was modified to:
- Integrate external heating elements
- Recalibrate motion and temperature sensors
- Adjust motion logic to account for liquid metal deposition dynamics

These changes significantly improved print repeatability and trace continuity compared to stock firmware behavior.

---

## Mold-Based Fabrication & Registration
To support fabrication inside custom molds, a **G-code transformation module** was implemented. Using user-defined registration points, print paths are transformed from global printer coordinates into mold-aligned coordinates.

This enabled:
- Accurate placement within irregular geometries
- Repeatable mold-based sensor fabrication
- Rapid iteration without redesigning toolpaths

---

## Fabrication Results
The system was used to fabricate spiral-pattern liquid metal strain sensors with consistent trace width and geometry across multiple runs.

![Printed liquid metal strain sensors](../assets/images/3d-printing/sensor_samples.jpg)

---

## My Contributions
- Designed and built an integrated 3D printing + air dispensing platform
- Developed a custom GUI for precise, real-time control of fabrication parameters
- Implemented a modular ROS2 architecture for coordinated multi-device control
- Modified printer firmware for heating integration and liquid metal–specific motion logic
- Developed a G-code transformation pipeline for mold-aligned printing using registration points

---

## Skills & Technologies
- ROS2
- G-code generation and streaming
- Embedded firmware modification
- Precision fluid dispensing
- Mechatronic system integration
- Experimental fabrication workflows
