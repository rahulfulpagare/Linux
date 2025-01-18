# LVM-Partitioning-Commands
# Comprehensive LVM Cheat Sheet

## Starting with Partition Creation

### Open fdisk to create a new partition
```bash
fdisk /dev/sdX
```
- Use `fdisk` to create, modify, or delete partitions.

### Inside fdisk
- Press `n` to create a new partition.
- Select `p` for primary or `e` for extended.
- Specify partition size.
- Press `t` to change the partition type to `8e` (Linux LVM).
- Press d to delete the partition.
- Save changes by pressing `w`.

### Verify the partition
```bash
lsblk
```
- Displays the current partition layout.

### Refresh the partition table (if needed)
```bash
partprobe /dev/sdX
```
- Reloads the partition table without rebooting.

## Physical Volume (PV) Management

### Create a Physical Volume
```bash
pvcreate /dev/sdX1
```
- Prepares the disk or partition for LVM.

### Display Physical Volumes
```bash
pvdisplay
pvs
```
- Shows details about physical volumes.

### Remove a Physical Volume
```bash
pvremove /dev/sdX1
```
- Removes the specified physical volume.

### Rename a Physical Volume
```bash
pvrename /dev/sdX1 /dev/sdX2
```
- Rename the specified physical volume.

## Volume Group (VG) Management

### Create a Volume Group
```bash
vgcreate VG_NAME /dev/sdX1
```
- Combines physical volumes into a volume group.

### Display Volume Groups
```bash
vgdisplay
vgs
```
- Displays information about volume groups.

### Extend a Volume Group
```bash
vgextend VG_NAME /dev/sdY1
```
- Adds a physical volume to the volume group.

### Reduce a Volume Group
```bash
vgreduce VG_NAME /dev/sdX1
```
- Removes a physical volume from the volume group.

### Rename a Volume Group
```bash
vgrename OLD_VG_NAME NEW_VG_NAME
```
- Renames the volume group.

### Remove a Volume Group
```bash
vgremove VG_NAME
```
- Deletes the specified volume group.

## Logical Volume (LV) Management

### Create a Logical Volume
```bash
lvcreate -L SIZE -n LV_NAME VG_NAME
```
- Allocates space for a logical volume.

### Create a Logical Volume with All Free Space
```bash
lvcreate -l 100%FREE -n LV_NAME VG_NAME
```
- Uses all remaining free space.

### Format the Logical Volume
```bash
mkfs.ext4 /dev/VG_NAME/LV_NAME
```
- Formats the logical volume with ext4.

### Mount the Logical Volume
```bash
mkdir /mount/point
mount /dev/VG_NAME/LV_NAME /mount/point
```
- Mounts the logical volume.

### Display Logical Volumes
```bash
lvdisplay
lvs
```
- Displays information about logical volumes.

### Extend a Logical Volume
```bash
lvextend -L+SIZE /dev/VG_NAME/LV_NAME
resize2fs /dev/VG_NAME/LV_NAME
```
- Expands the logical volume and filesystem.

### Reduce a Logical Volume
```bash
umount /dev/VG_NAME/LV_NAME
e2fsck -f /dev/VG_NAME/LV_NAME
resize2fs /dev/VG_NAME/LV_NAME NEW_SIZE
lvreduce -L NEW_SIZE /dev/VG_NAME/LV_NAME
mount /dev/VG_NAME/LV_NAME /mount/point
```
- Shrinks the logical volume and filesystem.

### Rename a Logical Volume
```bash
lvrename VG_NAME OLD_LV_NAME NEW_LV_NAME
```
- Renames the logical volume.

### Remove a Logical Volume
```bash
lvremove /dev/VG_NAME/LV_NAME
```
- Deletes the logical volume.

## Swap Management

### Create Swap Space
```bash
lvcreate -L SIZE -n swap VG_NAME
mkswap /dev/VG_NAME/swap
swapon /dev/VG_NAME/swap
```
- Creates and enables swap space.

### Extend Swap Space
```bash
swapoff /dev/VG_NAME/swap
lvextend -L+SIZE /dev/VG_NAME/swap
mkswap /dev/VG_NAME/swap
swapon /dev/VG_NAME/swap
```
- Expands the swap space.

### Reduce Swap Space (Not Recommended)
```bash
swapoff /dev/VG_NAME/swap
lvreduce -L NEW_SIZE /dev/VG_NAME/swap
mkswap /dev/VG_NAME/swap
swapon /dev/VG_NAME/swap
```
- Shrinks the swap space.

## General LVM and Partition Management Commands

### Scan for LVM Devices
```bash
lvmdiskscan
```
- Scans for LVM-capable devices.

### Check LVM Status
```bash
lvs, vgs, pvs
```
- Displays LVM statuses.

### Activate a Volume Group
```bash
vgchange -ay VG_NAME
```
- Activates the specified volume group.

### Deactivate a Volume Group
```bash
vgchange -an VG_NAME
```
- Deactivates the specified volume group.



# Parted Commands Cheat Sheet with LVM

## Parted Commands

### 1. Create GPT Partition Table
To create a new GPT partition table on the disk:
```bash
parted /dev/sdX mklabel gpt
```
- Initializes the disk with a GPT partition table, required for disks larger than 2TB.

### 2. Create a Partition Using Full Disk Size for LVM
To use the full disk size for LVM:
```bash
parted /dev/sdX mkpart primary 0.00TB 100%
parted /dev/sdX set 1 lvm on
```
- Creates a primary partition spanning the entire disk and sets the LVM flag.

### 3. Check Partition Layout
To display the partition layout of the disk:
```bash
parted /dev/sdX print
```
- Displays the current partitions and available space.

### 4. Delete a Partition
To delete an existing partition:
```bash
parted /dev/sdX rm 1
```
- Removes the specified partition.

### 5. Create a Swap Partition
To create a swap partition on a large disk (e.g., 4TB to 4.2TB):
```bash
parted /dev/sdX mkpart primary linux-swap 4TB 4.2TB
```

## LVM Commands

### 1. Initialize Physical Volume (PV)
To create a physical volume from a partition:
```bash
pvcreate /dev/sdX1
```
- Prepares the partition for LVM usage.

### 2. Create Volume Group (VG)
To create a volume group:
```bash
vgcreate my_vg /dev/sdX1
```
- Groups physical volumes into a single storage pool.

### 3. Create Logical Volume (LV)
To create a logical volume:
```bash
lvcreate -n my_lv -L 50GB my_vg
```
- Allocates 50GB of storage to the logical volume.

### 4. Extend Logical Volume to Use All Free Space
To extend a logical volume:
```bash
lvextend -l +100%FREE /dev/my_vg/my_lv
```

### 5. Format Logical Volume
To format the logical volume with ext4:
```bash
mkfs.ext4 /dev/my_vg/my_lv
```

### 6. Mount Logical Volume
To mount the logical volume:
```bash
mount /dev/my_vg/my_lv /mnt
```

### 7. Reduce Logical Volume
To reduce the size of a logical volume:
```bash
lvreduce -L 30GB /dev/my_vg/my_lv
```

### 8. Remove Logical Volume
To remove a logical volume:
```bash
lvremove /dev/my_vg/my_lv
```

### 9. Remove Volume Group
To delete a volume group:
```bash
vgremove my_vg
```

### 10. Remove Physical Volume
To remove a physical volume:
```bash
pvremove /dev/sdX1
```

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
nano /etc/fstab
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


---

This guide provides a detailed list of commands for managing partitions, LVM (Logical Volume Manager), and persistently mounting logical volumes using /etc/fstab. These commands are intended to assist with disk partitioning and LVM management tasks, including resizing, formatting, and removing volumes
---

