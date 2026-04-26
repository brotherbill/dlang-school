# dlang-school — Architecture
###### /dev/repos/dlang-school/docs/ARCHITECTURE.md

## Purpose

This document contains system design, component specifications, and subsystem contracts for the dlang-school ecosystem. For the subsystem overview, see [README.md](../README.md). For product-facing information, see [ABOUT.md](ABOUT.md). For doctrine and naming conventions, see [MY_RULES.md](MY_RULES.md).

This document will be expanded as each subsystem reaches detailed design.

---

## Subsystem Contracts

### 1. RCA (Reference + Companion App)

- **Tech stack**: .NET. D is the teaching language — RCA is tooling, not curriculum.
- **Internet access**: Full privileges. RCA runs as a system service or under its own account, outside the per-user parental controls lockdown.
- **Packaging**: Ships as a `.deb` package. The `postinst` script runs as root and handles full initial setup — installs dependencies (dnsmasq, iptables-persistent), writes configuration, creates the child's limited user account, and applies parental controls. Parent enters OS password once during install. No terminal interaction after that.
- **Parental controls feature**: Menu item behind 2FA. Parent authenticates, then manages the domain whitelist through a GUI (add, remove, toggle). RCA calls `polkit/pkexec` for privilege escalation to modify dnsmasq configuration and restart the service. No terminal. No config files exposed.
- **Outbound mail**: RCA sends notification emails to the parent (update available, student activity summaries, shutdown override attempts). Outbound SMTP only. No inbound mail processing. No reply-based actions. Email replies are not trusted — a reply cannot approve, modify, or trigger any action on the machine.
- **Update flow**: RCA checks daily for available updates. When an update is found, RCA notifies the parent by email. No update is downloaded or applied without parental approval. The parent approves through the RCA's 2FA-protected menu. Once approved, RCA downloads and applies the update automatically.
- **Recovery model**: Two authentication tiers. Day-to-day operations (parental controls, whitelist changes, update approval) require the RCA password + 2FA. Recovery operations (forgot RCA password, forgot registered email, lost 2FA device, account lockout) fall through to OS admin credentials via `polkit/pkexec`. The OS admin account is the universal recovery channel — always local, no network, no email, no spoofing surface.

### 2. Folder Layout

*Detailed design pending.*

### 3. CLI Tools

*Detailed design pending.*

### 4. Versioning Model

*Detailed design pending.*

### 5. Lesson Architecture

*Detailed design pending.*

### 6. SQLite Schema

*Detailed design pending.*

### 7. Self-Guided Projects

*Detailed design pending.*

### 8. Parental Controls (Internet Lockdown)

Per-user internet lockdown. Standard users are restricted to a whitelist-only model. Admin accounts are unrestricted.

**Layer 1 — Limited User Account**
- Child operates under a non-sudo account. Cannot install software, change DNS, or modify firewall rules.

**Layer 2 — Restricted DNS Resolver (dnsmasq)**
- dnsmasq runs on port 5353 (non-standard). System DNS is untouched — admin uses the system's normal resolver with full internet access.
- Default rule: `address=/#/127.0.0.1` — all domains resolve to localhost (blocked).
- Whitelist entries override the default: `server=/approved-domain.com/8.8.8.8` — forwarded to a real DNS server.
- Living whitelist. Domains added or removed as curriculum evolves.

**Layer 3 — Per-User DNS Redirect (iptables --uid-owner)**
- All DNS queries from the child's UID are silently redirected to the restricted dnsmasq on port 5353:
  ```
  iptables -t nat -A OUTPUT -m owner --uid-owner $CHILD_UID -p udp --dport 53 -j REDIRECT --to-ports 5353
  iptables -t nat -A OUTPUT -m owner --uid-owner $CHILD_UID -p tcp --dport 53 -j REDIRECT --to-ports 5353
  ```
- Any DNS server the child's applications attempt to reach is intercepted at the kernel level. No bypass without sudo.
- Admin's DNS is untouched. Full internet.

**Layer 4 — Persistence**
- iptables rules persisted across reboots via `iptables-persistent` / `netfilter-persistent save`.

**Deployment**
- Manual procedure documented in `notes/wednesday-tasks.md` (reference implementation).
- When RCA ships, the `.deb` postinst script replaces the manual procedure entirely. Ongoing management through RCA's parental controls menu (2FA-protected).

**Interim Implementation (Pre-RCA)**
- Manual nftables lockdown applied directly to benny's UID (1000). Documented in `docs/benny-lockdown.md`.
- Blocks all outbound traffic except loopback and whitelisted IPs (matias.ca). IPv4 and IPv6.
- Persisted via `/etc/nftables.conf` and systemd service `restore-iptables.service`.
- Architecturally different from the planned dnsmasq whitelist model — IP-based DROP rather than DNS-based filtering.
- Will be superseded by the RCA's `.deb` postinst script when it ships.

---

## Rehydration Model

*Detailed design pending.*
