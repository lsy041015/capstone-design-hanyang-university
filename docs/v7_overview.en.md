# Automated Unmanned Laundry Pickup System — v7 release (archived)

> Archived README of the v7 release (before V2.0). For the current version see [README](../README.en.md).

A Hanyang University Capstone Design project.

> **A small-store concept: staff load inspected, packaged laundry; an automated cell stores it; customers authenticate at a kiosk and collect their own garment or shoes.**
> Shown through renders of a Blender model and a 2-minute process film. The robot only grips rigid adapters and trays, never fabric or shoes,
> and staff and customers stay outside the moving machinery.

> **Status: a visual and geometric design concept, not a built or certified machine.** The original inspection model passed 177 automated checks
> at 31 static poses, and the internal concept-review score rose from 32/100 to 74/100. The checks are geometric and scripted on the model; they do
> not prove continuous collision freedom or safety. The robot geometry is a UR5 CB-series stand-in for the target FR5e. Read [Limitations](#limitations) first.

## Why

Letting customers collect finished laundry without a clerk handing it over means solving three things at once:

- **The right item for the right person.** Only the authenticated customer's garment or shoes may come out, and two requests must never claim the same stock.
- **People stay outside the machine.** Neither staff loading items nor customers collecting them should be able to reach moving mechanisms.
- **Two very different items.** Hanging garments and shoes carried in trays need separate storage paths inside one small cell.

The concept assumes a small store in Korea and splits the work three ways: staff inspect, package, register and load; the automation cell stores,
retrieves, delivers and returns empty carriers; the customer only authenticates and picks up. The repository contains no market study or cost analysis.

## What it does

Demonstration capacity of the model (not the capacity or profitability of a commercial store):

- 8 garment carrier positions
- 4 shoe tray positions
- 1 shared staff loading station
- 1 robot cell using an official UR5 CB-series STL as a visual substitute for an intended FR5e application study
- 31 discrete inspection poses covering storage, retrieval, customer pickup, empty-carrier return and replenishment

| Feature | What it does |
|---|---|
| **Staff loading** | Staff put inspected, packaged and registered items into the C07 station. Automatic motion resumes only after the door is closed and locked and an operator resets. |
| **Storage** | Garments ride the C01 carrier loop; shoe trays go onto four shelf levels via the C03 lift and three-stage fork. |
| **Authentication** | A pickup number or QR code at the kiosk checks the order, pickup permission and item location, then reserves the item. The screens are mock-ups. |
| **Garment delivery** | The C05 robot grips the adapter tab and hands it to the C02 receiver, which travels 730 mm in Y and 300 mm in Z to the A garment compartment. |
| **Shoe delivery** | The fork extracts the tray onto the belt and rollers; the robot grips its handle and passes it via the output rollers into the B shoe compartment. |
| **Carrier return** | Once the customer has taken the item, the empty adapter or tray goes back to storage automatically. |
| **Control gates** | Door closed and locked, e-stop, reset, robot home, position and ID, occupancy and grip confirmation must all pass before the next motion; a failure holds the cell for operator review. |
| **Deliverables** | 12 renders plus a contact sheet, the 2-minute v7 film with two subtitle files, the earlier v4 video, and a JSON video-verification record. |

## How it works

Doors separate the places people can reach (C07 staff station, C06 customer compartments) from the interior where only machinery moves. Inside, the
C05 robot moves adapters and trays, and every transition has to pass door, position, ID and grip checks.

```text
 1. Staff loading  (C07 door closed + locked + operator reset before any automatic motion)
    C07 ─▶ C05 ─┬─▶ C01   garment: adapter onto an empty carrier (8 slots)
                └─▶ C03   shoes: tray via infeed → lift → fork (4 levels)

 2. Authentication (kiosk)
    pickup number or QR → check order and item location → reserve stock

 3. Garment delivery
    C01 index → C05 grips adapter tab → C02 receiver (Y 730 mm, Z 300 mm)
    → C06 A garment door (opens only after the inner door is closed and locked)

 4. Shoe delivery
    C03 fork extract + lift → infeed rollers → C05 central tray transfer → output rollers
    → C06 B shoe compartment (inner shutter opens only while the outer shutter is locked)

 5. Empty-carrier return (after item removal + customer door closed and locked)
    empty adapter: C02 receiver returns → C05 → C01 carrier
    empty tray:    internal rollers → C05 → C03 infeed, belt, lift, fork → free storage slot

 C04 cabinet: door locks, e-stop, reset, robot home, position, ID, occupancy and grip
 checks gate every transition; any failure holds the cell stopped for operator review.
```

The repository holds three different models and films:

- **v7 inspection model**: 31 static inspection poses, the subject of the 177 automated checks.
- **v7 teaser film**: a separate file built on the same v7 geometry; 21 shots, 3,600 animated frames, 120 s. Its frame numbers differ from the pose numbers.
- **v4 video**: 86 detailed steps in 60 s, using the previous geometry and slot count.

Neither the original pose numbers nor the film's two-minute runtime represents measured machine cycle time.

## Process teaser (v7)

The current design explained in 21 shots and two minutes.

[![v7 process teaser poster — click to open the video file](../images/v7_teaser_poster.jpg)](../videos/process_v7_teaser_120s_1080p30.mp4)

**[Watch the v7 process teaser](../videos/process_v7_teaser_120s_1080p30.mp4)** · 2:00 · 1920×1080 · 30 fps · Korean narration and graphics

[Chapter guide (Korean)](v7_process_teaser.md) · [English subtitles](../videos/process_v7_en.srt) · [한국어 자막](../videos/process_v7_ko.srt)

- **Playback**: GitHub does not play MP4 files stored in a repository inside the README, so the poster links to the video file. Clicking it opens
  the file page, where you can download it and play it locally.
- **Format**: 120.0 s, 3,600 frames, H.264 video, AAC 48 kHz stereo Korean audio, 25.7 MB.
- **Narration**: the Windows Microsoft Heami Desktop Korean text-to-speech voice. There is no background music.
- **Subtitles**: the MP4 has no embedded subtitle stream. Korean dialogue is in `videos/process_v7_ko.srt` and English subtitles in
  `videos/process_v7_en.srt`; load the one you need as an external track in a compatible player. The on-screen graphics are already in Korean,
  so the subtitles were left out of the MP4 on purpose to keep English from being overlaid on the Korean screens automatically.

**What the film shows.** Staff replenishment → authentication → garment delivery → shoe delivery → empty-carrier return → control conditions.
Close views show the rigid garment adapter, the coupled Y–Z receiver, the telescopic shoe fork, the tray handoff and the interlocked pickup
compartments. Korean graphics explain each action and the checks required before the next step.

**What to keep in mind.**

- It is a model-based process explanation. Editorial cuts summarize travel such as between the staff loading station and storage, and the motions
  are compressed for presentation. The 120 s cannot be used as machine cycle time.
- Staff loading and customer removal are illustrated without animated people: only the items move. This does not mean the cell hangs clothes or
  packages shoes automatically.
- Staff handle inspection, packaging, item registration, loading the station, and fault checks and resets; customers authenticate and take their
  garment or shoes. The cell is designed to store, hand off, move items to the pickup position and return empty carriers through adapters and trays.
- The QR interface, sensors and safety conditions are concept representations. Door locks, ID, item presence and grip confirmation are permit
  conditions that real sensor inputs would have to implement; the door animations alone do not verify physical safety.

### Chapters

Times are playback positions, not machine durations. C01–C07 match [System architecture](#system-architecture). The English cue text for each beat
is in `videos/process_v7_en.srt`.

| Time | Step | Motion and checks |
|---|---|---|
| 00:00–00:05 | Introduction | Introduces the whole cell (storage, authentication, delivery, carrier return) and the demo capacity. |
| 00:05–00:12 | C07 staff garment loading | Staff hang a registered garment on the adapter at the shared port. The robot stays stopped while the door is open; door closed and locked plus an operator reset are required before C05 grips the adapter. |
| 00:12–00:16 | C01 garment putaway | The adapter is set on an empty carrier support and released. By design, seating, item ID and slot position are confirmed before the inventory record is committed. Travel from the port to storage is summarized by a cut. |
| 00:16–00:23 | C07 staff shoe loading | A standard tray on the folding shelf is loaded with shoes. After the operator closes the door and resets, the robot grips the tray handle and lifts it. |
| 00:23–00:28 | C03 shoe storage | The lift's three-stage fork pushes the tray into position S02, sets it down and withdraws. The port and infeed segment is summarized by a cut. |
| 00:28–00:33 | Customer authentication and reservation | A pickup number or QR code confirms the order and item location, and that stock is reserved. The screen is a mock-up, not connected to an authentication server. |
| 00:33–00:38 | C01 garment indexing | The requested garment's carrier moves to the robot pickup position. Position locking and an item-ID recheck precede the next motion. |
| 00:38–00:43 | C05 garment grip | The robot grips the rigid adapter tab, not the fabric, and after grip confirmation lifts it above the support pin. |
| 00:43–00:48 | C02 receiver handoff | The robot sets the adapter on the receiving saddle. The saddle takes the load and the retention latch engages before the gripper opens. |
| 00:48–00:54 | C02 Y–Z travel | The receiver and Z module travel 730 mm in Y together, then lower 300 mm in Z. The inner door must be closed and locked before the customer door may open. |
| 00:54–00:59 | C06 garment pickup | The customer takes the hanger and garment; the transport adapter stays on the receiver. Recovery starts only after item removal and customer-door closure are confirmed. |
| 00:59–01:05 | Empty adapter recovery | After the customer door locks, the receiver returns. The robot lifts the empty adapter and puts it back on its original carrier, H05. |
| 01:05–01:12 | C03 shoe extraction and lift | The fork extends 600 mm under the tray, raises it 20 mm and retracts; the lift moves to the belt transfer height and sets the tray down. |
| 01:12–01:17 | C03 infeed | Linked rollers carry the tray to the pickup stop. Position and ID are checked before the robot grips the handle. |
| 01:17–01:23 | C05 central tray transfer | The tray is raised clear of the pedestal and carried through the center, set on the output rollers and released; the robot retreats. |
| 01:23–01:28 | C06 tray into the shoe compartment | The inner shutter opens while the outer shutter is locked. Once the tray is in, the inner shutter closes and locks. |
| 01:28–01:33 | C06 shoe pickup | The customer takes only the shoes and leaves the reusable tray. Item removal, cleared hand detection and outer-shutter closure gate the next motion. |
| 01:33–01:41 | Empty tray return | After the customer-side shutter locks, internal rollers bring the tray back. The robot lifts it and sets it on the infeed rollers again. |
| 01:41–01:47 | C03 empty tray restock | The belt, lift and fork return the empty tray to S02. Pickup-complete and carrier-return records close one order cycle. |
| 01:47–01:54 | C04 control conditions | Door closed and locked, position and ID, grip and seating are confirmed. On a fault the cell holds a stopped state until an operator checks and resets it. |
| 01:54–02:00 | Wrap-up | Summarizes the connected flow from staff loading to customer pickup and carrier return. |

<details>
<summary>Previous v4 video — preserved for comparison</summary>

[Watch or download the earlier 60-second v4 video](../videos/process_v4_60s_1080p30.mp4). It contains 86 detailed steps at 1920×1080 and 30 fps
(24.3 MB, no audio track), using the previous geometry and slot count. Use the v7 teaser above when presenting the current design.

</details>

## Gallery

Stills from the model. Numbers match the files in `images/`.

<p align="center"><img src="../images/01_overall_process_cell.png" alt="Isometric view of the whole process cell" width="100%"></p>
<p align="center"><sub>01 Overall process cell — A garment door, B shoe shutter and the "authenticate here" kiosk in front, the robot and C01 loop inside, the C04 cabinet on the right</sub></p>

<table>
<tr>
<td><img src="../images/08_customer_pickup.png" alt="Customer wall: A garment glass door, B shoe shutter, kiosk, staff-only door"></td>
<td><img src="../images/04_shoe_lift_and_fork.png" alt="C03 shoe lift tower with telescopic fork, energy chain and roller infeed"></td>
</tr>
<tr>
<td align="center"><sub>08 Customer pickup — "1 authenticate → 2 pick up" signage, A garment glass door, B shoe shutter, kiosk mid-transaction</sub></td>
<td align="center"><sub>04 Shoe lift and fork (C03) — lift tower, telescopic fork, energy chain, roller infeed</sub></td>
</tr>
</table>

<table>
<tr>
<td><img src="../images/12_robot_tray_transfer.png" alt="Robot holding a shoe tray with two shoe forms by its handle above the pedestal"></td>
<td><img src="../images/07_control_cabinet.png" alt="Labeled interior of the C04 control cabinet"></td>
</tr>
<tr>
<td align="center"><sub>12 Robot tray transfer (C05) — the tray held by its handle and raised above the pedestal in the central transfer pose</sub></td>
<td align="center"><sub>07 Control cabinet (C04) — protection, contactor, 24 V supply, safety relay, PLC and I/O laid out as a maintenance concept</sub></td>
</tr>
</table>

<p align="center"><img src="../images/process_gallery.jpg" alt="Process gallery: twelve captioned stills from 01 overall process cell to 12 robot tray transfer" width="100%"></p>
<p align="center"><sub>Process gallery — all twelve stills on one sheet, captioned in Korean</sub></p>

## How to view

This is a documentation and media repository; there is nothing to install or build.

**Needs**: a web browser, plus a video player that can load external SRT subtitles if you want captions.

```bash
git clone https://github.com/lsy041015/capstone-design-hanyang-university.git
cd capstone-design-hanyang-university

# e.g. play the v7 teaser in mpv with English subtitles (Korean dialogue: process_v7_ko.srt)
mpv --sub-file=videos/process_v7_en.srt videos/process_v7_teaser_120s_1080p30.mp4
```

- Reading order: this README → [docs/v7_process_teaser.md](v7_process_teaser.md) (chapter guide, verification scope, version notes; Korean)
  → [docs/v7_video_verification.json](v7_video_verification.json) (verification record of the published file).
- The two videos total about 50 MB.
- Editable Blender models (`.blend`) and production scripts are not included, so the model cannot be opened or the checks re-run from this repository.

## Operating flow

The sequence below describes the proposed service operation. The model's 31 numbered frames are **separate inspection poses**, not elapsed time or
proof of a continuous cycle. Frame 1 shows inventory waiting for pickup. Frames 23–31 show replenishment after the illustrated pickup, although staff
loading occurs before a newly completed item can be stored and later collected.

### 1. Staff inspection and item registration

- Staff check the completed garment or shoes, package them, and link an item ID to the customer's order ID at a staff terminal.
- Before loading begins, the proposed workflow rejects a duplicate ID, an unsuitable item or package, or an order with no available storage position.
- Inspection, packaging and registration remain staff tasks. The project does not claim to remove wet laundry from a washer or hang clothing automatically.

### 2. Protected loading and storage — C07, C05, C01, C03

- An empty garment adapter or shoe tray is brought to the shared staff loading station, C07.
- Customer retrieval is paused, the robot is at home, and the guarded sliding door can open only in the service state.
- Staff fold the tray shelf away when hanging a garment on its dedicated adapter; for shoes they unfold the shelf and place the packaged pair in a tray.
- After the door is closed and locked, an operator reset is required before automatic motion can resume.
- The robot grips the **rigid adapter tab or tray handle**, rather than fabric or shoes. For garments, it transfers the loaded adapter to an empty
  carrier on the C01 loop. For shoes, it puts the tray on the C03 infeed, where the rollers, belt, lift and telescopic fork place it on one of four levels.
- The proposed inventory record changes from provisional to occupied only after the destination, item ID and presence checks agree.
- The saved replenishment poses are frames 23–31. They illustrate one garment and one shoe-tray refill, not a measured loading time.

### 3. Customer authentication and reservation

- At the kiosk the customer enters a pickup number or presents a QR code.
- The intended control flow checks the order, pickup permission and item location, then reserves the item so the same stock cannot be retrieved by two requests.
- The A garment and B shoe compartments remain closed while the mechanism prepares the item.
- The customer interface follows a short two-step structure: **1. Authenticate → 2. Pick up**. Garments and shoes use clearly separated compartments
  marked A and B. The kiosk supports pickup-number entry and a QR reader, while a visible assistance control provides a recovery path when
  authentication or retrieval fails.
- The kiosk screens and logical conditions are represented in the concept; a live payment, QR, order or inventory server has not been implemented.

### 4. Garment selection and robot handoff — C01, C05, frames 2–6

- The C01 carrier loop indexes the requested garment to the robot pickup station and locks at the target position.
- After checking the carrier and item ID, the robot closes its garment jaws on the adapter's rigid tab, lifts the adapter off its open-bottom saddle
  and carries it to the C02 receiving saddle.
- A retention latch holds the adapter before the gripper releases it.
- The poses show indexing, locking, gripping, lifting and placement; they do not establish a collision-free continuous robot trajectory between poses.

### 5. Garment delivery and adapter recovery — C02, C06, frames 7–10

- With the robot clear and the customer door locked, the internal door opens.
- The complete C02 Z module travels with the Y carriage for the nominal **730 mm Y movement**, then lowers the garment by **300 mm** to the customer
  pickup position.
- The internal door must close and lock before the outer garment door can unlock.
- The customer removes the hanger and garment, while the transport adapter stays available for return inside the system.
- Removal and door closure are required before the order is marked collected and the empty adapter is recovered. If either door or the item check
  fails, the design keeps the next movement blocked.

### 6. Shoe retrieval and transfer — C03, C05, frames 11–18

- The C03 lift stops at the shelf assigned to the shoe order.
- Its three-stage fork reaches **600 mm** into the shelf, raises the tray **20 mm** for clearance, retracts and moves to the belt transfer height.
  The fork then lowers the tray onto the belt.
- Linked rollers move it to an infeed stop where its presence and ID are checked before robot pickup.
- The robot grips the tray handle, passes it through the central cell and sets it on the output rollers.
- The belt and roller tops share a nominal **1,080 mm** transfer height; these are model design dimensions, not tested handling performance.

### 7. Shoe pickup and empty-tray return — C06, C03, frames 19–22

- The tray reaches the B shoe compartment while its customer shutter remains locked.
- After the robot returns home, the internal shutter can open to admit the tray; it then closes and locks before the outer shutter opens.
- The customer takes the shoes and packaging but leaves the reusable tray.
- Only after removal and outer-shutter closure does the empty tray move back through the internal route. The robot, infeed, belt, lift and fork
  return it to an available storage position, and the inventory record is cleared after the return checks succeed.

### 8. Fault gates and reset

- Before permitting the relevant motion, the intended sequence checks service-door closure, emergency-stop state, operator reset, robot home,
  door interlocks, obstruction, timeout, item ID, occupancy and grip confirmation.
- A failed check holds the item or transport carrier in a defined position and calls for operator review instead of silently completing the order.
- These are **design requirements and tested logical conditions in the model**, not a working PLC, independent safety circuit or validated recovery procedure.

## System architecture

The cell is divided into seven modules, C01–C07. Dimensions are nominal model design values.

| Code | Module | Design |
|---|---|---|
| **C01** | Garment storage conveyor | Continuous guided carrier loop with 25.4 mm design pitch, 212 links and two 54-tooth sprockets. Captive trolley wheels, a chain connection, a drive unit, a tensioner, an encoder and an index locking pin are represented. Garments are aligned tangent to the rail to reduce sleeve overlap. |
| **C02** | Garment transfer axis | A Y-axis carriage moves the complete Z-axis assembly rather than leaving the vertical guide fixed. Nominal travel is 730 mm in Y and 300 mm in Z. The receiving saddle includes a retention latch so the robot can release the garment adapter before customer delivery. |
| **C03** | Shoe lift, fork and roller transfer | Four shoe storage levels served by a guided lift and a three-stage telescopic fork: 600 mm horizontal travel, at least 130 mm overlap between nested stages, and a separate 20 mm vertical transfer motion. The infeed and output conveyors use 40 mm rollers at 80 mm pitch, with linked drive elements and side guides. |
| **C04** | Electrical and control cabinet | The layout distinguishes the main disconnect, circuit protection, contactor, 24 V power supply, safety relay, PLC, digital I/O, motor drives, terminals, protective earth, cable ducts and cable glands. It is a spatial maintenance concept rather than a certified electrical schematic. |
| **C05** | Robot end effector | Replaceable jaws with two contact profiles: a flat underside grip handles the garment adapter stem, while V-shaped pads locate the shoe tray handle. The robot handles rigid adapters and trays instead of directly gripping fabric or shoes. |
| **C06** | Interlocked customer pickup compartments | The garment compartment uses framed hinged doors. The shoe compartment uses rolling slat shutters with guides, a tubular motor, monitored locks, hand detection and a pressure-sensitive lower edge. The internal and customer doors are intended to remain mutually exclusive. |
| **C07** | Staff replenishment station | A guarded sliding door, an emergency stop, a reset control, an identification reader, a garment docking pin and a folding shoe-tray shelf. The shelf folds down for tray loading and folds away to prevent interference with hanging garments. |

Renders of C03, C04 and C06 (04, 07, 08) are in the [Gallery](#gallery).

<table>
<tr>
<td><img src="../images/02_garment_drive_and_carrier.png" alt="C01 rail loop, drive sprocket and carriers with hanging garments"></td>
<td><img src="../images/03_garment_yz_transfer.png" alt="C02 Y beam over a Z leadscrew column"></td>
</tr>
<tr>
<td align="center"><sub>02 Garment drive and carrier (C01) — rail loop, drive sprocket, carriers with hanging garments</sub></td>
<td align="center"><sub>03 Garment Y–Z transfer (C02) — Y beam carrying a Z leadscrew column, labeled "Y 730 / Z 300 mm"</sub></td>
</tr>
</table>

<table>
<tr>
<td><img src="../images/10_staff_garment_replenishment.png" alt="Garment and robot at the C07 staff station, emergency stop visible"></td>
<td><img src="../images/11_staff_shoe_replenishment.png" alt="Shoe tray on the unfolded shelf of the C07 staff station"></td>
</tr>
<tr>
<td align="center"><sub>10 Staff garment replenishment (C07) — a garment at the station with the robot, e-stop and reset visible</sub></td>
<td align="center"><sub>11 Staff shoe replenishment (C07) — a shoe tray on the unfolded folding shelf</sub></td>
</tr>
</table>

### Transfer details

- The robot transfers the shoe tray by its rigid handle and places it onto the roller conveyor.
- During review, the central tray transfer pose was raised to avoid the robot pedestal. The nominal robot TCP for this pose is 1,700 mm above the model
  floor, and the tray origin is 1,545 mm (render 12 in the gallery).
- The roller and belt transfer surfaces are aligned at 1,080 mm. Across 201 sampled positions, a 320 mm tray base remained supported by at least four
  rollers on both conveyor lanes.

<table>
<tr>
<td><img src="../images/05_garment_adapter_grip.png" alt="Close-up of the gripper's flat pads on a rigid garment adapter tab"></td>
<td><img src="../images/06_roller_and_belt_transfer.png" alt="Roller conveyor feeding a shoe tray toward the lift"></td>
</tr>
<tr>
<td align="center"><sub>05 Garment adapter grip (C05) — flat pads closing on the rigid adapter tab</sub></td>
<td align="center"><sub>06 Roller and belt transfer (C03) — roller conveyor feeding a shoe tray toward the lift</sub></td>
</tr>
</table>

## Design review and verification

The design was scored in an internal review, and the model and film were checked geometrically by script. All of it happens on the Blender model;
none of it establishes a continuously collision-free robot trajectory or validated hardware safety.

### Concept review: 32/100 → 74/100

The revised design received an internal concept-review score of **74/100**, improved from 32/100 for the previous display-oriented layout. This score
is an author assessment, not a certification or safety approval. The fixes made during review:

1. Reduced the garment demonstration capacity from 10 to 8 positions to eliminate clothing-envelope overlap.
2. Rotated garments to follow the conveyor tangent instead of placing adjacent garments in the same global orientation.
3. Connected the Z-axis assembly to the moving Y carriage.
4. Moved the garment transfer beam after a robot-link interference was identified.
5. Replaced sparse output rollers with an 80 mm pitch arrangement.
6. Added a guided lift, nested fork stages, a tip-lift transfer, brakes, limits, bumpers and a constant-length energy chain.
7. Replaced overlapping garment pins with an open-bottom lift-off saddle.
8. Added a guarded staff replenishment workflow and automatic return of empty transport carriers.
9. Added fixed upper and lower infill panels around the closed shoe shutter.
10. Raised the central shoe-tray transfer pose after a tray-to-pedestal collision was found.

### Inspection model: 177 automated checks

The original v7 inspection model passed 177 automated checks, including:

- 31 native inspection poses
- maximum nominal TCP position error of approximately 0.0098 mm at the saved poses
- zero overlap among 560 × 140 mm garment envelopes across 201 indexed positions
- minimum garment separating-axis gap of approximately 113.10 mm
- at least four supporting rollers under a 320 mm tray throughout both conveyor paths
- fork-to-tray transfer contact within 0.1 mm at the checked poses
- fail-closed logical checks for door state, emergency stop, reset, obstruction, timeout, ID, occupancy and grip confirmation
- no detected triangle intersections between the selected robot/tool meshes and 1,685 selected surrounding rigid parts at the 31 saved poses
- no detected intersections between the moving garment or tray and the selected pedestal, frame, guard and robot meshes at the saved poses

### v7 film verification

The film was checked separately from the inspection model. Sources: the [verification scope](v7_process_teaser.md#검증-범위) in the chapter
guide and [docs/v7_video_verification.json](v7_video_verification.json).

| Check | Result and scope |
|---|---|
| Source preserved | The original Blender file's SHA-256 was compared before and after production. The 31 inspection poses were not overwritten; the film is a separate file. |
| All frames computed | All 3,600 commanded positions were checked for UR5 nominal inverse kinematics, no simultaneous opening of both doors on the garment and shoe compartments, robot stopped while the staff station is open, and tray grip offset. Maximum nominal TCP error is about 0.0248 mm, a numerical residual. |
| Saved animation | The saved file was reopened and 351 sampled frames were inspected for TCP position, camera cuts, Y–Z coupling and opposing garment-door states. Maximum TCP residual in saved coordinates is about 0.0198 mm. |
| Selected rigid interference | No triangle intersections between the visible garment or tray and the selected pedestal, frame, rail and robot-link meshes at the sampled frames (0 found). This is not a continuous collision check between samples. |
| Published file | Verdict `PASS`. 1920×1080, 30 fps, 120.0 s, all 3,600 frames decoded, 0 embedded subtitle streams, 21 chapters. Audio peak about -2.0 dBFS; per-shot RMS from -19.8 to -17.7 dBFS. |

- SHA-256 of the published MP4: `e23c7f0ac8fb6980a4849bce0c312be8e3feea5e0cb3d035f861ee1cb7fe3d0a`
- Stated scope of the record: nominal positions and selected sampled rigid-body checks only. It excludes full continuous swept volume, robot
  self-collision, flexible garment dynamics, the actual FR5e, load and hardware safety certification.

<p align="center"><img src="../images/09_completed_return_state.png" alt="Cell state after pickup and empty-carrier return" width="100%"></p>
<p align="center"><sub>09 Completed return state — the cell after pickup and empty-carrier return</sub></p>

## Repository contents

This public repository contains project images, the v7 process teaser, the earlier v4 video, Korean and English subtitle files, process explanations
and a compact video verification record.

```text
capstone-design-hanyang-university/
├── README.md                     # project write-up (Korean)
├── README.en.md                  # project write-up (English)
├── assets/capstone-banner.png    # README banner
├── docs/
│   ├── v7_process_teaser.md        # v7 chapter guide, verification scope, version notes (Korean)
│   ├── v7_video_verification.json  # verification record: hashes, decode, audio, chapters
│   └── UR5_LICENSE.txt             # BSD-3-Clause notice for the UR5 mesh
├── images/                       # 12 renders (01–12), contact sheet, video poster
└── videos/
    ├── process_v7_teaser_120s_1080p30.mp4   # current v7 teaser: 2:00, 21 shots, 25.7 MB
    ├── process_v7_ko.srt                    # Korean dialogue (external subtitles)
    ├── process_v7_en.srt                    # English subtitles (external)
    └── process_v4_60s_1080p30.mp4           # earlier v4 video: 60 s, 86 steps, 24.3 MB
```

Editable Blender models and production scripts are not included.

## Tools

Tools and third-party material behind the design and film. Versions are listed only where the repository records them.

| Tool or asset | Used for | Version or pin |
|---|---|---|
| Blender | 3D model, the 31 inspection poses, the v7 film | not recorded; no `.blend` files, only their hashes in the verification record |
| UR5 CB-series mesh | robot geometry and nominal kinematics standing in for the target FR5e | Universal Robots ROS 2 Description, commit `89bbe795f38a7ab00fb66fe8831dfff79dc99edf` |
| Microsoft Heami Desktop | Korean text-to-speech narration (Windows voice) | not recorded |
| Published video | H.264 (yuv420p) 1920×1080 30 fps, AAC 48 kHz stereo | `docs/v7_video_verification.json` |
| Verification record | hashes, decode, loudness and sampled-frame results as JSON | script and tool names are not in the repository |

## Design notes

- **The robot never grips fabric or shoes.** The C05 gripper's flat underside takes the garment adapter's rigid tab, and V-pads take the shoe-tray
  handle. Customers take the hanger and garment, or the shoes; adapters and trays stay in the cell for reuse (`images/05_garment_adapter_grip.png`).
- **The tray hit the pedestal.** Review found a tray-to-pedestal collision in the central tray-transfer pose, so the pose was raised: nominal TCP
  1,700 mm above the floor, tray origin 1,545 mm. In the film the robot lifts the tray clear of the pedestal before moving it (`images/12_robot_tray_transfer.png`).
- **From 32 to 74.** The earlier display-oriented layout scored 32/100 in the internal concept review. Ten fixes, from cutting garment capacity
  10 → 8 to remove envelope overlap to mounting the Z axis on the moving Y carriage, brought it to 74/100. It is the author's own assessment.
- **The film has a machine-readable QA report.** `docs/v7_video_verification.json` records the MP4's SHA-256, a full decode of all 3,600 frames,
  per-shot loudness (RMS) for all 21 chapters, zero embedded subtitle streams, and hashes of the source and film Blender files. Before-and-after hashes
  also confirm the original inspection model was not changed while the film was made.
- **The stand-in robot is pinned to a commit.** The target is the FR5e, but the robot in every render and in the film is the official Universal Robots
  UR5 CB-series mesh, pinned to commit `89bbe795…` of the ROS 2 Description repository and shipped with its BSD-3-Clause notice
  (`docs/UR5_LICENSE.txt`). UR5 specifications are never presented as FR5e performance.
- **One door at a time.** For garments the customer door opens only after the inner door is closed and locked; for shoes the inner shutter opens only
  while the outer shutter is locked. Carrier return starts only after the item is removed and the door is closed. The film check computed across
  all 3,600 frames that both doors of the garment and shoe compartments are never open at the same time (`docs/v7_process_teaser.md`).

## Limitations

This repository documents a visual and geometric capstone concept. It does not demonstrate a production-ready machine. The following work remains
necessary before fabrication or commercial deployment:

- Replace the substitute UR5 geometry and nominal kinematics with the exact FR5e CAD, calibration, payload, tool mass and center of gravity.
- Validate continuous robot trajectories, approach and departure paths, full self-collision and garment motion.
- Calculate motor torque, brake capacity, fork deflection, frame stiffness, vibration, tolerances, grip force and rated payload limits.
- Select real purchased components and produce manufacturing drawings, wiring diagrams and a thermal/electromagnetic-compatibility review.
- Implement and validate the actual PLC, robot controller, independent safety circuit, stop distance, restart procedure and fault recovery.
- Conduct physical usability testing for staff reach, loading time, cleaning access, customer accessibility and maintenance tasks.
- Validate the payment, QR, order, inventory and notification services.

Also:

- The demonstration slot count must not be interpreted as the capacity or profitability of a commercial store.
- The film's 120 s and the 31 pose numbers are not cycle times.
- Without the `.blend` files and production scripts, the model cannot be opened or the checks reproduced from this repository.

## Credits and license

- **License**: there is no repository-wide license file. The UR5 notice below covers only the UR5 mesh, not this repository's text, images or video.
- **UR5 mesh**: the robot in the renders and film is the official Universal Robots UR5 CB-series simplified STL geometry from the
  [Universal Robots ROS 2 Description](https://github.com/UniversalRobots/Universal_Robots_ROS2_Description/tree/89bbe795f38a7ab00fb66fe8831dfff79dc99edf/meshes/ur5/collision)
  repository, pinned to commit `89bbe795f38a7ab00fb66fe8831dfff79dc99edf`. Its BSD-3-Clause notice is in [docs/UR5_LICENSE.txt](UR5_LICENSE.txt).
  It is not original work of this project.
- **Model note**: the intended application robot is the FR5e. The visualization uses the UR5 mesh as a substitute for layout and nominal kinematic
  review; UR5 specification values and geometry must not be presented as FR5e performance data.
- **Narration voice**: the Windows Microsoft Heami Desktop Korean text-to-speech voice; no separate music track.
- **Reference material**: [Universal Robots UR5 technical specification](https://www.universal-robots.com/media/1828033/ur5_tech_spec_web_en.pdf) ·
  [Universal Robots ROS 2 Description repository](https://github.com/UniversalRobots/Universal_Robots_ROS2_Description) ·
  [Metalprogetti automated dry-cleaning systems](https://www.metalprogetti.it/en/products/automated-dry-cleaning/)

Hanyang University Capstone Design · Automated Unmanned Laundry Pickup System
