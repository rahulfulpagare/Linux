# Mastering Linux Logs with `journalctl`

In Linux systems, **`journalctl`** is a powerful command-line tool for viewing and managing logs created by `systemd`. Whether you're troubleshooting issues or monitoring system activity, this tool is essential for every Linux administrator.

---

## **Key Features of `journalctl`:**
1. **View System Logs**: Access logs stored by the `systemd-journald` service.  
2. **Real-Time Log Monitoring**: Tail logs as they are generated.  
3. **Filter Logs**: Narrow down logs by time, service, priority, or even specific keywords.  
4. **Persistent Logging**: Ensure logs are retained across system reboots.
5. **Searchable and Structured Logs**: Quickly find logs based on detailed criteria.

---

## **Practical Usage Examples:**

### 1️⃣ View All Logs
```bash
journalctl
```
View all logs since the system started, including application, kernel, and system logs.

### 2️⃣ View Logs in Real-Time
```bash
journalctl -f
```
*(Equivalent to `tail -f` for systemd logs)*. Useful for actively monitoring ongoing events.

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
- Logs from a specific boot session:
  ```bash
  journalctl -b 2
  ```

### 4️⃣ Focus on a Specific Unit (e.g., Nginx)
```bash
journalctl -u nginx.service
```
Displays logs specific to the Nginx service. Replace `nginx.service` with the desired service name.

### 5️⃣ Search for Keywords in Logs
```bash
journalctl | grep "ERROR"
```
Combines `journalctl` with `grep` to search for specific keywords like "ERROR".

### 6️⃣ View Kernel Logs
```bash
journalctl -k
```
Retrieve only kernel-related logs for debugging hardware or kernel issues.

### 7️⃣ Filter Logs by Priority
- View errors and critical messages:
  ```bash
  journalctl -p err
  ```
- View alert-level messages:
  ```bash
  journalctl -p alert
  ```
Priority levels range from 0 (emergency) to 7 (debug). Use these for targeted troubleshooting.

### 8️⃣ Filter Logs by Service (e.g., SSH)
```bash
journalctl -u sshd
```
Fetch logs related to the SSH service. Ideal for troubleshooting connectivity or authentication issues.

### 9️⃣ View Logs from Today
```bash
journalctl --since=today
```
Quickly access logs generated on the current date.

### 🔟 View Logs in Reverse Order
```bash
journalctl -r
```
Display logs starting from the newest entries.

### 11️⃣ Show Logs with Full Information
```bash
journalctl -a
```
Include all log fields in the output, ensuring maximum detail.

### 12️⃣ Display Limited Number of Lines
```bash
journalctl -n 50
```
Show the last 50 log entries. Adjust the number as needed.

---

## **Persistent Logging**
To ensure logs are persistent across reboots, enable persistent storage:
```bash
sudo mkdir -p /var/log/journal
sudo systemd-tmpfiles --create --prefix /var/log/journal
sudo systemctl restart systemd-journald
```
Without this, logs might be stored only in memory and lost after a reboot.

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
For previous boots, specify the boot index (e.g., `-b -1` for the previous boot).

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

### Scenario 5: Finding Hardware Errors
Combine kernel logs with specific search terms:
```bash
journalctl -k | grep -i "hardware"
```

### Scenario 6: Monitoring SSH Login Attempts
Track successful and failed SSH logins:
```bash
journalctl -u sshd | grep -i "authentication"
```

---

## **Why Use `journalctl`?**
- Simplifies log management with centralized storage.  
- Enables advanced filtering and debugging.  
- Provides detailed, structured logs for precise troubleshooting.  
- Makes system administration more efficient and reliable.

---

## **Pro Tip**
Combine `journalctl` with other tools like `grep` or monitoring solutions for enhanced log management. For example, use `watch` to monitor real-time logs:
```bash
watch -n 1 'journalctl -u nginx.service -n 10'
```

How do you use `journalctl` in your daily workflow? Share your thoughts in the comments! 🚀
