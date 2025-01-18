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

---

These commands are designed to help with disk partitioning and LVM management, including resizing, formatting, and removing volumes.
