# Day 3 – Design Library Cell using Magic Layout and ngspice Characterization

## Objective

The objective of Day 3 was to understand the design and characterization of a CMOS inverter cell using Magic Layout and ngspice. The session focused on inverter switching characteristics, switching threshold (Vm), transistor sizing effects, body effect, propagation delay analysis, and CMOS inverter layout implementation in the SKY130 technology.

---

## CMOS Inverter Robustness and Switching Threshold

The switching threshold voltage (Vm) is one of the most important parameters in a CMOS inverter. It is defined as the point where the input voltage is equal to the output voltage.

\[
V_{in}=V_{out}=V_m
\]

At this operating point, both NMOS and PMOS transistors conduct simultaneously, and the drain currents satisfy:

\[
I_{DSP} = -I_{DSN}
\]

The switching threshold directly affects noise margins and inverter robustness.

### Switching Threshold Comparison

![Switching Threshold Comparison](images/switching_threshold_comparison.png)

### Observation

Two different inverter sizing configurations were analyzed:

| Configuration | Vm |
|--------------|------|
| Wn = Wp = 0.375µm | ~0.98 V |
| Wn = 0.375µm, Wp = 0.9375µm | ~1.2 V |

Increasing the PMOS width strengthens the pull-up network and shifts the switching threshold towards higher voltages.

---

## Static Behavior Evaluation

The Voltage Transfer Characteristic (VTC) curve of the CMOS inverter was studied to understand transistor operating regions.

![Static Behavior Evaluation](images/switching_threshold_regions.png)

### Operating Regions

During switching:

- NMOS OFF, PMOS Linear
- NMOS Saturation, PMOS Linear
- NMOS Saturation, PMOS Saturation
- NMOS Linear, PMOS Saturation
- NMOS Linear, PMOS OFF

The transition region represents the switching activity of the inverter.

---

## Mathematical Analysis of Switching Threshold

The switching threshold can be derived from the current balance equations of PMOS and NMOS devices.

![Vm Equations](images/vm_equations.png)

The switching threshold is given by:

\[
V_m = \frac{R \cdot V_{DD}}{1+R}
\]

where

\[
R=\frac{K_pV_{dsatp}}{K_nV_{dsatn}}
\]

The value of R depends on:

- NMOS width-to-length ratio
- PMOS width-to-length ratio
- Carrier mobility
- Saturation voltages

By adjusting transistor dimensions, the switching threshold can be shifted to achieve balanced inverter performance.

---

## Delay Analysis

The effect of transistor sizing on rise and fall delays was analyzed.

![Delay Analysis](images/delay_analysis.png)

### Results

| Parameter | Value |
|-----------|--------|
| Rise Delay | 148 ps |
| Fall Delay | 71 ps |
| Switching Threshold | 0.99 V |

### Observation

The rise delay is larger than the fall delay because the PMOS transistor generally has lower mobility compared to the NMOS transistor. Proper transistor sizing is therefore necessary to balance rise and fall times.

---

## Body Effect and Threshold Voltage Variation

The body effect causes the threshold voltage of a MOSFET to increase when there is a voltage difference between the source and body terminals.

![Body Effect](images/body_effect.png)

### Threshold Voltage Equation

\[
V_t = V_{t0} + \gamma
\left(
\sqrt{| -2\phi_f + V_{SB}|}
-
\sqrt{| -2\phi_f|}
\right)
\]

Where:

- \(V_{t0}\) = Threshold voltage at \(V_{SB}=0\)
- \(\gamma\) = Body effect coefficient
- \(\phi_f\) = Fermi potential
- \(V_{SB}\) = Source-to-body voltage

### Observation

As the source-to-body voltage increases, the threshold voltage also increases. This phenomenon affects switching speed and must be considered during transistor characterization.

---

## CMOS Inverter Layout using Magic

A CMOS inverter layout was implemented using the Magic VLSI layout editor in SKY130 technology.

![CMOS Inverter Layout](images/inverter_layout_magic.png)

### Layout Features

- PMOS transistor connected to VDD rail.
- NMOS transistor connected to GND rail.
- Common polysilicon gate used as input (A).
- Shared drain connection used as output (Y).
- Designed following SKY130 design rules.

### Observation

The layout successfully implements the CMOS inverter structure with proper power rails, input/output routing, and transistor placement. This layout serves as the foundation for extraction and SPICE characterization.

---

## Conclusion

Day 3 focused on understanding CMOS inverter behavior from both theoretical and physical design perspectives. The switching threshold was analyzed using SPICE simulations and mathematical models. The effects of transistor sizing, body effect, and propagation delays were studied. Finally, a CMOS inverter layout was implemented in Magic using SKY130 technology, providing the basis for further extraction and characterization in subsequent stages of the VLSI design flow.
