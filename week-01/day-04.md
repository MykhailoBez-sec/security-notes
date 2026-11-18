# Day 4. THM: Linux Fundamentals Part 3

## What I did

I spent the day on the TryHackMe room Linux Fundamentals Part 3.

## Commands of the day

- `nano filename` opens a file in the Nano terminal editor.
- `wget URL` downloads a file over HTTP.
- `scp source destination` copies a file to or from a remote machine over SSH.
- `python3 -m http.server` turns the current folder into a small web server on port 8000.
- `ps` lists the processes in my current terminal session.
- `ps aux` lists all processes, including other users' and those without a terminal.
- `ps aux | grep "flag"` shows only the lines that contain "flag".
- `top` shows live statistics about running processes.
- `kill PID` terminates a process by its ID.
- `systemctl start` / `stop [service]` starts or stops a service now.
- `systemctl enable` / `disable [service]` turns automatic start at boot on or off.
- `command &` runs a command in the background and gives the terminal back.
- `Ctrl+Z` pauses the foreground process and gives the terminal back.
- `Ctrl+C` kills the foreground process.
- `bg` resumes a paused process in the background.
- `fg` brings a background or paused process back to the foreground.
- `jobs` lists the tasks of this terminal and their status.
- `crontab -l` shows my crontab; `crontab -e` opens it for editing.
- `sudo apt update` refreshes the package catalogue.
- `sudo apt install package` installs a program.
- `add-apt-repository` adds an extra repository.
- `wget ... | sudo apt-key add -` downloads a vendor's signing key and adds it to
  the trusted keys (deprecated, newer systems use `/etc/apt/keyrings/`).

## What I learned

- How to share files from my machine with `python3 -m http.server` and download
  them from another machine with `wget`.
- How to inspect and control processes and services with `ps`, `top`, `kill` and
  `systemctl`.
- How to keep the terminal free: run commands in the background with `&`, pause
  them with `Ctrl+Z`, resume them with `bg` or `fg`.
- How to automate tasks with cron and read a crontab line:
  `minute hour day_of_month month day_of_week command`, or a shortcut like `@reboot`.
- The difference between official and third-party repositories, and why a
  third-party repository is a security risk: its owner can install software
  on my machine with root rights.

## Open questions

- `top` shows many processes. How do I tell which ones are normal and which are suspicious?
- I did not fully understand how `apt-key` and signing keys work.
- If logs were rotated or deleted, how can an incident still be investigated?
