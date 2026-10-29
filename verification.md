---
title: Deliverables
layout: default
nav_order: 7
---

## Verification and Deliverables

A concise project report must be submitted to Gradescope by December 5th
at 11:59 PM. The report should be typeset in a 12pt font with 1-inch
margins and include clearly marked, legible plots. The total length of
the report must be under 25 pages including figures. You must use the
following template:

1.  **Overview (1--2 pages)**: Complete schematic and basic description
    of circuit operation (including biasing), followed by a comparison
    table between the design specifications for each block and your
    achieved simulated results. Your schematic must include legible
    device types (NMOS/PMOS, 1 V/2 V), transistor sizes, and
    resistance/capacitance values for all passives next to each
    component. This section should not contain design discussion.

2.  **Design (3--5 pages)**: A brief discussion of your design approach,
    particularly how you mapped the provided block specifications to
    transistor-level parameters. Provide the fundamental equations used
    in your design and the high-level methodology for transistor sizing.
    Please detail your hand calculations and Cadence design approach
    here. You may separately attach any $g_m/I_D$ scripts used to size
    and bias your devices. Discuss your biasing network and techniques,
    as well as any modifications required during system-level
    integration. This section should not contain simulation results.

3.  **Results ($<15$ pages including figures)**: A
    discussion of circuit performance with special attention paid to
    unique areas in your design that helped or hindered your attempts to
    meet the specifications. This is the most important section, as it
    provides validation for your design. You must submit clear and
    legible plots for the following simulations from your testbenches:

    - **CTLE results**\
      (a) Power consumption and frequency responses of the CTLE across
      all settings.\
      (b) Eye diagrams before and after CTLE equalization for all
      channel models.\
      (c) Eye heights and eye widths after CTLE equalization for all
      channel models.

    - **LDO results**\
      (a) DC Voltage Output($V_\mathrm{DD,AFE}$ with all conditions of
      $V_\mathrm{DD,LDO}$, $I_\mathrm{LOAD}$\
      (b) Stability Analysis with all condition of $V_\mathrm{DD,LDO}$,
      $I_\mathrm{LOAD}$\
      (c) AC simulation result of PSRR with 50uA, 25mA of
      $I_\mathrm{LOAD}$\
      (d) Transient Voltage Output simulation of the Startup, Load step
      changes\
      (e) Quiescent current simulation of the Startup

    - **System-Level Results**:

      1.  Time-domain plots showing the wake-up (**WU**) and enable
          (**EN**) signals.

      2.  Zoomed-in transient plots showing the PRBS sequence input,
          channel output, CTLE output, PGA output (if applicable),
          comparator output, and the error flag. If your simulation has
          bit errors, zoom in on the specific time window where an error
          occurs. If your design achieves zero bit errors, zoom in on
          any valid data window after the enable signal is asserted.
          This must be done for each channel (A, B, and C).

      3.  A summary table comparing your system-level simulation results
          against each specification.

    - **EE240A Only (Corner Analysis)**: Discuss performance at SS /
      125C process corner, and $\pm 10\%$ supply variations.

    - **PGA Results (Optional)**: If you designed a custom PGA,
      summarize the relevant results, including (but not limited to):

      1.  Bode magnitude plots showing midband gain and 3 dB bandwidth.

      2.  Loop-gain magnitude, phase margin, and overall stability
          analysis.

4.  **Conclusion ($<2$ pages)**: A summary of your
    design experience. You should summarize the overall op amp
    characteristics, and should describe your overall experience and
    what you learned in doing this design. It would be good to get some
    feedback from you, both good and bad. Tell us if the design problem
    was worth the effort. (Your comments here will not adversely affect
    your grade.)