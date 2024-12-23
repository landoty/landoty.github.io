---
layout: page
title: Assesing Source Language for Binary Similarity
img: assets/img/bsim.png
importance: 1
category: security
---

This work is an early-stage investigation on the effects of source language on binary similarity tools. These tools are used broadly across the binary analysis space to quickly identify similar or identical binary code segments. Without access to source code, binary similarity is a difficult problem as even the slightest of modification in software build processes - optimization, debugging, etc. - can cause significant changes to the generated code. 

Further, the emergence of new systems programming languages has already demonstrated a unique challenge to binary analysts as unique, source-level code constructs manifest much differently in their generated binaries than those produced by languages like C. While ongoing work is underway to address these challenges, little (actually, none) has been done to determine if these issues cascade into binary similarity tools. 

As such, my first research thrust as a Master's student is to do just that - **Asses the Effects of Source Language on Binary Similarity Tools**. We have completed a first stage of research towards this objective and have submitted our work to a few peer-reviewed venues. Each submission is under review, so I include only the abstract and introduction here, but I hope to share the full work in the coming months. 

Thanks for reading! 

<object data="/assets/files/binary-sim-lang-abstract.pdf" type="application/pdf" width="100%" height="900"></object>
