Reinstalling ESXi while **preserving the datastore** is a useful process when the ESXi installation has become corrupted, but the VM data on the datastore is intact. This process ensures that the VM data is not lost while reinstalling the hypervisor. Below are the detailed steps:

---

## **Before You Begin**

### 1. **Ensure You Have Backups**
- **VM Backups**: If possible, back up your VMs to another location (e.g., external storage, another host, or via a Veeam or similar backup tool).
- **Host Configuration Backup**:
  - Export the configuration if the host is accessible:
    ```bash
    vim-cmd hostsvc/firmware/backup_config
    ```
    This creates a `.tgz` file that can be downloaded via an SCP client like WinSCP or from `/scratch/downloads/`.

---

### 2. **Download the Correct ESXi Installer**
- Get the ESXi installer ISO from the VMware website:
  - Ensure it matches the version (or a compatible upgrade path) with your existing installation.
  - If your hardware requires custom drivers, download a **custom ISO** from the vendor or use VMware’s **Image Builder**.

---

### 3. **Prepare Bootable Media**
- Create a bootable USB or CD using tools like **Rufus** or **UNetbootin** for USB creation.

---

## **Reinstalling ESXi Without Affecting Datastores**

### Step 1: **Boot from the Installation Media**
1. Insert the bootable USB or CD into the ESXi host.
2. Restart the server and enter the BIOS/UEFI to select the boot device.
3. Boot into the ESXi installer.

---

### Step 2: **Start the Installation Process**
1. On the initial ESXi installer screen, press **Enter** to begin the installation.
2. Accept the VMware EULA when prompted by pressing **F11**.

---

### Step 3: **Select the Target Disk**
1. The installer will scan and display the available disks.
   - Look for the disk/USB where ESXi was previously installed.
   - **DO NOT SELECT THE DATASTORE DISK(S)**.
2. Select the disk that contains the corrupted or old ESXi installation (usually a USB drive, SD card, or a small SSD).

---

### Step 4: **Preserve the Datastore**
1. If the installer detects an existing ESXi installation, you will see an option to **overwrite the system partitions**.
   - The installer should notify you about existing VMFS datastores.
2. **Confirm that you want to install ESXi while preserving the datastore** by carefully reading the on-screen prompts:
   - VMware provides an explicit warning before overwriting the datastore. Ensure you choose the option to preserve VMFS.

---

### Step 5: **Complete the Installation**
1. Follow the on-screen instructions to finalize the installation.
2. Once the installation is complete, remove the installation media, and reboot the host.

---

## **Post-Installation Tasks**

### Step 6: **Reconnect the Datastore**
1. After the host boots into the newly installed ESXi environment:
   - Open the **DCUI (Direct Console User Interface)** or connect via the **vSphere Web Client**.
   - Check if the datastore is visible under **Storage**:
     - If the datastore is not visible, manually add it:
       - Go to **Storage > Add Datastore > Existing Datastore**.
       - Select the disk with the VMFS partition and mount it.

---

### Step 7: **Re-register Virtual Machines**
1. If the VMs are not automatically detected:
   - Go to the **datastore browser** under the **Storage** menu.
   - Navigate to the VM folders and locate the `.vmx` files for each VM.
   - Right-click on the `.vmx` file and select **Register VM**.
   - Assign the VMs to the inventory.

---

### Step 8: **Restore Host Configuration (Optional)**
If you previously backed up the host configuration, restore it:
1. Upload the `.tgz` backup file to the host.
2. Restore the configuration via SSH or CLI:
   ```bash
   vim-cmd hostsvc/firmware/restore_config <path-to-backup-file>
   ```
3. Reboot the host.

---

### Step 9: **Verify the Environment**
1. Confirm the following:
   - All VMs are operational.
   - Network configurations (vSwitches, port groups, VLANs) are correct.
   - Datastores are accessible and healthy.
2. Update ESXi to the latest patch level if necessary.

---

## **Troubleshooting Tips**
- **Accidental Overwrite of Datastore**: If the datastore is overwritten, you may need to use a VMFS recovery tool (e.g., **VMFS Recovery by Stellar** or **EaseUS**).
- **Missing VMs**: Double-check datastore paths or use the `find` command in SSH to locate `.vmx` files.
- **Driver Issues**: If hardware is not detected post-installation, use a customized ISO or load drivers manually.

Let me know if you need additional details on any step!
