
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
- Example: 
  ```sh
  ps -aux
  ```
  Displays all running processes with information like PID, CPU, memory usage, etc.

- `ps -fu alex` - Show processes belonging to user `alex`.
- Example:
  ```sh
  ps -fu alex
  ```
  Shows processes that are running under the user `alex`.

- `pstree -p` - Display a tree-like hierarchy of parent-child processes.
- Example:
  ```sh
  pstree -p
  ```
  Displays a tree-like view of processes, where each process is shown with its PID.

- `top` - Show live process activity.
- Example:
  ```sh
  top
  ```
  Opens an interactive session showing system processes, memory usage, and CPU utilization in real-time.

- `pidof bc` - Get the PID of `bc` (basic calculator).
- Example:
  ```sh
  pidof bc
  ```
  Outputs the PID of the `bc` process.

### Killing Processes
- `kill 1234` - Terminate process with PID `1234`.
- Example:
  ```sh
  kill 1234
  ```
  Kills the process with PID 1234.

- `kill -9 1234` - Forcefully terminate process `1234`.
- Example:
  ```sh
  kill -9 1234
  ```
  Immediately kills the process with PID 1234, even if it is stuck.

- `killall -9 bc` - Kill all instances of `bc` by name.
- Example:
  ```sh
  killall -9 bc
  ```
  Terminates all running `bc` processes.

- `kill -u alex` - Kill all processes belonging to user `alex`.
- Example:
  ```sh
  kill -u alex
  ```
  Terminates all processes that belong to the user `alex`.

- `killall -u alex -u susan` - Kill processes of multiple users.
- Example:
  ```sh
  killall -u alex -u susan
  ```
  Terminates all processes belonging to the users `alex` and `susan`.

- `killall -gh -u alex` - Kill all background processes associated with user `alex`.
- Example:
  ```sh
  killall -gh -u alex
  ```
  Kill all background processes associated with user `alex`.

## Nice Value and Process Priority
- **Nice value range:** `-20` (highest priority) to `19` (lowest priority).
- `nice -n -20 firefox` - Set highest priority for `firefox`.
- Example:
  ```sh
  nice -n -20 firefox
  ```
  Launches Firefox with the highest priority.

- `nice -n -10 -p 7124` - Change nice value of process `7124`.
- Example:
  ```sh
  nice -n -10 -p 7124
  ```
  Adjusts the nice value of process with PID `7124` to `-10`.

## Foreground & Background Process Control
- `&` - Run a process in the background (e.g., `firefox &`).
- Example:
  ```sh
  firefox &
  ```
  Launches Firefox in the background.

- `jobs` - List background jobs.
- Example:
  ```sh
  jobs
  ```
  Displays a list of background jobs currently running in the terminal session.

- `fg %1` - Bring job 1 to the foreground.
- Example:
  ```sh
  fg %1
  ```
  Brings the first background job to the foreground.

- `bg %1` - Resume job 1 in the background.
- Example:
  ```sh
  bg %1
  ```
  Resumes job 1 and moves it back to the background.

- `disown -h %1` - Remove job from shell job list without stopping it.
- Example:
  ```sh
  disown -h %1
  ```
  Removes job 1 from the shell's job table, so it will not be affected by the shell's exit.

## Process Scheduling & CPU Affinity
- `renice -n 10 -p <PID>` - Adjust process priority dynamically.
- Example:
  ```sh
  renice -n 10 -p 1234
  ```
  Changes the priority of process with PID 1234 to `10`.

- `taskset -c 0,1 <command>` - Bind a process to specific CPU cores.
- Example:
  ```sh
  taskset -c 0,1 firefox
  ```
  Runs `firefox` on CPU cores 0 and 1.

- `chrt -p <PID>` - Show process scheduling policy.
- Example:
  ```sh
  chrt -p 1234
  ```
  Displays the scheduling policy of process `1234`.

## Monitoring System Performance
- `uptime` - Show system uptime and load average.
- Example:
  ```sh
  uptime
  ```
  Displays the system's uptime, load averages, and number of users.

- `who` - Display logged-in users.
- Example:
  ```sh
  who
  ```
  Lists the users currently logged into the system.

- `w` - Show active users and system load.
- Example:
  ```sh
  w
  ```
  Displays a list of currently logged-in users along with their activity and system load.

- `uname -r` - Display kernel version.
- Example:
  ```sh
  uname -r
  ```
  Shows the current running kernel version.

- `last` - Show last login history.
- Example:
  ```sh
  last
  ```
  Displays the login history of users.

- `lastb` - Show failed authentication attempts.
- Example:
  ```sh
  lastb
  ```
  Displays information about failed login attempts.

- `htop` - Interactive process viewer (better than `top`).
- Example:
  ```sh
  htop
  ```
  Launches a more interactive version of `top` for monitoring system processes.

- `iostat -c 1` - Monitor CPU usage in real-time.
- Example:
  ```sh
  iostat -c 1
  ```
  Displays CPU usage statistics every second.

- `vmstat 1` - Display system performance statistics.
- Example:
  ```sh
  vmstat 1
  ```
  Shows system memory and swap statistics every second.

## Tracing & Debugging Processes
- `strace -p <PID>` - Trace system calls of a process.
- Example:
  ```sh
  strace -p 1234
  ```
  Traces all system calls made by the process with PID `1234`.

- `lsof -p <PID>` - List files opened by a process.
- Example:
  ```sh
  lsof -p 1234
  ```
  Lists all files opened by the process with PID `1234`.

- `gdb -p <PID>` - Debug running process.
- Example:
  ```sh
  gdb -p 1234
  ```
  Starts the GDB debugger for the process with PID `1234`.

## Handling Zombie Processes
- `ps aux | grep Z` - Identify zombie processes.
- Example:
  ```sh
  ps aux | grep Z
  ```
  Lists processes in a "zombie" state.

- `kill -SIGCHLD <Parent_PID>` - Ask parent process to reap zombies.
- Example:
  ```sh
  kill -SIGCHLD 1234
  ```
  Sends the `SIGCHLD` signal to the parent process of PID 1234 to clean up zombie processes.

## Process Environment Variables
- `env` - Show all environment variables.
- Example:
  ```sh
  env
  ```
  Displays all environment variables.

- `printenv PATH` - Show the value of the `PATH` variable.
- Example:
  ```sh
  printenv PATH
  ```
  Displays the current value of the `PATH` environment variable.

- `export VAR=value` - Set an environment variable.
- Example:
  ```sh
  export MY_VAR=my_value
  ```
  Sets an environment variable `MY_VAR` to `my_value`.

- `unset VAR` - Remove an environment variable.
- Example:
  ```sh
  unset MY_VAR
  ```
  Removes the environment variable `MY_VAR`.

## Daemon Processes
- Long-running background processes.
- Example: `systemd` manages daemons on modern Linux systems.
- `systemctl list-units --type=service` - List all active services.
- Example:
  ```sh
  systemctl list-units --type=service
  ```
  Displays a list of all active services managed by `systemd`.

## Process Resource Limits
- `ulimit -a` - Show all resource limits.
- Example:
  ```sh
  ulimit -a
  ```
  Displays all current resource limits for the shell session.

- `ulimit -u 500` - Set the max number of user processes.
- Example:
  ```sh
  ulimit -u 500
  ```
  Sets the maximum number of processes a user can create to 500.

- `ulimit -n 1024` - Set the max number of open file descriptors.
- Example:
  ```sh
  ulimit -n 1024
  ```
  Sets the maximum number of open file descriptors to 1024.

## Cgroup (Control Groups) for Process Control
- `cgcreate -g cpu,memory:/mygroup` - Create a new cgroup.
- Example:
  ```sh
  cgcreate -g cpu,memory:/mygroup
  ```
  Creates a new cgroup named `mygroup` for CPU and memory.

- `cgexec -g cpu,memory:/mygroup command` - Run a process inside a cgroup.
- Example:
  ```sh
  cgexec -g cpu,memory:/mygroup firefox
  ```
  Runs `firefox` within the `mygroup` cgroup.

- `cat /sys/fs/cgroup/cpu/mygroup/cpu.shares` - Check CPU shares assigned.
- Example:
  ```sh
  cat /sys/fs/cgroup/cpu/mygroup/cpu.shares
  ```
  Displays the CPU share of the `mygroup` cgroup.
