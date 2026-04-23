# Code Documentation — PMR Team 09.08

## Architecture Overview

The robot is programmed as a **state machine** in LabVIEW. A single master VI controls all robot behavior by cycling through defined states. Each state handles one aspect of the robot's task, and transitions between states are triggered by sensor readings or completion conditions.

```
START
  └─→ NAVIGATE
        └─→ DETECT BOX (ultrasonic trigger)
              └─→ PICKUP SEQUENCE
                    └─→ CLASSIFY WEIGHT (gyro)
                          └─→ NAVIGATE TO DROP-OFF (color zone detection)
                                └─→ DEPOSIT SEQUENCE
                                      └─→ RETURN TO LINE
                                            └─→ NAVIGATE (loop)
```

---

## State Descriptions

### 1. NAVIGATE (Line Following)
**File:** `code/line_following/`

The robot uses two EV3 color sensors in **Detect mode** mounted approximately 1 cm from the ground. The sensors read the track and the robot steers based on which sensor detects the black line.

- If left sensor sees black → turn left
- If right sensor sees black → turn right
- If both see black → stop (gap handling / intersection logic)
- If neither sees black → continue straight at reduced speed (gap bridging)

**Key parameters:**
- Drive speed: 50% motor power (optimal from testing)
- Turn speed: slightly lower than drive speed for smoother correction
- Sensor mode: Detect (more consistent than Ambient on our hardware)

---

### 2. DETECT BOX
**File:** `code/pickup/`

The ultrasonic sensor continuously monitors distance while the robot navigates. When distance drops below the threshold (5–10 cm, tuned during testing), the robot exits NAVIGATE and enters PICKUP.

**Key parameters:**
- Detection threshold: 5 cm (final tuned value after testing at 15 cm and 10 cm)

---

### 3. PICKUP SEQUENCE
**File:** `code/pickup/`

Once a box is detected, the robot stops forward motion and activates the lift motor.

Sequence:
1. Stop drive motors
2. Close forklift arms (lift motor forward)
3. Raise forklift to carrying height
4. Confirm grip (motor stall detection)
5. Transition to CLASSIFY WEIGHT

**Key parameters:**
- Grab power: 40 (final value after testing at 25 and 20)
- Lift height: determined by motor rotation count

---

### 4. CLASSIFY WEIGHT
**File:** `code/weight_classification/`

The gyro sensor detects the change in the robot's tilt angle when holding a box. A heavier box (metal) produces a measurably different gyro rate than a lighter box (organic). A threshold value determined during testing classifies the box.

- Above threshold → Metal → navigate to far drop-off zone
- Below threshold → Organic → navigate to near drop-off zone

---

### 5. NAVIGATE TO DROP-OFF
**File:** `code/dropoff/`

After classification, the robot resumes line following while watching for the drop-off zone color marker (red for one zone, blue for the other). When the color sensor detects the target color, the robot stops and enters DEPOSIT.

---

### 6. DEPOSIT SEQUENCE
**File:** `code/dropoff/`

1. Stop drive motors
2. Lower forklift arms
3. Open arms fully (extend outward to release box)
4. Back up slightly off the deposit zone
5. Transition to RETURN TO LINE

**Key fix:** Arms must extend fully outward after deposit — partial extension caused the robot to re-detect and re-pick up the box on early versions.

---

### 7. RETURN TO LINE
**File:** `code/return_to_line/`

After deposit the robot navigates back to the main track line and resumes the NAVIGATE state to complete the loop back to the pickup station.

---

## Known Issues & Workarounds

| Issue | Cause | Fix Applied |
|---|---|---|
| Robot stops when both sensors see black | Intersection/gap logic triggering incorrectly | Adjusted case handling for dual-black detection |
| Arms re-pick up box after deposit | Arms not fully extended after drop | Extended arm open sequence to full range |
| Robot detects drop-off color too early | Color sensor angle | Slanted sensors slightly forward |
| Heavy box not picked up | Arms too small, grab power too low | Inverted claw orientation, increased grab power to 40 |
| Back wheel dragging on course | High friction rear wheel | Replaced with metal ball bearing |
| Robot tilting, sensors out of range | Ball bearing instability | Adjusted chassis balance and sensor height |
