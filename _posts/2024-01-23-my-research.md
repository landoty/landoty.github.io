---
layout: post
title: Accelerating Rust Binary RE - Ideas and Goals
date: 2024-January-23
description: A short on my research goals
tags: research
---
In the rapidly-evolving landscape of binary reverse engineering, the emergence of new languages present new and unique challenges for analysts and reverse engineers. As I begin my graduate studies, I will be exploring novel techniques to accelerate Rust binary RE with the goal of providing a more efficient and effective experience to RE practicioners. 

## 1. Efficient Decompilation

Rust binaries, with their "inherent" safety, come with a price—larger size due to added runtime code for features like panics, stack unwinding, and bounds checking. My first goal is to develop a more efficient decompilation experience, enabling reverse engineers to navigate through these complexities and quicky identify user-written code emitted in the binary. 

## 2. Reconstructing the Memory Model

Understanding the memory model is paramount in Rust binary reverse engineering. My focus is on determining crucial components of the lifetime and ownership system used by Rust and then annotating these results in disassembly and decompilation. By deciphering these elements, analysts can more precisely identify the flow of data throughout Rust programs.

## 3. Harmonizing Calling Conventions

Rust's ability to emit varying calling conventions adds another layer of complexity to reverse engineering efforts. My goal is to harmonize these calling conventions, simplifying the tracking of parameters and their memory locations at the binary level. By bringing order to this variability, more precision is provided to the analyst and may also further assist decompilation.

## 4. Tool Development

Research is only impactful if it translates into practical tools that can be readily utilized by the community. My final goal revolves around developing user-friendly tools that cater to the aforementioned objectives. To ensure accessibility specific to the RE community, I will focus mainly on integrating my results with the Ghidra SRE suite.

Stay tuned for further updates on this work. Feel free to contact me if you are interested in collaborating! 