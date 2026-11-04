---
title: LDO
layout: default
nav_order: 4
---

## Block Specific Design Targets: LDO

The supply block must hold $V_\mathrm{DD,AFE}$ within
$\pm 1 \%$ while the whole AFE switches on, with no
external capacitor and only 200 mV of headroom.



### LDO 
{:#ldo .unnumbered}

| Parameter | Specification |
| --- | --- |
| $V_\mathrm{IN}$ / $V_\mathrm{OUT}$ | 1.2 V / 1.00 V |
| Dropout | $\le 200$ mV at 25 mA |
| Load range | 50 μA (standby) to 25 mA |
| Output capacitance | none external; $\le 300$ pF on chip in total, including AFE decap |
| Load step 50 μA $\to$ 25 mA in 1 ns | undershoot $\le 100$ mV; back within $\pm 1\%$ in $\le 50$ ns |
| Load step 25 mA $\to$ 50 μA in 1 ns | overshoot $\le 100$ mV; |
| Stability | phase margin $\ge 45^\circ$ across load |
| Load regulation | $\le 5$ mV over the $V_\mathrm{IN}$ range |
| Line regulation | $\le 0.5\%$ |
| PSRR | $\ge 40$ dB to 1 MHz, $\ge 15$ dB avergage to 10 GHz |
| Output noise (optional) | $\le 100\ \mu\text{V}_{\text{rms}}$, 1 kHz to 1 GHz |
| Quiescent current | $\le 300\ \mu\text{A}$ active, $\le 10\ \mu\text{A}$ standby |
| Cold start (Wake up) | $V_\mathrm{OUT}$ within $\pm 1\%$ in $\le 0.3\ \mu\text{s}$; no overshoot above 1.10 V |
| PVT-Robust Design (240A Only) | Meet the above spec with SS, 125°C corner as well |


The AFE turning on *is* the load step: your LDO transient result and the
wake-up time are the same measurement.

### Instructions to Copy the TB

You can copy the LDO test bench from the instructor account using the instructions below

1. Open the Terminal and go to the Cadence folder `cd ~/cadence`
2. To copy the LDO testbench `cp -r /home/ff/ee140/fa26/project_fa26/project_lib_LDO/ .`
3. Open Viruoso. In the **Library Manager** window, go to **Edit** $\rightarrow$ **Library Path...**. At the bottom of the list, add the above two libraries by giving a 
suitable name and the path on your local instructional account (to find this path, you can enter `pwd` in your terminal once you navigate 
into the copied folder)
4. Click **File** $\rightarrow$ **Save** (or press `Ctrl+S`), then close the Library Path Editor window. Library will now appear in your Library Manager. If not, click view -> refresh on the Library Manager
5. Try running simulation on the tb_AC_LDO, and change parameters if needed (You may have to add VDD on parameter with 1.2V)
