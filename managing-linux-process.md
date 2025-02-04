# Linux Process Management

## What is a Process?
A process is a running instance of a program. It consists of code, data, and system resources allocated by the OS. 

## Types of Processes
1. **Foreground Process** - Runs interactively and requires user input.
2. **Background Process** - Runs without user intervention.

## Parent and Child Processes
- A parent process creates a child process using the `fork()` system call.
- Each child process has a unique Process ID (PID) and a reference to its parent process.

## Process Status Codes
- **D** - Uninterruptible sleep (waiting for I/O)
- **R** - Running or ready to run
- **S** - Sleeping (idle but can be woken up)
- **T** - Stopped (suspended)
- **Z** - Zombie (terminated but not reaped by the parent)

## First Process in Linux
- **RHEL 6** - First process is `init` (PID = 1)
- **RHEL 7, 8, 9** - First process is `systemd` (PID = 1)

## Process Management Commands
### Viewing Process Information
- `ps -aux` - Show all processes with detailed information
- `ps -fu alex` - Show processes for the user `alex`
- `pstree -p` - Display parent-child process hierarchy
- `top` - Show active processes in real time
- `top -u alex` - Monitor processes of a specific user
- `pidof bc` - Show PID of the `bc` (basic calculator) process
- `pidof bc firefox` - Show PIDs of multiple processes

### Killing Processes
- `kill 1234` - Terminate process with PID `1234`
- `kill -9 1234` - Forcefully terminate process with PID `1234`
- `killall -9 bc` - Kill all instances of `bc` by name
- `kill -u alex` - Kill all processes belonging to user `alex`
- `killall -u alex -u susan` - Kill processes of multiple users
- `killall -gh -u alex` - Kill all processes of user `alex`

## Nice Value and Process Priority
- **Nice value range:** `-20` to `19` (Lower value = Higher priority)
- `nice -n -20 firefox` - Set highest priority for `firefox`
- `nice -n -10 -p 7124` - Change nice value of process `7124`

## System Monitoring Commands
- `uptime` - Show system uptime and load average
- `who` - Display logged-in users
- `w` - Show active users and system load
- `uname -r` - Display kernel version
- `last` - Show last login history
- `lastb` - Show failed authentication attempts
