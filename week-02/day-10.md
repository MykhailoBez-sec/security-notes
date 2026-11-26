# Day 10 — umask, chown, sticky bit and SGID

## What I did
Practiced on my Kali VM to finish the Linux permissions block: where a new
file's default permissions come from (umask), changing owner and group
(chown/chgrp), and the two remaining special bits (sticky and SGID). Built a
working, secured shared team folder from all of it. Main takeaway: ownership
and permissions are separate mechanisms, set once per folder/session - not
per file.

## Commands of the day
- `umask` — show the current mask (what permissions to strip from new files)
- `umask 077` / `umask 007` — set a stricter mask for the session
- `stat -c %a file` — show permissions as a number
- `chown user file` / `chown user:group file` — change owner (and group)
- `chgrp group file` — change only the group
- `chown -R user dir` — recursive (whole directory tree)
- `ls -ld dir` — show a directory's own permissions, not its contents
- `chmod g+s dir` / `chmod 2770 dir` — set SGID on a directory
- `find / -perm -2000 2>/dev/null` — find SGID files (like -4000 for SUID)
- `rm -rf ./*` — delete everything in the current dir (dangerous, no undo)

## What I learned
- **umask** decides how closed a new file is born, automatically. Base for a
  file is 666 (no x, so new files aren't executable), for a dir 777. The mask
  REMOVES bits (think in letters: 7=remove rwx, 2=remove w). So umask 077 ->
  file 600, umask 007 -> file 660.
- **Secure by default (default deny):** set a strict umask before working with
  sensitive files, so everything is born closed and you open exceptions by
  hand. Forgetting then means "someone lacked access" (safe), not a leak. A DB
  backup written with umask 022 is 644 -> any user on the server can read it;
  with umask 077 it's 600. This is what `UMask=0077` does for my bot on prod.
- **chmod vs chown:** chmod changes permissions (what you can do), chown/chgrp
  change ownership (who owns it). A service file must have the right owner, or
  the service can't read it (e.g. `chown -R giftbot /opt/giftbot`).
- **Deleting a file depends on the DIRECTORY's permissions, not the file's.**
  If I own the dir (w, no sticky), I can delete a root-owned file inside it
  without sudo - deletion changes the directory listing, not the file.
- **Sticky bit** (t, leading digit 1) on a world-writable dir (like /tmp,
  which is 1777) means only a file's owner can delete it - stops users
  deleting each other's files. A world-writable dir WITHOUT sticky is an audit
  finding.
- **SGID** has two behaviours: on an executable file it runs with the file's
  GROUP (mirror of SUID); on a directory, new files inside INHERIT the dir's
  group. The dir case is the useful one - shared team folders.
- The kernel strips SGID if a non-root user who isn't in the file's group tries
  to set it (anti-privesc). Lowercase s/t = special bit + x; uppercase S/T =
  special bit but no x (a warning sign).
- **Ownership = SGID, permissions = umask.** A real shared folder needs BOTH:
  SGID on the dir so new files get the team group, and the right umask so the
  group gets rw. Directory permissions do NOT set new files' permissions -
  those always come from `base 666 - umask`.
- umask choice by case: 077 -> 600 (private, e.g. the bot); 007 -> 660 (shared
  with the group, outsiders blocked - for sensitive team data); 002 -> 664
  (shared, others can read).
- **rm -rf** deletes silently with no recovery. Always check `pwd` first and
  `echo ./*` to preview what the glob matches. zsh asks before `rm *` even with
  -f; bash (on the server) does not - it forgives less.
