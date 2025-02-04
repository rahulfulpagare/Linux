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

## Foreground & Background Process Control
- `&` - Run a process in the background (e.g., `firefox &`).
- `jobs` - List background jobs.
- `fg %1` - Bring job 1 to the foreground.
- `bg %1` - Resume job 1 in the background.
- `disown -h %1` - Remove job from shell job list without stopping it.

## Process Scheduling & CPU Affinity
- `renice -n 10 -p <PID>` - Adjust process priority dynamically.
- `taskset -c 0,1 <command>` - Bind a process to specific CPU cores.
- `chrt -p <PID>` - Show process scheduling policy.

## Monitoring System Performance
- `uptime` - Show system uptime and load average
- `who` - Display logged-in users
- `w` - Show active users and system load
- `uname -r` - Display kernel version
- `last` - Show last login history
- `lastb` - Show failed authentication attempts
- `htop` - Interactive process viewer (better than `top`).
- `iostat -c 1` - Monitor CPU usage in real-time.
- `vmstat 1` - Display system performance statistics.

## Tracing & Debugging Processes
- `strace -p <PID>` - Trace system calls of a process.
- `lsof -p <PID>` - List files opened by a process.
- `gdb -p <PID>` - Debug running process.

## Handling Zombie Processes
- `ps aux | grep Z` - Identify zombie processes.
- `kill -SIGCHLD <Parent_PID>` - Ask parent process to reap zombies.

## Process Environment Variables
- `env` - Show all environment variables.
- `printenv PATH` - Show the value of the `PATH` variable.
- `export VAR=value` - Set an environment variable.
- `unset VAR` - Remove an environment variable.

## Daemon Processes
- Long-running background processes.
- Example: `systemd` manages daemons on modern Linux systems.
- `systemctl list-units --type=service` - List all active services.

## Process Resource Limits
- `ulimit -a` - Show all resource limits.
- `ulimit -u 500` - Set the max number of user processes.
- `ulimit -n 1024` - Set the max number of open file descriptors.

## Cgroup (Control Groups) for Process Control
- `cgcreate -g cpu,memory:/mygroup` - Create a new cgroup.
- `cgexec -g cpu,memory:/mygroup command` - Run a process inside a cgroup.
- `cat /sys/fs/cgroup/cpu/mygroup/cpu.shares` - Check CPU shares assigned.
