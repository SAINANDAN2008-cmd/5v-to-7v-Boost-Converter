⚡ 5 V → 7 V High-Frequency Boost Converter
Mathematical Design • Python Analysis • KiCad • SPICE • PWM Generation • Parametric Analysis • Hardware Development

![Power Electronics](https://img.shields.io/badge/Domain-Power%20Electronics-blue?style=for-the-badge)
![Input](https://img.shields.io/badge/Input-5V-success?style=for-the-badge)
![Output](https://img.shields.io/badge/Output-7V-orange?style=for-the-badge)
![Switching](https://img.shields.io/badge/Switching-100kHz-red?style=for-the-badge)

![KiCad](https://img.shields.io/badge/KiCad-10.0-blue?style=flat-square)
![Python](https://img.shields.io/badge/Python-3.x-yellow?style=flat-square)
![SPICE](https://img.shields.io/badge/SPICE-Simulation-purple?style=flat-square)
![Arduino](https://img.shields.io/badge/Arduino-UNO-00979D?style=flat-square)
![Wokwi](https://img.shields.io/badge/Wokwi-PWM%20Verification-green?style=flat-square)
![GitHub](https://img.shields.io/badge/GitHub-Version%20Control-black?style=flat-square)

---
📌 Project Overview
This project presents the design, mathematical analysis, simulation, parametric analysis, PWM generation, and hardware development of a 5 V to 7 V high-frequency DC-DC boost converter.
The converter uses approximately 100 kHz switching and PWM control to drive an N-channel MOSFET.
The project follows a complete engineering workflow:
```text
Theory
   ↓
Design Calculations
   ↓
Python Mathematical Model
   ↓
KiCad Schematic
   ↓
SPICE Simulation
   ↓
Parametric Analysis
   ↓
PWM Generation
   ↓
Wokwi Verification
   ↓
PCB Design
   ↓
Hardware Prototype
   ↓
Experimental Validation
```
The primary research question is:
> How do duty cycle, switching frequency, inductance, capacitance, and load resistance influence the output voltage, current ripple, output ripple, transient response, and power behavior of a high-frequency boost converter?
---
🎯 Project Objectives
The project aims to:
Design a 5 V to approximately 7 V boost converter.
Operate the converter at approximately 100 kHz.
Calculate the ideal duty cycle.
Calculate a practical duty cycle considering diode forward voltage.
Calculate switching period, ON time, and OFF time.
Determine suitable inductor and capacitor values.
Calculate output current and power.
Calculate input-current requirements.
Analyze inductor-current ripple.
Estimate output-voltage ripple.
Determine the approximate critical inductance.
Develop the converter schematic in KiCad.
Perform SPICE transient simulation.
Generate 100 kHz PWM using Arduino UNO.
Verify PWM behavior using Wokwi.
Generate analytical datasets using Python.
Perform parametric sweeps.
Compare theoretical and simulation results.
Develop a PCB in a later stage.
Build and experimentally validate the converter.
---
⚙️ System Specifications
Baseline Design
Parameter	Value
Input voltage	5 V DC
Target output voltage	7 V DC
Switching frequency	100 kHz
Switching period	10 µs
Practical duty cycle	≈35.06%
Simulation duty cycle	36%
Inductor	100 µH
Simulation capacitor	1000 µF
Simulation load	24 Ω
PWM amplitude	0–5 V
Switching device	NMOS
Diode model	SPICE diode
---
🔧 Hardware Configuration
The planned hardware prototype uses:
Component	Specification
Input supply	5 V / 2 A DC
MOSFET	IRLZ44N
Inductor	100 µH
Diode	1N5822
Output capacitor	220 µF / 35 V
Load resistor	27 Ω / 10 W
Gate resistor	10 Ω
Gate pulldown	10 kΩ
Controller	Arduino UNO
Switching frequency	100 kHz
Initial duty cycle	≈35–36%
Important distinction
The current KiCad simulation configuration and the planned hardware configuration are intentionally documented separately.
Simulation
```text
L = 100 µH
C = 1000 µF
R = 24 Ω
D = 36%
f = 100 kHz
```
Hardware
```text
L = 100 µH
C = 220 µF / 35 V
R = 27 Ω / 10 W
D ≈ 35–36%
f = 100 kHz
```
This prevents simulation parameters from being incorrectly presented as hardware measurements.
---
🔌 Boost Converter Circuit
The fundamental boost topology is:
```text
                         L1
                      100 µH
5 V DC ───────────────coil──────────●─────────|>|────────── +VOUT
                                    │           D1
                                    │         1N5822
                                    │
                                    D
                              ┌─────┴─────┐
                              │ IRLZ44N   │
                              │           │
Arduino PWM ── 10 Ω ──────────G           │
                              │           │
                         10 kΩ│           S
                              │           │
                              └───────────┴──────── GND
                                          │
                                         GND


                              +VOUT
                                │
                    ┌───────────┴───────────┐
                    │                       │
                220 µF                  27 Ω / 10 W
                 35 V                     LOAD
                    │                       │
                    └───────────┬───────────┘
                                │
                               GND
```
---
🔐 MOSFET Gate Driver Network
The Arduino does not connect directly to the MOSFET gate in the documented hardware configuration.
The gate network is:
```text
Arduino UNO PWM
       │
      10 Ω
       │
       ▼
IRLZ44N Gate
       │
      10 kΩ
       │
      GND
```
10 Ω Gate Resistor
The 10 Ω resistor is used to:
Limit instantaneous gate-current transients.
Reduce high-frequency ringing.
Reduce electromagnetic interference caused by very fast gate transitions.
10 kΩ Gate Pulldown
The 10 kΩ resistor ensures that:
```text
Arduino OFF / RESET
        ↓
Gate pulled LOW
        ↓
MOSFET OFF
```
This prevents the MOSFET from unintentionally turning ON when the Arduino is not actively driving the gate.
---
🔄 Operating Principle
A boost converter transfers energy through two main switching states.
MOSFET ON
When the MOSFET is ON:
```text
5 V → Inductor → MOSFET → GND
```
The inductor stores energy.
Approximately:
$$
V_L \approx V_{in}
$$
Therefore:
$$
\frac{dI_L}{dt}=\frac{V_{in}}{L}
$$
The inductor current increases during the ON interval.
---
MOSFET OFF
When the MOSFET is OFF:
```text
Inductor → Diode → Output Capacitor + Load
```
The stored energy in the inductor is transferred to the output.
The output voltage becomes greater than the input voltage.
---
<details>
<summary>📐 <strong>Complete Design Calculations — Click to Expand</strong></summary>
📐 Complete Design Calculations
> **GitHub math rendering:** All equations in this README use GitHub-supported LaTeX math delimiters (`$...$` for inline equations and `$$...$$` for displayed equations). No boxed-equation commands are used, avoiding the brace/rendering errors that can occur in GitHub preview.
1. Ideal Boost Converter Equation
For an ideal boost converter operating in continuous conduction mode:
$$
V_o=\frac{V_{in}}{1-D}
$$
where:
$V_o$ = output voltage
$V_{in}$ = input voltage
$D$ = duty cycle
Rearranging:
$$
D=1-\frac{V_{in}}{V_o}
$$
---
2. Ideal Duty Cycle for 5 V → 7 V
Given:
$$
V_{in}=5\text{ V}
$$
$$
V_o=7\text{ V}
$$
Using:
$$
D=1-\frac{V_{in}}{V_o}
$$
Substituting:
$$
D=1-\frac{5}{7}
$$
$$
D=1-0.7142857
$$
$$
D=0.285714
$$
Therefore:
$$
D_{ideal}=28.57%
$$
---
3. Practical Duty Cycle
Real boost converters have losses caused by:
Diode forward voltage
MOSFET conduction resistance
Inductor winding resistance
Switching losses
Capacitor ESR
PCB parasitics
For a simplified calculation, assume:
$$
V_D\approx0.7\text{ V}
$$
The approximate practical relation is:
$$
V_o\approx\frac{V_{in}}{1-D}-V_D
$$
Rearranging:
$$
V_o+V_D=\frac{V_{in}}{1-D}
$$
Therefore:
$$
1-D=\frac{V_{in}}{V_o+V_D}
$$
Hence:
$$
D=1-\frac{V_{in}}{V_o+V_D}
$$
Substituting:
$$
D=1-\frac{5}{7+0.7}
$$
$$
D=1-\frac{5}{7.7}
$$
$$
D\approx0.35065
$$
Therefore:
$$
D_{practical}\approx35.06%
$$
The KiCad simulation uses:
$$
D_{simulation}=36%
$$
The small difference provides a practical margin in the simplified model.
---
4. Switching Frequency
The selected switching frequency is:
$$
f_s=100\text{ kHz}
$$
or:
$$
f_s=100000\text{ Hz}
$$
The switching period is:
$$
T=\frac{1}{f_s}
$$
Therefore:
$$
T=\frac{1}{100000}
$$
$$
T=10\times10^{-6}\text{ s}
$$
Hence:
$$
T=10\ \mu s
$$
---
5. ON Time
For a 36% duty cycle:
$$
T_{ON}=DT
$$
$$
T_{ON}=0.36(10\ \mu s)
$$
Therefore:
$$
T_{ON}=3.6\ \mu s
$$
---
6. OFF Time
The OFF time is:
$$
T_{OFF}=T-T_{ON}
$$
$$
T_{OFF}=10-3.6
$$
Therefore:
$$
T_{OFF}=6.4\ \mu s
$$
Alternatively:
$$
T_{OFF}=(1-D)T
$$
---
7. PWM Parameters
The SPICE PWM source uses approximately:
```text
V1  = 0 V
V2  = 5 V
td  = 0
tr  = 10 ns
tf  = 10 ns
tw  = 3.6 µs
per = 10 µs
```
Conceptually:
```text
5 V       ┌──────────┐                 ┌──────────┐
          │          │                 │          │
0 V ──────┘          └─────────────────┘          └────

          ← 3.6 µs →
          ←────────────── 10 µs ────────────────→
```
Therefore:
$$
D=\frac{T_{ON}}{T}
$$
$$
D=\frac{3.6}{10}
$$
$$
D=36%
$$
---
8. Output Current
For the simulation load:
$$
R_{load}=24\Omega
$$
At the target output:
$$
V_o=7\text{ V}
$$
Using Ohm's law:
$$
I_o=\frac{V_o}{R_{load}}
$$
$$
I_o=\frac{7}{24}
$$
Therefore:
$$
I_o\approx0.292\text{ A}
$$
---
9. Output Power
Output power is:
$$
P_o=V_oI_o
$$
Substituting:
$$
P_o=7(0.292)
$$
Therefore:
$$
P_o\approx2.04\text{ W}
$$
Alternatively:
$$
P_o=\frac{V_o^2}{R_{load}}
$$
$$
P_o=\frac{7^2}{24}
$$
$$
P_o\approx2.04\text{ W}
$$
---
10. Ideal Input Current
Ignoring converter losses:
$$
P_{in}\approx P_o
$$
Therefore:
$$
I_{in}\approx\frac{P_o}{V_{in}}
$$
$$
I_{in}\approx\frac{2.04}{5}
$$
Therefore:
$$
I_{in}\approx0.408\text{ A}
$$
The actual hardware input current will be higher because of losses.
---
11. Inductor Current Ripple
For a boost converter, the approximate inductor current ripple is:
$$
\Delta I_L=
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
L=100\ \mu H
$$
$$
f_s=100\text{ kHz}
$$
Substituting:
$$
\Delta I_L=
\frac{5(0.36)}
{(100\times10^{-6})(100000)}
$$
Therefore:
$$
\Delta I_L\approx0.18\text{ A}
$$
---
12. Inductor Peak Current
The approximate average inductor current is close to the input current:
$$
I_{L,avg}\approx I_{in}
$$
Therefore:
$$
I_{L,avg}\approx0.408\text{ A}
$$
Peak current:
$$
I_{L,peak}
I_{L,avg}+\frac{\Delta I_L}{2}
$$
$$
I_{L,peak}
0.408+\frac{0.18}{2}
$$
$$
I_{L,peak}
0.498\text{ A}
$$
Therefore:
$$
I_{L,peak}\approx0.498\text{ A}
$$
---
13. Minimum Inductor Current
$$
I_{L,min}
I_{L,avg}-\frac{\Delta I_L}{2}
$$
$$
I_{L,min}
0.408-\frac{0.18}{2}
$$
Therefore:
$$
I_{L,min}\approx0.318\text{ A}
$$
Since the calculated minimum current is positive, the simplified model indicates continuous-conduction operation for this operating point.
---
14. Inductor Sizing
The inductor design equation is:
$$
L=
\frac{V_{in}D}
{f_s\Delta I_L}
$$
A common design approach is to select an allowable inductor-current ripple as a percentage of the average input current.
For example, assuming:
$$
\Delta I_L\approx30%I_{in}
$$
and:
$$
I_{in}\approx0.408\text{ A}
$$
we obtain:
$$
\Delta I_L\approx0.3(0.408)
$$
$$
\Delta I_L\approx0.122\text{ A}
$$
Then:
$$
L=
\frac{5(0.3506)}
{(100000)(0.122)}
$$
Therefore:
$$
L\approx144\ \mu H
$$
A practical standard value is approximately:
$$
L\approx150\ \mu H
$$
However, the current KiCad simulation uses:
$$
L=100\ \mu H
$$
The 100 µH value is therefore treated as the simulation/design study value, while 150 µH is a possible value from a 30% ripple-based sizing approach.
---
15. Output Capacitor Calculation
The approximate output-voltage ripple is:
$$
\Delta V_o
\approx
\frac{I_oD}{f_sC}
$$
For the current simulation:
$$
I_o=0.292\text{ A}
$$
$$
D=0.36
$$
$$
f_s=100\text{ kHz}
$$
$$
C=1000\ \mu F
$$
Therefore:
$$
\Delta V_o
\approx
\frac{0.292(0.36)}
{(100000)(1000\times10^{-6})}
$$
Therefore:
$$
\Delta V_o\approx1.05\text{ mV}
$$
This is an idealized capacitor-ripple estimate.
Actual ripple will also depend on:
Capacitor ESR
Capacitor ESL
Inductor ripple
Diode behavior
MOSFET switching
PCB parasitics
---
16. Capacitor Sizing
For a selected maximum ripple:
$$
C\approx
\frac{I_oD}
{f_s\Delta V_o}
$$
For example, if:
$$
\Delta V_o=0.07\text{ V}
$$
which corresponds to approximately 1% of 7 V:
$$
C\approx
\frac{0.292(0.36)}
{(100000)(0.07)}
$$
Therefore:
$$
C\approx15\ \mu F
$$
A larger practical capacitor can be selected for additional ripple margin.
The hardware prototype uses:
$$
C=220\ \mu F/35\text{ V}
$$
---
17. Critical Inductance
An approximate boundary between continuous and discontinuous conduction is:
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
R=24\Omega
$$
$$
f_s=100\text{ kHz}
$$
we obtain:
$$
L_{crit}
\approx
\frac{0.36(1-0.36)^2(24)}
{2(100000)}
$$
Therefore:
$$
L_{crit}\approx17.7\ \mu H
$$
Since:
$$
100\ \mu H>17.7\ \mu H
$$
the selected 100 µH inductor is above this simplified critical value.
---
18. Voltage Gain
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
The converter therefore requires approximately a 1.4× voltage gain.
---
19. Hardware Load Calculation
The planned hardware load is:
$$
R_{load}=27\Omega
$$
At 7 V:
$$
I_o=\frac{7}{27}
$$
Therefore:
$$
I_o\approx0.259\text{ A}
$$
The load power is:
$$
P_o=\frac{7^2}{27}
$$
Therefore:
$$
P_o\approx1.81\text{ W}
$$
The selected resistor is rated at:
$$
10\text{ W}
$$
---
20. Hardware Load at 12 V
The same 27 Ω load can also be used for a future 12 V operating point.
$$
I_o=\frac{12}{27}
$$
Therefore:
$$
I_o\approx0.444\text{ A}
$$
The load power becomes:
$$
P_o=\frac{12^2}{27}
$$
Therefore:
$$
P_o\approx5.33\text{ W}
$$
The resistor will become hot at this power level and must be positioned safely with adequate ventilation.
---
21. Efficiency
Converter efficiency is:
$$
\eta=
\frac{P_{out}}{P_{in}}\times100
$$
Since:
$$
P_{out}=V_oI_o
$$
and:
$$
P_{in}=V_{in}I_{in}
$$
the efficiency can be written as:
$$
\eta=
\frac{V_oI_o}
{V_{in}I_{in}}
\times100
$$
No experimental efficiency value is claimed until actual input and output measurements are obtained.
---
22. Theoretical Error
The theoretical and simulation results can be compared using:
$$
Error(%)=
\frac{
|V_{theory}-V_{simulation}|
}
{V_{theory}}
\times100
$$
Python:
```python
error_percent = (
    abs(V_theory - V_simulation)
    / V_theory
) * 100
```
---
</details>
🤖 PWM GENERATOR
The PWM signal is responsible for controlling the MOSFET.
Target:
```text
Frequency : 100 kHz
Period    : 10 µs
HIGH      : 5 V
LOW       : 0 V
Duty      : approximately 35–36%
```
The Arduino UNO is based on a 16 MHz ATmega328P.
For accurate 100 kHz PWM, the hardware Timer1 is used rather than relying on the default `analogWrite()` frequency.
---
⚙️ Arduino Timer1 PWM
For Fast PWM with ICR1 as TOP:
$$
f_{PWM}=
\frac{F_{CPU}}
{N(1+ICR1)}
$$
For:
$$
F_{CPU}=16\text{ MHz}
$$
and prescaler:
$$
N=1
$$
for 100 kHz:
$$
100000=
\frac{16000000}
{1(1+ICR1)}
$$
Therefore:
$$
1+ICR1=160
$$
Hence:
$$
ICR1=159
$$
---
🎛️ 36% PWM Calculation
PWM duty cycle is approximately:
$$
D\approx
\frac{OCR1A+1}
{ICR1+1}
$$
For 36%:
$$
OCR1A+1
\approx
0.36(160)
$$
$$
OCR1A+1\approx57.6
$$
A practical integer value is:
$$
OCR1A=57
$$
which produces approximately:
$$
D\approx36.25%
$$
---
💻 Arduino 100 kHz PWM Code
```cpp
/*
  5 V -> 7 V Boost Converter
  Arduino UNO 100 kHz PWM

  PWM output:
  Pin 9 (OC1A)

  Frequency:
  100 kHz

  Duty:
  approximately 36%
*/

void setup()
{
  pinMode(9, OUTPUT);

  // Stop Timer1
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
  // PWM is generated automatically by Timer1.
}
```
PWM output:
```text
Arduino UNO Pin 9
       │
       ▼
     10 Ω
       │
       ▼
IRLZ44N Gate
       │
     10 kΩ
       │
      GND
```
---
🧪 Wokwi PWM Verification
Before connecting the Arduino to the physical converter, PWM can be verified using Wokwi.
The expected waveform is approximately:
```text
Frequency ≈ 100 kHz
Period    ≈ 10 µs
Duty      ≈ 36%
HIGH      ≈ 5 V
LOW       ≈ 0 V
```
A logic analyzer can be used to inspect:
Frequency
Period
Duty cycle
Pulse width
Logic HIGH
Logic LOW
---
🐍 Python PWM Simulation
Python is also used to create an analytical PWM waveform.
```python
import numpy as np
import matplotlib.pyplot as plt

frequency = 100_000
duty_cycle = 36

V_high = 5
V_low = 0

cycles = 5

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

samples_per_cycle = 1000
total_samples = cycles * samples_per_cycle

t = np.linspace(
    0,
    cycles * T,
    total_samples,
    endpoint=False
)

pwm = np.where(
    (t % T) < Ton,
    V_high,
    V_low
)

plt.figure(figsize=(12, 4))

plt.plot(
    t * 1e6,
    pwm,
    drawstyle="steps-post"
)

plt.xlabel("Time (µs)")
plt.ylabel("Voltage (V)")

plt.title(
    f"PWM Signal — "
    f"{frequency/1000:.0f} kHz, "
    f"{duty_cycle:.1f}% Duty Cycle"
)

plt.ylim(-0.5, 5.5)
plt.grid(True)
plt.show()
```
---
🐍 Python Boost Converter Analysis
The basic analytical model can be implemented using:
```python
Vin = 5.0
Vout_target = 7.0

Vf = 0.7

L = 100e-6
C = 220e-6

Rload = 24

frequency = 100_000

D = 0.36

Vout_ideal = Vin / (1 - D)

Vout_approx = Vout_ideal - Vf

Iout = Vout_approx / Rload

Pout = Vout_approx * Iout

Delta_IL = (
    Vin * D
    / (L * frequency)
)

Iin = Pout / Vin

IL_peak = Iin + Delta_IL / 2

IL_min = Iin - Delta_IL / 2

Delta_Vout = (
    Iout * D
    / (frequency * C)
)

print("Output voltage:", Vout_approx)
print("Output current:", Iout)
print("Output power:", Pout)
print("Input current:", Iin)
print("Inductor ripple:", Delta_IL)
print("Peak inductor current:", IL_peak)
print("Minimum inductor current:", IL_min)
print("Output ripple:", Delta_Vout)
```
---
🗃️ Parametric Dataset
A Python-generated analytical dataset can be used to study converter behavior over a large parameter space.
The dataset parameters include:
```text
Input Voltage
Duty Cycle
Switching Frequency
Inductance
Capacitance
Load Resistance
```
Calculated quantities include:
```text
Ideal Output Voltage
Approximate Output Voltage
Output Current
Output Power
Input Current
Inductor Current Ripple
Peak Inductor Current
Minimum Inductor Current
Output Voltage Ripple
Critical Inductance
Conduction Mode
```
The analytical dataset is clearly separated from SPICE and experimental data.
---
📊 PARAMETRIC ANALYSIS
Duty Cycle Sweep
Example:
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
V_o=\frac{V_{in}}{1-D}
$$
As duty cycle changes, the theoretical output voltage changes accordingly.
---
⚡ Switching Frequency Sweep
Example:
```text
50 kHz
75 kHz
100 kHz
150 kHz
200 kHz
```
Increasing switching frequency generally changes the required energy-storage components and ripple characteristics.
---
🌀 Inductance Sweep
Example:
```text
47 µH
68 µH
100 µH
150 µH
220 µH
330 µH
```
The simplified current-ripple relationship is:
$$
\Delta I_L=
\frac{V_{in}D}{Lf_s}
$$
Therefore, for constant input voltage, duty cycle, and switching frequency:
$$
\Delta I_L\propto\frac{1}{L}
$$
Increasing inductance generally reduces inductor-current ripple.
---
🔋 Capacitance Sweep
Example:
```text
47 µF
100 µF
220 µF
470 µF
1000 µF
```
The simplified capacitor ripple relationship is:
$$
\Delta V_o
\approx
\frac{I_oD}{f_sC}
$$
Therefore:
$$
\Delta V_o\propto\frac{1}{C}
$$
Increasing capacitance generally reduces the idealized capacitive component of output-voltage ripple.
---
🔌 Load Resistance Sweep
Example:
```text
10 Ω
15 Ω
22 Ω
24 Ω
27 Ω
33 Ω
47 Ω
68 Ω
```
Load current is:
$$
I_o=\frac{V_o}{R}
$$
and output power is:
$$
P_o=\frac{V_o^2}{R}
$$
---
📈 Planned Analysis Graphs
The following plots will be generated as the analysis is completed:
```text
1. Output Voltage vs Duty Cycle

2. Output Ripple vs Capacitance

3. Inductor Ripple vs Inductance

4. Output Voltage vs Switching Frequency

5. Output Voltage vs Load Resistance

6. Theoretical Output Voltage vs Simulation Output Voltage

7. Input Current vs Duty Cycle

8. Peak Inductor Current vs Inductance

9. Output Power vs Load Resistance

10. Efficiency vs Load
```
No broken image links are included in this README.
Actual graphs will be added only after their corresponding files are generated and committed to the repository.
---
🖥️ KiCad / SPICE Simulation
The KiCad project contains the boost-converter schematic.
Current project structure:
```text
kicad/
└── Exp_1/
    ├── Exp_1.kicad_pro
    └── Exp_1.kicad_sch
```
The schematic contains:
5 V DC input
100 µH inductor
NMOS switching device
PWM voltage source
Diode
Output capacitor
Resistive load
Ground
Switching network
---
📈 SPICE Waveforms
The simulation can be used to investigate:
Output Voltage
$$
V_{out}(t)
$$
Gate Voltage
$$
V_G(t)
$$
Inductor Current
$$
I_L(t)
$$
Diode Current
$$
I_D(t)
$$
MOSFET Current
$$
I_{MOSFET}(t)
$$
Capacitor Current
$$
I_C(t)
$$
These waveforms can be used to analyze:
Startup transient
Output settling
Switching behavior
Inductor ripple
Output ripple
Peak current
Switching-node behavior
MOSFET current
Diode current
---
⚡ Startup Transient
A switching converter can show a startup transient when the simulation begins.
Possible causes include:
Initially uncharged capacitor
Inductor initial conditions
Sudden application of PWM
Switching-node ringing
Diode behavior
MOSFET model behavior
Parasitic elements
Therefore, the startup peak should not automatically be treated as the steady-state output voltage.
Steady-state values should be measured after the transient has settled.
---
🧠 Theory vs Simulation
The project distinguishes between three types of results.
Result	Meaning
Theoretical	Obtained from mathematical equations
Analytical	Obtained using Python mathematical models
Simulation	Obtained from KiCad/SPICE
Experimental	Obtained from physical hardware
The results are not mixed together.
For example:
```text
Theoretical:
Vout calculated from boost equation

Analytical:
Vout calculated by Python

Simulation:
Vout measured from SPICE waveform

Experimental:
Vout measured from physical prototype
```
---
🔬 Experimental Validation
The planned physical test setup is:
```text
5 V / 2 A DC Supply
        │
        ▼
    100 µH Inductor
        │
        ●───────────┐
        │           │
        ▼           ▼
    IRLZ44N       1N5822
        │           │
       GND          ├──────── +VOUT
                    │
              ┌─────┴─────┐
              │           │
           220 µF      27 Ω / 10 W
              │           │
             GND         GND
```
Arduino control:
```text
Arduino UNO
     │
100 kHz PWM
     │
    10 Ω
     │
IRLZ44N Gate
     │
    10 kΩ
     │
    GND
```
---
📏 Planned Hardware Measurements
The physical prototype will eventually be evaluated for:
Input voltage
Input current
Output voltage
Output current
Output ripple
PWM frequency
PWM duty cycle
Inductor current
MOSFET temperature
Diode temperature
Load temperature
Input power
Output power
Efficiency
Startup transient
---
📊 Theory vs SPICE vs Hardware
Parameter	Theory	SPICE	Hardware
Input voltage	5 V	To be verified	Pending
Output voltage	7 V target	To be measured	Pending
Duty cycle	28.57% ideal / ≈35.06% practical	36%	Pending
Switching frequency	100 kHz	100 kHz target	Pending
Output current	Calculated	To be measured	Pending
Output power	Calculated	To be measured	Pending
Input current	Estimated	To be measured	Pending
Output ripple	Calculated	To be measured	Pending
Inductor ripple	Calculated	To be measured	Pending
Efficiency	Calculated from measurements	Pending	Pending
---
🔥 Real-World Component Effects
The ideal equations do not include every real-world loss.
MOSFET
Important parameters include:
$$
R_{DS(on)}
$$
and switching losses.
Diode
The diode introduces:
$$
V_D
$$
and may also introduce switching and reverse-recovery losses.
Inductor
A real inductor has:
DC resistance
Core losses
Saturation
Parasitic capacitance
Capacitor
A real capacitor has:
ESR
ESL
Leakage
Ripple-current limitations
PCB
PCB parasitics can introduce:
Additional inductance
Additional resistance
Switching-node ringing
EMI
Therefore, theoretical calculations provide the starting point, while SPICE and hardware measurements provide progressively more realistic results.
---
🌡️ Thermal Analysis
For the 27 Ω hardware load at 7 V:
$$
P_R=\frac{V^2}{R}
$$
$$
P_R=\frac{7^2}{27}
$$
$$
P_R\approx1.81\text{ W}
$$
At 12 V:
$$
P_R=\frac{12^2}{27}
$$
$$
P_R\approx5.33\text{ W}
$$
Therefore, the 27 Ω / 10 W resistor can become significantly hot during operation.
The MOSFET and diode also require consideration of their power dissipation and temperature.
---
⚠️ Safety
This project is intended for low-voltage DC operation.
Do not connect the boost converter directly to household AC mains.
The converter input is:
$$
5\text{ V DC}
$$
The switching frequency is:
$$
100\text{ kHz}
$$
The 100 kHz switching frequency is generated electronically by PWM and is unrelated to the 50 Hz household AC frequency.
Before applying power:
Verify MOSFET pin connections.
Verify diode polarity.
Verify capacitor polarity.
Verify input and output grounds.
Verify there is no short circuit.
Verify PWM frequency.
Verify PWM duty cycle.
Verify gate pulldown.
Keep the switching loop physically short.
Do not touch the 27 Ω resistor during operation.
Use an appropriate current-limited supply when possible.
---
📁 Repository Structure
```text
5V-to-7V-Boost-Converter/
│
├── kicad/
│   └── Exp_1/
│       ├── Exp_1.kicad_pro
│       └── Exp_1.kicad_sch
│
├── notebooks/
│   ├── boost_converter_analysis.ipynb
│   └── boost_converter_pwm.ipynb
│
├── results/
│   ├── schematic/
│   ├── plots/
│   ├── simulation/
│   └── animations/
│
├── data/
│   └── boost_converter_theoretical_dataset.csv
│
├── Arduino/
│   └── 100kHz_PWM/
│       └── pwm_100khz.ino
│
├── Wokwi/
│   ├── diagram.json
│   └── pwm_test.ino
│
├── documentation/
│
├── README.md
├── requirements.txt
└── .gitignore
```
---
🧰 Software and Tools
Tool	Purpose
KiCad 10	Schematic and PCB design
SPICE	Circuit simulation
Python	Numerical analysis
NumPy	Numerical computation
Pandas	Dataset processing
SciPy	Scientific computation
Matplotlib	Graph generation
Jupyter Notebook	Interactive analysis
Arduino IDE	Arduino programming
Arduino UNO	PWM generation
Wokwi	PWM verification
PulseView	Digital waveform analysis
GitHub	Version control
---
📦 Hardware Components
Component	Specification	Purpose
DC Supply	5 V / 2 A	Input source
L1	100 µH	Energy storage
Q1	IRLZ44N	Switching device
D1	1N5822	Rectification
C1	220 µF / 35 V	Output filtering
RLOAD	27 Ω / 10 W	Hardware load
RG	10 Ω	Gate resistor
RPD	10 kΩ	Gate pulldown
Controller	Arduino UNO	PWM generation
---
<details>
<summary>📚 <strong>Key Equations — Click to Expand</strong></summary>
📚 Key Equations
Boost Converter
$$
V_o=\frac{V_{in}}{1-D}
$$
Ideal Duty Cycle
$$
D=1-\frac{V_{in}}{V_o}
$$
Practical Duty Cycle
$$
D\approx1-\frac{V_{in}}{V_o+V_D}
$$
Switching Period
$$
T=\frac{1}{f_s}
$$
ON Time
$$
T_{ON}=DT
$$
OFF Time
$$
T_{OFF}=(1-D)T
$$
Inductor Ripple
$$
\Delta I_L=
\frac{V_{in}D}{Lf_s}
$$
Inductor Sizing
$$
L=
\frac{V_{in}D}
{f_s\Delta I_L}
$$
Output Current
$$
I_o=\frac{V_o}{R}
$$
Output Power
$$
P_o=V_oI_o
$$
Capacitor Ripple
$$
\Delta V_o
\approx
\frac{I_oD}{f_sC}
$$
Capacitor Sizing
$$
C\approx
\frac{I_oD}
{f_s\Delta V_o}
$$
Critical Inductance
$$
L_{crit}
\approx
\frac{D(1-D)^2R}
{2f_s}
$$
Voltage Gain
$$
M=\frac{V_o}{V_{in}}
$$
Efficiency
$$
\eta=
\frac{P_{out}}
{P_{in}}\times100
$$
Percentage Error
$$
Error(%)=
\frac{|V_{theory}-V_{simulation}|}
{V_{theory}}\times100
$$
---
</details>
📊 Design Summary
Quantity	Calculated / Selected Value
Input voltage	5 V
Target output voltage	7 V
Voltage gain	1.4
Ideal duty cycle	28.57%
Practical duty cycle	≈35.06%
Simulation duty cycle	36%
Switching frequency	100 kHz
Switching period	10 µs
ON time	3.6 µs
OFF time	6.4 µs
Simulation inductor	100 µH
Approx. inductor ripple	0.18 A
Approx. peak inductor current	0.498 A
Approx. minimum inductor current	0.318 A
Simulation load	24 Ω
Simulation output current at 7 V	0.292 A
Simulation output power at 7 V	2.04 W
Simulation capacitor	1000 µF
Approx. capacitor ripple	1.05 mV
Approx. critical inductance	17.7 µH
Hardware capacitor	220 µF / 35 V
Hardware load	27 Ω / 10 W
Hardware current at 7 V	0.259 A
Hardware load power at 7 V	1.81 W
Hardware load current at 12 V	0.444 A
Hardware load power at 12 V	5.33 W
---
🧠 Engineering Questions Investigated
The project investigates:
1. Duty Cycle
How does PWM duty cycle influence output voltage?
2. Switching Frequency
How does switching frequency affect ripple and component requirements?
3. Inductance
How does inductance affect inductor-current ripple?
4. Capacitance
How does capacitance affect output-voltage ripple?
5. Load Resistance
How does load resistance affect output current and power?
6. Component Non-Idealities
How do MOSFET, diode, inductor, capacitor, and PCB losses affect the converter?
7. Theory vs Simulation
How closely does the analytical model agree with SPICE?
8. Simulation vs Hardware
How closely does the physical prototype reproduce the simulated behavior?
---
🚀 Development Roadmap
```text
Theory
  ↓
Design Calculations
  ↓
Python Model
  ↓
KiCad Schematic
  ↓
SPICE Simulation
  ↓
Parametric Analysis
  ↓
Arduino PWM
  ↓
Wokwi Verification
  ↓
PCB Design
  ↓
Hardware Prototype
  ↓
Experimental Measurements
  ↓
Theory vs Hardware
  ↓
Optimization
  ↓
Closed-Loop Control
```
---
📌 Current Project Status
Development Area	Status
Boost converter theory	🟢 Completed
Design calculations	🟢 Completed
Python analytical model	🟢 Completed
KiCad schematic	🟢 Completed
Initial SPICE simulation	🟢 Completed
PWM mathematical model	🟢 Completed
Wokwi PWM verification	🟢 Completed
Dataset generation	🟡 In progress
Parameter sweep	🟡 In progress
Result graphs	🟡 In progress
Arduino hardware PWM	🟡 In progress
PCB design	🔴 Planned
Physical prototype	🟡 In progress
Experimental measurements	🔴 Pending
Efficiency measurement	🔴 Pending
Thermal measurements	🔴 Pending
Theory vs hardware	🔴 Pending
Closed-loop regulation	🔴 Planned
Status Legend
🟢 Completed  
🟡 In Progress  
🔴 Planned / Pending
---
⚠️ Current Limitations
The current work combines analytical modelling and simulation with planned hardware development.
Current limitations include:
Real component tolerances are not fully represented.
MOSFET switching losses may differ from the simplified model.
Diode forward voltage changes with current and temperature.
Inductor DCR and saturation are not completely represented in the simplified equations.
Capacitor ESR and ESL are not included in the ideal ripple equations.
PCB parasitics have not yet been included.
Experimental efficiency has not yet been measured.
Experimental oscilloscope measurements are pending.
Hardware results will be added only after actual measurements.
The current control method is open-loop PWM.
Closed-loop voltage regulation is future work.
---
🔬 Future Work
Future development includes:
[x] Mathematical boost-converter analysis
[x] Duty-cycle calculation
[x] Inductor calculation
[x] Capacitor calculation
[x] Python model
[x] KiCad schematic
[x] Initial SPICE simulation
[x] PWM analysis
[x] Wokwi PWM verification
[x] GitHub repository
[ ] Complete parameter-sweep graphs
[ ] Automated SPICE data extraction
[ ] Arduino hardware PWM
[ ] PCB layout
[ ] Gerber generation
[ ] Hardware prototype
[ ] Experimental waveform capture
[ ] Efficiency measurement
[ ] Thermal analysis
[ ] Theory vs SPICE comparison
[ ] SPICE vs hardware comparison
[ ] Closed-loop voltage regulation
[ ] Soft-start implementation
[ ] Protection circuitry
[ ] Optimization of switching components
---
🏁 Engineering Workflow
The final objective of this project is not simply to simulate a boost converter.
The project follows a complete engineering methodology:
```text
                  ┌────────────────────┐
                  │  REQUIREMENTS       │
                  │  5 V → 7 V          │
                  │  100 kHz            │
                  └─────────┬──────────┘
                            ↓
                  ┌────────────────────┐
                  │ THEORY             │
                  │ Boost equations    │
                  └─────────┬──────────┘
                            ↓
                  ┌────────────────────┐
                  │ DESIGN CALCULATIONS│
                  │ D, L, C, I, P      │
                  └─────────┬──────────┘
                            ↓
                  ┌────────────────────┐
                  │ PYTHON MODEL       │
                  │ Analytical study   │
                  └─────────┬──────────┘
                            ↓
                  ┌────────────────────┐
                  │ KiCad / SPICE      │
                  │ Circuit simulation │
                  └─────────┬──────────┘
                            ↓
                  ┌────────────────────┐
                  │ PARAMETRIC ANALYSIS│
                  │ D, f, L, C, R      │
                  └─────────┬──────────┘
                            ↓
                  ┌────────────────────┐
                  │ PWM GENERATION     │
                  │ Arduino UNO        │
                  └─────────┬──────────┘
                            ↓
                  ┌────────────────────┐
                  │ WOKWI VERIFICATION │
                  └─────────┬──────────┘
                            ↓
                  ┌────────────────────┐
                  │ PCB DESIGN         │
                  └─────────┬──────────┘
                            ↓
                  ┌────────────────────┐
                  │ HARDWARE           │
                  └─────────┬──────────┘
                            ↓
                  ┌────────────────────┐
                  │ MEASUREMENTS       │
                  └─────────┬──────────┘
                            ↓
                  ┌────────────────────┐
                  │ VALIDATION         │
                  │ Theory vs SPICE    │
                  │ vs Hardware        │
                  └────────────────────┘
```
---
👨‍💻 Author

Sai Nandan
Electronics and Communication Engineering
Power Electronics • Embedded Systems • Circuit Simulation • Python • PCB Design

---
⚡ Final Project Statement
This project demonstrates the design and analysis of a high-frequency boost converter through mathematical modelling, Python-based analysis, KiCad schematic development, SPICE simulation, PWM generation, parametric analysis, and planned hardware validation.
The design is based on a 5 V DC input, 7 V target output, 100 kHz switching frequency, and approximately 35–36% practical PWM duty cycle.
The project separates:
```text
THEORY
   ↓
ANALYTICAL MODEL
   ↓
SPICE SIMULATION
   ↓
HARDWARE
   ↓
EXPERIMENTAL VALIDATION
```
so that calculated, simulated, and measured results are not incorrectly represented as one another.

⚡ 5 V → 7 V Boost Converter
From Mathematical Design → Simulation → PWM → Hardware Validation