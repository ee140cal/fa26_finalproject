---
title: Motivation
layout: home
nav_order: 1
math: mathjax
---

# EE140/240A Final Project 
{: style="text-align: center;"}
## A Fast Wake-Up USB 3.2 Gen 2 Receiver Analog Front End
{: style="text-align: center;"}



We will use this site to handle the any project updates. 


## Motivation: Why a Reciver Front End?


### The rate doubles every generation; the wire does not get better
{: #the-rate-doubles-every-generation-the-wire-does-not-get-better .unnumbered}

USB speed has progressed from **480 Mb/s** (USB 2.0) to **5 Gb/s** (USB 3.0), to **10 Gb/s** (USB 3.2 Gen 2), and up to **20 Gb/s** per lane in USB4. Through all of it, the bits still travel over copper: a cable, two connectors, and several inches of circuit-board trace on each end. Every other wireline standard tells the same story (see Table 1 below). 

In this project, we will specifically focus on **USB 3.2 Gen 2** with a per-lane rate of **10 Gb/s**.

---

### Table 1: Per-lane rates of common serial links (NRZ signaling)

*Newer generations (PCIe 6.0, USB4 v2.0) switch to multi-level signaling to halve the Nyquist frequency—a sign of how hard the channel has become.*

| Standard | Rate per lane | Nyquist frequency |
| :--- | :--- | :--- |
| **USB 2.0** | 480 Mb/s | 240 MHz |
| **USB 3.2 Gen 1** | 5 Gb/s | 2.5 GHz |
| **USB 3.2 Gen 2** | 10 Gb/s | 5 GHz |
| **USB4 Gen 3** | 20 Gb/s | 10 GHz |
| **PCIe 3.0 / 4.0 / 5.0** | 8 / 16 / 32 GT/s | 4 / 8 / 16 GHz |
| **10GBASE-KR (backplane)** | 10.3125 Gb/s | 5.16 GHz |
| **HDMI 2.1 (FRL)** | 12 Gb/s | 6 GHz |

---

### Where the signal goes: loss
{: #where-the-signal-goes-loss .unnumbered}

A transmission line in a USB 3.2 high-speed link loses energy through
two primary physical mechanisms: skin-effect conductor loss and
dielectric loss. Conductor loss grows with the skin effect, where
current crowds into a skin depth
$\delta=\sqrt{\rho/(\pi f\mu_0 \mu_r)}$, causing the series resistance
and attenuation in dB to scale as $\sqrt{f}$. Dielectric loss grows
linearly as $f\tan\delta$; on standard FR-4 board substrates
($\tan\delta\approx 0.02$), it dominates above a few gigahertz. A
standard analytical loss model is given by:


$$\text{Loss}(f)\;[\mathrm{dB}] \;=\; \ell\left(k_s\sqrt{f}+k_d\,f\right),
  \label{eq:loss}$$ 
  
  
  where $\ell$ represents the total physical link
length.


Under the USB-IF specification for USB 3.2 Gen 2 (10 Gb/s, Nyquist
frequency $f_N =$ 5.0 GHz), the maximum compliance insertion loss
budget is 23 dB at Nyquist [[1]({% link references.md %}#ref-usb31)]. Spanning from the
host PCB and package, through a USB Type-C cable, to the device PCB and
package, this total channel loss budget consists of:

- **Host PCB trace & package:** up to 8.5 dB at 5.0 GHz

- **USB-C to USB-C cable assembly:** up to 6 dB at 5.0 GHz

- **Device PCB trace & package:** up to 8.5 dB at 5.0 GHz

Let us examine the implications of the frequency-dependent attenuation
introduced by the channel. In
Figure [1](#fig:channels), we model the insertion loss transfer function
for three representative channels: Channel A, B, and C, with Nyquist
losses of 4 dB, 10 dB, and 16 dB at 5.0 GHz, respectively.


![USB 3.2 Gen 2 channel model]({{ '/assets/images/fig_channels_cv.svg' | relative_url }}){: #fig:channels style="width: 100%;"}
*Figure 1: The USB 3.2 Gen 2 channel model. **Left:** Channel insertion loss $|H(f)|$ up to Nyquist (5.0 GHz). **Center:** Transmitted (TX) NRZ spectrum versus the distorted spectrum received after channel attenuation. **Right:** Time-domain response to a single 1 V, 1-UI transmit pulse, demonstrating severe peak attenuation and intersymbol interference (ISI).*

The center plot in Figure [1](#fig:channels) illustrates the power spectral density (PSD)
of the transmitted Non-Return-to-Zero (NRZ) data stream alongside the
degraded spectra arriving at the receiver. To prevent data corruption,
the receiver's analog front end must equalize this spectrum.

The rightmost plot in Figure [1](#fig:channels) demonstrates the time-domain impact of this
spectral attenuation. When a single 1 V pulse with a duration of one
Unit Interval (UI) is transmitted, channel attenuation severely degrades
its peak amplitude while dispersion spreads its energy over neighboring
bit periods. Note that one Unit Interval is defined as the inverse of
the data rate:
$$\text{UI} = \frac{1}{\text{Data Rate}} = \frac{1}{10 \, \text{Gb/s}} = 100 \text{ps}.$$

### Loss is not the whole story: dispersion and ISI 
{: #loss-is-not-the-whole-story-dispersion-and-isi .unnumbered}


A physical channel is causal, so its phase response is fundamentally
tied to its magnitude response: a channel whose loss varies with
frequency also exhibits frequency-dependent delay (dispersion). Because
low-frequency signal components travel slower than high-frequency
components, energy at different frequencies arrives at different times.

Attenuation and dispersion together smear each transmitted bit into its
neighbors. As shown in the time-domain pulse response
(Fig. [1](#fig:channels), right), a 1 V, 1-UI pulse transmitted through Channel C peaks at only
0.39 V at the receiver. The subsequent bit period sees 0.19 V of
residual tail energy, and energy persists for more than ten UI. Summed
over ten UI, the post-cursor interference exceeds the main cursor
amplitude, completely closing the eye in worst-case data patterns. This
distortion is *intersymbol interference* (ISI), and it is the primary
non-ideality your analog front end must undo.

### Transmission-Line Reflection and Termination 
{: #transmission-line-reflection-and-termination .unnumbered}


In high-speed links, impedance discontinuities cause signal reflections,
which can introduce ringing and ISI, degrading the received eye opening.
The reflection coefficient is given by
$$\Gamma = \frac{Z_L-Z_0}{Z_L+Z_0},$$ where $Z_0$ is the characteristic
impedance of the transmission line and $Z_L$ is the load impedance.
Consider a single-ended link consisting of a transmitter with a 50 Ω
output resistance, a 50 Ω lossy transmission line, and a receiver with
or without a 50 Ω termination. With a matched receiver termination,
$\Gamma=0$, and no signal is reflected at the receiver. In contrast, an
unterminated receiver has $\Gamma\approx+1$, causing the incident wave
to reflect back toward the transmitter. Although this can approximately
double the received voltage amplitude, in practical SerDes systems,
process variations and packaging parasitics can cause the transmitter
output impedance to deviate from the characteristic impedance of the
transmission line. Consequently, the reflected wave from the receiver
may be reflected again at the transmitter and propagate back toward the
receiver, introducing delayed echoes and additional ISI, which degrades
the received eye opening. Therefore, proper receiver termination is
important for minimizing reflections and maintaining signal integrity in
high-speed links.

### Visualizing Link Health: The Eye Diagram 
{: #visualizing-link-health-the-eye-diagram .unnumbered}

To evaluate the impact of intersymbol interference (ISI), noise, and
equalization on a high-speed data stream, we wrap and overlay
consecutive time-domain segments of a long pseudo-random bit sequence
(PRBS) across a fixed window of two Unit Intervals (2 UI). The resulting
composite plot is an *eye diagram* [[2]({% link references.md %}#ref-razavi2012optical)].

The step-by-step construction of an eye diagram is illustrated in
Figure [2](#fig:eye_construction):

1.  **Continuous PRBS Stream:** A 10 Gb/s PRBS data sequence is
    transmitted through the channel.

2.  **2-UI Partitioning:** The time-domain response (shown for
    Channel A) is partitioned into consecutive, non-overlapping 2 UI
    time slices ( through ).

3.  **Overlaying Segments:** Folding these 2 UI slices onto a common
    time axis constructs the basic eye pattern.

![Eye Construction]({{ '/assets/images/fig_eye_construction.svg' | relative_url }}){: #fig:eye_construction style="width: 100%;"}
*Figure 2: Construction of an eye diagram. **Top:** A 10 Gb/s PRBS input pattern
and corresponding time-domain responses for Channels A, B, and C.
**Center:** Partitioning the Channel A response (4 dB loss) into five
consecutive 2 UI segments. **Bottom:** Overlapping these 2 UI segments
onto a single time base reveals the open eye
structure.*

An eye diagram directly yields three critical system-level performance
metrics:

- **Eye Height (Vertical Opening):** Measured at the center of the UI,
  representing the minimum noise margin available to the decision
  circuit (slicer/comparator).

- **Eye Width (Horizontal Opening):** Measured at the differential
  zero-crossing, representing the timing margin available for clock and
  data recovery (CDR) sampling.

- **Jitter & Crossover Spreading:** The horizontal thickness of the
  zero-crossings, reflecting edge-timing uncertainty caused by
  deterministic ISI and random noise.

![High Density Eye]({{ '/assets/images/fig_high_density.svg' | relative_url }}){: #fig:high_density style="width: 100%;"}
*Figure 3 Simulated 1,000-bit PRBS7 eye diagrams at the output of the three
project channels [[2]({% link references.md %}#ref-razavi2012optical)]. **Channel A (4 dB loss):** Open
eye with large vertical and horizontal margins. **Channel B (10 dB
loss):** Reduced eye height due to moderate ISI. **Channel C (16 dB
loss):** Completely closed eye where ISI trajectories cross through the
sampling center, causing severe bit errors without
equalization.*


As channel loss increases across long pseudo-random sequences, trailing
post-cursors spill into adjacent bit periods, progressively degrading
the signal as demonstrated in Figure [3](#fig:high_density).

A closed eye means a simple slicer cannot distinguish logic '1's from
'0's, resulting in an unacceptably high Bit Error Rate (BER). The
primary objective of your receiver front end is to invert channel
distortion and reopen the eye prior to slicing.


### What the Front End Does 
{:#what-the-front-end-does .unnumbered}

A basic wireline analog front end (AFE) performs several essential
signal-conditioning functions:

- **Termination** absorbs the incoming wave to minimize reflections at
  impedance discontinuities, such as connectors, package vias, and chip
  bond pads.

- **Channel Equalization** boosts high frequencies to approximately
  invert the frequency-dependent channel loss. The trade-off is that it
  also amplifies high-frequency noise and crosstalk, so more boost is
  not always better.

- **Gain Stages** bring the equalized signal up to the voltage swing
  required by the slicer. A gain stage must remain linear---as
  non-linear distortion directly induces ISI---and is typically
  programmable to accommodate varying channel lengths.

- **Stable Reference and Regulation** keep supply nodes quiet, as supply
  noise on the gain stages lands directly on the signal eye.

In this project, you will focus on two critical AFE blocks: a
**continuous-time linear equalizer (CTLE)** for channel inversion and a
**low-dropout (LDO) voltage regulator** for supply stability.

### Why Fast Wake-Up 
{:#why-fast-wake-up .unnumbered}

High-speed links sit idle most of the time. Standards like USB 3.2
define low-power states (U1/U2/U3) to eliminate idle power dissipation
[[3]({% link references.md %}#ref-usb32)]. While deep states (U2/U3) allow $\ge 80 \, \mu s$ for
PLL cold-starts, fast standby (**U1**) mandates an aggressive **900 ns**
wake-up response upon sensing a Low Frequency Periodic Signal burst
[[3]({% link references.md %}#ref-usb32)].

To ensure the receiver front end (AFE) is operational within the total
900 ns wake-up window, 300 ns is allotted for the LDO regulator to turn
on and settle $V_{\text{DD\_AFE}}$ within $1.00\text{ V} \pm 2\%$
($0.98\text{--}1.02\text{ V}$). To prevent false triggering from ringing
or higher-order transient overshoot, $V_{\text{DD\_AFE}}$ must remain
continuously inside this $\pm 2\%$ window for at least 50 ns
(500 clock   cycles at 10 GHz) before the bit checker enable signal
latches HIGH. The remainder of the 900 ns budget is reserved for
internal AFE RC nodes to settle to steady state.

Waking and settling in nanoseconds allows the receiver to sleep during
micro-gaps between packet bursts, enabling energy-proportional operation
where power scales directly with transmitted payload data.

