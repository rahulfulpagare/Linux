### Open `fdisk` to Create a New Partition

```bash
fdisk /dev/sdX
```

- Opens the `fdisk` utility to manage disk partitions.

---

### Find the UUID of the Logical Volume

```bash
blkid /dev/VG_NAME/LV_NAME
```

- Retrieves the `UUID` of the specified logical volume.
- The `UUID` is required for persistent mounts in `/etc/fstab`.

---

### Edit the `/etc/fstab` File

```bash
vim /etc/fstab
```

- Opens the `/etc/fstab` file for editing.
- Use this file to define persistent mounts.

---

### Add an Entry in `/etc/fstab`

```plaintext
UUID=<UUID> /mount/point ext4 defaults 0 0
```

- Adds a persistent mount entry for the logical volume.
- Replace `<UUID>` with the actual UUID from the `blkid` command.
- Replace `/mount/point` with the desired mount location.
  - A **mount point** is a directory in the filesystem where the logical volume will be accessible. For example, `/data` or `/mnt/backup`.
  - Ensure the directory exists before adding the entry by creating it with `mkdir -p /mount/point`.
- Adjust the `ext4` filesystem type as necessary.

---

### Remount All `/etc/fstab` Entries

```bash
mount -a
```

- Reloads and mounts all entries specified in `/etc/fstab`.
- Use this command to test the configuration without rebooting.

---

### Example Workflow for Persistent Mounting with LVM

1. **Create a Logical Volume** (if not already done):
   ```bash
   lvcreate -L 10G -n my_lv my_vg
   mkfs.ext4 /dev/my_vg/my_lv
   ```
2. **Identify the UUID**:
   ```bash
   blkid /dev/my_vg/my_lv
   ```
   Example output:
   ```plaintext
   /dev/my_vg/my_lv: UUID="123e4567-e89b-12d3-a456-426614174000" TYPE="ext4"
   ```
3. **Prepare the Mount Point**:
   ```bash
   mkdir -p /data
   ```
4. **Edit `/etc/fstab`** to Add the Entry:
   ```plaintext
   UUID=123e4567-e89b-12d3-a456-426614174000 /data ext4 defaults 0 0
   ```
5. **Apply the Changes**:
   ```bash
   mount -a
   ```
6. **Verify the Mount**:
   ```bash
   df -h
   ```

This guide provides a detailed list of commands for managing partitions, LVM (Logical Volume Manager), and mounting logical volumes persistently using `/etc/fstab`.

---
