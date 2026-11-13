# Day 1. First login to the server and reading logs

## What I did

I used an Ubuntu server I own as a practice machine. I logged in over SSH and
worked out how the server is set up: how I log in and how Tailscale fits in.
I learned basic navigation in the terminal, explored the file system, and
covered the basics of users, groups and file permissions.

## Commands of the day

| Command | What it does |
| --- | --- |
| `whoami` | Shows which user I am logged in as |
| `pwd` | Shows which folder I am in |
| `ls -la` | Lists all files, including hidden ones |
| `ls /` | Lists the system root: `etc`, `var`, `home` and so on |
| `cd /etc` | Moves into `/etc`; `ls` then shows its contents |
| `cd /var/log` | Moves into the folder with log files |
| `ls -l` | Lists files with permissions, owner, size and date |
| `sudo tail -n 3 auth.log` | Shows the last 3 lines of the file |
| `sudo grep "Accepted" auth.log` | Shows only the lines containing that word, here successful SSH logins |
| `id` | Shows my user ID and the groups I belong to |
| `sudo -l` | Shows what I am allowed to run with sudo and whether a password is required |
| `systemctl show <service> -p User` | Shows which user a service runs as |

## What I learned

- How to read a line of `ls -l` output.
- What a log line is made of: time, host, program, message.
- The difference between a file's owner and its group.
- What root is, and why sudo exists instead of giving a user full rights.
- Why it matters which users belong to which groups.
- Why services should run as separate users, and why a service running as a
  user with sudo rights is risky.
- How to read text logs with `tail`, `grep` and `less`, and service logs with
  `journalctl`.
- What Tailscale does and does not do: it limits who can reach SSH over the
  network, but it does not protect against a vulnerable service or a stolen key.

## Findings

- sudo is configured with `NOPASSWD` for my user on the practice server.
- My SSH private key may not be protected by a passphrase (to verify).
- The Telegram bots on the server run as services under a dedicated user with
  limited rights.
- Tailscale is set up correctly: my logins come from a Tailscale address.
- Overall the server is in good shape and needs a few small fixes.

## Where I was wrong

- I mixed up two user names and concluded that a bot was running as my admin
  user, with all of its rights. I then checked the primary source, the command
  output itself, and found that the services run under a separate user with
  limited rights.
- I thought Tailscale was a cure-all and that once it was set up there were no
  risks left.

## Open questions

- How should I learn and remember commands?
- How can I tell which files belong on a server and which might be unknown or
  malicious? Do I need to know every folder and file?
