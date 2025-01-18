
# How to Fix GRUB Terminal and Restore Windows Bootloader

This guide explains the steps to fix the **GRUB terminal** issue and restore the **Windows bootloader** after removing a Linux partition (Ubuntu).

## Prerequisites:
- **Windows installation media** (USB or DVD)
- Access to **Command Prompt** during boot

## Steps to Fix GRUB Terminal Issue

### Step 1: Boot from Windows Installation Media
1. Insert your **Windows installation USB or DVD** into the system.
2. Restart your computer and boot from the **installation media** (select **USB** or **DVD** in the boot menu).
3. On the Windows installation screen, select **Next**.

### Step 2: Access Repair Options
1. Instead of clicking **Install Now**, click on **Repair your computer** (bottom-left corner).
2. Choose **Troubleshoot > Advanced Options > Command Prompt**.

### Step 3: Use Diskpart to Select the Correct Disk and Partition
1. In the **Command Prompt**, type `diskpart` to enter the **disk partitioning tool**:
   ```
   diskpart
   ```

2. List all the available disks:
   ```
   list disk
   ```

   Identify **Disk 0** (where Windows is installed) and **Disk 1** (where Ubuntu was removed).

3. Select **Disk 0** (the disk where Windows is installed):
   ```
   select disk 0
   ```

4. List the partitions on **Disk 0**:
   ```
   list partition
   ```

5. Identify the **EFI partition** (usually **Partition 1**, 100-500 MB, and labeled **System**).

### Step 4: Assign a Letter to the EFI Partition
1. If the EFI partition is listed, **select it**:
   ```
   select partition 1
   ```

2. Assign a letter (e.g., **Z**) to the EFI partition:
   ```
   assign letter=Z:
   exit
   ```

### Step 5: Remove GRUB Files (If Present)
1. Navigate to the EFI folder:
   ```
   cd /d Z:\EFI
   ```

2. List the contents of the folder:
   ```
   dir
   ```

3. Look for any **Ubuntu** or **GRUB** folders. If found, remove them:
   ```
   rmdir /s /q ubuntu
   ```

### Step 6: Repair the Windows Bootloader
1. Run the following commands to repair the bootloader:

   - **Fix the Master Boot Record (MBR)**:
     ```
     bootrec /fixmbr
     ```

   - **Fix the Boot Sector**:
     ```
     bootrec /fixboot
     ```

   - **Rebuild the Boot Configuration Data (BCD)**:
     ```
     bootrec /rebuildbcd
     ```

2. Optionally, scan for Windows installations:
   ```
   bootrec /scanos
   ```

   If Windows is detected, type **Y** to add it to the boot list.

### Step 7: Exit Command Prompt and Restart
1. Type **exit** to close the **Command Prompt**.
2. Click **Continue** to restart your computer.

### Step 8: Verify Windows Booting Properly
After restarting, your system should now boot directly into **Windows** without showing the **GRUB terminal**.

---

## Additional Troubleshooting (If Windows Doesn't Boot)
1. **Check BIOS/UEFI Settings**: Ensure that **Windows Boot Manager** is set as the primary boot option.
2. **Startup Repair**: If Windows doesn't boot, return to **Repair your computer > Troubleshoot > Advanced Options > Startup Repair**.

---

### Conclusion:
Following these steps will successfully remove the GRUB bootloader and restore the Windows bootloader, allowing your system to boot directly into Windows.

