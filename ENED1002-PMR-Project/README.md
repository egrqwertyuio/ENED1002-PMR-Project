# ENED 1002c — Prototype Mover Robot (PMR)
**Team 09.08 | University of Cincinnati | Spring 2026**

Baolong Phan · Ethan Myhal · Yegor Lushpin · Andrew Sullivan

---

## Overview

This repository contains the full codebase, documentation, and test data for our Prototype Mover Robot (PMR) built in ENED 1002c. The robot was designed to autonomously navigate a closed-loop track, detect and pick up bins of shredded material, classify them by weight (organic vs. metal), and deposit them at the correct location before returning to the start.

The robot was built on the LEGO Mindstorms EV3 platform and programmed in LabVIEW.

---

## Final Robot Capabilities

| Capability | Details |
|---|---|
| Line Following | Dual color sensor, solid and dashed black lines |
| Gap Navigation | Reliable up to 2 cm, recoverable up to 4 cm |
| Object Detection | Ultrasonic sensor, activation threshold 5–10 cm |
| Weight Classification | Gyro sensor rate threshold (organic vs. metal) |
| Pickup Mechanism | Forklift-style lift arm with chopstick forks |
| Drop-off | 2 color-coded zones detected via color sensor |
| Drive System | Dual tread (converted from wheels late in development) |
| Rear Support | Metal ball bearing for low-friction turning |
| Mean Lap Time | 33.69 seconds |
| Clean Run Rate | 85% (17/20 trials) |

---

## Repository Structure

```
ENED1002-PMR-Project/
│
├── code/
│   ├── line_following/        # Dual sensor line following logic
│   ├── pickup/                # Object detection and grab/lift sequence
│   ├── weight_classification/ # Gyro-based organic vs. metal classification
│   ├── dropoff/               # Drop-off zone detection and deposit sequence
│   └── return_to_line/        # Return navigation after deposit
│
├── docs/
│   ├── code_documentation.md  # State machine breakdown and logic explanation
│   ├── design_decisions.md    # Key design decisions and why we made them
│   └── lessons_learned.md     # What we'd do differently
│
├── data/
│   └── activity9_test_data.csv  # Raw test data from Activity 9 statistical testing
│
├── media/
│   ├── early_build/           # Photos from early robot builds
│   ├── final_build/           # Photos of final robot configuration
│   └── testing/               # Photos and screenshots from testing sessions
│
└── README.md
```

---

## Code Architecture

The robot operates as a state machine with the following states:

```
START → NAVIGATE (line follow) → DETECT BOX → PICKUP → CLASSIFY WEIGHT
      → NAVIGATE TO DROP-OFF → DEPOSIT → RETURN TO LINE → NAVIGATE → ...
```

Each state is implemented as a separate LabVIEW case within a master VI. See [`docs/code_documentation.md`](docs/code_documentation.md) for a full breakdown of each state's logic.

> **Note:** LabVIEW `.vi` files require LEGO Mindstorms EV3 LabVIEW to open. Exported block diagram images are included in each code subfolder for reference without needing LabVIEW installed.

---

## Hardware Configuration

| Component | Sensor/Part | Port | Purpose |
|---|---|---|---|
| Color Sensor 1 | EV3 Color Sensor | Port 1 | Left line detection |
| Color Sensor 2 | EV3 Color Sensor | Port 2 | Right line detection |
| Ultrasonic Sensor | EV3 Ultrasonic | Port 3 | Box detection |
| Gyro Sensor | EV3 Gyro Sensor | Port 4 | Weight classification |
| Left Tread Motor | EV3 Large Motor | Port B | Drive |
| Right Tread Motor | EV3 Large Motor | Port C | Drive |
| Lift Motor | EV3 Medium Motor | Port A | Forklift arm |

---

## Test Results

Full test data and statistical analysis are available in [`data/activity9_test_data.csv`](data/activity9_test_data.csv).

**Test 1 — Correction Rate (20 trials)**
- Mean corrections per run: 0.15
- Standard deviation: 0.366
- 17/20 runs completed with zero corrections

**Test 2 — Lap Completion Time (20 trials)**
- Mean: 33.69 seconds
- Standard deviation: 1.11 seconds
- Range: 31.92s – 35.48s

Errors observed were primarily surface-related (paper track deformation) rather than sensor or code failures.

---

## Development Timeline

| Week | Milestone |
|---|---|
| Weeks 1–2 | Team formation, kit inventory, Code of Cooperation |
| Week 3 | LabVIEW setup, color sensor testing and data collection |
| Week 4 | Subtask 1 — basic line following prototype |
| Week 5 | Robot reconstruction, dual sensor integration, grab/lift prototype |
| Week 6 | Status Update 1, timeline adjustment |
| Weeks 7–8 | Activity 9 statistical testing, code and mechanical refinement |
| Week 9 | Status Update 2, dedicated practice track built |
| Week 10 | Full claw mechanism rebuild, intensive pre-demo testing |
| Week 11 | Final demonstration, presentation, disassembly |

---

## Lessons Learned

**After Subtask 1:** A single color sensor was unreliable on dashed lines. We redesigned with dual sensors and repositioned sensor placement, which became the foundation for all subsequent development.

**After Status Update 1:** Our timeline was too optimistic. Splitting into subteams — one on line logic, one on the arm — made sessions significantly more productive.

**After Status Update 2:** Testing subsystems individually is not enough. Full end-to-end integration testing needs to be prioritized earlier so issues surface before the final stretch.

See [`docs/lessons_learned.md`](docs/lessons_learned.md) for a full write-up.

---

## Team

| Name | Major | Role |
|---|---|---|
| Yegor Lushpin | Electrical Engineering | Lead Programmer |
| Baolong Phan | Mechanical Engineering | Design & Manufacturing Lead |
| Ethan Myhal | Biomedical Engineering | Testing & Research Lead |
| Andrew Sullivan | Chemical Engineering | Documentation Lead |

---

## References

- LEGO Mindstorms EV3 Platform — LEGO Education
- Grab and Lift Mechanism inspiration: https://www.youtube.com/watch?v=PQI66KsRsqM
- Forklift EV3 design reference: https://roboticsindonesia.blogspot.com/2013/10/
- EV3 Gyro Sensor documentation: https://ev3-help-online.api.education.lego.com
