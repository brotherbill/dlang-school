# dlang‑school Architecture  
###### /dev/repos/dlang-school/docs/ARCHITECTURE.md  

## Purpose
dlang‑school is the apprenticeship ecosystem for teaching the D programming language to young engineers.  
It provides:

- a structured learning path  
- a reference project  
- an adventure project  
- a versioned workflow  
- a badge system  
- a local RCA (Reference + Companion App)  
- a 900‑lesson curriculum  
- a subscription model for families  

This repo contains the architecture, specifications, and doctrine that govern the entire system.

---

## Core Philosophy
dlang‑school is built on apprenticeship, not lectures.

The principles:

- Learn by doing  
- Reference + Adventure  
- Small steps, no overwhelm  
- Version everything  
- No answer sheets  
- Craftsmanship over speed  
- Joy over drudgery  
- Discipline over chaos  

The system must work for:

- Benny (2 hours/week with Brother Bill)  
- 10,000 young engineers (self‑paced with RCA)  

---

## System Components

### 1. RCA (Reference + Companion App)
The RCA is the local application that provides:

- lesson navigation  
- md rendering  
- badge tracking  
- version management  
- project hydration/dehydration  
- parental controls  
- daily workflow guidance  
- reference project viewer  
- adventure project manager  

The RCA is the student’s daily companion.

---

### 2. Folder Layout
A deterministic, date‑based structure:

