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