# Linux Process Management

## What is a Process?
A process is an instance of a program that is being executed by the operating system. It consists of code, data, system resources, and a unique identifier called a Process ID (PID). Each process runs in its own memory space and interacts with the system based on permissions and resource availability.

## Types of Processes
1. **Foreground Process** - A process that runs interactively and requires user input. For example, when you open a text editor like `vim`, it runs as a foreground process.
   ```sh
   vim file.txt
   ```
   The terminal remains engaged with `vim` until you exit.

2. **Background Process** - A process that runs without requiring user input, allowing the terminal to remain free for other tasks. You can start a background process by appending `&` to a command.
   ```sh
   firefox &
   ```
   This launches Firefox in the background, freeing up the terminal for other commands.

## Parent and Child Processes
- Every process is created by another process, known as its **parent process**.
- The newly created process is called a **child process**.
- The **init** or **systemd** process (PID=1) is the first process in Linux and is responsible for creating other processes.
- You can check the parent process using:
  ```sh
  ps -o ppid= -p <PID>
  ```

## Process Status Codes
- **D** - Uninterruptible sleep (waiting for I/O)
- **R** - Running or ready to run
- **S** - Sleeping (idle but can be woken up)
- **T** - Stopped (suspended)
- **Z** - Zombie (terminated but not reaped by the parent)

## First Process in Linux
- **RHEL 6** - The first process is `init` (PID = 1)
- **RHEL 7, 8, 9** - The first process is `systemd` (PID = 1)

## Process Management Commands
### Viewing Process Information
- `ps -aux` - Display all processes with details.
- `ps -fu alex` - Show processes belonging to user `alex`.
- `pstree -p` - Display a tree-like hierarchy of parent-child processes.
- `top` - Show live process activity.
- `top -u alex` - Monitor processes of a specific user.
- `pidof bc` - Get the PID of `bc` (basic calculator).
- `pidof bc firefox` - Get PIDs of multiple processes.

### Killing Processes
- `kill 1234` - Terminate process with PID `1234`.
- `kill -9 1234` - Forcefully terminate process `1234`.
- `killall -9 bc` - Kill all instances of `bc` by name.
- `kill -u alex` - Kill all processes belonging to user `alex`.
- `killall -u alex -u susan` - Kill processes of multiple users.
- `killall -gh -u alex` - Kill all processes of user `alex`.

## Nice Value and Process Priority
- **Nice value range:** `-20` (highest priority) to `19` (lowest priority).
- `nice -n -20 firefox` - Set highest priority for `firefox`.
- `nice -n -10 -p 7124` - Change nice value of process `7124`.

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
- `uptime` - Show system uptime and load average.
- `who` - Display logged-in users.
- `w` - Show active users and system load.
- `uname -r` - Display kernel version.
- `last` - Show last login history.
- `lastb` - Show failed authentication attempts.
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

This guide provides an in-depth understanding of Linux process management with practical commands. Beginners can use this as a reference for essential Linux administration tasks.
