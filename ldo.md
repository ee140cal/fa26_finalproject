---
title: LDO
layout: default
nav_order: 4
---

## Block Specific Design Targets: LDO

The supply block must hold $V_\mathrm{DD,AFE}$ within
$\pm 1$ % while the whole AFE switches on, with no
external capacitor and only 200 mV of headroom.
All specifications should be met at $I_{\mathrm{LOAD}} = 50\\mu\mathrm{A}$ and $25\\mathrm{mA}$, unless otherwise specified.


### LDO 

| Parameter | Specification | Measurement |
| --- | --- | --- |
| $V_\mathrm{IN}$ / $V_\mathrm{OUT}$ | 1.2 V / 1.00 V $\pm 1$ %| DC simulation at $I_{\mathrm{LOAD}} = 50\\mu\mathrm{A}$ and $25\\mathrm{mA}$ |
| $V_\mathrm{REF}$ / $I_\mathrm{REF}$ | 0.4 V to 0.8 V / 1uA | Your Choice of $V_\mathrm{REF}$ to meet specification |
| Dropout | $\le 200$ mV at 25 mA ||
| Load range | 50 μA (standby) to 25 mA ||
| Total capacitance | none external; $\le 300$ pF on chip in total, including output, compensation, decaps ||
| Load step 50 μA $\to$ 25 mA in 25 ns | undershoot $\le 100$ mV; back within $\pm 1\%$ in $\le 50$ ns after Load current becomes 25mA| Transient Sim, you can use ipwl or ipulse |
| Load step 25 mA $\to$ 50 μA in 25 ns | overshoot $\le 100$ mV; | Transient Sim, you can use ipwl or ipulse |
| Stability | phase margin $\ge 45^\circ$ across load | Stability Sim, at $I_{\mathrm{LOAD}} = 50\\mu\mathrm{A}$ and $25\\mathrm{mA}$ |
| Load regulation | $\le 5$ mV over the $I_{\mathrm{LOAD}}$ range | DC Sim, Check $V_\mathrm{OUT}$ at $I_{\mathrm{LOAD}} = 50\\mu\mathrm{A}$ and $25\\mathrm{mA}$ |
| Line regulation | $\le 0.5\%$ | AC Sim, Check $\frac{dV_{\mathrm{out}}}{dV_{\mathrm{in}}}$ at 1Hz|
| PSRR | $\ge 40$ dB to 1 MHz, $\ge 15$ dB average to 10 GHz | AC Sim at 50µA and 25mA, Average over Log-scale, please see below for detailed instruction for averaging|
| Output noise (optional) | $\le 100\ \mu\text{V}_{\text{rms}}$, 1 kHz to 1 GHz |
| Quiescent current | $\le 300\ \mu\text{A}$ active (WU=1), $\le 10\ \mu\text{A}$ when sleeping. (WU=0 and $I_{\mathrm{LOAD}} = 0$) |DC Sim, Total Current Flows at VIN - $I_{\mathrm{LOAD}}\$  |
| Cold start (Wake up) | $V_\mathrm{OUT}$ within $\pm 1\%$ in $\le 3\ \mu\text{s}$; no overshoot above 1.10 V |Transient Sim, please see below for detailed instruction|
| PVT-Robust Design (240A Only) | Meet the above spec with SS, 125°C corner as well |


### Instructions to PSRR average calculation
PSRR Average is measured by the average of the PSRR, scaled by log-scale x.
You should measure the PSRR average, by using at least 20 points per decade AC simulation from 1Hz to 10GHz.
For example, you can use this equation in maestro (If you have AC magnitude of 1 source at VIN)

(integ(clip(((- dB20(VF("/VOUT"))) / xval(VF("/VOUT"))) 1 1e+10)) / 23)

each function does : 

1. integ : integration 
2. clip : clip between 1Hz to 10GHz 
3. xval : returns the freqeuncy 
4. -dB20 : calculates PSRR 
5. 23.02585093 : ln(10GHz/1Hz), which is distance of x axis

### Instructions for Wake-up Simulation
Wake-up is controlled by a separate WU signal. Keep VIN at 1.2 V and keep VREF and IREF at their selected nominal values. Do not ramp VREF to initiate wake-up.
Shutdown (WU = 0 V): You can choose your own strategy for the Shutdown. Example : Disable the internal bias currents and pull the pass PMOS gate to VIN to turn it off. Discharge VOUT to 0 V before starting the wake-up test. Set Load current as $I_{\mathrm{LOAD}} = 0\mathrm{A}$ for Shutdown.
Wake-up: Raise WU from 0 V to 1.2 V with a 1ns rise time. Raise $I_{\mathrm{LOAD}} = 0\mathrm{A} to 50\\mu\mathrm{A}$ with a 100ns rise time. Enable the bias circuitry, release the pass-gate clamp, and turn off any output-discharge circuit.
Regulation: The error amplifier takes control of the pass transistor and charges the output capacitor until VOUT reaches 1.0 V.
VOUT must enter and remain within 0.99–1.01 V within 3 µs of this time (3 µs after WU = 1.2V), and must not exceed 1.10 V during startup.

   
### Instructions to Copy the TB

You can copy the LDO test bench from the instructor account using the instructions below

1. Open the Terminal and go to the Cadence folder `cd ~/cadence`
2. To copy the LDO testbench `cp -r /home/ff/ee140/fa26/project_fa26/project_lib_LDO/ .`
3. Open Viruoso. In the **Library Manager** window, go to **Edit** $\rightarrow$ **Library Path...**. At the bottom of the list, add the above two libraries by giving a 
suitable name and the path on your local instructional account (to find this path, you can enter `pwd` in your terminal once you navigate 
into the copied folder)
4. Click **File** $\rightarrow$ **Save** (or press `Ctrl+S`), then close the Library Path Editor window. Library will now appear in your Library Manager. If not, click view -> refresh on the Library Manager
5. Try running simulation on the tb_AC_LDO, and change parameters if needed (You may have to add VDD on parameter with 1.2V)

- If you use different name for the library, you might lose the equations on maestro. Please use same name (project_lib_LDO) when you first copy the library.

### Checkoff Contents
1. Build your own testbench or add equation, to obtain the specification in the table.
2. Determine the initial specifications needed for ideal 1-pole OP Amp (Gain / BW), Passives(R1,R2), Pass TR Size and describe how you determined those specs
3. Show the plots for each specification, with the ideal 1-pole OP Amp (No need for meet all specs at this point, but just showing that your testbench works for generating specs!)





