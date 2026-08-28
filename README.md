# 1 kW EV Wheelchair Charger — PFC + PSFB

A MATLAB/Simulink-based **1 kW EV wheelchair charging system** designed using a cascaded **AC–DC Boost PFC** and **Phase-Shift Full-Bridge (PSFB) DC–DC converter**.

The system is designed to convert **325 V peak AC input → 400 V DC link → 24 V DC output**, with closed-loop control for voltage regulation and improved input power quality.

## System Architecture

```text
AC Input
  │
  ▼
Bridge Rectifier
  │
  ▼
Boost PFC ──────────► 400 V DC Link
  │
  ▼
Phase-Shift Full-Bridge
  │
  ▼
High-Frequency Transformer
  │
  ▼
Secondary Rectifier + LC Filter
  │
  ▼
24 V DC Output
  │
  ▼
EV Wheelchair Battery
```

## Key Specifications

| Parameter                | Specification          |
| ------------------------ | ---------------------- |
| Rated Power              | 1 kW                   |
| AC Input                 | 325 V Peak / 230 V RMS |
| DC-Link Voltage          | 400 V DC               |
| Output Voltage           | 24 V DC                |
| Output Current           | ~41.66 A               |
| PFC Switching Frequency  | 50 kHz                 |
| PSFB Switching Frequency | 50 kHz                 |
| Target Power Factor      | ≈ 1                    |
| Target THD               | < 5%                   |
| Topology                 | Boost PFC + PSFB       |

## AC–DC Boost PFC

The first stage uses a **Boost PFC converter** to generate a regulated 400 V DC link while shaping the input current to follow the AC input voltage.

### Control

* Closed-loop average current mode control
* 400 V DC-link voltage regulation
* PWM switching at 50 kHz
* Input current shaping for improved power factor
* Designed for THD below 5%

### Simulation Results

| Parameter            | Result |
| -------------------- | -----: |
| DC-Link Voltage      |  400 V |
| Power Factor         |  0.999 |
| Current THD          |  1.73% |
| AC RMS Input Current | 6.36 A |
| Input Peak Current   |  9.0 A |

Open-loop operation initially resulted in approximately **28.6% input current THD**. After implementing closed-loop average current mode control, THD was reduced to **1.73%** and power factor improved to approximately **0.993–0.999**.

## Phase-Shift Full-Bridge Converter

The second stage uses an isolated **Phase-Shift Full-Bridge DC–DC converter** to step down the 400 V DC link to the required 24 V DC output.

### Main Components

* Full-bridge MOSFET inverter
* High-frequency isolation transformer
* Secondary-side rectifier
* Output LC filter
* Closed-loop phase-shift controller

The output voltage is regulated by varying the **phase shift between the two bridge legs** while maintaining a constant switching frequency. The design also targets **Zero-Voltage Switching (ZVS)** to reduce switching losses.

### Simulation Results

| Parameter               |   Result |
| ----------------------- | -------: |
| DC-Link Voltage         |    400 V |
| Output Voltage          |     24 V |
| Output Current          | ~41.66 A |
| Rated Power             |     1 kW |
| Transformer Turns Ratio |     0.07 |

The closed-loop phase-shift controller maintained the output around **24 V** under load variation and supported ZVS operation at the rated load.

## Hardware Development

Along with the simulation work, supporting hardware development was carried out for future converter implementation.

### ESP32 Data Acquisition

Developed an ESP32-based data acquisition system for monitoring:

* Voltage
* Current
* Temperature

The measured parameters can be stored on external memory in **CSV format** for analysis.

### TI-Based PWM Generation

Implemented complementary Phase-Shift Full-Bridge PWM generation using a **TI F28027F** controller.

* Switching frequency: 100 kHz
* Duty cycle: ~30%
* Phase shift: 90°
* Dead time: ~200 ns
* Complementary PWM outputs

### Initial Hardware Testing

Individual converter sections were tested before full-power operation, including:

* Isolated DC–DC power supplies
* MOSFET gate-driver circuits
* Complementary PWM signals
* Voltage sensing circuit

The supporting circuits showed expected operation; however, the complete power-transfer path still requires further debugging before achieving the desired full-power output.

## Tools & Technologies

* **MATLAB / Simulink**
* Power Electronics
* Boost PFC
* Phase-Shift Full-Bridge (PSFB)
* PI Control
* Average Current Mode Control
* PWM / Phase-Shift Modulation
* ZVS
* ESP32
* TI F28027F
* Oscilloscope-based Hardware Testing

## Project Objectives

* Design and simulate a 1 kW AC–DC Boost PFC converter
* Generate a regulated 400 V DC link
* Design a 24 V isolated PSFB DC–DC stage
* Implement closed-loop control for voltage regulation
* Improve input power factor and reduce harmonic distortion
* Develop measurement and PWM control hardware for future implementation

## Current Status

**Simulation:** Successfully demonstrated regulated 400 V DC-link and 24 V DC output with improved power quality and closed-loop control.

**Hardware:** Supporting circuits and control signals have been individually verified. Full converter operation requires further debugging of the power stage, gate drive, feedback/sensing, and control paths before higher-power testing.
