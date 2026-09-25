# ⚡ 5 V → 7 V High-Frequency Boost Converter

<div align="center">

### Mathematical Design • Python Analysis • KiCad • SPICE • PWM Generation • Parametric Analysis • Hardware Development

![Power Electronics](https://img.shields.io/badge/Domain-Power%20Electronics-blue?style=for-the-badge)
![Input](https://img.shields.io/badge/Input-5V-success?style=for-the-badge)
![Target](https://img.shields.io/badge/Target%20Output-7V-orange?style=for-the-badge)
![Switching](https://img.shields.io/badge/Switching-100kHz-red?style=for-the-badge)

</div>

---

## 📌 Project Overview

This project presents the mathematical design, Python-based analysis, KiCad schematic development, SPICE simulation, PWM generation, parametric analysis, and hardware development of a 5 V to 7 V high-frequency DC-DC boost converter.

The current design target is:

```text
Input Voltage       : 5 V DC
Target Output       : 7 V DC
Switching Frequency : 100 kHz
Ideal Duty Cycle    : 28.57%
Practical Estimate  : ≈35.06%
Simulation Duty     : 36%
Inductor            : 100 µH
Simulation Load     : 24 Ω
Hardware Load       : 27 Ω / 10 W
# 📐 Complete Design Calculations

## 1. Ideal Boost Converter Equation

For an ideal boost converter operating in continuous conduction mode:

$$
V_o = \frac{V_{in}}{1-D}
$$

where:

- $V_o$ = output voltage
- $V_{in}$ = input voltage
- $D$ = duty cycle

Rearranging:

$$
D = 1 - \frac{V_{in}}{V_o}
$$

---

## 2. Ideal Duty Cycle for 5 V → 7 V

Given:

$$
V_{in} = 5\text{ V}
$$

$$
V_o = 7\text{ V}
$$

Therefore:

$$
D = 1 - \frac{5}{7}
$$

$$
D = 1 - 0.7142857
$$

$$
D = 0.285714
$$

Therefore:

$$
D_{ideal} = 28.57\%
$$

---

## 3. Practical Duty Cycle

A real boost converter has losses caused by the diode, MOSFET, inductor resistance, capacitor ESR, switching losses, and other parasitic effects.

For a simplified calculation, assume a diode forward voltage of:

$$
V_D = 0.7\text{ V}
$$

The approximate practical boost-converter relationship is:

$$
V_o \approx \frac{V_{in}}{1-D} - V_D
$$

Rearranging:

$$
D \approx 1 - \frac{V_{in}}{V_o + V_D}
$$

Substituting:

$$
D \approx 1 - \frac{5}{7 + 0.7}
$$

$$
D \approx 1 - \frac{5}{7.7}
$$

$$
D \approx 0.35065
$$

Therefore:

$$
D_{practical} \approx 35.06\%
$$

The KiCad/SPICE simulation uses:

$$
D_{simulation} = 36\%
$$

The 36% value is close to the simplified practical estimate and is used as the simulation operating point.

---

## 4. Switching Frequency

The selected switching frequency is:

$$
f_s = 100\text{ kHz}
$$

Therefore:

$$
f_s = 100000\text{ Hz}
$$

The switching period is:

$$
T = \frac{1}{f_s}
$$

$$
T = \frac{1}{100000}
$$

$$
T = 10 \times 10^{-6}\text{ s}
$$

Therefore:

$$
T = 10\ \mu\text{s}
$$

---

## 5. PWM ON Time

For a 36% duty cycle:

$$
T_{ON} = DT
$$

$$
T_{ON} = 0.36 \times 10\ \mu\text{s}
$$

Therefore:

$$
T_{ON} = 3.6\ \mu\text{s}
$$

---

## 6. PWM OFF Time

The OFF time is:

$$
T_{OFF} = T - T_{ON}
$$

$$
T_{OFF} = 10 - 3.6
$$

Therefore:

$$
T_{OFF} = 6.4\ \mu\text{s}
$$

Alternatively:

$$
T_{OFF} = (1-D)T
$$

---

## 7. PWM Parameters Used in SPICE

The SPICE pulse source is configured approximately as:

```text
V1  = 0 V
V2  = 5 V
td  = 0
tr  = 10 ns
tf  = 10 ns
tw  = 3.6 µs
per = 10 µs
```

The duty cycle is:

$$
D = \frac{T_{ON}}{T}
$$

$$
D = \frac{3.6}{10}
$$

$$
D = 0.36
$$

Therefore:

$$
D = 36\%
$$

---

## 8. Output Current

For the simulation load:

$$
R_{load} = 24\ \Omega
$$

At the target output voltage:

$$
V_o = 7\text{ V}
$$

Using Ohm's law:

$$
I_o = \frac{V_o}{R_{load}}
$$

$$
I_o = \frac{7}{24}
$$

Therefore:

$$
I_o \approx 0.292\text{ A}
$$

---

## 9. Output Power

Output power is:

$$
P_o = V_o I_o
$$

Substituting:

$$
P_o = 7 \times 0.292
$$

Therefore:

$$
P_o \approx 2.04\text{ W}
$$

Alternatively:

$$
P_o = \frac{V_o^2}{R_{load}}
$$

$$
P_o = \frac{7^2}{24}
$$

$$
P_o \approx 2.04\text{ W}
$$

---

## 10. Approximate Input Current

Ignoring converter losses:

$$
P_{in} \approx P_o
$$

Therefore:

$$
I_{in} \approx \frac{P_o}{V_{in}}
$$

$$
I_{in} \approx \frac{2.04}{5}
$$

Therefore:

$$
I_{in} \approx 0.408\text{ A}
$$

The actual hardware input current will be higher when converter losses are included.

---

## 11. Inductor Current Ripple

The approximate inductor-current ripple is:

$$
\Delta I_L =
\frac{V_{in}D}{Lf_s}
$$

Given:

$$
V_{in}=5\text{ V}
$$

$$
D=0.36
$$

$$
L=100\ \mu\text{H}
$$

$$
f_s=100\text{ kHz}
$$

Therefore:

$$
\Delta I_L =
\frac{5\times0.36}
{(100\times10^{-6})(100000)}
$$

$$
\Delta I_L = 0.18\text{ A}
$$

Therefore:

$$
\Delta I_L \approx 0.18\text{ A}
$$

---

## 12. Average Inductor Current

For an idealized boost converter:

$$
I_{L,avg} \approx I_{in}
$$

Using the calculated input current:

$$
I_{L,avg} \approx 0.408\text{ A}
$$

---

## 13. Peak Inductor Current

The approximate peak inductor current is:

$$
I_{L,peak}
=
I_{L,avg}
+
\frac{\Delta I_L}{2}
$$

Substituting:

$$
I_{L,peak}
=
0.408
+
\frac{0.18}{2}
$$

$$
I_{L,peak}
=
0.498\text{ A}
$$

Therefore:

$$
I_{L,peak} \approx 0.498\text{ A}
$$

---

## 14. Minimum Inductor Current

The minimum inductor current is:

$$
I_{L,min}
=
I_{L,avg}
-
\frac{\Delta I_L}{2}
$$

Substituting:

$$
I_{L,min}
=
0.408
-
\frac{0.18}{2}
$$

$$
I_{L,min}
=
0.318\text{ A}
$$

Therefore:

$$
I_{L,min} \approx 0.318\text{ A}
$$

Since:

$$
I_{L,min} > 0
$$

the simplified calculation indicates continuous-conduction operation for this operating point.

---

## 15. Inductor Sizing

The basic inductor-sizing equation is:

$$
L =
\frac{V_{in}D}
{f_s\Delta I_L}
$$

Assume an allowable inductor-current ripple of approximately 30% of the average input current:

$$
\Delta I_L \approx 0.3I_{in}
$$

Using:

$$
I_{in}\approx0.408\text{ A}
$$

we obtain:

$$
\Delta I_L \approx 0.3\times0.408
$$

$$
\Delta I_L \approx 0.122\text{ A}
$$

Using the practical duty cycle:

$$
D\approx0.3506
$$

the required inductance is approximately:

$$
L =
\frac{5\times0.3506}
{100000\times0.122}
$$

$$
L \approx 144\ \mu\text{H}
$$

A practical standard value would therefore be approximately:

$$
L \approx 150\ \mu\text{H}
$$

However, the current KiCad simulation uses:

$$
L = 100\ \mu\text{H}
$$

The 100 µH value is therefore documented as the actual simulation value, while approximately 150 µH is the value obtained from this particular 30% ripple design criterion.

---

## 16. Output Capacitor Ripple

The simplified output-voltage ripple equation is:

$$
\Delta V_o
\approx
\frac{I_oD}{f_sC}
$$

For the simulation:

$$
I_o \approx 0.292\text{ A}
$$

$$
D=0.36
$$

$$
f_s=100\text{ kHz}
$$

$$
C=1000\ \mu\text{F}
$$

Therefore:

$$
\Delta V_o
\approx
\frac{0.292\times0.36}
{100000\times1000\times10^{-6}}
$$

$$
\Delta V_o
\approx1.05\text{ mV}
$$

This is an idealized capacitive-ripple estimate. Actual SPICE and hardware ripple can differ because of ESR, ESL, switching behavior, diode characteristics, MOSFET characteristics, and PCB parasitics.

---

## 17. Capacitor Sizing

The approximate capacitor-sizing equation is:

$$
C \approx
\frac{I_oD}
{f_s\Delta V_o}
$$

For a 1% output-voltage ripple target:

$$
\Delta V_o = 0.01\times7
$$

$$
\Delta V_o = 0.07\text{ V}
$$

Therefore:

$$
C \approx
\frac{0.292\times0.36}
{100000\times0.07}
$$

$$
C \approx15\ \mu\text{F}
$$

A larger practical capacitor can be selected to provide additional ripple margin.

The planned hardware capacitor is:

```text
220 µF / 35 V
```

---

## 18. Critical Inductance

The approximate critical inductance for the boundary between continuous and discontinuous conduction is:

$$
L_{crit}
\approx
\frac{D(1-D)^2R}
{2f_s}
$$

For:

$$
D=0.36
$$

$$
R=24\ \Omega
$$

$$
f_s=100\text{ kHz}
$$

Therefore:

$$
L_{crit}
\approx
\frac{0.36(1-0.36)^2(24)}
{2(100000)}
$$

$$
L_{crit}\approx17.7\ \mu\text{H}
$$

The selected simulation inductance is:

$$
L=100\ \mu\text{H}
$$

Therefore:

$$
100\ \mu\text{H} > 17.7\ \mu\text{H}
$$

The simplified calculation therefore indicates CCM operation at this design point.

---

## 19. Voltage Gain

The required voltage gain is:

$$
M=\frac{V_o}{V_{in}}
$$

$$
M=\frac{7}{5}
$$

Therefore:

$$
M=1.4
$$

The converter therefore requires a voltage gain of approximately 1.4.

---

## 20. Hardware Load Calculation

The planned hardware load is:

$$
R_{load}=27\ \Omega
$$

At 7 V:

$$
I_o=\frac{7}{27}
$$

$$
I_o\approx0.259\text{ A}
$$

The load power is:

$$
P_o=\frac{7^2}{27}
$$

$$
P_o\approx1.81\text{ W}
$$

The selected resistor is:

```text
27 Ω / 10 W
```

---

## 21. Hardware Load at 12 V

For an additional 12 V operating-point study:

$$
I_o=\frac{12}{27}
$$

$$
I_o\approx0.444\text{ A}
$$

The load power becomes:

$$
P_o=\frac{12^2}{27}
$$

$$
P_o\approx5.33\text{ W}
$$

The 27 Ω / 10 W resistor can therefore become hot at this operating point and must be handled accordingly.

---

## 22. Efficiency

Converter efficiency is calculated as:

$$
\eta =
\frac{P_{out}}
{P_{in}}
\times100
$$

where:

$$
P_{out}=V_oI_o
$$

and:

$$
P_{in}=V_{in}I_{in}
$$

Therefore:

$$
\eta =
\frac{V_oI_o}
{V_{in}I_{in}}
\times100
$$

An experimental efficiency value will only be reported after actual input and output power measurements are obtained.

---

## 23. Percentage Error

The difference between theoretical and simulation results can be calculated using:

$$
Error(\%) =
\frac{
|V_{theory}-V_{simulation}|
}
{V_{theory}}
\times100
$$

For example, in Python:

```python
error_percent = (
    abs(V_theory - V_simulation)
    / V_theory
) * 100
```

---

# 🎛️ PWM Generator

The PWM signal controls the MOSFET switching.

Target PWM:

```text
Frequency : 100 kHz
Period    : 10 µs
HIGH      : 5 V
LOW       : 0 V
Duty      : approximately 36%
```

The Arduino UNO uses the ATmega328P 16 MHz system clock.

For Timer1 Fast PWM with ICR1 as TOP:

$$
f_{PWM}
=
\frac{F_{CPU}}
{N(1+ICR1)}
$$

For:

$$
F_{CPU}=16\text{ MHz}
$$

and:

$$
N=1
$$

For 100 kHz:

$$
100000
=
\frac{16000000}
{1+ICR1}
$$

Therefore:

$$
1+ICR1=160
$$

and:

$$
ICR1=159
$$

For approximately 36% duty:

$$
OCR1A\approx0.36(160)-1
$$

$$
OCR1A\approx56.6
$$

An integer value of approximately:

$$
OCR1A=57
$$

can be used.

---

# 💻 Arduino UNO 100 kHz PWM Code

```cpp
/*
  5 V -> 7 V Boost Converter
  Arduino UNO 100 kHz PWM

  PWM output:
  Pin 9 (OC1A)
*/

void setup()
{
  pinMode(9, OUTPUT);

  // Reset Timer1
  TCCR1A = 0;
  TCCR1B = 0;

  // Fast PWM, TOP = ICR1
  TCCR1A |= (1 << COM1A1);
  TCCR1A |= (1 << WGM11);

  TCCR1B |= (1 << WGM13);
  TCCR1B |= (1 << WGM12);

  // No prescaler
  TCCR1B |= (1 << CS10);

  // 100 kHz PWM
  ICR1 = 159;

  // Approximately 36% duty cycle
  OCR1A = 57;
}

void loop()
{
  // Timer1 generates PWM automatically.
}
```

---

# 📊 Theory / Simulation / Hardware Separation

This project maintains a strict distinction between:

| Category | Meaning |
|---|---|
| Theory | Equations and analytical calculations |
| Python | Mathematical computational model |
| SPICE | Circuit-level simulation |
| Wokwi | Digital PWM verification |
| Hardware | Physical implementation |
| Experimental | Measurements from physical hardware |

No hardware measurement is presented as a simulation result, and no simulation result is presented as an experimental measurement.
---

# 🐍 Python-Based Analysis

Python is used as an analytical and computational environment for studying the boost converter before circuit-level simulation and hardware testing.

The Python model is used to:

- Generate PWM waveforms
- Calculate boost-converter parameters
- Calculate output voltage
- Calculate output current
- Calculate output power
- Calculate inductor-current ripple
- Calculate peak and minimum inductor current
- Estimate capacitor voltage ripple
- Calculate critical inductance
- Perform parameter sweeps
- Generate plots for analysis
- Create theoretical datasets for further study

The Python calculations are analytical and should not be interpreted as measured hardware results.

---

## 📈 Python PWM Simulation

The following Python program generates a 100 kHz PWM waveform with approximately 36% duty cycle.

```python
import numpy as np
import matplotlib.pyplot as plt

# ==============================
# PWM PARAMETERS
# ==============================

frequency = 100_000       # 100 kHz
duty_cycle = 36           # 36%
V_high = 5                # HIGH voltage
V_low = 0                 # LOW voltage
cycles = 5

# ==============================
# CALCULATIONS
# ==============================

T = 1 / frequency
D = duty_cycle / 100

Ton = D * T
Toff = T - Ton

print("========== PWM PARAMETERS ==========")
print(f"Frequency  : {frequency/1000:.2f} kHz")
print(f"Period     : {T*1e6:.3f} µs")
print(f"Duty Cycle : {duty_cycle:.2f} %")
print(f"Ton        : {Ton*1e6:.3f} µs")
print(f"Toff       : {Toff*1e6:.3f} µs")

# ==============================
# TIME VECTOR
# ==============================

samples_per_cycle = 1000
total_samples = cycles * samples_per_cycle

t = np.linspace(
    0,
    cycles * T,
    total_samples,
    endpoint=False
)

# ==============================
# PWM GENERATION
# ==============================

pwm = np.where(
    (t % T) < Ton,
    V_high,
    V_low
)

# ==============================
# PLOT
# ==============================

plt.figure(figsize=(12, 4))

plt.plot(
    t * 1e6,
    pwm,
    drawstyle="steps-post"
)

plt.xlabel("Time (µs)")
plt.ylabel("Voltage (V)")

plt.title(
    f"PWM Signal - "
    f"{frequency/1000:.0f} kHz, "
    f"{duty_cycle:.1f}% Duty Cycle"
)

plt.ylim(-0.5, 5.5)
plt.grid(True)
plt.show()
```

### Expected PWM parameters

```text
Frequency : 100 kHz
Period    : 10 µs
Duty      : 36%
Ton       : 3.6 µs
Toff      : 6.4 µs
HIGH      : 5 V
LOW       : 0 V
```

---

# 🧮 Python Boost Converter Calculation

The following Python model calculates the main operating parameters.

```python
# ==============================
# BOOST CONVERTER PARAMETERS
# ==============================

Vin = 5.0
Vf = 0.7

L = 100e-6
C = 220e-6

Rload = 24
frequency = 100_000
duty_cycle = 36

D = duty_cycle / 100

# ==============================
# OUTPUT VOLTAGE
# ==============================

Vout_ideal = Vin / (1 - D)

Vout_approx = Vout_ideal - Vf

# ==============================
# OUTPUT CURRENT
# ==============================

Iout = Vout_approx / Rload

# ==============================
# OUTPUT POWER
# ==============================

Pout = Vout_approx * Iout

# ==============================
# INPUT CURRENT
# ==============================

Iin = Pout / Vin

# ==============================
# INDUCTOR RIPPLE
# ==============================

Delta_IL = (
    Vin * D
) / (
    L * frequency
)

# ==============================
# PEAK AND MINIMUM INDUCTOR CURRENT
# ==============================

IL_peak = Iin + Delta_IL / 2

IL_min = Iin - Delta_IL / 2

# ==============================
# OUTPUT RIPPLE
# ==============================

Delta_Vout = (
    Iout * D
) / (
    frequency * C
)

# ==============================
# DISPLAY RESULTS
# ==============================

print("========== BOOST CONVERTER ==========")

print(f"Input Voltage        : {Vin:.2f} V")
print(f"Duty Cycle           : {duty_cycle:.2f} %")
print(f"Switching Frequency  : {frequency/1000:.2f} kHz")

print(f"Ideal Output Voltage : {Vout_ideal:.3f} V")
print(f"Approx. Output       : {Vout_approx:.3f} V")

print(f"Output Current       : {Iout:.3f} A")
print(f"Output Power         : {Pout:.3f} W")

print(f"Input Current        : {Iin:.3f} A")

print(f"Inductor Ripple      : {Delta_IL:.3f} A")
print(f"Peak Inductor Current: {IL_peak:.3f} A")
print(f"Minimum Inductor     : {IL_min:.3f} A")

print(f"Output Ripple        : {Delta_Vout:.6f} V")
```

---

# 📊 Parametric Analysis

One of the main objectives of this project is to study how different converter parameters influence its behavior.

The following parameters can be varied:

| Parameter | Values |
|---|---|
| Input voltage | 5 V |
| Duty cycle | 20–60% |
| Switching frequency | 50–200 kHz |
| Inductance | 47–330 µH |
| Capacitance | 47–470 µF |
| Load resistance | 10–68 Ω |

The main output parameters are:

- Output voltage
- Output current
- Output power
- Inductor-current ripple
- Output-voltage ripple
- Peak inductor current
- Minimum inductor current
- Conduction mode

---

# 🔄 Duty Cycle Sweep

The duty cycle can be varied while keeping the other parameters constant.

Example values:

```text
20%
25%
30%
35%
36%
40%
45%
50%
55%
60%
```

The theoretical relationship is:

$$
V_o = \frac{V_{in}}{1-D}
$$

This sweep can be used to study the relationship between PWM duty cycle and output voltage.

### Objective

Determine how accurately the simulated converter follows the theoretical boost-converter relationship.

---

# ⚡ Switching Frequency Sweep

Example frequencies:

```text
50 kHz
75 kHz
100 kHz
150 kHz
200 kHz
```

The switching frequency affects:

- Inductor ripple
- Capacitor ripple
- Switching losses
- Component stress
- Filter requirements

For a fixed duty cycle and inductance:

$$
\Delta I_L =
\frac{V_{in}D}
{Lf_s}
$$

Therefore, increasing switching frequency generally reduces the calculated inductor-current ripple.

---

# 🌀 Inductance Sweep

Example values:

```text
47 µH
68 µH
100 µH
150 µH
220 µH
330 µH
```

The inductor ripple relationship is:

$$
\Delta I_L =
\frac{V_{in}D}
{Lf_s}
$$

Therefore:

$$
\Delta I_L \propto \frac{1}{L}
$$

Increasing inductance reduces the theoretical current ripple for the same operating point.

---

# 🔋 Capacitance Sweep

Example values:

```text
47 µF
100 µF
220 µF
470 µF
```

The simplified output ripple relationship is:

$$
\Delta V_o
\approx
\frac{I_oD}
{f_sC}
$$

Therefore:

$$
\Delta V_o \propto \frac{1}{C}
$$

Increasing capacitance generally reduces the calculated capacitive component of output-voltage ripple.

Actual ripple also depends on capacitor ESR, ESL, switching behavior, diode characteristics, MOSFET characteristics, and layout parasitics.

---

# 🔌 Load Resistance Sweep

Example values:

```text
10 Ω
15 Ω
22 Ω
27 Ω
33 Ω
47 Ω
68 Ω
```

For a resistive load:

$$
I_o = \frac{V_o}{R}
$$

and:

$$
P_o = \frac{V_o^2}{R}
$$

Changing the load allows the converter to be evaluated under different output-current conditions.

---

# 📐 Theoretical vs Simulation Analysis

Theoretical calculations provide an idealized reference.

KiCad/SPICE provides a circuit-level simulation that includes the modeled behavior of the components.

The comparison can be expressed as:

$$
Error(\%)
=
\frac{
|V_{theory}-V_{simulation}|
}
{V_{theory}}
\times100
$$

The same method can be applied to other parameters such as:

- Output voltage
- Inductor current
- Ripple voltage
- Ripple current

Theoretical and SPICE results should be clearly identified separately.

---

# 🖥️ KiCad / SPICE Simulation

The converter is modeled in KiCad using a switching power-stage topology.

### Main simulation components

```text
Input source       : 5 V DC
Inductor           : 100 µH
MOSFET             : IRLZ44N / NMOS model
Diode              : Schottky diode
Output capacitor   : 220 µF
Simulation load    : 24 Ω
PWM source         : 0–5 V
Switching frequency: 100 kHz
Duty cycle         : 36%
```

### Simulation topology

```text
                 L1
5 V ─────────── 100 µH ────────●──────|>|────── +VOUT
                               │        D1
                               │
                              D
                           Q1 NMOS
                              S
                               │
                              GND

+VOUT ───────────── C1 ───────────── GND
             220 µF

+VOUT ─────────── RLOAD ─────────── GND
                  24 Ω

PWM ───────────── Gate
```

The schematic is maintained in the KiCad project files.

---

# 🔬 Simulation Measurements

The following waveforms can be examined in KiCad SPICE:

- PWM gate voltage
- MOSFET drain voltage
- MOSFET current
- Inductor current
- Diode current
- Capacitor current
- Output voltage
- Load current
- Input current

The transient response can also be used to study:

- Startup overshoot
- Startup undershoot
- Steady-state output voltage
- Switching behavior
- Ripple
- Current transients

---

# 📡 PWM Verification Using Wokwi

The Arduino PWM generation was also verified using a virtual Arduino environment.

The verification setup uses:

```text
Arduino UNO
     │
     │ Pin 9 / OC1A
     ▼
Logic Analyzer
```

The generated PWM target is approximately:

```text
Frequency : 100 kHz
Duty      : ≈36%
Voltage   : 0–5 V
```

The digital waveform can be exported and analyzed using logic-analyzer software such as PulseView.

This provides a software-based verification method before connecting the Arduino to the physical power stage.

---

# 🧪 Hardware Development

The physical prototype is being developed separately from the simulation.

### Planned hardware components

| Component | Value / Part |
|---|---|
| Input supply | 5 V / 2 A |
| MOSFET | IRLZ44N |
| Inductor | 100 µH / approximately 2 A |
| Diode | 1N5822 Schottky |
| Output capacitor | 220 µF / 35 V |
| Load resistor | 27 Ω / 10 W |
| Gate resistor | 10 Ω |
| Gate pulldown | 10 kΩ |
| Controller | Arduino UNO |

The physical implementation will be tested initially at the lower-voltage 5 V → 7 V operating point.

The 5 V → 12 V operating point can then be investigated after the lower-voltage operation is verified.

---

# ⚠️ Hardware Limitations

The breadboard implementation is intended as a prototype.

At 100 kHz, parasitic effects can become significant.

Important practical effects include:

- MOSFET switching losses
- MOSFET RDS(on)
- Diode forward voltage
- Diode reverse recovery
- Inductor DCR
- Inductor saturation
- Capacitor ESR
- Capacitor ESL
- Breadboard parasitic capacitance
- Wiring inductance
- Gate-drive limitations
- Switching-node ringing
- Thermal effects

Therefore, the physical results may differ from the ideal calculations and SPICE model.

---

# 🔥 Thermal Considerations

At the 12 V operating point using a 27 Ω load:

$$
P_{load}
=
\frac{12^2}{27}
$$

$$
P_{load}\approx5.33\text{ W}
$$

The 10 W resistor therefore dissipates significant heat.

The resistor should be positioned away from heat-sensitive components and should not be touched during operation.

The MOSFET may also require thermal management depending on switching losses and operating conditions.

---

# 🛡️ Safety

This project is designed around a low-voltage DC input.

The converter must **not** be connected directly to household AC mains.

The initial hardware test should use a regulated 5 V DC source.

Before applying power:

- Verify MOSFET pin connections.
- Verify diode polarity.
- Verify capacitor polarity.
- Verify input polarity.
- Verify the load resistance.
- Verify Arduino and power-stage ground.
- Check for input-to-ground shorts.
- Check for output-to-ground shorts.
- Verify the PWM signal before connecting it to the MOSFET gate.
- Start with the 5 V → 7 V operating point.

---

# 🧠 Why 5 V → 7 V Was Selected

The 5 V → 7 V conversion provides a relatively low conversion ratio while still demonstrating the fundamental operation of a boost converter.

It allows the project to investigate:

```text
PWM control
      ↓
MOSFET switching
      ↓
Inductor energy storage
      ↓
Diode energy transfer
      ↓
Capacitor filtering
      ↓
Boosted DC output
```

The same power-stage concept can subsequently be evaluated for a higher output such as 12 V.

---

# 🚀 Future Scope

The current project provides a foundation for several improvements.

## 1. Closed-Loop Voltage Regulation

The current design primarily uses fixed-duty PWM.

A future version can measure the output voltage and automatically adjust the PWM duty cycle.

```text
VOUT
 │
 ▼
Voltage Sensor
 │
 ▼
Controller
 │
 ▼
PWM
 │
 ▼
MOSFET
 │
 ▼
Boost Converter
 │
 └─────────────── feedback ────────────────┘
```

This would allow the converter to compensate for changes in input voltage and load.

---

## 2. PID Control

A PID controller can be investigated for closed-loop voltage regulation.

The controller can use:

$$
e(t)=V_{ref}-V_o(t)
$$

where:

- $V_{ref}$ = desired output voltage
- $V_o$ = measured output voltage
- $e(t)$ = voltage error

The PWM duty cycle can then be adjusted according to the control error.

---

## 3. Soft-Start

A soft-start mechanism can gradually increase the duty cycle during startup.

This can reduce:

- Startup current
- Output-voltage overshoot
- Component stress

---

## 4. Gate Driver

A dedicated MOSFET gate driver can be investigated instead of driving the MOSFET directly from the Arduino.

A gate driver can provide:

- Higher peak gate current
- Faster switching
- Better MOSFET turn-on
- Better MOSFET turn-off
- Reduced switching losses

---

## 5. PCB Implementation

The breadboard prototype can eventually be converted into a dedicated PCB.

The PCB design can focus on:

- Short switching loops
- Low parasitic inductance
- Proper grounding
- Thermal management
- Decoupling
- EMI reduction
- Component placement

---

## 6. Efficiency Optimization

Future analysis can investigate:

$$
\eta =
\frac{P_{out}}
{P_{in}}
\times100
$$

The effect of:

- MOSFET selection
- Diode selection
- Switching frequency
- Inductor DCR
- Gate-drive losses
- Load current

can be studied.

---

## 7. Automatic Parameter Optimization

Python can be extended to search for suitable combinations of:

```text
Duty cycle
Inductance
Capacitance
Switching frequency
Load resistance
```

The objective can be to minimize:

```text
Output ripple
Inductor ripple
Power loss
```

while maintaining the required output voltage.

---

## 8. Machine-Learning-Assisted Optimization

A future research extension can generate large numbers of simulation cases and use them to train a machine-learning model.

Possible inputs:

```text
Vin
Duty cycle
Switching frequency
Inductance
Capacitance
Load resistance
```

Possible predictions:

```text
Vout
Output ripple
Inductor ripple
Efficiency
Peak current
```

The ML model can then be investigated as a fast surrogate for repeated converter simulations.

---

## 9. Automated SPICE Dataset Generation

Python can automatically generate many converter operating points.

For example:

```text
Duty cycle
Frequency
Inductance
Capacitance
Load
```

can be swept automatically.

The resulting dataset can be used for:

- Statistical analysis
- Parameter optimization
- Machine-learning experiments
- Theoretical comparison
- Design-space exploration

---

# 📁 Project Structure

```text
5V-to-7V-Boost-Converter/
│
├── README.md
│
├── KiCad/
│   └── Exp_1/
│       ├── Exp_1.kicad_pro
│       ├── Exp_1.kicad_sch
│       └── Exp_1.kicad_pcb
│
├── Arduino/
│   └── 100kHz_PWM/
│       └── pwm_100khz.ino
│
├── Python/
│   ├── pwm_simulation.py
│   ├── boost_converter_analysis.py
│   └── parametric_analysis.py
│
├── Wokwi/
│   ├── diagram.json
│   ├── wokwi.toml
│   └── pwm_test.ino
│
├── Data/
│   └── boost_converter_theoretical_dataset.csv
│
├── Results/
│   ├── KiCad/
│   ├── Wokwi/
│   └── Python/
│
├── Documentation/
│   └── design_calculations.pdf
│
└── LICENSE
```

---

# 📋 Current Project Status

| Stage | Status |
|---|---|
| Converter topology | ✅ Completed |
| Mathematical design | ✅ Completed |
| Component calculations | ✅ Completed |
| Python PWM model | ✅ Completed |
| Python analytical model | ✅ Completed |
| KiCad schematic | ✅ Completed |
| KiCad SPICE simulation | ✅ Completed |
| Arduino PWM generation | ✅ Developed |
| Wokwi PWM verification | ✅ Completed |
| Parametric analysis | 🔄 In progress |
| Physical prototype | 🔄 In progress |
| Hardware measurements | ⏳ Pending |
| Theory vs hardware comparison | ⏳ Pending |
| Efficiency measurement | ⏳ Pending |
| Closed-loop control | 🔮 Future |
| PCB optimization | 🔮 Future |
| ML-based optimization | 🔮 Future |

---

# 📊 Planned Results

The following results will be added after the corresponding simulations or measurements are completed:

```text
1. Output Voltage vs Duty Cycle

2. Output Ripple vs Capacitance

3. Inductor Ripple vs Inductance

4. Output Voltage vs Switching Frequency

5. Output Voltage vs Load

6. Inductor Current Waveform

7. MOSFET Gate PWM Waveform

8. Output Voltage Transient Response

9. Theoretical vs SPICE Simulation

10. Simulation vs Hardware

11. Efficiency vs Load

12. Input Current vs Output Power
```

Only completed and verified plots should be placed in the final results section.

---

# 📌 Important Engineering Distinction

This repository contains multiple levels of analysis.

### Mathematical Model

Uses ideal or simplified equations.

### Python Model

Uses numerical calculations and analytical equations.

### SPICE Model

Uses circuit-level simulation with component models.

### Wokwi

Used for digital PWM verification.

### Hardware

Represents the physical converter and requires actual measurements.

These levels are kept separate to avoid presenting theoretical or simulated values as experimental measurements.

---

# 📚 Applications

A low-voltage boost converter can be used in applications such as:

- Battery-powered electronics
- Embedded systems
- Portable electronics
- Sensor systems
- Low-voltage DC power supplies
- Microcontroller-based power systems
- Energy harvesting systems
- Power-management circuits

---

# 🎯 Project Objectives

The main objectives are:

1. Design a 5 V to 7 V boost converter.
2. Derive the required duty cycle analytically.
3. Select suitable inductor and capacitor values.
4. Analyze current and voltage ripple.
5. Develop the circuit in KiCad.
6. Perform SPICE transient simulation.
7. Generate 100 kHz PWM using an Arduino UNO.
8. Verify PWM digitally using Wokwi.
9. Perform Python-based parameter analysis.
10. Compare theoretical and simulation results.
11. Develop a physical prototype.
12. Investigate efficiency and practical losses.
13. Provide a foundation for closed-loop control and optimization.

---

# 📝 Limitations

The current design has several limitations:

- The initial control strategy is open-loop PWM.
- The analytical model uses simplified component assumptions.
- Real component parasitics are not fully represented by simple equations.
- Hardware measurements are required for experimental validation.
- Breadboard parasitics can influence high-frequency switching behavior.
- Converter efficiency cannot be established from theoretical calculations alone.
- A dedicated gate driver is not included in the initial prototype.
- Closed-loop regulation is not yet implemented.

---

# 🔬 Research Extension

The project can be extended from a conventional converter-design project into a systematic research study by investigating the relationship between converter parameters and performance.

A possible research workflow is:

```text
Mathematical Model
        ↓
Python Parameter Sweep
        ↓
SPICE Simulation
        ↓
Simulation Dataset
        ↓
Statistical / ML Analysis
        ↓
Parameter Optimization
        ↓
Hardware Validation
        ↓
Theory vs Simulation vs Hardware
```

This provides a structured path from theoretical design to computational analysis and experimental validation.

---

# 🏁 Conclusion

This project presents the design and analysis of a high-frequency 5 V to 7 V boost converter operating at approximately 100 kHz.

The design process includes:

```text
Theory
  ↓
Component Calculations
  ↓
Python Analysis
  ↓
KiCad Schematic
  ↓
SPICE Simulation
  ↓
Arduino PWM
  ↓
Wokwi Verification
  ↓
Hardware Prototype
  ↓
Experimental Validation
```

The analytical design establishes the expected converter behavior, while KiCad/SPICE provides circuit-level simulation. Python provides a flexible environment for parameter sweeps and data analysis, and Arduino/Wokwi provides a method for developing and verifying the PWM control signal.

Future development can focus on closed-loop voltage regulation, dedicated gate driving, PCB implementation, efficiency optimization, automated SPICE dataset generation, and machine-learning-assisted converter optimization.

---

# 👨‍💻 Author

**Sai Nandan**

Electronics and Communication Engineering

This repository documents the development process, calculations, simulations, software models, and future hardware validation of the boost-converter project.

---

# ⭐ Acknowledgment

This project is developed as an academic engineering project for studying:

- Power electronics
- DC-DC converters
- PWM control
- SPICE simulation
- Embedded systems
- Numerical analysis
- Parameter optimization

---

# 📜 License

This project is intended for educational and research purposes.

You may study, modify, and extend the implementation with appropriate attribution.