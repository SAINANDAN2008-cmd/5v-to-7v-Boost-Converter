<div align="center">

# ⚡ 5 V → 7 V High-Frequency Boost Converter

### Design • Simulation • Parametric Analysis • PWM Control • Hardware Development

<p>
  <img src="https://img.shields.io/badge/Power%20Electronics-Boost%20Converter-blue?style=for-the-badge">
  <img src="https://img.shields.io/badge/Input-5V-success?style=for-the-badge">
  <img src="https://img.shields.io/badge/Target%20Output-7V-orange?style=for-the-badge">
  <img src="https://img.shields.io/badge/Switching-100%20kHz-red?style=for-the-badge">
</p>

<p>
  <img src="https://img.shields.io/badge/KiCad-10.0-blue?style=flat-square&logo=kicad">
  <img src="https://img.shields.io/badge/Python-3.x-blue?style=flat-square&logo=python">
  <img src="https://img.shields.io/badge/Arduino-UNO-00979D?style=flat-square&logo=arduino">
  <img src="https://img.shields.io/badge/SPICE-Simulation-purple?style=flat-square">
  <img src="https://img.shields.io/badge/Wokwi-PWM%20Verification-green?style=flat-square">
</p>

<br>

**A complete engineering study of a high-frequency DC-DC boost converter designed to step up a 5 V DC input toward a 7 V output.**

</div>

---

# 📌 Project Overview

This project focuses on the design, simulation, analysis, and hardware development of a **5 V to 7 V high-frequency boost converter** operating at approximately **100 kHz**.

The project combines:

- Boost converter theory
- Mathematical design
- Component calculations
- KiCad schematic design
- SPICE transient simulation
- Python-based numerical analysis
- Parametric analysis
- Dataset generation
- Arduino UNO PWM generation
- Wokwi PWM verification
- Hardware component selection
- Future PCB design
- Future experimental validation

The main objective is to investigate how **duty cycle, switching frequency, inductance, capacitance, and load resistance** influence the behavior of a boost converter.

---

# 🎯 Project Objectives

- Design a 5 V to approximately 7 V boost converter.
- Operate the converter at approximately 100 kHz.
- Calculate the theoretical PWM duty cycle.
- Analyze practical duty-cycle requirements.
- Calculate load current and output power.
- Determine inductor current ripple.
- Estimate capacitor voltage ripple.
- Develop the circuit in KiCad.
- Perform transient SPICE simulation.
- Generate PWM using an Arduino UNO.
- Verify PWM digitally using Wokwi.
- Perform Python-based parameter sweeps.
- Generate a theoretical engineering dataset.
- Compare theoretical calculations with simulation results.
- Develop a PCB in a later stage.
- Validate the design experimentally in the future.

---

# ⚙️ Design Specifications

## Simulation Configuration

The current KiCad/SPICE schematic uses:

| Parameter | Value |
|---|---:|
| Input voltage | **5 V DC** |
| Target output | **7 V DC** |
| Switching frequency | **100 kHz** |
| Switching period | **10 µs** |
| PWM duty cycle | **36%** |
| ON time | **3.6 µs** |
| OFF time | **6.4 µs** |
| Inductor | **100 µH** |
| Output capacitor | **1000 µF** |
| Simulation load | **24 Ω** |
| PWM amplitude | **0–5 V** |
| Switching device | **NMOS SPICE model** |
| Diode | **SPICE diode model** |

---

## Hardware Prototype Configuration

The planned physical prototype uses:

| Parameter | Value |
|---|---:|
| Input voltage | **5 V DC** |
| Target output | **7 V DC** |
| Switching frequency | **100 kHz** |
| Initial duty cycle | **≈35–36%** |
| Inductor | **100 µH** |
| Output capacitor | **220 µF / 35 V** |
| Hardware load | **27 Ω / 10 W** |
| MOSFET | **IRLZ44N** |
| Diode | **1N5822** |
| PWM controller | **Arduino UNO** |
| Input supply | **5 V / 2 A regulated adapter** |

> The simulation and hardware configurations intentionally use different capacitor and load values. Simulation uses **1000 µF / 24 Ω**, while the physical prototype uses **220 µF / 27 Ω**.

---

# 🔄 Boost Converter Operating Principle

A boost converter increases DC voltage by storing energy in an inductor during the MOSFET ON interval and transferring that energy to the output during the MOSFET OFF interval.

```mermaid
flowchart LR
    A["5 V DC Input"] --> B["100 µH Inductor"]
    B --> C["Switching Node"]
    C --> D["Schottky / Diode"]
    D --> E["Output Capacitor"]
    E --> F["7 V Target"]
    F --> G["Load"]

    H["100 kHz PWM"] --> I["MOSFET"]
    I --> C
    I --> J["Ground"]