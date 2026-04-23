# Design Decisions — PMR Team 09.08

A record of the major design decisions made throughout the project, what alternatives we considered, and why we chose what we did.

---

## 1. Dual Color Sensors (vs. Single)

**Decision:** Switch from one color sensor to two after Subtask 1.

**Why:** A single sensor could not reliably handle dashed lines — it would lose the line at gaps and not recover consistently. Two sensors, one on each side of the line, gave the robot redundancy and allowed it to detect which direction it had drifted, enabling active correction rather than just stop-and-search recovery.

---

## 2. Detect Mode (vs. Ambient or Reflected)

**Decision:** Use Detect mode for line following rather than Ambient or Reflected.

**Why:** During sensor testing (2/12/26) we found our color sensor was highly inconsistent in Ambient mode, particularly on black. Reflected mode worked for black/white but was unreliable on dashed lines. Detect mode, while less granular, was by far the most consistent and repeatable on our specific hardware.

---

## 3. Forklift Mechanism (vs. Claw Gripper)

**Decision:** Pivot from a claw gripper to a forklift-style lift arm with chopstick forks.

**Why:** The claw mechanism could not reliably grip the boxes — the arms were too small and the grip power inconsistent. A forklift design slides under the box rather than gripping it, which is more mechanically forgiving and doesn't rely on precise positioning. The chopstick forks gave us a low-profile lifting surface that fit under the custom boxes.

---

## 4. Tread System (vs. Wheels)

**Decision:** Convert from standard wheels to dual treads late in development.

**Why:** Wheels caused the robot to slip on the paper track and struggle with directional stability during turns. Treads provided more consistent contact with the surface and better handled the slight unevenness of our paper track.

---

## 5. Metal Ball Bearing Rear Support (vs. Rear Wheel)

**Decision:** Replace the rear wheel with a metal ball bearing.

**Why:** The rear wheel created significant drag during turns, causing the robot to arc inconsistently. A ball bearing provides omnidirectional low-friction support, allowing the robot to pivot cleanly. This came with its own stability challenges but was ultimately worth the trade-off.

---

## 6. Gyro Sensor for Weight Classification (vs. Force Sensor)

**Decision:** Use the gyro sensor to detect tilt angle change as a proxy for weight.

**Why:** We did not have a force sensor in our kit. The gyro sensor detects the robot's change in tilt when the forklift lifts a heavier load, which produces a measurably different rate than a lighter load. While less precise than a dedicated force sensor, the gyro threshold approach was reliable enough to distinguish between our two box weights consistently.

---

## 7. Inverted Claw Orientation

**Decision:** Invert the claw arms so the wider part faces the box rather than the narrow part.

**Why:** Initial testing showed the narrow ends of the arms could not generate enough grip area to hold the heavier box. Inverting them increased the contact surface significantly and combined with the pivot to a forklift design, solved the heavy box pickup problem.
