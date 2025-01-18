# Disk Quotas in Linux Systems

Managing disk space efficiently is a crucial responsibility for system administrators, especially in multi-user environments. **Disk quotas** allow admins to set limits on how much disk space users or groups can consume, ensuring fair usage and preventing any single user from monopolizing storage.

## User/Group Quotas

- You can restrict the amount of disk space available to users or groups by implementing disk quotas.
- The **XFS quota subsystem** manages limits on disk space (blocks) and file (inode) usage. XFS quotas control or report on usage at the user, group, directory, or project level.

## Why Use Disk Quotas?
Imagine a shared server where multiple users store files. Without restrictions, one user could potentially fill the entire disk, leaving no room for others. Disk quotas solve this problem by:
- **Restricting disk usage** per user, group, or project.
- **Preventing system crashes** due to full disks.
- **Ensuring balanced resource allocation** across all users.

## Implementing Disk Quotas on XFS File Systems

### Example Scenario
In a shared environment, if users A, B, and C are consuming varying amounts of disk space, user D may run out of space when trying to create a new file. To prevent such issues, an admin can set disk quotas to limit usage for each user.

### Steps to Implement Quota

1. **Create a partition.**
   ```bash
   cfdisk /dev/sda
   ```

2. **Format the partition with XFS.**
   ```bash
   mkfs.xfs /dev/sda3
   ```

3. **Make entries in `/etc/fstab` with quota options.**
   ```bash
   /dev/sda3   /data   xfs   defaults,uquota,gquota  0  0
   ```

4. **Create a mount point and mount the partition.**
   ```bash
   mkdir /data
   mount -o uquota,gquota /dev/sda3 /data
   mount -a
   ```
   Verify quota activation:
   ```bash
   mount | grep quota
   ```

5. **Create a user and set permissions.**
   ```bash
   useradd harry
   passwd harry
   chmod o+rwx /data
   ```

6. **Set quota limits using `xfs_quota`.**
   ```bash
   xfs_quota -x -c 'limit bsoft=1G bhard=1536M harry' /data
   ```
   Here, the soft limit is set to **1 GB**, and the hard limit is **1.5 GB**.

7. **Test the quota by switching to the user and creating files.**
   ```bash
   su - harry
   fallocate -l 1.2G /data/new_file
   ```
   If the user tries to create another file exceeding the hard limit, an error will occur:
   ```bash
   fallocate -l 1G /data/new_file1
   # Disk quota exceeded
   ```

## Quota Management Commands

### Report Current Quota Usage
```bash
xfs_quota -x -c 'report -h' /data
```
Use this command to display a summary of current disk usage and limits for all users on the filesystem.

### Disable XFS Quota Temporarily
```bash
xfs_quota -x -c 'disable -uv' /data
```
The `-uv` flag disables user quotas, while `-gv` disables group quotas, and `-pv` disables project quotas. Once disabled, users can create files without restrictions.

### Turn Off Quota for a Specific User (e.g., user Harry)
```bash
xfs_quota -x -c 'off -uv' /data
```
This removes quota restrictions for the specified user.

### Disable Quotas Permanently
Edit `/etc/fstab` and replace `uquota,gquota` with `noquota`, then reboot the system.

## Additional Useful Commands with Explanation

1. **Check quota status for a specific user:**
   ```bash
   xfs_quota -x -c 'quota -u harry' /data
   ```
   This displays the current disk usage and limits for the user `harry`. It is useful for verifying whether quotas are being enforced correctly.

2. **Check quota status for a specific group:**
   ```bash
   xfs_quota -x -c 'quota -g groupname' /data
   ```
   This command shows the current quota limits and usage for a specific group.

3. **Set inode limits for a user:**
   ```bash
   xfs_quota -x -c 'limit isoft=1000 ihard=1200 harry' /data
   ```
   This command limits the number of inodes (files and directories) the user `harry` can create. `isoft` specifies the soft limit, while `ihard` sets the hard limit.

4. **List all active quotas:**
   ```bash
   xfs_quota -x -c 'report -u' /data
   ```
   This generates a report of all users with quotas on the specified partition, showing their current usage and limits.

5. **Remove quota limits for a user:**
   ```bash
   xfs_quota -x -c 'limit bsoft=0 bhard=0 harry' /data
   ```
   Setting both block limits to zero effectively removes any quota restrictions for the user `harry`.

6. **Enable project quotas (for directories):**
   ```bash
   xfs_quota -x -c 'project -s proj1' /data
   ```
   This enables quotas for a specific project. To use project quotas, define the project in `/etc/projects` and assign a project ID in `/etc/projid`.

   **Example:**
   - Add an entry in `/etc/projects`:
     ```
     1:/data/project1
     ```
   - Add a corresponding entry in `/etc/projid`:
     ```
     proj1:1
     ```
   - Enable the project quota:
     ```bash
     xfs_quota -x -c 'project -s proj1' /data
     ```

7. **Display quota statistics:**
   ```bash
   xfs_quota -x -c 'state' /data
   ```
   This command shows whether quotas are enabled and provides basic statistics about their usage.

8. **Remount a filesystem with quotas:**
   ```bash
   mount -o remount,uquota,gquota /data
   ```
   This command can be used to reapply quota options without rebooting the system.

## Key Quota Parameters
- **bsoft/bhard**: Block limits (soft and hard limits for disk space usage).
- **isoft/ihard**: Inode limits (soft and hard limits for file counts).
- **rtbsoft/rtbhard**: Real-time block limits.

## Expert Mode Options
When using `xfs_quota`, the `-x` flag enables expert mode, allowing advanced quota management features. The `-c` option allows commands to be run interactively or as command-line arguments.

---

Disk quotas are indispensable in environments where multiple users share storage resources. By implementing quotas, administrators can ensure efficient disk usage, prevent resource abuse, and maintain system stability.

Feel free to contribute or share your own experiences in implementing disk quotas!
