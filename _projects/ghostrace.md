---
layout: page
title: Reproducing GhostRace
description: EECS750 - Advanced Operating Systems - Final Project
img: assets/img/spectre.png
importance: 1
category: security
---

This past semester I took **Advanced Operating Systems** with [Dr. Heechul Yun](https://www.ittc.ku.edu/~heechul/), which covered topics in the design and analysis of operating systems for modern hardware platforms. Topics included multicore scheduling, virtualizatin, cache and DRAM management, security, and heterogeneous computing. 

For this course, we were required to conduct a half semester-long project, building on the knowledge gained from the course as well as supplemental research paper readings. My partner, [Cole Strickler](https://github.com/ColeStrickler), and I decided to investigate findings from [**GhostRace**](https://www.vusec.net/projects/ghostrace/), a paper published this year in USENIX Security. GhostRace presents *Speculative Race Conditions*, showing that all common synchronization primitives can be microarchitecturally bypassed. Further, they show that these primitives can be manipulated to perform Spectre-like information disclousre attacks.

Our report is included below and the source code can be found [here](https://github.com/ColeStrickler/EECS750-FinalProject/).

<object data="/assets/files/ghostrace-final.pdf" type="application/pdf" width="100%" height="900"></object>