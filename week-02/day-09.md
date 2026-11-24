# Day 9 — SUID, SGID and Backdoors

## What I did
Practiced on my Kali Linux VM: learned the special permission bits (SUID, SGID).
I created a SUID backdoor on the VM and saw how a non-privileged user became
root. I also ran into `nosuid` protection and understood how it works. Then I
switched from the attacker's role to the defender's: used `find` with
`-perm -4000` to locate the planted files and cleaned everything up.

## Commands of the day
- `find / -perm -4000 -type f 2>/dev/null` — find all SUID files, discarding
  "Permission denied" errors from the output.
- `find / -perm -4000 -type f -newermt "1 hour ago" 2>/dev/null` — same, but
  only files changed in the last hour (used to spot a freshly planted backdoor).
- `sudo chmod 4755 file` — set the SUID bit on a file (leading 4).
- `mount | grep -w /tmp` — check whether a directory is a separate filesystem
  and show its mount options (e.g. `nosuid`).

## What I learned
- `-rwsr-xr-x` equals 4755. The `s` means two things at once: the file is
  executable (`x`) AND the SUID bit (Set User ID) is set.
- What is SUID for? It lets a normal user run a program with the privileges of
  the file's owner (often root) for a specific task — e.g. `passwd` writing a
  new hash into `/etc/shadow` — without giving the user full root access.
- Why is SUID dangerous? It gives the program the owner's full power. It is safe
  only if the program itself is carefully restricted. A SUID-root `cat` would
  let anyone read `/etc/shadow`. Even a legit SUID can be abused through a bug —
  e.g. PwnKit (CVE-2021-4034) in `pkexec`.
- How an analyst reads the SUID list: compare it against a baseline and look for
  anything that shouldn't be there — a shell or a program that can run commands
  with the SUID bit (`bash`, `find`, `vim`, `python`), a SUID file in an odd
  place (`/tmp`, a home directory, `/dev/shm`), or a SUID that wasn't there
  before.
- `uid` vs `euid`: `uid` is who you really are; `euid` (effective UID) is whose
  privileges you actually run with. The system checks `euid` when deciding
  access, so `euid=0` means full root even if `uid` is a normal user.
- `nosuid` protection ignores the SUID bit for every file in that mount. This is
  exactly what systemd hardening (`PrivateTmp`, `nosuid`) does on my prod server.
- Persistence: an attacker who briefly had root leaves a backdoor (like this
  SUID shell) to come back and regain root later, even after the original hole
  is patched. You can't always prevent the initial access (e.g. an insider
  threat), so detection is the key defense.
- Timestomping is a MITRE technique: an attacker fakes a file's timestamp with
  `touch -t` so it looks old or normal. That's why file times must be checked
  against logs, which are harder to forge.