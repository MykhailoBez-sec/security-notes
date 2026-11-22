# Day 7 — Shell, Quoting and Users

## What I did
Learned how the Linux shell works and its "punctuation": the parts of a command
(command, options, arguments), quoting and escaping. Started the topic of users
and groups: `/etc/passwd`, `/etc/shadow`, creating and deleting a user.
Practiced on my Kali Linux VM.

## Commands of the day
- `echo *.txt` / `echo '*.txt'` / `echo "*.txt"` — in the first one the shell
  expands the glob into matching file names; in the second and third it passes
  `*.txt` as is.
- `echo '$HOME'` prints `$HOME` literally (single quotes: nothing is expanded);
  `echo "$HOME"` prints `/home/miho` (double quotes: spaces and `*` are
  protected, but `$variables` are expanded).
- `find . -name "*.txt"` — the pattern is quoted so `find` gets it whole and
  searches all subfolders. Without quotes the shell expands it first and `find`
  fails.
- `alias`, `\ls` — show aliases / run a command without its alias.
- `grep username /etc/passwd` — show a user's record.
- `head -5 /etc/passwd` — first 5 lines of `/etc/passwd`, the list of all users
  on the system.
- `awk -F: '$3 == 0' /etc/passwd` — find users with UID 0.
- `sudo useradd -m -s /bin/bash username` — create a user with a home directory
  and bash as the shell.
- `sudo passwd username` — set a user's password.
- `su - username` — switch to another user.
- `sudo userdel -r username` — delete a user and their home directory.

## What I learned
- The shell rewrites my command before the program runs: it expands `*`, `~`,
  `$variables` and aliases, as the current user. Quoting decides who processes
  a special character and when: the shell right away, or the program later.
- `/etc/passwd` format: `username:password:UID:GID:comment:home:shell`.
  `x` in the password field means the hash is stored in `/etc/shadow`.
- UID 0 = root. Root privileges come from the number, not the name, so a second
  user with UID 0 is a second root — a classic persistence trick to check for.
- Service/system users get `/usr/sbin/nologin`: they can't get an interactive
  shell even with the right password.
- `/etc/passwd` is world-readable (644) because programs need it to map UIDs to
  names. `/etc/shadow` (640) is readable only by root and the `shadow` group,
  because even password hashes can be cracked offline.
- Hash format: `$algorithm$parameters$salt$hash`, e.g. `$y$` = yescrypt.
  `!` instead of a hash means password login is locked (SSH keys still work).
- `sudo` logs every attempt to the system journal (viewed with `journalctl`).
  A failed attempt like `user NOT in sudoers` is a possible privilege
  escalation attempt that a SOC analyst should notice.
