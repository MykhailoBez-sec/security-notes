# Day 5 — OverTheWire Bandit (levels 0–6)

## What I did
Started the Bandit wargame to keep practicing Linux basics. Connected via SSH from my Kali Linux VM. Completed 6 levels.

## Level by level

### Level 0 → 1
- Goal: Log into the game using SSH. The password for the next level is stored in a `readme` file in the home directory.
- Problem: Some confusion about how to start and how to log into the next level. Otherwise a simple level.
- Tip from the game: keep a local file for notes and passwords — they are not saved anywhere.

### Level 1 → 2
- Goal: The password is stored in a file called `-` in the home directory.
- Problem: The filename `-` is tricky: programs treat a lone `-` as "read from stdin", not as a file.
- Concept learned: Make the name look like a path, so the program sees a file, not an option. The short way is the `./` prefix. I went a longer way: `find` prints the full path (`/home/bandit1/-`), and `xargs` passes it to `cat` as an argument.

### Level 2 → 3
- Goal: The password is stored in a file called `--spaces in this filename--` in the home directory.
- Problem: At first I thought the only problem was the `--` at the beginning, but while trying to read the file I realized the bigger problem was the spaces.
- Concept learned: Two problems need two fixes, combined: `./` (or `--`) for the leading dashes, and quotes or `\` before each space for the spaces.

### Level 3 → 4
- Goal: The password is stored in a hidden file in the `inhere` directory.
- Problem: The filename starts with a dot, which is why plain `ls` doesn't show it.
- Concept learned: `ls -a` shows hidden files.

### Level 4 → 5
- Goal: The password is stored in the only human-readable file in the `inhere` directory.
- Problem: There are 10 files, each name starts with `-`, and only one of them is human-readable.
- What I did: Opened each file with `cat` until I found the password. `ls -la` gave no useful information.
- Better way: `file` shows the type of each file (e.g. "ASCII text" vs "data"), so one command over all files finds the readable one.

### Level 5 → 6
- Goal: The password is stored in a file somewhere under `inhere` with all of these properties: human-readable, 1033 bytes in size, not executable.
- Problem: 20 directories with several files in each — opening them one by one would take an hour.
- What I did: Tried `ls -l`, `ls -la`, `du`, `du -h`, `find . -type f ! -executable` — they listed too many files. `find . -size 1033c` found the file right away.
- Better way: `find` can combine several conditions in one command (type, size, not executable), so each condition narrows the result.

## Mistakes I made
- Reconnected as `bandit0` instead of `bandit1`, so the new password was "wrong". The username is the part before `@`.
- Tried to create my passwords file while connected to the Bandit server (`touch: Permission denied`). I was in the remote session, where home directories are read-only. Check the prompt: `bandit1@bandit` is remote, `miho@kali` is local.
- `find ... | cat` printed the file's path, not its content. A pipe sends output to the next command's stdin; `xargs` turns it into arguments.
- `cat -- --spaces in this filename--` failed: `--` fixes the dashes, but unquoted spaces still split the name into 4 separate arguments.
- `xargs` also splits on spaces. The safe version is `find ... -print0 | xargs -0 ...`.
- A `\` at the end of a line means "the command continues on the next line", which is why I got the `>` prompt. Ctrl+C cancels it.
- Typo: I wrote `file name` instead of `filename`. Tab completion prevents this.
- Quotes alone did not help with `'--spaces...'`: quotes protect spaces, not a leading dash.
- `find -name` matches only the filename, not a path, so `-name "./..."` never matches.
- Opened 10 files one by one instead of using `file`.
- Tried `find` conditions one at a time instead of combining them.
- Shared a screenshot with a password visible. Fine for a game, but a bad habit: crop secrets out of screenshots.

## New commands / options
| Command | What it does |
|---|---|
| `ssh user@host -p 2220` | Connect via SSH to a non-default port |
| `nano file.txt` | Open a file in nano; creates it on save if it doesn't exist |
| `touch file.txt` | Create an empty file (or update the timestamp of an existing one) |
| `~` | Shortcut for the home directory (`/home/miho`) |
| `cat ./-` | Read a file whose name starts with `-` by giving it a path |
| `cmd -- -name` | `--` means "end of options": everything after it is a filename |
| `'name with spaces'` / `name\ with\ spaces` | Two ways to keep spaces inside one argument |
| Tab | Autocomplete filenames and escape special characters automatically |
| `ls -a` | Show hidden files (names starting with `.`) |
| `ls -ld ~` | Show permissions of the directory itself, not its contents |
| `file *` | Show the type of each file (text, data, executable...) |
| `du -h` | Show disk usage in human-readable sizes |
| `find . -type f` | Find regular files only |
| `find . -size 1033c` | Find files of exactly 1033 bytes (`c` = bytes) |
| `find . ! -executable` | Find files that are not executable (`!` = NOT) |
| `cmd1 \| xargs cmd2` | Pass the output of cmd1 to cmd2 as arguments |
| `find ... -print0 \| xargs -0 cmd` | Same, but safe for filenames with spaces |
| Ctrl+Shift+C / Ctrl+Shift+V | Copy / paste in the Linux terminal |
| Ctrl+C | Cancel the current command |

## Takeaways
- Weird filenames (dashes, spaces, dots) are a parsing problem: understand how the shell splits arguments.
- Don't brute-force by hand: if there are many files, there is a command that filters them.
- Read the "Helpful Reading Material" section on each level page first.