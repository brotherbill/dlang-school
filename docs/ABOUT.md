# dlang-school — What It Is and Who It's For
###### /dev/repos/dlang-school/docs/ABOUT.md

## What is dlang-school?

dlang-school is an offline learning and apprenticeship ecosystem for teaching young engineers Computer Science through the D programming language, secured with built-in parental controls. The curriculum covers:

- Computer science fundamentals
- Best practices and engineering discipline

This is university-level instruction, made accessible to young engineers by getting dirty — writing code and building small, interesting projects. The learning path expands on *Programming in D*, supported by a local companion app (the RCA), command-line interface (CLI) tooling, and a paired Reference + Adventure project model.

*Note: "RCA" (Reference + Companion App) is a working name used throughout this project until a brand name is finalized.*

---

## Who Is It For?

dlang-school is designed to serve four audiences:

- **Benny** — 2 hours/week, hands-on mentorship with Brother Bill.
- **10,000 young engineers** — self-paced with the RCA as daily companion.
- **Working developers** — adding D to their toolbox through structured, hands-on projects.
- **Correctional facility residents** — acquiring a real job skill in a controlled environment that satisfies institutional internet restrictions.

---

## Offline by Student, Safe by Design

While a student is using the RCA, no student-controlled internet access occurs. The student works entirely offline — no browsing, no external requests, no exposure to inappropriate content.

The RCA app checks daily for available updates and notifies the parent by email. No updates are applied without parental approval. Once approved, the RCA downloads and applies the update automatically. The parent as admin may also perform tooling upgrades manually, with the RCA guiding them through each step.

The RCA manages device-level parental controls directly. During installation, the RCA locks down the student's account to a whitelist-only internet model — only approved domains are reachable. The parent manages the whitelist through the RCA's built-in parental controls menu, protected by 2FA. No external parental control software is required.

The RCA's parental controls also govern the RCA itself — for example, enforcing shutdown between 8:30 PM and 3:30 PM on weekdays.

For technical details on the lockdown architecture, see [ARCHITECTURE.md](ARCHITECTURE.md), subsystem #8.

---

## Support

Three tiers of support serve the full range of parent technical ability:

1. **Self-service** — Documentation ships with the course. Parents who are comfortable with a terminal can follow the written procedures directly.
2. **RCA-managed** — For the standard case, the RCA handles parental controls setup and ongoing management through a GUI. No terminal. No documentation required.
3. **Remote tech support** — For genuine edge cases (broken hardware, corrupted installs, network problems), delegated technicians connect via RealVNC. $100/hour, one-hour minimum. Each additional 15 minutes or part thereof: $25. Five-minute free grace period after the hour.

---

## How Lessons Work

Every lesson is sequenced so the student experiences the problem before encountering the solution.

If you repeat the same five lines of code in three places, functions arrive as relief — not assignment. If working with fractions produces an explosion of variables, structs bring order — not abstraction for its own sake.

The student never hears "here is the next lesson." The student thinks "I need this."

By the time a concept appears — whether it is a language feature, a data structure, or a database query — the student has already felt the friction it eliminates. The skill becomes obvious, welcome, and worth committing to muscle memory.

Every Reference project is hand-crafted by Brother Bill — never by AI. The reference code is there to read, study, and understand. When the student enters an Adventure, they hand-type the reference code into an empty file. Copy-paste transfers characters. Hand-typing transfers understanding. There is a real difference in memory retention between the two, and dlang-school is built on the one that works.

---

## What the Course Covers

All projects in this course are greenfield toy projects — small, self-contained, and built from scratch.

The course covers most of the features of the D programming language. For each feature, we provide an opinion: which are standard practice, and which are to be avoided. We do this because professional D programmers will encounter features in production codebases that are no longer considered best practice. Those features still exist in the language. They still appear in other people's code. You will see them in the wild.

We are preparing you to enter that wild kingdom — eyes open, opinions formed, and ready to write clean code even when the code around you isn't.

---

## A Resource, Not a Certification

This is not a "certified" course in any manner. There are no quizzes and no exams.

Students may choose what lessons to take, and in what order, although the course is built to be taken in the order given. dlang-school is a resource, and it is not opinionated about how it is used.

dlang-school does not use Git or GitHub. Git is not appropriate for beginning software engineers — it introduces complexity that distracts from learning to code. The course provides a simpler versioning interface designed for young engineers. Students who are already familiar with Git and GitHub are welcome to use them independently.

---

## Core Philosophy

dlang-school is built on apprenticeship, not lectures.

| Principle | What it means |
|---|---|
| Learn by doing | Every lesson produces working code |
| Reference + Adventure | Hand-crafted examples typed from scratch, paired with creative exploration |
| Small steps, no overwhelm | Micro-increments; one concept per step |
| Version everything | Every artifact, every session, every milestone |
| No answer sheets | Students build understanding, not copy-paste muscle |
| Craftsmanship over speed | Quality of understanding over velocity of completion |
| Joy over drudgery | Engagement drives retention |
| Discipline over chaos | Deterministic workflows eliminate friction |

---

## Versioning

Every project evolves through versions, starting at version 1 with no upper limit:

- **hello-world-v1** — first attempt
- **hello-world-v2** — improved attempt
- **hello-world-v*N*** — as many iterations as the student needs to reach mastery

Versions are immutable once locked. The student always has a clean history of every attempt.

---

## Self-Guided Projects

Students may create their own projects outside the course materials. Self-guided projects are marked as "self-guided" and carry their own name, description, and version history.

Students can author their own lesson files within these projects. There is no structural difference between course-provided content and student-created content — the same tooling, versioning, and folder layout apply to both.

Self-guided projects encourage ownership, experimentation, and the transition from following lessons to initiating work independently.

If the student and parent believe that a self-guided project should be made generally available, they may submit it to the mothership for evaluation. If the project is accepted and added to the course, a free year of subscription will be extended.

---

## Journaling

Students may keep a journal within the RCA to document their frustrations, successes, and plans of attack. This is not graded, not reviewed, and not required — it is simply there for the student who wants to think out loud.

These journal entries become a living memory. Years from now, when that student is grown and has children of their own, they can open those entries and show their children that Mom or Dad wasn't any smarter than them at eleven years old. That is a gift no course grade can give.

Journal entries are private — as private as they can be when Mom or Dad is admin.

---

## Long-Term Vision

dlang-school becomes:

- The reference standard for learning D
- A lifetime subscription for young engineers
- A scalable apprenticeship model
- A community of disciplined builders
- A platform for future tools (caTools, etc.)
