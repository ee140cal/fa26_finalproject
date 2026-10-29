---
title: CTLE 
layout: default
nav_order: 3
---

## Block Specific Design Targets: CTLE

The CTLE must take channel C (16 dB at Nyquist for 140 and 23 dB for
240A), whose eye arrives closed, and open it to at least 0.6 UI and 45 %
of the settled amplitude.

### CTLE 
{:#ctle .unnumbered}

| Parameter | Specification |
| --- | --- |
| Boost (gain at 5 GHz minus DC gain) | 0 to $\ge 16$ dB for 140, $\ge 23$ dB for 240A, $\ge 3$ settings |
| Peak frequency | 5 GHz–7 GHz at every boost setting |
| Gain at 5 GHz | $\ge 0$ dB at every setting |
| Bandwidth above the peak ($-3$ dB) | $\ge 8$ GHz |
| Power | $\le 6$ mW from $V_{\mathrm{DD,AFE}}$ |
| Eye at CTLE output, channel C (best setting, PRBS7, noiseless) | Height $\ge 45\%$ of the settled long-run amplitude and $\ge 50\text{ mV}_{\text{pp}}$; width $\ge 0.6$ UI |
| Eye at CTLE output, channels A and B | Same, with one boost setting each that you report |

**Deliverables:** Before/after eyes for all three channels, and eye
height versus boost setting for channel C.

