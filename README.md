# sysprobe
_Element take home exercise_ 

## Overview
Write a program called sysprobe in your preferred language (C++, Python, or Bash).
The program should demonstrate your ability to use Linux system features that are common in embedded development.

You will implement a few subcommands that inspect or interact with the system.
You can work on any linux platform with any distribution (we recommend using Ubuntu over a VM/WSL).

No hardware or extra setup is required.
You will hand back your source code, readme with installation and run instruction, and linux configuration files if they will be needed.

## Requirements
**Program name**: `sysprobe`  
**Interface:** command-line tool with subcommands  
**Usage:** sysprobe <subcommand> [options]

The subcommands will be chosen from the tasks list below.  
You must pick any 3 tasks to implement in part 1, and 1 task in part 2 (doing more is optional but not required).

### Constrains
- Use only standard libraries (no 3rd-party packages).
- Be robust: we want your program to be able to run on every linux embedded board/server/computer without changes, so handle missing files/devices/permissions gracefully (don’t crash).
- Keep it small, readable, and comment where it matters.

## Tasks 
### _Part 1 - General linux for embedded systems (Choose 3 out of 4)_

**Task A: sysfs**  

> Sysfs is a pseudo-filesystem in Linux that provides a structured, virtual interface to view and modify kernel data, particularly for devices and drivers. Mounted at /sys, it exposes information about the kernel's device model through a file-based tree, allowing user-space programs to read device attributes and control kernel parameters.

Give the user of sysprobe information about its hardware using sysfs.

Find useful data about temperature and CPU, and display print it in valid JSON (no need to match a specific format).  
`Exampmle` 
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

**Task B: Filesystem watch**  
Watch a given directory for create/modify/move/delete events for N seconds.
Print one line per event. Match the following format 
`%Y-%m-%dT%H:%M:%SZ <event> <dir>` (see example below).
If directory doesn’t exist, create it first.

```bash
# Command
sysprobe watch --dir /tmp/sysprobe --secs 10

# Possible output
2025-08-08T10:00:00Z CREATE /tmp/sysprobe/a
2025-08-08T10:00:01Z MODIFY /tmp/sysprobe/a
2025-08-08T10:00:02Z DELETE /tmp/sysprobe/a

```
**Notice** - 
Use inotify (or similar). No busy loops.

**Task C: Tracing (debugfs)**  

**Task D: Init system**  

### _Part 2 - Serial protocols (Choose 1 out of 3)_
Serial protocols are used to communicate with small hardware devices, such as different sensors, microcontrollers and FPGA. For this part, you should read a little bit about one of the 3 most common protocols - UART, SPI & I2C. For each protocol there is a different way to simulate it so you can test your code.

**Task E: UART**  

**Task F: I2C**  

**Task F: SPI**  

## Deliverables
- Source code
- README with
    - How to build/run
    - Which tasks you implemented, and notes about them if you have any
    - "Demo" (text/screenshot) of the usage on your machine.

### **Good Luck!**