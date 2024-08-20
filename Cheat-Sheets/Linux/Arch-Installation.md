# Arch Installation

## Pre-Installation

### Download ISO

Download the newest ISO from a mirror nearby: https://archlinux.org/download/

### Flash USB

Flash the ISO to a USB using rufus or balena etcher

### Set keyboard and font

To load all the keymaps
```bash
localectl list-keymaps
```

To select be-latin1
```bash
loadkeys be-latin1
```

Use a bigger font
```bash
setfont ter-132b
```

### Verify boot mode

Use the following command to determine the boot mode (UEFI or BIOS)
```bash
cat /sys/firmware/efi/fw_platform_size
```

- 64: UEFI
- 32: BIOS

### Connect to internet

using iwctl for wifi
```bash
iwctl

station wlan0 connect [SSID]
```

###

