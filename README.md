# sysprobe
_Element take-home exercise_ 

## Overview
Write a program called `sysprobe` in your preferred language (C++, Python, or Bash).
The program should demonstrate your ability to use Linux system features that are common in embedded development.

You will implement several subcommands that inspect or interact with the system.
You can work on any Linux platform with any distribution (we recommend using Ubuntu on a VM/WSL).

No hardware or extra setup is required.
You will submit your source code, a README with installation and run instructions, and any Linux configuration files if needed.

## Requirements
**Program name**: `sysprobe`  
**Interface:** command-line tool with subcommands  
**Usage:** sysprobe <subcommand> [options]

The subcommands will be chosen from the tasks list below.  
You must pick any 3 tasks to implement (doing more is optional but not required).

### Constraints
- Use only standard libraries (no third-party packages).
- Be robust: your program should run on any Linux embedded board/server/computer without changes, so handle missing files/devices/permissions gracefully (don’t crash).
- Because you will work with advanced internal features, we recommend working as the root user (or with `sudo su`).
- Keep it small, readable, and comment where necessary.

## Tasks (Choose 3 out of 4)

### Task A: sysfs

> Sysfs is a pseudo-filesystem in Linux that provides a structured, virtual interface to view and modify kernel data, particularly for devices and drivers. Mounted at `/sys`, it exposes information about the kernel's device model through a file-based tree, allowing user-space programs to read device attributes and control kernel parameters.

Provide the user of sysprobe with information about the hardware using sysfs.

Find useful data about temperature and CPU, and display it in valid JSON (no need to match a specific format).  
`Example` 
```bash
# Command
sysprobe get-sys-data

# Possible output
{
 "thermal": [{"name": "zone0", "temp_c": 47.5}],
  "cpufreq_hz": [2400000, 2400000]
}
```
If files are missing, return an empty array instead of failing.

### Task B: Filesystem watch  
> From man: The inotify API provides a mechanism for monitoring filesystem events. Inotify can be used to monitor individual files or directories. When a directory is monitored, inotify will return events for the directory itself and for files inside the directory.  

Monitor a given directory for create/modify/move/delete events for N seconds.
Print one line per event. Match the following format:  
`%Y-%m-%dT%H:%M:%SZ <event> <dir>` (see example below).
If the directory doesn’t exist, create it first.

```bash
# Command
sysprobe watch --dir /tmp/sysprobe --secs 10

# Possible output
2025-08-08T10:00:00Z CREATE /tmp/sysprobe/a
2025-08-08T10:00:01Z MODIFY /tmp/sysprobe/a
2025-08-08T10:00:02Z DELETE /tmp/sysprobe/a

```
**Notice** - Use inotify (or similar). No busy loops.

### Task C: Tracing (debugfs)  
> Read about kernel debugfs  

Mount the kernel debugfs if it is not already mounted: `mount -t debugfs none /sys/kernel/debug`.  
Inside `tracing/events`, choose a few events you would like to trace and enable them via the filesystem (only read/write to files are needed for enabling).  
Then, read and print the trace pipe to the user for N seconds.  

```bash
# Command
sysprobe trace --secs 2

# Possible output
sudo-3501    [003] ...1. 23908.815071: sys_write(fd: 7, buf: 5c8099305920, count: 64)
cat-4110    [006] ...1. 23908.814982: sys_write(fd: 1, buf: 7a6c7ccd9000, count: 62)
```

### Task D: Init system  
Add a monitoring script to your system that will run on init.
* Choose one of the tasks you implemented, and create an init script using it.
* The script should automatically run after you restart your computer.
* Its content should probe anything you choose from the previous tasks, and write the output to a file with a timestamp. This way, you will have a log file you can track through several boots.
* The script can run for a few seconds and exit; it does not need to be a daemon.
* For example - on every boot, the command `sysprobe get-sys-data` will run, and these lines will be added to the file `sysprobe.log`:
```
System data for Wed May 18 06:45:59 PM EEST 2025:
{
 "thermal": [{"name": "zone0", "temp_c": 47.5}],
  "cpufreq_hz": [2400000, 2400000]
}
```
* You may use any Linux method for init scripts, as long as it works :)

To submit this task, provide a commands file with explanations on how you made the script run as an init script, and the script itself.

## Deliverables
- Source code

- README with:
  - How to build/run
  - Which tasks you implemented, and any notes about them
  - "Demo" (text/screenshot) of the usage on your machine.

### **Good Luck!**
