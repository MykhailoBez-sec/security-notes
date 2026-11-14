# Day 2. Kali in a virtual machine and TryHackMe

## What I did

I reviewed the material from day 1 with a short quiz: file owner and group,
reading log lines, showing the last 5 lines of a file and finding lines that
contain a given word.

I subscribed to TryHackMe Premium and completed the Linux CLI Basics room,
which covers navigation, search and other basic commands.

I took a snapshot of my practice server before experimenting on it.

I installed UTM on my MacBook (M1 Pro) and installed Kali Linux in a virtual
machine. I also fixed a boot problem.

## Commands of the day

- `cd ..` moves one level up from the current directory.
- `find ~ -name "filename.txt"` looks for a file by name inside my home directory.
- `cat` prints the contents of a text file.
- `uname -a` prints information about the system, kernel and hardware architecture.
- `command1 | command2` (a pipe) sends the output of one command to the next,
  for example `sudo grep "Failed" auth.log | tail -n 5`.
- `findmnt /` shows how the root disk is mounted (`ro` or `rw`).
- `sudo dmesg | grep -i ext4` shows kernel messages about the file system.
- `sudo mount -o remount,rw /` remounts the root disk with write access.
- `sudo systemctl restart lightdm` restarts the service that draws the graphical login screen.
- `sudo reboot` restarts the machine; `sudo poweroff` shuts it down cleanly.

## The boot problem

After I removed the serial device, Kali booted to a text login instead of the
graphical one. The message `read-only file system` was the clue. `findmnt /`
confirmed the root disk was mounted `ro`. `dmesg` showed no disk errors, so
I remounted the disk as `rw` and the graphical screen started. After a reboot
the system came up normally.

## Where I was wrong

- I typed `=o` instead of `-o` and got `bad usage`.
- I thought `sudo grep "Failed" tail -n 5` would work. It needs a file name
  and a pipe: `sudo grep "Failed" auth.log | tail -n 5`.
