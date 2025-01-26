# Mastering Linux Logs with `journalctl`

In Linux systems, **`journalctl`** is a powerful command-line tool for viewing and managing logs created by `systemd`. Whether you're troubleshooting issues or monitoring system activity, this tool is essential for every Linux administrator.

---

## **Key Features of `journalctl`:**
1. **View System Logs**: Access logs stored by the `systemd-journald` service.  
2. **Real-Time Log Monitoring**: Tail logs as they are generated.  
3. **Filter Logs**: Narrow down logs by time, service, priority, or even specific keywords.  

---

## **Practical Usage Examples:**

### 1️⃣ View All Logs
```bash
journalctl
```

### 2️⃣ View Logs in Real-Time
```bash
journalctl -f
```
*(Equivalent to `tail -f` for systemd logs)*  

### 3️⃣ Filter Logs by Date and Time
- Logs since yesterday:
  ```bash
  journalctl --since=yesterday
  ```
- Logs for a specific time range:
  ```bash
  journalctl --since="2025-01-17 12:00" --until="2025-01-17 14:00"
  ```
- Logs from a specific date till today:
  ```bash
  journalctl --since="2022-10-01"
  ```

### 4️⃣ Focus on a Specific Unit (e.g., Nginx)
```bash
journalctl -u nginx.service
```

### 5️⃣ Search for Keywords in Logs
```bash
journalctl | grep "ERROR"
```

### 6️⃣ View Kernel Logs
```bash
journalctl -k
```

### 7️⃣ Filter Logs by Priority
- View errors and critical messages:
  ```bash
  journalctl -p err
  ```
- View alert-level messages:
  ```bash
  journalctl -p alert
  ```

### 8️⃣ Filter Logs by Service (e.g., SSH)
```bash
journalctl -u sshd
```

### 9️⃣ View Logs from Today
```bash
journalctl --since=today
```

### 🔟 View Logs in Reverse Order
```bash
journalctl -r
```

### 11️⃣ Show Logs with Full Information
```bash
journalctl -a
```

### 12️⃣ Display Limited Number of Lines
```bash
journalctl -n 50
```

---

## **Persistent Logging**
To ensure logs are persistent across reboots, enable persistent storage:
```bash
sudo mkdir -p /var/log/journal
sudo systemd-tmpfiles --create --prefix /var/log/journal
sudo systemctl restart systemd-journald
```
# **Managing and Cleaning Old Logs**

`journalctl` not only helps you view logs but also provides tools to manage and clean them, ensuring your system doesn’t run out of disk space due to excessive logs.

## **Commands for Cleaning Logs:**

### 1. **Delete Logs Based on Size**
Limit the size of the log storage to, say, 500 MB:
```bash
journalctl --vacuum-size=500M
```

### 2. **Delete Logs Older Than a Certain Time**
Remove logs older than 14 days:
```bash
journalctl --vacuum-time=14d
```

### 3. **Limit the Number of Logs Kept**
Restrict the number of archived journal files to 10:
```bash
journalctl --vacuum-files=10
```

---

## **Scenario-Based Examples:**

### Scenario 1: Debugging a Failed Service
Use the following command to get detailed logs for a specific service:
```bash
journalctl -u <service_name> --since=yesterday
```
Example:
```bash
journalctl -u sshd --since=today
```

### Scenario 2: Investigating Kernel Boot Issues
Fetch logs from the last boot:
```bash
journalctl -b
```

### Scenario 3: Analyzing Logs for a Time-Critical Incident
Retrieve logs for a priority level "error" to quickly assess issues:
```bash
journalctl -p error --since="2025-01-17 10:00" --until="2025-01-17 12:00"
```

### Scenario 4: Tracking Network-Related Events
Filter logs for a specific network-related service:
```bash
journalctl -u NetworkManager --since=yesterday
```

---

## **Why Use `journalctl`?**
- Simplifies log management with centralized storage.  
- Enables advanced filtering and debugging.  
- A must-have skill for Linux professionals striving for efficiency.  

---

## **Pro Tip**
Combine `journalctl` with other tools like `grep` or monitoring solutions for enhanced log management.  

How do you use `journalctl` in your daily workflow? Share your thoughts in the comments! 🚀
