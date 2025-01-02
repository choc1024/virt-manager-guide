### Step 1: Ensure Your CPU Supports Virtualization
1. **Check CPU virtualization support**:
   ```bash
   lscpu | grep Virtualization
   ```
   If you see `VT-x` (Intel) or `AMD-V` (AMD), your CPU supports virtualization.

2. **Enable Virtualization in BIOS/UEFI**:
   If virtualization is disabled, enable it in your system's BIOS/UEFI settings.

---

### Step 2: Install Required Packages
Install KVM and related tools:
```bash
sudo pacman -S qemu libvirt virt-manager ebtables dnsmasq bridge-utils
```

---

### Step 3: Start and Enable the libvirtd Service
1. Start the service:
   ```bash
   sudo systemctl start libvirtd
   ```
2. Enable it to start at boot:
   ```bash
   sudo systemctl enable libvirtd
   ```

---

### Step 4: Add Your User to the `libvirt` Group
To manage VMs without using `sudo`:
```bash
sudo usermod -aG libvirt $(whoami)
```
Log out and log back in for the changes to take effect.

---

### Step 5: Launch virt-manager
1. Install a graphical virtualization manager:
   ```bash
   sudo pacman -S virt-manager
   ```
2. Launch it from the terminal or your application menu:
   ```bash
   virt-manager
   ```

---

### Step 6: Create a Virtual Machine from an ISO
1. Open **virt-manager**.
2. Click **"File" → "New Virtual Machine"**.
3. Select **"Local install media (ISO image or CDROM)"**.
4. Browse to your ISO file and proceed.
5. Configure:
   - **Memory and CPU allocation**: Allocate appropriate resources.
   - **Disk storage**: Create or assign a virtual disk.
6. Complete the setup, and your VM will start.

---

### Step 7: Networking Setup (Optional)
By default, KVM uses NAT for networking. If you want the VM to appear as a separate device on your local network, configure **bridged networking**:
1. Open `/etc/libvirt/qemu.conf` and enable:
   ```bash
   user = "your_username"
   group = "libvirt"
   ```
2. Restart `libvirtd`:
   ```bash
   sudo systemctl restart libvirtd
   ```

---

### Step 8: Managing VMs
Use **virt-manager** for graphical management or `virsh` for command-line control:
- **List running VMs**:
  ```bash
  virsh list
  ```
- **Start a VM**:
  ```bash
  virsh start <vm-name>
  ```
- **Stop a VM**:
  ```bash
  virsh shutdown <vm-name>
  ```

---

### Why Use KVM Over VirtualBox?
- **Performance**: KVM offers near-native performance.
- **Integration**: It is deeply integrated into Linux.
- **Customizability**: Highly configurable, suitable for advanced use cases.
- **Open Source**: No licensing restrictions or proprietary components.
