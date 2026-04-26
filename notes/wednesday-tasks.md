# Wednesday Tasks
###### /dev/repos/dlang-school/notes/wednesday-tasks.md
###### Benny's machine: Pop!_OS (Linux). Brother Bill's machines: Windows and Pop!_OS (Linux)

## Sunday, April 26 — Foundation

### Hardware

1. Convert Keychron Q5 MAX to silent switches, swap keycaps to DVORAK layout. Use Keychron website (VIA/QMK) to program DVORAK layout. Wired USB-C connection only — no wireless setup needed. (Keycap/switch puller ships with keyboard.)

### Purchases

2. Purchase Typora — $15 one-time.

### Software Setup (Benny's Machine — Pop!_OS)

3. Verify D toolchain on Benny's machine (dmd, dub). Install via `curl -fsS https://dlang.org/install.sh | bash` or apt if available.
4. Verify VS Code D extension on Benny's machine. Install VS Code via `.deb` package or `sudo snap install code --classic` if not already installed.
5. Install Typora on Benny's machine: `sudo snap install typora` or via apt repo (see typora.io/#linux).

### Internet Lockdown — Standard Users Only (Benny's Machine — Pop!_OS)

6. Lock down internet access for standard users — whitelist-only model. Admin account stays unrestricted.

    **Layer 1 — Limited User Account**
    a. Create a limited (non-sudo) user account for Benny. Brother Bill retains the admin account.
    b. Benny cannot install software, change DNS, or modify firewall rules.

    **Layer 2 — Restricted DNS Resolver (dnsmasq on port 5353)**
    c. Install dnsmasq: `sudo apt install dnsmasq`.
    d. Configure dnsmasq to listen on port 5353 (non-standard — system DNS stays untouched for admin):
        In `/etc/dnsmasq.conf`:
        ```
        port=5353
        listen-address=127.0.0.1
        address=/#/127.0.0.1
        ```
    e. Whitelist approved domains by forwarding them to a real DNS server:
        ```
        server=/services.vnc.com/8.8.8.8
        server=/dlang.org/8.8.8.8
        server=/code.visualstudio.com/8.8.8.8
        server=/learn.dvorak.nl/8.8.8.8
        ```
    f. Whitelist the SMTP server used by msmtp for outbox email notifications (e.g., `server=/smtp.gmail.com/8.8.8.8`).
    g. Add more whitelisted domains as needed — this is the living whitelist.
    h. Restart dnsmasq: `sudo systemctl restart dnsmasq`.

    **Layer 3 — Per-User DNS Redirect (iptables --uid-owner)**
    i. Get Benny's UID: `BENNY_UID=$(id -u benny)`.
    j. Redirect all of Benny's DNS queries to the restricted resolver — any DNS server Benny's apps try to reach gets silently rerouted to the whitelist-only dnsmasq:
        ```
        sudo iptables -t nat -A OUTPUT -m owner --uid-owner $BENNY_UID -p udp --dport 53 -j REDIRECT --to-ports 5353
        sudo iptables -t nat -A OUTPUT -m owner --uid-owner $BENNY_UID -p tcp --dport 53 -j REDIRECT --to-ports 5353
        ```
    k. Admin's DNS is untouched — goes to the system's normal DNS server. Full internet access.

    **Layer 4 — Persist iptables Rules Across Reboots**
    l. Install iptables-persistent: `sudo apt install iptables-persistent`.
    m. Save rules: `sudo netfilter-persistent save`.
    n. Rules survive reboots automatically.

    **Verification**
    o. Log in as admin. Browse freely — youtube.com, anything. Confirm unrestricted.
    p. Log in as Benny. Try a non-whitelisted site (e.g., youtube.com). Confirm it fails.
    q. Visit a whitelisted site (e.g., learn.dvorak.nl). Confirm it loads.
    r. Confirm RealVNC connects from Brother Bill's machine.
    s. Drop a test file in outbox. Confirm email notification sends.

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
19. Keychron Q5 MAX keyboard in staging bag. Confirm USB-C cable is packed with it. Wired connection (USB-C) — wireless keyboards cannot send keystrokes during pre-boot for BIOS/BOOT access (F7/F12).

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

### Hardware Hookup — Keychron Q5 MAX Keyboard

34. Remove Keychron Q5 MAX keyboard from staging bag.
35. Remove USB-C cable from staging bag.
36. Set the keyboard's connection switch (left side) to wired/USB mode.
37. Plug the USB-C end of the cable into the keyboard.
38. Plug the USB-A end of the cable into Benny's PC.
39. Open a text editor (gedit or VS Code). Type a few characters. Confirm keystrokes register.
40. Confirm DVORAK layout is active — type "hello" and verify correct output.
41. Restart Benny's PC.
42. During boot, repeatedly tap F7 (or F12, depending on hardware). Confirm BIOS/BOOT page appears.
43. Exit BIOS without changes. Let Pop!_OS boot normally.

### Teaching

44. Teach Benny how to increase font size in VS Code.
45. Teach Brother Bill how to change stylized colors for `(` and `)` — selected, not selected, and unpaired. Brother Bill is red-green color-blind.
46. Give Benny the Dvorak touch typing URL: https://learn.dvorak.nl/

### On-Site Verification

47. Post-transport verification — not full E2E (already done pre-Wednesday).
    a. Second monitor displays and extends desktop.
    b. Keychron Q5 MAX responds over USB-C. F7/F12 reaches BIOS/BOOT page.
    c. DVORAK layout types correctly in VS Code.
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

48. Learn Bitwarden basics: vault, entries, folders, password generator.
49. Install Bitwarden browser extension on both machines.
50. Install Bitwarden desktop app on both machines (Windows installer for Brother Bill, `.deb`/snap for Benny's Pop!_OS).
51. Store first few credentials in the vault — start with accounts used for dlang-school work.
52. Practice the workflow: visit a login page → Bitwarden auto-fills → log in.
