To achieve this setup, you'll need to install a lightweight Linux distribution on your Dell Wyse 3040, configure it to run a script at startup, and set up a simple menu to choose between the different VMs, each connecting via VNC to the Proxmox server. Here’s a step-by-step guide:

### 1. Install a Lightweight Linux Distribution
- **Choose a Distribution**: Select a lightweight Linux distribution, such as **Ubuntu Server**, **Alpine Linux**, **Lubuntu**, or **Raspberry Pi OS Lite**.
- **Create a Bootable USB**: Use a tool like **Rufus** (Windows) or **Etcher** (Linux/Mac) to create a bootable USB stick with the chosen distribution.
- **Install on Dell Wyse 3040**: Boot from the USB stick on the Dell Wyse 3040 and follow the installation instructions.

### 2. Install Necessary Software
Once the Linux distribution is installed, you’ll need to install:
- **VNC Viewer**: This will be used to connect to the VMs on the Proxmox server. You can use a lightweight VNC client like `tigervnc-viewer` or `xtightvncviewer`.
- **Dialog or Whiptail**: To create a simple text-based menu for selecting the VMs.

Install these packages with the following commands:
```bash
sudo apt-get update
sudo apt-get install tigervnc-viewer whiptail
```

### 3. Create the Startup Script
Create a script that will display a menu of your VMs and start the VNC session for the selected VM.

1. **Create the Script**:
    ```bash
    sudo nano /home/user/vm_menu.sh
    ```
    Replace `/home/user` with the appropriate user directory.

2. **Add the Following Content to the Script**:
    ```bash
    #!/bin/bash

    # Define VM options
    VM=$(whiptail --title "VM Selection Menu" --menu "Choose a VM to connect to:" 15 60 4 \
    "1" "VM 1" \
    "2" "VM 2" \
    "3" "VM 3" \
    "4" "VM 4" 3>&1 1>&2 2>&3)

    case $VM in
        1)
            vncviewer proxmox-server-ip:5901
            ;;
        2)
            vncviewer proxmox-server-ip:5902
            ;;
        3)
            vncviewer proxmox-server-ip:5903
            ;;
        4)
            vncviewer proxmox-server-ip:5904
            ;;
        *)
            echo "Invalid option."
            ;;
    esac
    ```

    Replace `proxmox-server-ip` with the actual IP address of your Proxmox server and adjust the VNC ports (5901, 5902, etc.) based on your VMs.

3. **Make the Script Executable**:
    ```bash
    chmod +x /home/user/vm_menu.sh
    ```

### 4. Set the Script to Run at Startup
You can set the script to run automatically when the Dell Wyse 3040 boots.

1. **Edit the `.bashrc` File**:
    ```bash
    nano /home/user/.bashrc
    ```

2. **Add the Following Line at the End**:
    ```bash
    /home/user/vm_menu.sh
    ```

    This will automatically run the script when the terminal session starts.

### 5. Test the Setup
1. Reboot the Dell Wyse 3040.
2. When it starts, you should see the menu prompting you to choose a VM.
3. Selecting a VM should open a VNC connection to the corresponding VM on your Proxmox server.

### 6. Optional: Auto-Login Configuration
To make this setup more seamless, you can configure your Linux distribution to auto-login, so the script starts without manual intervention.

For **Ubuntu**, you can enable auto-login by editing the `/etc/lightdm/lightdm.conf` file:
```bash
sudo nano /etc/lightdm/lightdm.conf
```
Add or modify the following lines:
```bash
[SeatDefaults]
autologin-user=user
autologin-user-timeout=0
```
Replace `user` with your actual username.

### Summary
With this setup, your Dell Wyse 3040 will boot into a lightweight Linux environment, automatically start a script that lets you choose a VM, and open a VNC connection to the selected VM. This approach should provide you with a simple, user-friendly interface to access your Proxmox VMs.


## Installation script on debian machine

```bash
#!/bin/bash

# Ensure the script is run as root
if [[ $EUID -ne 0 ]]; then
   echo "This script must be run as root" 
   exit 1
fi

# Variables
USER_NAME="your_user"  # Replace with your actual username
PROXMOX_IP="192.168.1.100"  # Replace with your Proxmox server IP
SCRIPT_PATH="/home/$USER_NAME/vm_menu.sh"

# Update package list and install required packages
apt-get update
apt-get install -y tigervnc-viewer whiptail

# Create the VM selection script
cat <<EOF > $SCRIPT_PATH
#!/bin/bash

# VM selection menu
VM=\$(whiptail --title "VM Selection Menu" --menu "Choose a VM to connect to:" 15 60 4 \\
"1" "VM 1" \\
"2" "VM 2" \\
"3" "VM 3" \\
"4" "VM 4" 3>&1 1>&2 2>&3)

case \$VM in
    1)
        vncviewer $PROXMOX_IP:5901
        ;;
    2)
        vncviewer $PROXMOX_IP:5902
        ;;
    3)
        vncviewer $PROXMOX_IP:5903
        ;;
    4)
        vncviewer $PROXMOX_IP:5904
        ;;
    *)
        echo "Invalid option."
        ;;
esac
EOF

# Make the VM selection script executable
chmod +x $SCRIPT_PATH

# Add the script to .bashrc to run at login
echo "$SCRIPT_PATH" >> /home/$USER_NAME/.bashrc

# Optionally enable auto-login (uncomment if needed)
# Auto-login configuration for Debian-based systems with LightDM
#cat <<EOF > /etc/lightdm/lightdm.conf
#[SeatDefaults]
#autologin-user=$USER_NAME
#autologin-user-timeout=0
#EOF

# Display a message indicating the setup is complete
echo "Setup complete! Reboot your system to start using the VM selection menu."

# Optionally reboot the system to apply changes (uncomment if needed)
#reboot
```

## Proxmox setup

To enable VNC connections to your VMs from your Dell Wyse 3040 device, you’ll need to configure the Proxmox server accordingly. Here’s a step-by-step guide on what you need to do on the Proxmox side:

### 1. Ensure VNC is Enabled on Proxmox

Proxmox uses VNC by default for console access to VMs. However, by default, this access is usually bound to the web interface, and not directly exposed via VNC ports. To make it accessible from an external VNC viewer, you'll need to ensure each VM has a VNC port open.

### 2. Assign Static VNC Ports to VMs

You need to assign specific VNC ports to each VM manually. Follow these steps:

1. **SSH into Your Proxmox Server**:
   Access your Proxmox server via SSH.

2. **Locate the Configuration File for the VM**:
   The configuration files for VMs are located in `/etc/pve/qemu-server/`. Each VM has a configuration file named `<VMID>.conf`, where `<VMID>` is the ID of the VM.

   Example:
   ```bash
   nano /etc/pve/qemu-server/101.conf
   ```

3. **Assign a Static VNC Port**:
   Add or modify the `vncport` parameter in the VM's configuration file to assign a static VNC port. For example:

   ```bash
   args: -vnc 0.0.0.0:1
   ```
   This line will expose the VNC server on port `5901` (`5900 + 1`).

   You can assign different ports for different VMs like so:
   - VM 1: `args: -vnc 0.0.0.0:1` (port `5901`)
   - VM 2: `args: -vnc 0.0.0.0:2` (port `5902`)
   - VM 3: `args: -vnc 0.0.0.0:3` (port `5903`)
   - VM 4: `args: -vnc 0.0.0.0:4` (port `5904`)

4. **Save and Exit**:
   Save the file and exit the editor.

5. **Restart the VM**:
   After editing the configuration, restart the VM for the changes to take effect:
   ```bash
   qm stop <VMID>
   qm start <VMID>
   ```

### 3. Configure Firewall (If Necessary)
If you have a firewall configured on Proxmox or your network, make sure that the VNC ports (e.g., `5901`, `5902`, etc.) are open so that the Dell Wyse 3040 can connect to them.

### 4. Test VNC Access
1. From a machine with a VNC client installed, try connecting to the Proxmox server using the IP and the specified port, e.g., `192.168.1.100:5901`.

2. If everything is configured correctly, you should be able to connect to the VM's console via VNC.

### Summary
On the Proxmox side, you need to ensure that each VM has a static VNC port assigned and that these ports are accessible from your network. This involves editing the VM configuration files and possibly adjusting firewall settings. Once done, the setup on your Dell Wyse 3040 should be able to connect directly to the VMs via VNC.
