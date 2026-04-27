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

---

### 2026‑04‑26 09:07 EDT — Document Split and Doctrine Formalization  
- Split README.md into README.md (architecture) and docs/ABOUT.md (parents, students, developers, buyers).  
- Removed Wednesday Execution Plan from README.md — one‑time session content.  
- Relocated governing principle to README.md Overview: "If the adventure becomes a chore, the apprenticeship ends."  
- Removed all badge references from every document.  
- Reduced subsystem count from nine to seven.  
- Reworded rehydration triggers from session‑based to project‑state‑based.  
- Added to ABOUT.md: A Resource, Not a Certification; no Git/GitHub in the course; offline‑first; journaling.  
- Established terminology rule: "kids" = goat offspring, "children" = human offspring.  
- Generated docs/MY_RULES.md with full doctrine.  
- Regenerated docs/ARCHITECTURE.md as stub with subsystem contract placeholders.  
- Noted RCA is a working name — brand name TBD.  

---

### 2026‑04‑26 afternoon — ABOUT.md and ARCHITECTURE.md Updates  
- Regenerated ABOUT.md with seven changes; regenerated ARCHITECTURE.md with three additions to subsystem #1.  
- Removed "Data structures" and "Database access" from curriculum list — come back when real lessons exist.  
- Changed section heading to "Offline by Student, Safe by Design" — lockdown targets the student, not the machine.  
- Added parental approval required before RCA updates — no auto‑updates.  
- Established outbound mail only — RCA sends SMTP out, no inbound mail processing, email replies not trusted.  
- Defined recovery model — OS admin is the universal recovery channel with two authentication tiers.  
- Added three‑tier support model to ABOUT.md (self‑service, RCA‑managed, remote tech support).  
- Established hand‑crafted reference projects — every reference project hand‑crafted by Brother Bill, never by AI.  
- Added "What the Course Covers" section — greenfield toy projects, most D features with opinions.  
- Updated ARCHITECTURE.md subsystem #1 with outbound mail, update flow, recovery model.  
- Audited MY_RULES.md and notes/wednesday‑tasks.md — no changes needed.  
- Flagged README.md for review — pending user paste.  
- Open: outbound mail subsystem design details not yet specified.  
- Open: RCA self‑governance shutdown hours referenced in ABOUT.md, not yet in ARCHITECTURE.md.  

---

### 2026‑04‑26 17:41 EDT — Benny Account Network Lockdown  
- Configured nftables on Pop!_OS to restrict benny (UID 1000) to loopback and matias.ca only.  
- Applied IPv4 and IPv6 rules — ACCEPT loopback, ACCEPT matias.ca (116.203.249.104 / 2a01:4f8:c010:a08c::1), DROP all else.  
- Diagnosed IPv6 bypass — Google resolved to IPv6, missing DROP rule allowed traffic through.  
- Added IPv6 DROP rule via nft to close the gap.  
- Removed duplicate IPv4 DROP rule.  
- Persisted full ruleset to /etc/nftables.conf using nft list ruleset (ip6tables‑save incompatible with nft‑added rules).  
- Updated systemd service restore‑iptables.service to restore via nft ‑f /etc/nftables.conf.  
- Verified lockdown: admin gets 301 from Google, benny gets 000 (connection dropped).  
- Created docs/benny‑lockdown.md documenting rules, verification, persistence, and troubleshooting.  

### 2026‑04‑26 20:07 EDT — DNS‑Based Lockdown (dnsmasq + iptables NAT)
- Replaced IP‑based nftables whitelist with domain‑based DNS filtering using dnsmasq on port 5353.
- Configured /etc/dnsmasq.d/benny‑whitelist.conf — `address=/#/` returns NXDOMAIN for all non‑whitelisted domains, `no‑resolv` with upstream 8.8.8.8 and 8.8.4.4.
- Whitelisted learn.dvorak.nl, dlang.org, dlang.school via `server=/domain/8.8.8.8` directives.
- Created iptables NAT OUTPUT rules to redirect benny's DNS (UDP and TCP port 53) to port 5353.
- Diagnosed nsswitch.conf bypass — Pop!_OS default `hosts` line includes `mdns4_minimal [NOTFOUND=return]`, which short‑circuits DNS before dnsmasq can return NXDOMAIN. Fixed to `hosts: files dns`.
- Created /etc/systemd/system/iptables‑restore.service to persist NAT rules via `/sbin/iptables‑restore /etc/iptables/rules.v4` at boot. Enabled with systemctl.
- Saved iptables NAT rules to /etc/iptables/rules.v4.
- Verified lockdown: admin gets 301 from Google, benny gets NXDOMAIN. benny reaches learn.dvorak.nl (200) and dlang.org (200).
- Updated docs/benny‑lockdown.md with full DNS filtering architecture, persistence, verification, and troubleshooting.
- Updated notes/wednesday‑tasks.md step 6 with completion markers, implementation notes, and pending whitelist items (services.vnc.com, code.visualstudio.com).

