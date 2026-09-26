# Capstone Design (Hanyang University)

## Automated Unmanned Laundry Pickup System

This capstone project proposes an unmanned pickup service for completed laundry in a small store in Korea. Staff inspect, package, and register the finished items; a protected automation cell then stores them and retrieves the correct garment or pair of shoes after customer authentication. The project concept separates staff preparation, automated storage, and customer pickup while keeping people outside the moving machinery.

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

## Process Demonstration Video (v4)

[Watch or download the 60-second process video](videos/process_v4_60s_1080p30.mp4).

This earlier v4 animation shows 86 detailed process steps over 60 seconds at 1920 × 1080 and 30 fps. It uses a UR5 substitute model to examine a possible FR5e application. The v7 design described below adds revised mechanisms and 31 static inspection poses; this v4 video does not depict the final v7 geometry or slot count. The animation is a visual demonstration, not a physical performance or safety validation.

## Operating Flow

The sequence below describes the proposed service operation. The model's 31 numbered frames are **separate inspection poses**, not elapsed time or proof of a continuous cycle. Frame 1 shows inventory waiting for pickup. Frames 23–31 show replenishment after the illustrated pickup, although staff loading occurs before a newly completed item can be stored and later collected.

### 1. Staff inspection and item registration

Staff first check the completed garment or shoes, package them, and link an item ID to the customer's order ID at a staff terminal. The proposed workflow rejects a duplicate ID, an unsuitable item or package, or an order with no available storage position before loading begins. Inspection, packaging, and registration remain staff tasks; the project does not claim to remove wet laundry from a washer or hang clothing automatically.

### 2. Protected loading and storage — C07, C05, C01, C03

An empty garment adapter or shoe tray is brought to the shared staff loading station, C07. Customer retrieval is paused, the robot is at home, and the guarded sliding door can open only in the service state. Staff fold the tray shelf away when hanging a garment on its dedicated adapter; for shoes they unfold the shelf and place the packaged pair in a tray. After the door is closed and locked, an operator reset is required before automatic motion can resume.

The robot grips the **rigid adapter tab or tray handle**, rather than fabric or shoes. For garments, it transfers the loaded adapter to an empty carrier on the C01 loop. For shoes, it puts the tray on the C03 infeed, where the rollers, belt, lift, and telescopic fork place it on one of four levels. The proposed inventory record changes from provisional to occupied only after the destination, item ID, and presence checks agree. The saved replenishment poses are frames 23–31; they illustrate one garment and one shoe-tray refill, not a measured loading time.

### 3. Customer authentication and reservation

At the kiosk the customer enters a pickup number or presents a QR code. The intended control flow checks the order, pickup permission, and item location, then reserves the item so the same stock cannot be retrieved by two requests. The A garment and B shoe compartments remain closed while the mechanism prepares the item. The kiosk screens and logical conditions are represented in the concept; a live payment, QR, order, or inventory server has not been implemented.

### 4. Garment selection and robot handoff — C01, C05, frames 2–6

The C01 carrier loop indexes the requested garment to the robot pickup station and locks at the target position. After checking the carrier and item ID, the robot closes its garment jaws on the adapter's rigid tab, lifts the adapter off its open-bottom saddle, and carries it to the C02 receiving saddle. A retention latch holds the adapter before the gripper releases it. The illustrated poses show indexing, locking, gripping, lifting, and placement; they do not establish a collision-free continuous robot trajectory between poses.

### 5. Garment delivery and adapter recovery — C02, C06, frames 7–10

With the robot clear and the customer door locked, the internal door opens. The complete C02 Z module travels with the Y carriage for the nominal **730 mm Y movement**, then lowers the garment by **300 mm** to the customer pickup position. The internal door must close and lock before the outer garment door can unlock. The customer removes the hanger and garment, while the transport adapter stays available for return inside the system. Removal and door closure are required before the order is marked collected and the empty adapter is recovered. If either door or the item check fails, the design keeps the next movement blocked.

### 6. Shoe retrieval and transfer — C03, C05, frames 11–18

The C03 lift stops at the shelf assigned to the shoe order. Its three-stage fork reaches **600 mm** into the shelf, raises the tray **20 mm** for clearance, retracts, and moves to the belt transfer height. The fork then lowers the tray onto the belt. Linked rollers move it to an infeed stop where its presence and ID are checked before robot pickup. The robot grips the tray handle, passes it through the central cell, and sets it on the output rollers. The belt and roller tops share a nominal **1,080 mm** transfer height; these are model design dimensions, not tested handling performance.

### 7. Shoe pickup and empty-tray return — C06, C03, frames 19–22

The tray reaches the B shoe compartment while its customer shutter remains locked. After the robot returns home, the internal shutter can open to admit the tray; it then closes and locks before the outer shutter opens. The customer takes the shoes and packaging but leaves the reusable tray. Only after removal and outer-shutter closure does the empty tray move back through the internal route. The robot, infeed, belt, lift, and fork return it to an available storage position, and the inventory record is cleared after the return checks succeed.

### 8. Fault gates and reset

The intended sequence checks service-door closure, emergency-stop state, operator reset, robot home, door interlocks, obstruction, timeout, item ID, occupancy, and grip confirmation before permitting the relevant motion. A failed check holds the item or transport carrier in a defined position and calls for operator review instead of silently completing the order. These are **design requirements and tested logical conditions in the model**, not a working PLC, independent safety circuit, or validated recovery procedure.

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
