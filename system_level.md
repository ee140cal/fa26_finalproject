---
title: System-level TB
layout: default
nav_order: 5
---

## System-level Specifications

You will be provided with a system-level testbench to integrate your
CTLE and LDO into an otherwise ideal receiver chain. You will delete the
CTLE and LDO blocks given and replace them with your blocks.

Key testbench behaviors and operational sequences are defined below:

- **Wake-up**: At $t = 50$ ns, a pulse (**WU**) with a 100 ps rise
  time is asserted to the reference (VREF) of the LDO. The LDO must
  settle within 300 ns after **WU** reaches 0.6 V. Settling is defined
  identically to the standalone LDO testbench.

- **Enable**: Once the LDO settles, an enable generator monitors the
  supply line. If the regulated rail remains stable within specification
  for 50 ns (500 clock cycles), an enable signal (**EN**) is asserted to
  activate the bit-checker.

- **Channel Selection**: You can toggle between the three channels using
  the one-hot selection control bits `<sel_a, sel_b, sel_c>`. For
  example, `<010>` selects Channel B.

- **Input Bias Setting**: You must set up the input common-mode bias for
  the CTLE on the testbench, which automatically sets the comparator
  threshold voltage ($V_\mathrm{REF}$).

- **Error Flag**: An error flag (**ERR**) is asserted high whenever the
  comparator decision disagrees with the bit-checker's expected
  sequence. You must achieve zero bit errors to pass the system-level
  testbench.

The testbench includes a PRBS7 sequence generator clocked at 10 GHz.
This signal has a 400 mV peak-to-peak swing centered at 500 mV
($V_{DD}/2$) and passes through the channel to the CTLE input even prior
to wake-up (**WU** = 0). The CTLE output directly drives the comparator
throughout this phase. Decisions made while the LDO ramps up are ignored
because the bit-checker remains disabled. Once **WU** is asserted and
**EN** goes high, the bit-checker evaluates every decision against the
expected PRBS pattern and flags any mismatch.

In this testbench, you must set the input bias ($V_\mathrm{bias}$) for
your CTLE according to your design. Nominally, this is set to
$V_{DD}/2 = 0.5\text{ V}$. This bias voltage is also used to set the
reference voltage of the comparator to
$V_\mathrm{bias} + 200\text{ mV}$. You may adjust this reference voltage
in the testbench depending on your CTLE's output bias point.

The testbench also includes an ideal PGA that can help you tune the
midband gain of your receiver so that the comparator in
Figure [4]({% link project_overview.md %}#fig:block_diagram) receives a full-scale input.

Because the testbench introduces no noise, a properly designed CTLE
should open the eye sufficiently to achieve zero bit errors over the
entire transient simulation. Once again, ensure that you set the input
bias and comparator reference voltages correctly. If you need to make
any specific modifications to the testbench to accommodate your design,
please consult the GSIs first.

| Parameter | Specification |
| --- | --- |
| Data rate | 10 Gb/s NRZ (UI $= 100\text{ ps}$, Nyquist $= 5\text{ GHz}$) |
| External supply reference | $V_\mathrm{REF} = 0.6\text{ V}$ |
| Regulated supply voltage | $V_\mathrm{DD} = 1.0\text{ V}$ (same as LDO target) |
| LDO Settling Time | 300 ns (same as LDO target) |
| Channels | (**140**) A/B/C: 4/10/16 dB loss at 5 GHz<br> <br>(**240A**) A/B/C: 10/16/23 dB loss at 5 GHz |
| Bit-error rate (BER) | Zero errors over test sequence |