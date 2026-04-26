# dlang-school — Apprenticeship Ecosystem for Young D Engineers
###### /dev/repos/dlang-school/README.md

## Overview

dlang-school is the architecture, specification, and doctrine repo for teaching young engineers Computer Science through the D programming language. The learning path expands on *Programming in D*, supported by a local companion app (RCA), CLI tooling, and a paired Reference + Adventure project model.

If the adventure becomes a chore, the apprenticeship ends.

For product-facing information — audiences, curriculum philosophy and offline operation — see [docs/ABOUT.md](docs/ABOUT.md).

*Note: "RCA" is a working name used throughout this repo until a brand name is finalized.*

---

## Architecture Summary

The dlang-school ecosystem is composed of seven subsystems coordinated through a deterministic, date-based folder structure and governed by versioned doctrine documents.

All subsystems are designed for single-operator governance at the architecture level and zero-configuration student operation at the consumption level.

---

## Subsystem Map

### 1. RCA (Reference + Companion App)

The student's daily companion. A local application providing:

- Lesson navigation and Markdown rendering
- Progress tracking and visualization
- Version management and enforcement
- Project hydration and dehydration
- Parental controls
- Daily workflow guidance
- Reference project viewer
- Adventure project manager

### 2. Folder Layout

A deterministic, date-based directory structure:

```
projects/
    yyyy/
        mm/
            dd/
                project-name-v1/
                project-name-v2/
                project-name-v3/
```

This ensures clean history, reproducible debugging, no overwriting, and clear progression.

### 3. CLI Tools

Six tools enforce discipline across the workflow:

| Tool | Purpose |
|---|---|
| create-d-project | Scaffold a new project |
| dehydrate-d-project | Strip a project to its minimal snapshot |
| rehydrate-d-project | Restore a project to full operational state |
| version-next | Advance to the next version |
| version-lock | Freeze a version as immutable |
| open-version | Open a specific version for review |

These tools guarantee no corruption, no drift, no lost work, and consistent structure.

### 4. Versioning Model

Every project evolves through versions, starting at v1 with no upper limit:

- **hello-world-v1** — first attempt
- **hello-world-v2** — improved attempt
- **hello-world-v*N*** — as many iterations as the student needs to reach mastery

Rules:

- Versions are immutable once locked.
- Only one active version at a time.
- The RCA enforces version discipline.

### 5. Lesson Architecture

Each lesson expands on a concept from *Programming in D* as a Markdown file containing:

- Purpose
- Prerequisites
- Reference example
- Adventure task
- Micro-steps
- Reflection questions

Lessons must be short, clear, actionable, and testable.

### 6. SQLite Schema

The RCA stores all student state locally:

- Lessons completed
- Versions created
- Timestamps
- Adventure progress
- Parental control logs

The schema is simple, durable, and local-first.

### 7. Self-Guided Projects

Students may create their own projects outside the course materials. Self-guided projects are marked as "self-guided" and carry their own name, description, and version history.

Students can author their own Markdown lesson files within these projects. There is no structural difference between course-provided content and student-created content — the same tooling, versioning, and folder layout apply to both.

Self-guided projects encourage ownership, experimentation, and the transition from following lessons to initiating work independently.

If the student and parent believe that a self-guided project should be made generally available, they may submit it to the mothership for evaluation. If the project is accepted and added to the course, a free year of subscription will be extended.

---

## Rehydration Model

dlang-school uses a hydration/dehydration model to manage project state, enforced by CLI tooling.

- **Dehydration** (`dehydrate-d-project`) — When the project is finished for now, the project state is reduced to its minimal representation: source files, metadata, and progress markers. Build artifacts, caches, and transient state are stripped.
- **Rehydration** (`rehydrate-d-project`) — When ready to work on a dehydrated project, it is restored to a fully operational state from the dehydrated snapshot. Dependencies are resolved, the build environment is initialized, and the student resumes exactly where they left off.

This model ensures:

- Clean session boundaries — no stale state carries forward
- Minimal storage footprint per student
- Deterministic project recovery regardless of environment
- Version-safe transitions between curriculum updates

---

## Governing Documents

| Document | Path | Purpose |
|---|---|---|
| About | [docs/ABOUT.md](docs/ABOUT.md) | Product-facing overview for parents, students, and evaluators |
| Architecture | [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md) | System design, component specs, subsystem contracts |
| Rules | [docs/MY_RULES.md](docs/MY_RULES.md) | Doctrine, naming conventions, workflow governance |

All artifacts in this repo are governed by the documents above. No file is committed without alignment to doctrine.
