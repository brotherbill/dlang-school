# Benny account network lockdown on Pop!_OS
###### /dev/repos/dlang-school/docs/benny-lockdown.md

# Benny Account Lockdown — Pop!_OS

## Purpose

Restrict the `benny` user account (UID 1000) to loopback traffic and a single whitelisted destination (the Dvorak typing site at `matias.ca`). All other outbound network access is dropped.

## Architecture

- **Firewall backend:** nftables (managed via `iptables-nft` compatibility layer + direct `nft` commands)
- **Persistence file:** `/etc/nftables.conf`
- **Restore service:** `/etc/systemd/system/restore-iptables.service`
- **Restore method:** `nft -f /etc/nftables.conf` at boot, before networking

## Active Rules

### IPv4 (`table ip filter`, chain OUTPUT)

| # | Action | Destination | Interface | Match | Purpose |
|---|--------|-------------|-----------|-------|---------|
| 1 | ACCEPT | any | lo | UID 1000 | Loopback (localhost) |
| 2 | ACCEPT | 116.203.249.104 | any | UID 1000 | matias.ca (Dvorak site) |
| 3 | DROP | any | any | UID 1000 | Block everything else |

### IPv6 (`table ip6 filter`, chain OUTPUT)

| # | Action | Destination | Interface | Match | Purpose |
|---|--------|-------------|-----------|-------|---------|
| 1 | ACCEPT | any | lo | UID 1000 | Loopback (localhost) |
| 2 | ACCEPT | 2a01:4f8:c010:a08c::1 | any | UID 1000 | matias.ca (Dvorak site) |
| 3 | DROP | any | any | UID 1000 | Block everything else |

## Verification

```bash
# Admin reaches Google (expect 301):
curl --connect-timeout 5 -s -o /dev/null -w "ADMIN: %{http_code}\n" https://google.com

# Benny cannot reach Google (expect 000 after timeout):
sudo -u benny curl --connect-timeout 5 -s -o /dev/null -w "BENNY: %{http_code}\n" https://google.com
```

## Persistence

Rules are saved in `/etc/nftables.conf` and restored on boot by the systemd service `restore-iptables.service`.

### To re-save after any rule changes

```bash
sudo nft list ruleset > /tmp/nft.rules && sudo mv /tmp/nft.rules /etc/nftables.conf
```

### To verify service status

```bash
sudo systemctl is-enabled restore-iptables.service
```

## Systemd Service

```ini
[Unit]
Description=Restore nftables rules
Before=network-pre.target
Wants=network-pre.target

[Service]
Type=oneshot
ExecStart=/usr/sbin/nft -f /etc/nftables.conf

[Install]
WantedBy=multi-user.target
```

## Troubleshooting Notes

- **IPv6 matters.** Google and most modern sites resolve to IPv6 first. If only IPv4 rules are applied, traffic passes through IPv6 unblocked.
- **`ip6tables-save` may fail** with "unknown ipv6 meta key" on Pop!_OS. Use `nft list ruleset` to save instead.
- **Check packet counters** to confirm rules are matching: `sudo nft list ruleset` — nonzero `counter packets` on the DROP rules confirms blocking.
- **Check iptables backend** with `iptables --version` — Pop!_OS uses `nf_tables`, not legacy.

## Confirmed Working

- **Date:** 2026-04-26
- **OS:** Pop!_OS (nf_tables backend, iptables v1.8.10)
- **Admin result:** 301 (Google responds)
- **Benny result:** 000 (connection dropped)
