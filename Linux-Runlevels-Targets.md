**Understanding Linux Runlevels and Systemd Targets**

In Linux, **runlevels** and **systemd targets** define different states of the system, determining which services and processes run at a given time.

### **Traditional SysV Init Runlevels**

| Runlevel | Description |
|----------|------------|
| **0** | Halt (Shutdown the system) |
| **1** | Single-user mode (Maintenance mode, no networking) |
| **2** | Multi-user mode (Without network services, used in older distros) |
| **3** | Full multi-user mode (With networking, no GUI) |
| **4** | Unused (Custom runlevel, rarely used) |
| **5** | Multi-user mode with GUI (Default for most desktop environments) |
| **6** | Reboot |

However, modern Linux distributions (RHEL 7+, Ubuntu 16.04+, Rocky Linux, etc.) use **systemd**, which replaces runlevels with **targets**.

### **Systemd Targets and Their Equivalent Runlevels**

| Target Name | Equivalent Runlevel | Description |
|------------|-----------------|-------------|
| `poweroff.target` | `runlevel 0` | Shuts down the system |
| `rescue.target` | `runlevel 1` | Single-user mode (Maintenance mode) |
| `multi-user.target` | `runlevel 3` | Multi-user mode with networking (No GUI) |
| `graphical.target` | `runlevel 5` | Multi-user mode with GUI |
| `reboot.target` | `runlevel 6` | Reboots the system |
| `emergency.target` | N/A | Emergency mode (More restricted than rescue mode) |

### **Checking and Changing Systemd Targets**

🔹 **Check the current target:**
```bash
systemctl get-default
```

🔹 **List all available targets:**
```bash
systemctl list-units --type=target
```

🔹 **Change the target (Switch Runlevel):**
To switch to **multi-user mode** (equivalent to runlevel 3):
```bash
systemctl isolate multi-user.target
```
To switch to **graphical mode** (equivalent to runlevel 5):
```bash
systemctl isolate graphical.target
```

🔹 **Set a default target (Persistent change):**
To make **multi-user mode** the default:
```bash
systemctl set-default multi-user.target
```
To restore **graphical mode** as default:
```bash
systemctl set-default graphical.target
```

Understanding these concepts is crucial for managing Linux systems effectively! 🚀


