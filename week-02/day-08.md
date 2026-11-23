# Day 8 — File and Directory Permissions

## What I did
Practiced on my Kali Linux VM: learned how to read a permissions line and how
to change permissions of files and directories using letters and numbers.

## Commands of the day
- `ls -l file` — show permissions as letters (e.g. `-rwxrw-r--`)
- `stat -c %a file` — show permissions as a number (e.g. `764`)
- `chmod u+x file` — add execute permission for the owner (letter mode)
- `chmod 700 file` — set exact permissions with a number (numeric mode)
- `./script.sh` — run a script from the current directory

## What I learned
- A permissions line always has 10 characters: the first is the file type,
  then three sets of three (owner, group, others).
- A new file can't be run because it is created without `x`: by default it gets
  `-rw-r--r--` (644) or `-rw-rw-r--` (664), depending on the umask. To run it,
  you need to add `x`, e.g. `chmod u+x file`.
- Each letter has a fixed position and value: `r w x` = `4 2 1`. Each set is
  the sum, so every digit from 0 to 7 maps to exactly one combination.
- To run a script you need at least `r` and `x` (5): `x` allows execution,
  `r` lets the shell read the commands from the file.
- Directory permissions mean something different: `r` — list the names inside
  (`ls`), `w` — create and delete files inside, `x` — enter the directory
  (`cd`) and access files inside by path.
- Hiding names is not security: with only `x` on a directory, you can't list
  it, but if you know or guess a file name and the file's own permissions
  allow it, you can still read it ("security through obscurity" doesn't work).
- `777` is risky: anyone can modify the file. If root or cron runs it,
  an attacker can add a command that will execute with root privileges.
