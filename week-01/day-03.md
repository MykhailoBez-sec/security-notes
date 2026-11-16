# Day 3. Git from the terminal

## What I did

I set up Git on my Mac: created a separate SSH key for GitHub with a
passphrase, added the public key to my account, cloned my notes repository
and pushed my first commit from the terminal.

I completed the Linux Fundamentals Part 1 and Part 2 rooms on TryHackMe and
reviewed the material from day 2.

## Commands of the day

- `ssh-keygen -t ed25519` creates a key pair: a private key and a public key.
- `pbcopy < file` copies the contents of a file to the clipboard on macOS.
- `>` overwrites a file; `>>` appends to the end of it.
- `echo` prints text.
- `git status` shows what has changed.
- `git add file` selects a file for the next commit.
- `git commit -m "message"` saves a snapshot with a message.
- `git push` sends commits to GitHub.
- `ssh user@host` logs in to a remote machine.
- `command --help` shows a quick reference; `man command` opens the full manual.
- `touch` creates an empty file; `mkdir` creates a folder.
- `cp` copies a file; `mv` moves or renames it; `rm` removes it.
- `su -l user` switches to another user.

## Where I was wrong

- I copied only the middle part of my public key, so GitHub rejected it.
  The full line has three parts: key type, the key itself, and a comment.
