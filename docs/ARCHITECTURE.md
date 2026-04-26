# dlang‑school Architecture

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

```
projects/
    yyyy/
        mm/
            dd/
                project-name-v1/
                project-name-v2/
                project-name-v3/
```

This ensures:

- clean history  
- reproducible debugging  
- no overwriting  
- clear progression  

---

### 3. CLI Tools
The CLI tools enforce discipline:

- create-d-project  
- dehydrate-d-project  
- rehydrate-d-project  
- version-next  
- version-lock  
- open-version  

These tools guarantee:

- no corruption  
- no drift  
- no lost work  
- consistent structure  

---

### 4. Versioning Model
Every project evolves through versions:

- v1 → first attempt  
- v2 → improved attempt  
- v3 → mastery attempt  

Rules:

- versions are immutable once locked  
- only one active version at a time  
- RCA enforces version discipline  

---

### 5. Lesson Architecture
Each lesson is an md file with:

- purpose  
- prerequisites  
- reference example  
- adventure task  
- micro‑steps  
- badges  
- reflection questions  

Lessons must be:

- short  
- clear  
- actionable  
- testable  

---

### 6. Badge System
Badges reward:

- completion  
- mastery  
- debugging  
- discipline  
- creativity  
- consistency  

Badges are stored locally in SQLite.

---

### 7. SQLite Schema
The RCA stores:

- lessons completed  
- badges earned  
- versions created  
- timestamps  
- adventure progress  
- parental control logs  

The schema is simple, durable, and local‑first.

---

### 8. Wednesday Execution Plan
Every tutoring session ends with:

- a living adventure  
- clear expectations  
- a versioned project  
- a badge earned  
- a next step  

If the adventure becomes a chore, the apprenticeship ends.

---

## Long‑Term Vision
dlang‑school becomes:

- the reference standard for learning D  
- a lifetime subscription for young engineers  
- a scalable apprenticeship model  
- a community of disciplined builders  
- a platform for future tools (caTools, etc.)  

This repo is the foundation.
