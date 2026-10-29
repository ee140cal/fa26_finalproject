---
title: Overview
layout: default
nav_order: 2
---

## Project Overview

For this project, you must design the following blocks of a 10 Gb/s NRZ
receiver front end for USB 3.2 Gen 2 standards.

- An appropriate, single-ended input/output **CTLE** to equalize each
  channel response in [Figure 1]({% link index.md %}#fig:channels).

- A fast wake-up **LDO** regulator that takes a 1.2 V Power and 0.6 V
  reference voltage, and wakes up within 300 ns of asserting the wake-up
  signal.

Specifications for each block are described in detail in later sections.

In addition, you *may* choose to design a broadband programmable gain
amplifier (PGA) to earn **extra credit**. There is no penalty for using
the ideal PGA provided in any of the testbenches. More details on this
are given in later sections.

![System block diagram of the fast-wakeup receiver link]({{ '/assets/images/fig_block_diagram.svg' | relative_url }}){: #fig:block_diagram style="width: 100%;"}
*Figure 4: System block diagram of the fast-wakeup receiver link. Highlighted blocks (**CTLE** and **LDO**) represent the primary project design deliverables. The yellow **PGA** block is optional.*

### General Instructions 
{:#general-instructions .unnumbered}

1.  You are to work on this design project individually. Although you
    can discuss generalities with other students in the class, you are
    not to perform the design in teams or groups. The chances of any two
    people coming up with the same exact design are very slim.
    Therefore, we expect to see different designs from each one of you.

2.  You should use the **gpdk045** process provided in this class. You
    may use any device provided in the library. All components
    (including any resistors or capacitors you add to the design) should
    be from gpdk045. You can use mimcap for a passive capacitor with
    voltage-independent capacitance, and the cells starting with "res"
    can be used for passive resistors. Do not use ideal components from
    analogLib in designing your amplifier. (You don't need to change the
    ideal components given to you in the testbenches.)

3.  You must keep the VGS and VDS of all devices lower than the limits
    of their device types (1.1V for the 1v devices and 1.8V for the 2v
    devices). Additionally, the power supply voltages should not exceed
    the corresponding operating range of the devices (for example, you
    can't place a 1V device between 1.8V and GND, even if its VGS and
    VDS are both smaller than 1.1V).

4.  NMOS body connections must be tied to ground. PMOS body connections
    may be tied to source.

5.  You may not use any ideal voltage sources for biaisng. Ideal voltage
    sources and a ground reference will be provided in the testbench.
    You will also be provided with an ideal current source in each
    testbench that you can use to bias your block. Improper biasing
    (e.g. biasing a pmos with a nmos) will result in point deductions.

6.  You will receive 3 testbenches in total which test individual blocks
    and system-level performance metrics. You are also certainly not
    limited to these testbenches, and may find it helpful to generate
    your own - however, do note that your final specifications may be
    checked with these setups.

7.  You must meet specifications for each individual block and the
    system-level tests. Details are provided in the following sections.

8.  Final report is due on **December 5th** (Saturday) by **11:59 PM**
    on Gradescope. We will have a poster presentation on **December
    4th** (Friday) from 4 PM to 7 PM in Gateway Building 1016 (where
    Friday discussion sessions meet). More details about the
    presentation will be shared soon.

## Timeline and Check-ins 
{:#timeline-and-check-ins .unnumbered}

We will enforce the following schedule to help you complete the project
on time. We encourage you to work ahead if possible. There will be two
check-ins during the lab hours where you have to show relevant results
and progress on design to the lab GSIs.

- **Weeks 1--2**: CTLE design. Tasks include identifying the transfer
  function required to equalize each channel, selecting an appropriate
  equalizer topology, and sizing small-signal and large-signal
  parameters to meet standalone CTLE specifications. Keep in mind that
  the CTLE is powered by the LDO you will design, so consider these
  supply constraints when making design choices.

- **Check-in during Week 2**: You are expected to complete the relevant
  MATLAB/Python sims, and make significant progress on the
  transistor-level design for the CTLE. You will be checked-off by the
  lab GSIs.

- **Weeks 3--4**: LDO design. Tasks include determining open-loop
  specifications for the error amplifier, ensuring stability during
  wake-up transients, and meeting requirements for line/load regulation
  and settling time.

- **Check-in during Week 4**: You are expected to complete the relevant
  ideal models for the LDO error amplifier and make significant progress
  on the transistor-level design for the amplifier, pass transistor, and
  stability analysis. You will be checked-off by the lab GSIs.

- **Week 5**: Integration. Integrate the LDO and CTLE into the receiver
  signal chain shown in
  Figure [4](#fig:block_diagram) and fine-tune the circuit as necessary
  to meet all system-level specifications.

## Suggested Reading 
{:#suggested-reading .unnumbered}

To help you get started, we have provided a curated list of reading
materials on bCourses. Please review them to gain a deeper understanding
of each circuit block.
