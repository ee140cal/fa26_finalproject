---
title: Extra Credit (PGA)
layout: default
nav_order: 6
---





## Guidelines for PGA Design (Extra Credit)

If you choose to design a custom PGA instead of using the ideal model in
the testbench, please keep the following guidelines in mind:

1.  An open-loop PGA will likely fail to meet requirements in the real
    world, as your design must be robust across PVT variations and
    mismatch. A topology utilizing feedback is necessary, which demands
    careful stability analysis while simultaneously achieving a high
    gain-bandwidth product. This represents a significant design
    challenge.

2.  To start your design, we recommend reviewing the Cherry-Hooper
    amplifier architecture in the provided reading materials. As a
    reminder, passive inductors are prohibited in this project, meaning
    traditional inductive shunt-peaking techniques are not allowed.

3.  If you decide to take on this challenge, please prioritize finishing
    the required blocks (CTLE and LDO) on time and ensuring they meet
    all specifications first. Once the core system is functioning, we
    encourage you to discuss your ideas for implementing a broadband
    amplifier with the course staff.