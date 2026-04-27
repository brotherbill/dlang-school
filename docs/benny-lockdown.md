# Benny account network lockdown on Pop!_OS
###### /dev/repos/dlang-school/docs/benny-lockdown.md

# Benny Account Lockdown — Pop!_OS

## Purpose

Restrict the `benny` user account to whitelisted domains only. All other outbound DNS queries return NXDOMAIN, preventing the account from reaching unauthorized sites. Whitelisted domains are managed by name, not IP.

## Architecture

Two layers enforce the lockdown:

1. **DNS filtering (dnsmasq):** A local dnsmasq instance on port 5353 resolves only whitelisted domains. All other queries return NXDOMAIN.
2. **DNS redirect (iptables NAT):** All DNS traffic from UID `benny` is silently redirected from port 53 to port 5353, forcing resolution through the filtering dnsmasq instance.

The original nftables IP-based firewall rules (DROP all non-loopback, non-whitelisted-IP traffic) may still be active as a secondary layer. The dnsmasq approach supersedes it for day-to-day management since domains are whitelisted by name.

### Key files

| File | Purpose |
|------|---------|
| `/etc/dnsmasq.d/benny-whitelist.conf` | dnsmasq config: port, upstream, whitelist |
| `/etc/nsswitch.conf` | Name resolution order (must use `dns` only) |
| `/etc/iptables/rules.v4` | Saved iptables NAT rules |
| `/etc/systemd/system/iptables-restore.service` | Restores iptables NAT rules on boot |
| `/etc/nftables.conf` | Legacy nftables rules (IP-based layer) |
| `/etc/systemd/system/restore-iptables.service` | Restores nftables rules on boot |

## Whitelisted Domains

| Domain | Purpose |
|--------|---------|
| `learn.dvorak.nl` | Dvorak typing practice |
| `dlang.org` | D language reference |
| `dlang.school` | Course site (not live yet) |

## dnsmasq Configuration

**File:** `/etc/dnsmasq.d/benny-whitelist.conf`

```ini
port=5353
no-resolv
server=8.8.8.8
server=8.8.4.4

# Return NXDOMAIN for everything except whitelisted domains
address=/#/

# Whitelisted domains (resolve normally via upstream)
server=/learn.dvorak.nl/8.8.8.8
server=/dlang.org/8.8.8.8
server=/dlang.school/8.8.8.8
```

## iptables NAT Rules

Redirect Benny's DNS (UDP and TCP port 53) to dnsmasq on port 5353:

```bash
sudo iptables -t nat -A OUTPUT -m owner --uid-owner benny -p udp --dport 53 -j REDIRECT --to-port 5353
sudo iptables -t nat -A OUTPUT -m owner --uid-owner benny -p tcp --dport 53 -j REDIRECT --to-port 5353
```

## nsswitch.conf

**Critical:** The `hosts` line must use `dns` only. The default Pop!_OS entry includes `mdns4_minimal [NOTFOUND=return]`, which short-circuits DNS before dnsmasq can respond with NXDOMAIN — causing `curl` to bypass the block.

```
hosts: files dns
```

## Persistence

### dnsmasq

Persistent by default — runs as a systemd service.

```bash
sudo systemctl is-enabled dnsmasq
```

### iptables NAT rules

Saved to `/etc/iptables/rules.v4` and restored on boot by `iptables-restore.service`.

**Systemd service:** `/etc/systemd/system/iptables-restore.service`

```ini
[Unit]
Description=Restore iptables rules
Before=network-pre.target
Wants=network-pre.target

[Service]
Type=oneshot
ExecStart=/sbin/iptables-restore /etc/iptables/rules.v4
RemainAfterExit=yes

[Install]
WantedBy=multi-user.target
```

**To re-save after any NAT rule changes:**

```bash
sudo sh -c 'iptables-save > /etc/iptables/rules.v4'
```

### nftables (legacy IP-based layer)

Saved in `/etc/nftables.conf`, restored by `restore-iptables.service`.

## Verification

```bash
# Benny reaches whitelisted site (expect HTTP 200):
sudo -u benny curl -sI https://learn.dvorak.nl 2>&1 | head -3

# Benny reaches D language docs (expect HTTP 200):
sudo -u benny curl -sI https://dlang.org 2>&1 | head -3

# Benny cannot reach Google (expect no output — NXDOMAIN):
sudo -u benny curl -sI https://google.com 2>&1 | head -3

# Admin reaches Google normally (expect 301):
curl -sI https://google.com 2>&1 | head -3
```

**Check iptables NAT counters:**

```bash
sudo iptables -t nat -L OUTPUT -v --line-numbers
```

Nonzero `pkts` on the REDIRECT rules confirms DNS traffic is being intercepted.

## Adding a New Whitelisted Domain

1. Edit `/etc/dnsmasq.d/benny-whitelist.conf`
2. Add a line: `server=/newdomain.com/8.8.8.8`
3. Restart dnsmasq: `sudo systemctl restart dnsmasq`
4. Test: `sudo -u benny curl -sI https://newdomain.com 2>&1 | head -3`

## Troubleshooting

- **mdns4_minimal bypass:** If Benny can reach blocked sites, check `/etc/nsswitch.conf`. The `hosts` line must be `files dns` only — no `mdns4_minimal [NOTFOUND=return]`.
- **dnsmasq not running:** `sudo systemctl status dnsmasq` — restart if needed.
- **NAT rules missing after reboot:** `sudo iptables -t nat -L OUTPUT -v` — if empty, check `sudo systemctl status iptables-restore.service`.
- **dlang.school shows no output:** Expected until the domain goes live. Admin account shows the same behavior.
- **Check iptables backend:** `iptables --version` — Pop!_OS uses `nf_tables` backend (iptables v1.8.10).

## Confirmed Working

- **Date:** 2026-04-26
- **OS:** Pop!_OS (nf_tables backend, iptables v1.8.10)
- **learn.dvorak.nl:** 200 (Benny can access)
- **dlang.org:** 200 (Benny can access)
- **dlang.school:** No output (not live yet — same from admin)
- **google.com:** No output (blocked via NXDOMAIN)
