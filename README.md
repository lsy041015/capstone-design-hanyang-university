# Capstone Design (Hanyang University)

## Automated Unmanned Laundry Pickup System

This capstone project presents a detailed Blender concept for a small unmanned laundry store in Korea. The store separates the washing and drying area from the finished-goods storage and pickup area. A protected automation cell stores completed garments and shoes, identifies each order, and delivers the correct item after customer authentication at a kiosk.

![Overall process cell](images/01_overall_process_cell.png)

## Project Scope

The design focuses on the handling of finished laundry after staff inspection and packaging. Staff members inspect, package, and register each item. The automated system then stores it, retrieves it for the customer, and returns the empty carrier or tray.

The current demonstration capacity is:

- 8 garment carrier positions
- 4 shoe tray positions
- 1 shared staff loading station
- 1 robot cell using an official UR5 CB-series STL as a visual substitute for an intended FR5e application study
- 31 discrete inspection poses covering storage, retrieval, customer pickup, empty-carrier return, and replenishment

The Blender frame numbers represent inspection poses, not seconds or a validated cycle time.

## Operating Flow

1. Staff inspect and package the completed garments or shoes.
2. The order ID and item ID are registered at the staff terminal.
3. An empty garment adapter or shoe tray is supplied to the staff loading station.
4. The robot remains at its home position while the guarded loading door is open.
5. Staff load the garment adapter or shoe tray and close the guarded door.
6. The robot transfers the item to the garment conveyor or shoe infeed.
7. The storage location is recorded after the presence and ID checks succeed.
8. The customer authenticates using a pickup number or QR code.
9. The reserved item is retrieved and transferred to the correct customer compartment.
10. The customer removes the item.
11. The empty garment adapter or shoe tray is returned for the next order.

## System Architecture

### C01 — Garment Storage Conveyor

The garment system uses a continuous guided carrier loop with 25.4 mm design pitch, 212 links, and two 54-tooth sprockets. Captive trolley wheels, a chain connection, a drive unit, a tensioner, an encoder, and an index locking pin are represented. Garments are aligned tangent to the rail to reduce sleeve overlap.

![Garment drive and carrier](images/02_garment_drive_and_carrier.png)

### C02 — Garment Transfer Axis

A Y-axis carriage moves the complete Z-axis assembly rather than leaving the vertical guide fixed. The nominal travel is 730 mm in Y and 300 mm in Z. The receiving saddle includes a retention latch so the robot can release the garment adapter before customer delivery.

![Garment YZ transfer](images/03_garment_yz_transfer.png)

### C03 — Shoe Lift, Fork, and Roller Transfer

Four shoe storage levels are served by a guided lift and a three-stage telescopic fork. The fork has 600 mm horizontal travel, at least 130 mm overlap between nested stages, and a separate 20 mm vertical transfer motion. The infeed and output conveyors use 40 mm rollers at 80 mm pitch, with linked drive elements and side guides.

![Shoe lift and telescopic fork](images/04_shoe_lift_and_fork.png)

### C04 — Electrical and Control Cabinet

The cabinet layout distinguishes the main disconnect, circuit protection, contactor, 24 V power supply, safety relay, PLC, digital I/O, motor drives, terminals, protective earth, cable ducts, and cable glands. It is a spatial maintenance concept rather than a certified electrical schematic.

![Electrical cabinet](images/07_control_cabinet.png)

### C05 — Robot End Effector

The end effector uses replaceable jaws and two contact profiles. A flat underside grip handles the garment adapter stem, while V-shaped pads locate the shoe tray handle. The robot handles rigid adapters and trays instead of directly gripping fabric or shoes.

![Garment adapter gripping detail](images/05_garment_adapter_grip.png)

### C06 — Interlocked Customer Pickup Compartments

The garment compartment uses framed hinged doors. The shoe compartment uses rolling slat shutters with guides, a tubular motor, monitored locks, hand detection, and a pressure-sensitive lower edge. The internal and customer doors are intended to remain mutually exclusive.

![Customer pickup area](images/08_customer_pickup.png)

### C07 — Staff Replenishment Station

The staff-side loading station uses a guarded sliding door, an emergency stop, a reset control, an identification reader, a garment docking pin, and a folding shoe-tray shelf. The shelf folds down for tray loading and folds away to prevent interference with hanging garments.

![Staff garment replenishment](images/10_staff_garment_replenishment.png)

![Staff shoe replenishment](images/11_staff_shoe_replenishment.png)

## Transfer Details

The robot transfers the shoe tray by its rigid handle and places it onto the roller conveyor. During review, the central tray transfer pose was raised to avoid the robot pedestal. The nominal robot TCP for this pose is 1,700 mm above the model floor, and the tray origin is 1,545 mm.

![Robot shoe tray transfer](images/12_robot_tray_transfer.png)

The roller and belt transfer surfaces are aligned at 1,080 mm. Across 201 sampled positions, a 320 mm tray base remained supported by at least four rollers on both conveyor lanes.

![Roller and belt transfer](images/06_roller_and_belt_transfer.png)

## Customer Experience

The customer interface follows a short two-step structure: **1. Authenticate → 2. Pick up**. Garments and shoes use clearly separated compartments marked A and B. The kiosk supports pickup-number entry and a QR reader, while a visible assistance control provides a recovery path when authentication or retrieval fails.

## Design Review and Verification

The revised design received an internal concept-review score of **74/100**, improved from 32/100 for the previous display-oriented layout. This score is an author assessment, not a certification or safety approval.

The final Blender model passed 177 automated checks, including:

- 31 native inspection poses
- maximum nominal TCP position error of approximately 0.0098 mm at the saved poses
- zero overlap among 560 × 140 mm garment envelopes across 201 indexed positions
- minimum garment separating-axis gap of approximately 113.10 mm
- at least four supporting rollers under a 320 mm tray throughout both conveyor paths
- fork-to-tray transfer contact within 0.1 mm at the checked poses
- fail-closed logical checks for door state, emergency stop, reset, obstruction, timeout, ID, occupancy, and grip confirmation
- no detected triangle intersections between the selected robot/tool meshes and 1,685 selected surrounding rigid parts at the 31 saved poses
- no detected intersections between the moving garment or tray and the selected pedestal, frame, guard, and robot meshes at the saved poses

![Completed return state](images/09_completed_return_state.png)

## Design Improvements Made During Review

- Reduced the garment demonstration capacity from 10 to 8 positions to eliminate clothing-envelope overlap.
- Rotated garments to follow the conveyor tangent instead of placing adjacent garments in the same global orientation.
- Connected the Z-axis assembly to the moving Y carriage.
- Moved the garment transfer beam after a robot-link interference was identified.
- Replaced sparse output rollers with an 80 mm pitch arrangement.
- Added a guided lift, nested fork stages, a tip-lift transfer, brakes, limits, bumpers, and a constant-length energy chain.
- Replaced overlapping garment pins with an open-bottom lift-off saddle.
- Added a guarded staff replenishment workflow and automatic return of empty transport carriers.
- Added fixed upper and lower infill panels around the closed shoe shutter.
- Raised the central shoe-tray transfer pose after a tray-to-pedestal collision was found.

## Important Limitations

This repository documents a visual and geometric capstone concept. It does not demonstrate a production-ready machine. The following work remains necessary before fabrication or commercial deployment:

- Replace the substitute UR5 geometry and nominal kinematics with the exact FR5e CAD, calibration, payload, tool mass, and center of gravity.
- Validate continuous robot trajectories, approach and departure paths, full self-collision, and garment motion.
- Calculate motor torque, brake capacity, fork deflection, frame stiffness, vibration, tolerances, grip force, and rated payload limits.
- Select real purchased components and produce manufacturing drawings, wiring diagrams, and a thermal/electromagnetic-compatibility review.
- Implement and validate the actual PLC, robot controller, independent safety circuit, stop distance, restart procedure, and fault recovery.
- Conduct physical usability testing for staff reach, loading time, cleaning access, customer accessibility, and maintenance tasks.
- Validate the payment, QR, order, inventory, and notification services.

The demonstration slot count must not be interpreted as the capacity or profitability of a commercial store.

## Gallery

![Detailed process gallery](images/process_gallery.jpg)

## Model and Attribution Notes

The intended application robot is the FR5e. The visualization uses the official UR5 CB-series mesh as a substitute for layout and nominal kinematic review. The UR5 specification values and geometry must not be presented as FR5e performance data.

Reference material:

- [Universal Robots UR5 technical specification](https://www.universal-robots.com/media/1828033/ur5_tech_spec_web_en.pdf)
- [Universal Robots ROS 2 Description repository](https://github.com/UniversalRobots/Universal_Robots_ROS2_Description)
- [Metalprogetti automated dry-cleaning systems](https://www.metalprogetti.it/en/products/automated-dry-cleaning/)

## Repository Contents

This public repository contains project images and this English project description only. Blender files, automation scripts, verification scripts, and other source files are intentionally excluded.

---

Hanyang University Capstone Design · Automated Unmanned Laundry Pickup System
