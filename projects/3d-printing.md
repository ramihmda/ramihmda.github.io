---
title: Liquid Metal Sensor Fabrication
permalink: /projects/3d-printing/
author_profile: true
layout: single
classes: wide
---

## Overview
The platform is built on a modified **Prusa i3 MK3S+**, where the standard filament extruder was removed and replaced with a syringe-based toolhead connected to a **Nordson Ultimus V high-precision air dispenser**. This configuration allows controlled deposition of gallium-based liquid metal for fabricating conductive strain sensors.

<figure class="align-center">
  <img src="/assets/images/system_diagram.png" alt="Physical system overview" style="max-width: 900px; width: 100%;">
</figure>


---

## System Architecture
Printer motion, pneumatic control, and user interaction are handled as separate components using **ROS2**. By decoupling motion execution from extrusion control, the system maintains a high-speed G-code stream while allowing real-time adjustment of extrusion parameters. This architecture keeps deposition stable even when printing inside custom molds or on non-standard substrates.

Each major function operates as a separate ROS2 node:

- **Printer Node:** Serves as the hardware interface for the Prusa MK3S+. It handles G-code streaming, motion execution, and spatial transformations required for printing within custom molds. During path execution, it publishes start/stop triggers to the dispenser topic to synchronize extrusion with physical motion.
- **Dispenser Node:** Translates ROS messages into the serial command protocol used by the Nordson Ultimus V. It runs a dedicated worker loop to ensure pneumatic pulses are issued with minimal jitter.
- **GUI Node:** Provides the operator interface for manual jogging, coordinate registration, and real-time parameter tuning. Extrusion pressure and vacuum settings can be adjusted mid-print without interrupting the printer’s serial buffer.

Inter-node communication is handled through asynchronous ROS2 topics and services. The Printer Node acts as the primary coordinator during path execution, issuing commands to the Dispenser Node via the `/dispenser/cmd` topic. This architecture isolates time-critical deposition control from printer motion, allowing precise liquid metal extrusion while preserving the printer’s standard motion profile.

<figure class="align-center">
  <img src="/assets/images/ros_diagram.png" alt="ROS2 system architecture diagram" style="max-width: 900px; width: 100%;">
</figure>

---

## Software Control & GUI
The software interface was developed using **wxPython** and serves as the central control hub for the ROS2 system. The GUI runs as its own node, which keeps the interface responsive during long print jobs and allows manual interaction without interrupting the printer’s motion buffer.

The interface is organized into four primary control areas:

- **Manual Hardware Interface:** The GUI allows operators to manually jog printer axes, home the system, and send raw G-code commands. A dedicated control is provided for the Nordson dispenser, allowing the liquid metal to be primed and the syringe tip cleared before starting a print.
- **Coordinate Registration:** To support printing inside custom molds, the GUI includes a registration tool. The operator records three reference corners on the substrate, and the system computes a transformation matrix that aligns the digital toolpath with the physical mold.
- **Real-Time Parameter Tuning:** Extrusion pressure and vacuum retract settings can be adjusted live during a print. This makes it possible to compensate for changes in material viscosity or surface tension without re-slicing the original G-code.
- **Path Visualization:** A built-in matplotlib visualization provides a 3D preview of the loaded G-code, allowing the operator to verify the print path and coordinate offsets before committing to a deposition run.

### Data Flow & Monitoring
The GUI continuously monitors system state through a serial response terminal that displays real-time feedback from the Prusa firmware. Synchronous ROS2 services are used to poll the current nozzle position (`M114`), ensuring the graphical interface remains aligned with the printer’s physical state during manual setup and calibration.

<figure class="align-center">
  <img src="/assets/images/gui.png" alt="User Interface Preview" style="max-width: 900px; width: 100%;">
</figure>


---

## Custom Firmware Modifications
The stock Prusa firmware was modified to support the non-standard hardware and thermal behavior required for liquid metal printing. Gallium solidifies near room temperature but becomes overly fluid if overheated, so maintaining a narrow temperature range is essential for stable extrusion and consistent trace formation.

### Thermal Handling
The default firmware temperature limits and safety checks were adjusted to allow the use of an external heating element on the syringe-based toolhead. These changes prevented the printer from faulting due to assumptions made for thermoplastic extrusion, while still allowing the liquid metal to remain molten throughout a print.

Careful temperature control was necessary to balance material flow and trace definition, since small temperature changes significantly affect gallium viscosity.

### Kinematic Tuning
Motion parameters were recalibrated to account for the added mass and inertia of the syringe-based toolhead. Acceleration and jerk settings were reduced to limit mechanical vibrations that would otherwise cause breaks or thinning in conductive traces.

### Sensor Calibration
Thermistor tables and motion limits were updated to reflect the custom heating hardware and modified toolhead geometry. This ensured temperature readings and motion bounds remained consistent with the physical setup.

These firmware changes provide the mechanical and thermal stability needed for the ROS2 control system to execute precise motion and extrusion commands.

---

## Calibration and Path Optimization
Before fabricating functional sensors, a series of calibration routines were used to synchronize the pneumatic dispenser with the printer’s motion system. Gallium-based alloys have high surface tension and low viscosity, which makes them prone to beading or trace breakage when pressure and print speed are not properly matched.

### Pressure–Speed Synchronization
Spiral test patterns were used to evaluate trace continuity under changing motion conditions. Spirals are well suited for this purpose because they involve continuous changes in direction and velocity. By running these patterns at different feed rates and pressures, the optimal pressure settings in the Nordson node were identified to match the Prusa node’s motion profile. This prevented material pooling at corners and discontinuities along straight segments.

### Nozzle Height (Z Calibration)
Liquid metal deposition is highly sensitive to the gap between the syringe tip and the substrate. If the nozzle is too high, surface tension causes the material to bead into droplets; if it is too low, the tip drags through the deposited trace. A stepped calibration block was used to determine the Z-offset that maintains a continuous bridge of material between the nozzle and the surface.

### Start/Stop Latency Compensation
Unlike thermoplastic extrusion, liquid metal exhibits inertia and a short delay between pneumatic actuation and material flow. To compensate for this, G-code trigger timing and vacuum retract parameters in the Nordson node were tuned to eliminate tails and blobs at the ends of conductive paths. This ensured clean starts and stops for individual traces.

Together, these calibration steps enabled the transition from exploratory material testing to the repeatable, high-precision deposition required for research-scale sensor fabrication.

<figure class="align-center">
  <img src="/assets/images/sensor_samples.png" alt="Printed liquid metal strain sensors" style="max-width: 900px; width: 100%;">
</figure>
