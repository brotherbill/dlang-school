# notes/chat.md  
## dlang‑school Development Ledger  
### Append‑Only • Chronological • No Plans • No Tasks

---

### 2026‑04‑26 — Repository Initialization  
- Added `ARCHITECTURE.md` defining the system foundation.  
- Added `MY_RULES.md` establishing governance doctrine.  
- Confirmed RCA = Rich Client App (internal architectural term only).  
- Confirmed unlimited versioning model.  
- Confirmed positive‑only parental report philosophy.  
- Confirmed project‑name restrictions.  
- Confirmed badge = mastery doctrine.  

---

### 2026‑04‑26 — Repo Migration and File Restoration
- Migrated repo from C:/dev/ to C:/dev/repos/ to restore deterministic path structure.
- Corrected accidental nested folder (dlang-school/dlange-school) during migration.
- Restored MY_RULES.md from regenerated authoritative version.
- Restored ARCHITECTURE.md with correct header and full content.
- Confirmed that notes/chat.md is exempt from the 3‑line header rule.

### 2026‑04‑26 09:07 EDT — Document Split, Doctrine Formalization
- Identified audience mixing in README.md — architecture content mixed with product-facing language.
- Split into README.md (architecture/builder audience) and docs/ABOUT.md (parents, students, developers, institutional buyers).
- Removed Wednesday Execution Plan from README.md — one-time session content, not architecture.
- Relocated "If the adventure becomes a chore, the apprenticeship ends" to README.md Overview as governing principle.
- Removed all badge references from every document — no "badge" anything in this repo.
- Reduced subsystem count from nine to seven.
- Reworded rehydration triggers from session-based to project-state-based ("when project is finished for now" / "when ready to work on a dehydrated project").
- Added to ABOUT.md: A Resource, Not a Certification (no quizzes, no exams, unopinionated resource).
- Added to ABOUT.md: no Git/GitHub in the course — simpler versioning interface provided. Students welcome to use them independently.
- Strengthened ABOUT.md offline-first section — no student-controlled internet access. RCA parental controls scoped to itself only (8:30 PM – 3:30 PM weekday shutdown). Separate parental control apps recommended.
- Added to ABOUT.md: Journaling — frustrations, successes, plans of attack. Living memory. Private (as private as they can be when Mom or Dad is admin).
- Established terminology rule: "kids" = goat offspring, "children" = human offspring. Never use "kids" for human children. Saved to memory and added to MY_RULES.md.
- Generated docs/MY_RULES.md — full doctrine: terminology, naming, versioning, document governance, content rules, workflow constraints, architectural constraints.
- Regenerated docs/ARCHITECTURE.md as stub — subsystem contract placeholders, no duplicated content. Will expand as each subsystem reaches detailed design.
- Open: RCA is a working name — brand name TBD.

## Session — Sunday, 26 April 2026, afternoon

### Files Modified

- **ABOUT.md** — full regen (7 changes)
- **ARCHITECTURE.md** — full regen (3 additions to subsystem #1)

### Decisions Made

1. **Removed "Data structures" and "Database access" from curriculum list** — ABOUT.md lines 9–10 removed. Come back when real lessons exist.

2. **Section heading changed**: "Offline-First, Safe by Design" → **"Offline by Student, Safe by Design"** — the lockdown targets the student, not the machine. Parent has full internet. RCA has full internet.

3. **Parental approval required before updates** — RCA checks daily, notifies parent by email. No update is downloaded or applied without approval. Parent approves through RCA's 2FA-protected menu. Replaces prior language that implied RCA could auto-update.

4. **Outbound mail only** — RCA sends notification emails (SMTP out). No inbound mail processing. Email replies are not trusted. A reply cannot approve, modify, or trigger any action on the machine. Inbound email is a spoofing surface and a side door around 2FA.

5. **Recovery model — OS admin is the universal recovery channel** — Two authentication tiers:
   - Day-to-day: RCA password + 2FA (parental controls, whitelist, update approval)
   - Recovery: OS admin credentials via polkit/pkexec (forgot RCA password, forgot email, lost 2FA device, lockout)
   - No network, no email, no spoofing surface for recovery.
   - Forgot OS admin password = brick. Not RCA's problem.

6. **Three-tier support model added to ABOUT.md:**
   - Self-service (documentation)
   - RCA-managed (GUI, no terminal)
   - Remote tech support (RealVNC, $100/hr, 1hr minimum, $25/additional 15min, 5min grace)

7. **Hand-crafted by Brother Bill, never by AI** — Every Reference project is hand-crafted. Students hand-type the reference code when entering an Adventure. Copy-paste transfers characters; hand-typing transfers understanding. Added to "How Lessons Work" and updated the Core Philosophy table.

8. **New section: "What the Course Covers"** — All projects are greenfield toy projects. Course covers most D features with opinions on standard vs. avoid. Prepares students for legacy/non-best-practice features encountered in the wild.

9. **ARCHITECTURE.md subsystem #1 updated** — Three new bullet points: outbound mail, update flow, recovery model. Subsystem #8 was already consistent — no changes needed.

### Files Audited, No Changes Needed

- **MY_RULES.md** — clean. Today's changes were architectural and pedagogical, not naming or doctrine.
- **notes/wednesday-tasks.md** — clean. ARCHITECTURE.md already notes the .deb installer supersedes it.
- **README.md** — not reviewed (contents not provided). Flagged as potentially needing review if it carries a subsystem summary or feature overview.

### Open Items

- README.md audit — pending user paste
- Outbound mail subsystem — design details (SMTP provider, email templates, notification types) not yet specified
- RCA self-governance (shutdown hours 8:30 PM – 3:30 PM weekdays) — referenced in ABOUT.md, not yet in ARCHITECTURE.md


# End of notes/chat.md
