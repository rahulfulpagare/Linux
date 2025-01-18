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
