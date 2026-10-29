---
title: LDO
layout: default
nav_order: 4
---

## Block Specific Design Targets: LDO

The supply block must hold $V_\mathrm{DD,AFE}$ within
$\pm 1 \%$ while the whole AFE switches on, with no
external capacitor and only 140 mV of headroom.



### LDO 
{:#ldo .unnumbered}

| Parameter | Specification |
| --- | --- |
| $V_\mathrm{IN}$ / $V_\mathrm{OUT}$ | 1.2 V $\pm 5\%$ / 1.00 V |
| Dropout | $\le 140$ mV at 25 mA |
| Load range | 50 μA (standby) to 25 mA |
| Output capacitance | none external; $\le 300$ pF on chip in total, including AFE decap |
| Load step 50 μA $\to$ 25 mA in 1 ns | undershoot $\le 50$ mV; back within $\pm 1\%$ in $\le 10$ ns |
| Load step 25 mA $\to$ 50 μA in 1 ns | overshoot $\le 50$ mV; $V_\mathrm{OUT}$ never above 1.10 V |
| Stability | phase margin $\ge 45^\circ$ across load, PVT and every power state |
| Load / line regulation | $\le 0.5\%$ / $\le 5$ mV over the $V_\mathrm{IN}$ range |
| PSRR | $\ge 40$ dB to 1 MHz, $\ge 25$ dB at 100 MHz, $\ge 15$ dB worst case to 10 GHz |
| Output noise (optional) | $\le 100\ \mu\text{V}_{\text{rms}}$, 1 kHz to 1 GHz |
| Quiescent current | $\le 300\ \mu\text{A}$ active, $\le 10\ \mu\text{A}$ standby |
| Cold start, BG included | $V_\mathrm{OUT}$ within $\pm 1\%$ in $\le 1\ \mu\text{s}$; inrush $\le 30$ mA; no overshoot above 1.10 V |
| PVT-Robust Design (240A Only) | Meet the above spec with SS, 125°C corner as well |


The AFE turning on *is* the load step: your LDO transient result and the
wake-up time are the same measurement.
