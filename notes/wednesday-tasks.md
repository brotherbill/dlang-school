# Wednesday Tasks
###### /dev/repos/dlang-school/notes/wednesday-tasks.md
###### Benny's machine: Pop!_OS (Linux). Brother Bill's machines: Windows and Pop!_OS (Linux)

## Sunday, April 26 — Foundation

### Hardware

1. ~~Convert Keychron Q5 MAX to silent switches, swap keycaps to DVORAK layout.~~ **COMPLETE.** Keychron Q5 Max returned. Purchased Matias Dvorak Pro (FK207QPC) direct from matias.ca — DVORAK hardwired at firmware level, quiet Matias switches, wired USB-A, full-size, DVORAK+QWERTY legends, physical toggle switch. No conversion or programming needed. Benny uses his existing HP quiet wired QWERTY keyboard until the Matias arrives. HP keyboard stays as cheap backup.

### Purchases

2. ~~Purchase Typora — $15 one-time.~~ **COMPLETE.** Typora purchased, installed on Benny's Pop!_OS box, registered, and tested. Auto-formats markdown same as Windows.

### Software Setup (Benny's Machine — Pop!_OS)

3. ~~Verify D toolchain on Benny's machine (dmd, dub).~~ **COMPLETE.** Both dmd and dub verified installed.
4. ~~Verify VS Code D extension on Benny's machine.~~ **COMPLETE.** VS Code 1.117.0 installed. D extension (webfreak.code-d v0.23.2) already installed.
5. ~~Install Typora on Benny's machine.~~ **COMPLETE.** Typora installed, registered, and tested on Benny's Pop!_OS box.

### Internet Lockdown — Standard Users Only (Benny's Machine — Pop!_OS)

6. Lock down internet access for standard users — whitelist-only model. Admin account stays unrestricted.

    **Layer 1 — Limited User Account**
    a. ~~Create a limited (non-sudo) user account for Benny. Brother Bill retains the admin account.~~ **COMPLETE.**
    b. ~~Benny cannot install software, change DNS, or modify firewall rules.~~ **COMPLETE.** Non-sudo account confirmed.

    **Layer 2 — Restricted DNS Resolver (dnsmasq on port 5353)**
    c. ~~Install dnsmasq: `sudo apt install dnsmasq`.~~ **COMPLETE.**
    d. ~~Configure dnsmasq to listen on port 5353 (non-standard — system DNS stays untouched for admin).~~ **COMPLETE.** Config lives in `/etc/dnsmasq.d/benny-whitelist.conf` (not `/etc/dnsmasq.conf`). Uses `address=/#/` to return NXDOMAIN for non-whitelisted domains. `no-resolv` with upstream `server=8.8.8.8` and `server=8.8.4.4`.
    e. ~~Whitelist approved domains by forwarding them to a real DNS server.~~ **PARTIAL.** Current whitelist: `learn.dvorak.nl`, `dlang.org`, `dlang.school`. **Still pending:** `services.vnc.com` (add when setting up RealVNC, step 8) and `code.visualstudio.com` (add before Wednesday session for VS Code extension updates).
    f. Whitelist the SMTP server used by msmtp for outbox email notifications (e.g., `server=/smtp.gmail.com/8.8.8.8`). **BLOCKED** — msmtp not configured yet (steps 10–11).
    g. Add more whitelisted domains as needed — this is the living whitelist.
    h. ~~Restart dnsmasq: `sudo systemctl restart dnsmasq`.~~ **COMPLETE.**

    **Layer 3 — Per-User DNS Redirect (iptables --uid-owner)**
    i. ~~Get Benny's UID: `BENNY_UID=$(id -u benny)`.~~ **COMPLETE.**
    j. ~~Redirect all of Benny's DNS queries to the restricted resolver.~~ **COMPLETE.** Both UDP and TCP port 53 redirected to 5353 via iptables NAT OUTPUT chain.
    k. ~~Admin's DNS is untouched — goes to the system's normal DNS server. Full internet access.~~ **COMPLETE.**

    **Layer 4 — Persist iptables Rules Across Reboots**
    l. ~~Install iptables-persistent: `sudo apt install iptables-persistent`.~~ **COMPLETE (different approach).** Created custom systemd service `/etc/systemd/system/iptables-restore.service` using `/sbin/iptables-restore /etc/iptables/rules.v4` instead of `iptables-persistent` package.
    m. ~~Save rules.~~ **COMPLETE.** `sudo sh -c 'iptables-save > /etc/iptables/rules.v4'`.
    n. ~~Rules survive reboots automatically.~~ **COMPLETE.** Service enabled via `systemctl enable iptables-restore.service`.

    **Layer 3.5 — nsswitch.conf Fix (Not in Original Plan)**
    Added during implementation. Pop!_OS default `hosts` line in `/etc/nsswitch.conf` includes `mdns4_minimal [NOTFOUND=return]`, which short-circuits DNS before dnsmasq can return NXDOMAIN — bypassing the block entirely. Fixed to `hosts: files dns`. See `docs/benny-lockdown.md` for full details.

    **Verification**
    o. ~~Log in as admin. Browse freely — youtube.com, anything. Confirm unrestricted.~~ **COMPLETE.** Admin curl to google.com returns 301.
    p. ~~Log in as Benny. Try a non-whitelisted site (e.g., youtube.com). Confirm it fails.~~ **COMPLETE.** Benny curl to google.com returns nothing (NXDOMAIN).
    q. ~~Visit a whitelisted site (e.g., learn.dvorak.nl). Confirm it loads.~~ **COMPLETE.** Benny curl to learn.dvorak.nl returns 200. dlang.org returns 200.
    r. Confirm RealVNC connects from Brother Bill's machine. **BLOCKED** — RealVNC not set up yet (step 8).
    s. Drop a test file in outbox. Confirm email notification sends. **BLOCKED** — outbox notification not built yet (steps 9–14).

7. Set up base `projects/` folder on Benny's machine — deterministic date-based layout the CLI tools expect.

### RealVNC (Both Machines)

8. Install RealVNC Connect on both machines. On Benny's Pop!_OS machine: download `.deb` from realvnc.com, install via `sudo dpkg -i <package>.deb`. Create team account. Sign both machines into same account — Benny's machine appears permanently in Brother Bill's address book. No one-time codes. Persistent trusted access. File transfer via RealVNC File Transfer dialog (ClipboardFT is Windows-to-Windows only — does not apply). Set up "inbox" and "outbox" folders on Benny's machine — Brother Bill reads outbox, deposits to inbox, and deletes older files from both. Exercise end-to-end before Wednesday: remote control, text clipboard (Ctrl+C/V across machines), file transfer, inbox/outbox flow. Brother Bill connects anytime between weekly sessions to check outbox or deposit to inbox.

### Outbox Email Notification (Benny's Machine — Pop!_OS)

9. Install `inotify-tools` on Benny's machine: `sudo apt install inotify-tools`.
10. Install `msmtp` for lightweight email sending: `sudo apt install msmtp msmtp-mta`.
11. Configure `msmtp` with Brother Bill's email credentials (or an app-specific password).
12. Create a bash script (`outbox-notify.sh`) that:
    a. Uses `inotifywait -m -e create,modify,moved_to ~/outbox/` to watch the outbox folder.
    b. On any change, sends an email to Brother Bill with the filename and timestamp.
13. Create a systemd user service to run `outbox-notify.sh` at startup, so it survives reboots.
14. Test end-to-end: drop a file in outbox → email arrives at Brother Bill's inbox.

---

## Monday, April 27 — Build

### CLI Scripts (Bash)

15. Finish creating CLI scripts (bash, not PowerShell) for use before RCA is created. Test each on Benny's Pop!_OS machine.
    a. `create-d-project` — fresh named project with hello-world content.
    b. `next-version` — lock existing project, create clone with new version, refactor old name to new name.
    c. `show-versions` — display all versions with names.
    d. `dehydrate` and `rehydrate` — clean unneeded assets, zip up.
    e. Add journal — create journal markdown file in project, open Typora.

---

## Tuesday, April 28 — Content, Validation, Pack

### Lessons

16. Create first few lessons with initial docs, reference project, and adventure steps.
    a. Create homework assignment — repeat what was learned with variations and more steps. This is where the project can be a little more rich or complex.

### Pre-Wednesday Validation

17. Exercise the various flows end-to-end on Benny's machine: create project → write code → version → journal → dehydrate → rehydrate → show versions → homework. Verify everything works with all tooling, RealVNC remote access, inbox/outbox flow, and outbox email notification.

### Pack Staging Bag

18. HDMI to DisplayPort cable in Staging "XFinity" shopping bag.
19. Keychron Q5 MAX keyboard and USB-C cable in staging bag — **for return, not setup.** Return on Wednesday.

---

## Wednesday, April 29 — Cushion / Live Tutoring (2 Hours)

### Hardware Hookup — Second Monitor

20. Open Staging "XFinity" shopping bag.
21. Remove HDMI to DisplayPort cable from bag.
22. Locate the DisplayPort port on the back of Benny's PC.
23. Plug the DisplayPort end of the cable into Benny's PC.
24. Locate the HDMI port on the second monitor.
25. Plug the HDMI end of the cable into the second monitor.
26. Plug the second monitor's power cable into a power outlet.
27. Power on the second monitor.
28. On Benny's PC, open GNOME Settings → Displays.
29. Confirm Pop!_OS detects two displays.
30. Set arrangement to "Join Displays" (extended desktop).
31. Arrange the monitor positions (left/right) to match the physical layout on the desk.
32. Click Apply.
33. Drag a window from monitor 1 to monitor 2. Confirm it moves smoothly.

### Return — Keychron Q5 MAX

34. Return Keychron Q5 MAX keyboard and USB-C cable.

### Hardware Hookup — Matias Dvorak Pro (Deferred — When Keyboard Arrives)

35. Remove Matias Dvorak Pro keyboard from packaging.
36. Plug the USB-A cable into the keyboard.
37. Plug the USB-A end into Benny's PC.
38. Set the physical toggle switch to DVORAK.
39. Open a text editor (gedit or VS Code). Type "hello" — confirm DVORAK output.
40. Flip the toggle switch to QWERTY. Type "hello" — confirm QWERTY output.
41. Flip back to DVORAK.
42. Restart Benny's PC.
43. During boot, repeatedly tap F7 (or F12, depending on hardware). Confirm BIOS/BOOT page appears — USB-A wired keyboard sends keystrokes during pre-boot.
44. Exit BIOS without changes. Let Pop!_OS boot normally.

### Teaching

45. Teach Benny how to increase font size in VS Code.
46. Teach Brother Bill how to change stylized colors for `(` and `)` — selected, not selected, and unpaired. Brother Bill is red-green color-blind.
47. Give Benny the Dvorak touch typing URL: https://learn.dvorak.nl/

### On-Site Verification

48. Post-transport verification — not full E2E (already done pre-Wednesday).
    a. Second monitor displays and extends desktop.
    b. HP quiet wired QWERTY keyboard responds. F7/F12 reaches BIOS/BOOT page. Matias Dvorak Pro verification deferred until keyboard arrives.
    c. DVORAK layout verification deferred until Matias arrives. Benny uses HP QWERTY keyboard on Wednesday.
    d. VS Code opens, D extension loads, font size adjustment works.
    e. `create-d-project` runs and produces a valid project.
    f. Typora opens a journal file.
    g. RealVNC connects from Brother Bill's machine to Benny's machine. Text clipboard copies across. File drops into inbox via File Transfer dialog.
    h. Drop a test file in outbox. Confirm email notification arrives.
    i. Internet lockdown verified — admin browses freely, Benny's non-whitelisted site fails, whitelisted site loads.
    j. Parental controls are active and locked down.

---

## Bonus — If Time Permits

### Bitwarden

49. Learn Bitwarden basics: vault, entries, folders, password generator.
50. Install Bitwarden browser extension on both machines.
51. Install Bitwarden desktop app on both machines (Windows installer for Brother Bill, `.deb`/snap for Benny's Pop!_OS).
52. Store first few credentials in the vault — start with accounts used for dlang-school work.
53. Practice the workflow: visit a login page → Bitwarden auto-fills → log in.
