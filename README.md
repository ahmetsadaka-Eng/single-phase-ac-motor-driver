# Microcontroller-Based Single-Phase AC Motor Driver (48VAC / 2A)

A robust, isolated single-phase AC motor speed controller featuring hardware zero-crossing detection and flexible software-controlled firing modes.

## Overview
This project implements an opto-isolated AC motor drive circuit designed to handle up to **48VAC @ 2A**. It enables smooth voltage regulation and soft switching using a TRIAC, driven by a microcontroller running custom timing routines synchronized to the AC mains.

---

## Key Hardware Features
* **Zero-Crossing Detection (ZCD):** Custom-designed opto-isolated circuit providing precise interrupt signals to the MCU at AC voltage zero-points.
* **Power & Switching Stage:** TRIAC switched via an optocoupler/opto-triac (MOC series) to ensure full galvanic isolation between logic and high-power AC lines.
* **Snubber Protection:** Integrated RC snubber network across the TRIAC output to suppress inductive spikes ($dv/dt$) from motor windings.
* **Dual Terminals:** Dedicated industrial screw terminals for AC Input (48VAC) and AC Output (Motor connection).
* **Power Supply:** Microcontroller logic stage is designed for flexible operation (battery or regulated DC supply).

---

## Control & User Interface
* **Speed/Voltage Adjustment:** Linear potentiometer mapped via ADC for duty/firing angle tuning.
* **Start / Stop Toggle:** Debounced software pushbutton toggle to engage or shut down the AC output instantly.
* **Mode Selection Toggle:** Debounced push button switching between two regulation techniques:
  * **Phase Angle Control (PAC):** Precision delay firing for continuous RMS voltage tuning.
  * **Zero-Crossing / Integral Cycle Control:** Full-cycle burst firing to minimize switching harmonics and EMI.
* **Visual Status:** On-board LED indicators displaying current operating mode and drive status.

---

## Pinout & Connections

| Component / Function | Microcontroller Interface | Note |
| :--- | :--- | :--- |
| Zero-Crossing Input | External Interrupt Pin (INT) | Falling/Rising edge synchronization |
| TRIAC Gate Trigger | Digital Output (Timer-driven) | Opto-isolated pulse output |
| Speed Potentiometer | Analog Input (ADC) | 0–100% control reference |
| Start / Stop Button | Digital Input (Pull-up) | Software debounce toggle |
| Mode Select Button | Digital Input (Pull-up) | Software debounce toggle |
| Mode LED | Digital Output | Active state indicates selected mode |

---

## How It Works
1. The **Zero-Crossing Detector** senses each AC zero voltage crossing and triggers a hardware interrupt on the microcontroller.
2. An internal hardware timer calculates the required delay based on the potentiometer reading.
3. Depending on the active mode (**Phase Angle** or **Burst Cycle**), the MCU pulses the opto-triac gate driver to switch the TRIAC on at the exact calculated time slice.
