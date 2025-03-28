# Linux Troubleshooting Guide

## 🔐 Password Recovery

### RHEL-based Systems (CentOS, Fedora, Rocky Linux, AlmaLinux)
1. **Boot into GRUB Menu**
   - Restart the system and press **`Esc`** (for BIOS) or **`Shift`** (for UEFI) repeatedly during boot.
   - In the GRUB menu, select the kernel version you want to boot (usually the latest one at the top).
   - Highlight the kernel line (the line starting with `linux`) and press **`End`** to go to the last part of the line, then press **`e`** to edit.
   - Find the line starting with `linux` and add `rd.break` at the end.
   - Restart the system and press **`Esc`** (for BIOS) or **`Shift`** (for UEFI) repeatedly during boot.
   - In the GRUB menu, highlight the kernel line (the line starting with `linux`) and press **`End`** to go to the last part of the line, then press **`e`** to edit.
   - Find the line starting with `linux` and add `rd.break` at the end.
2. **Remount and Access Root Shell**
   ```bash
   mount -o remount,rw /sysroot
   chroot /sysroot
   ```
3. **Reset Password and Relabel SELinux**
   ```bash
   passwd root
   touch /.autorelabel
   exit
   reboot
   ```

### Debian-based Systems (Ubuntu, Debian, Kali Linux)
1. **Boot into Recovery Mode**
   - Restart the system and press **`Esc`** or **`Shift`** during boot to access GRUB.
   - Select `Advanced Options`, then choose the appropriate kernel version (usually the latest one) with `Recovery Mode`.
   - Select `Drop to root shell`.
   - If using GRUB, highlight the kernel line, press **`e`**, and add `init=/bin/bash`.
   - Restart the system and press **`Esc`** or **`Shift`** during boot to access GRUB.
   - Select `Advanced Options` → `Recovery Mode` → `Drop to root shell`.
   - If using GRUB, highlight the kernel line, press **`e`**, and add `init=/bin/bash`.
2. **Remount Root and Reset Password**
   ```bash
   mount -o remount,rw /
   passwd root
   reboot
   ```

---

## ⚠️ Ctrl+D Error (Maintenance Mode Issue)

### Possible Causes:
- If there is a mistake in `/etc/fstab`, the system will drop into maintenance mode, and pressing **Ctrl+D** will not fix the issue.
- Corrupt filesystem
- Corrupt filesystem
- Incorrect `/etc/fstab` entries
- Missing system files

### Fix 1: Filesystem Check
```bash
fsck -y /dev/sda
reboot
```

### Fix 2: Check `/etc/fstab`
- If you suspect an error in `/etc/fstab`, boot into **rescue mode** and correct it.
- Use `vim /etc/fstab` to check for incorrect mount points or syntax errors.
- Comment out problematic entries with `#` to prevent system failure.
- Reboot.
- Boot into **rescue mode**.
- Use `vim /etc/fstab` to check for incorrect mount points.
- Comment out problematic entries with `#`.
- Reboot.

### Fix 3: Restore `/etc/passwd` or `/etc/shadow`
```bash
cp /etc/passwd- /etc/passwd
cp /etc/shadow- /etc/shadow
reboot
```

---

**Note:** Always take a backup before making critical system changes! 🔥
