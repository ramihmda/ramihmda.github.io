---
title: Liquid Metal 3D Printer
permalink: /projects/3d-printing/
author_profile: true
layout: single
classes: wide
---

## Overview
This project involved building the software and control infrastructure needed to fabricate stretchable, liquid metal strain sensors using a modified 3D printing platform.

While the mechanical modifications to the Prusa i3 MK3S+ were handled separately, I was responsible for the entire software stack. This included the ROS2 control architecture, printer–dispenser coordination, GUI development, firmware modifications, and calibration tools that enable repeatable liquid metal deposition using a Nordson Ultimus V air dispenser.

<figure class="align-center">
  <img src="/assets/images/system_diagram.png" alt="Physical system overview" style="max-width: 900px; width: 100%;">
</figure>

## System Architecture
I designed the control system around ROS2 to separate printer motion, pneumatic control, and user interaction into independent nodes. This decoupling keeps the printer’s G-code stream fast and stable while allowing real-time adjustment of extrusion parameters.

- **Printer Node:** Streams G-code to the Prusa and handles motion execution and mold-aligned coordinate transforms. It also issues start/stop triggers to synchronize extrusion with movement.
- **Dispenser Node:** Converts ROS commands into serial messages for the Nordson Ultimus V, using a dedicated worker loop to minimize pressure timing jitter.
- **GUI Node:** Provides real-time control for jogging, registration, and parameter tuning without interrupting the printer’s serial buffer.

<figure class="align-center">
  <img src="/assets/images/ros_diagram.png"
       alt="ROS2 control architecture for liquid metal printing"
       style="max-width: 800px; display: block; margin: 0 auto;">
  <figcaption style="max-width: 800px; margin: 0 auto;">
  ROS2 node architecture showing how motion execution, pneumatic control, and the GUI are decoupled and synchronized during printing.
  </figcaption>
</figure>

## Software Control & GUI
I developed a wxPython-based GUI that serves as the control layer for the ROS2 system. Running the GUI as its own node keeps the interface responsive during long print jobs and allows manual interaction without interrupting the printer’s motion buffer.

The interface provides four core functions:
- **Manual Control:** Jogging axes, homing, and sending raw G-code, with dedicated controls to prime and clear the Nordson dispenser.
- **Coordinate Registration:** Recording reference points on a substrate and computing a transform to align print paths with custom molds.
- **Live Parameter Tuning:** Adjusting extrusion pressure, vacuum retract, and temperature settings mid-print to compensate for material behavior.
- **Path Visualization:** A 3D preview of the loaded G-code to verify print paths and coordinate offsets before execution.

<figure class="align-center">
  <img src="/assets/images/gui.png" alt="User Interface Preview" style="max-width: 900px; width: 100%;">
</figure>

## Custom Firmware Modifications
I modified the stock Prusa firmware to support the non-standard hardware and thermal behavior required for liquid metal printing. Because gallium solidifies near room temperature and becomes unstable if overheated, the printer’s default assumptions for thermoplastic extrusion had to be adjusted.

- **Thermal Handling:** Firmware temperature limits and safety checks were modified to support an external heating element on the syringe-based toolhead without triggering fault conditions.
- **Kinematic Tuning:** Acceleration and jerk parameters were reduced to account for the added mass of the syringe toolhead, minimizing vibrations that caused trace breakage.
- **Sensor Calibration:** Thermistor tables and motion limits were updated to reflect the custom heating hardware and modified toolhead geometry.

These changes provided the mechanical and thermal stability required for reliable software-controlled deposition.

## Calibration and Path Optimization
To achieve repeatable deposition, I calibrated the interaction between printer motion and pneumatic extrusion. Gallium’s high surface tension and low viscosity make it sensitive to mismatches between speed, pressure, and timing.

- **Pressure–Speed Matching:** Spiral test patterns were used to tune extrusion pressure against feed rate, preventing pooling at corners and discontinuities along straight paths.
- **Z-Height Calibration:** A stepped calibration block was used to determine the nozzle offset that maintains a continuous material bridge without dragging or beading.
- **Start/Stop Timing:** G-code trigger timing and vacuum retract settings were tuned to compensate for pneumatic latency, eliminating tails and blobs at trace endpoints.

These calibrations enabled consistent, high-precision deposition suitable for research-scale sensor fabrication.

<figure class="align-center">
  <img src="/assets/images/sensor_samples.png"
       alt="Liquid metal pressure sweep test"
       style="max-width: 600px; display: block; margin: 0 auto;">
  <figcaption style="max-width: 600px; margin: 0 auto;">
    Pressure sweep showing six gallium prints deposited at a fixed feed rate (12000) and Z height (15 mm).
    Pressure was increased from 1.2 bar to 1.7 bar in 0.1 bar increments to evaluate trace continuity and pooling behavior.
  </figcaption>
</figure>


## Sensor Fabrication Workflow
With repeatable liquid metal deposition established, the system enabled a layered fabrication process for stretchable strain sensors without requiring a dedicated mold for each geometry.

Liquid silicone was poured into a base mold and compressed with a flat lid press to produce a level surface. After curing, gallium was printed directly onto the silicone in the desired sensor geometry.

The mold was then placed in a freezer to solidify the gallium before a second layer of silicone was poured on top to encapsulate the conductive traces and complete the sensor.

This approach replaces an earlier mold-based injection method that was slow and difficult to scale to complex designs. Direct printing enables faster iteration, lower cost, and fabrication of complex sensor geometries without custom tooling.

<figure class="align-center">
  <img src="/assets/images/completed_sensor.png" alt="Completed liquid metal sensor" style="max-width: 900px; width: 100%;">
</figure>

**Impact:** Enabled rapid, low-cost fabrication of complex liquid metal strain sensors using a software-driven printing workflow.
