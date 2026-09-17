# Hardware & Devices

A practical approach to checking and configuring hardware on Fedora KDE.

Fedora normally provides kernel support and drivers for most common hardware automatically. The goal is to verify that important devices are working correctly and install additional components only when they are actually required.

---

## 1. Identify the Hardware

Start by checking the basic hardware information:

```bash
inxi -Fzxx
```

If `inxi` is not installed:

```bash
sudo dnf install inxi
```

`inxi` can provide a useful overview of:

* CPU
* GPU
* Memory
* Storage
* Network devices
* Audio
* Kernel
* Desktop environment

Use hardware detection tools to understand the system before installing additional drivers.

---

## 2. Check the Kernel

The Linux kernel provides much of Fedora's hardware support.

Check the running kernel:

```bash
uname -r
```

Check installed kernels:

```bash
rpm -q kernel
```

Keep the system updated so that hardware support and bug fixes are available through normal Fedora updates.

Avoid replacing the Fedora kernel with a custom kernel unless there is a specific requirement.

---

## 3. Firmware

Some hardware requires firmware provided separately from the Linux kernel.

Check whether firmware updates are available:

```bash
sudo fwupdmgr get-updates
```

If updates are available:

```bash
sudo fwupdmgr update
```

You can also inspect supported devices:

```bash
fwupdmgr get-devices
```

> **Recommendation:** Prefer firmware supplied through `fwupd` and the normal Fedora update process instead of downloading firmware manually from unofficial sources.

Firmware updates can affect low-level device functionality. Do not interrupt a firmware update.

Not every device supports firmware updates through `fwupd`, so an empty update list does not necessarily indicate a problem.

---

## 4. Graphics

First identify the installed graphics hardware:

```bash
lspci | grep -Ei 'vga|3d|display'
```

For Intel and AMD graphics, the standard Fedora kernel and Mesa stack should generally be used.

For NVIDIA hardware, follow the graphics guidance in [Graphics](05-graphics.md).

Check the active renderer when troubleshooting graphics:

```bash
glxinfo -B
```

If `glxinfo` is unavailable:

```bash
sudo dnf install mesa-demos
```

On systems with multiple GPUs, verify which GPU is being used rather than assuming that the discrete GPU is always active.

---

## 5. Hardware Acceleration

Hardware video acceleration can reduce CPU usage during video playback and encoding.

For VA-API devices, check the available capabilities with:

```bash
vainfo
```

If `vainfo` is unavailable:

```bash
sudo dnf install libva-utils
```

Hardware acceleration depends on the GPU, driver, application, codec, and media format.

Do not install additional graphics packages simply because a command is missing. First determine whether the feature is actually required.

See [Multimedia](04-multimedia.md) and [Graphics](05-graphics.md) for more details.

---

## 6. Audio

Fedora KDE uses PipeWire and WirePlumber for modern desktop audio.

Check the audio devices:

```bash
wpctl status
```

You can also check the user services:

```bash
systemctl --user status pipewire pipewire-pulse wireplumber
```

For normal desktop use, there is usually no need to replace PipeWire with another audio system.

If an audio device is missing:

1. Check whether the hardware is detected.
2. Check `wpctl status`.
3. Check the selected output/input device in KDE.
4. Check application-specific audio settings.
5. Only then investigate driver or firmware issues.

---

## 7. Network Devices

Check network interfaces:

```bash
ip link
```

For PCI network hardware:

```bash
lspci | grep -Ei 'ethernet|network'
```

For USB network hardware:

```bash
lsusb
```

Use KDE's network management tools for normal configuration.

Avoid installing third-party network drivers unless the hardware actually requires them.

---

## 8. Wi-Fi and Bluetooth

Check network devices:

```bash
nmcli device
```

For Bluetooth:

```bash
bluetoothctl show
```

If Bluetooth is not available, check the service:

```bash
systemctl status bluetooth
```

If the hardware is detected but unavailable, investigate firmware and service status before installing additional software.

For normal KDE systems, prefer the Plasma network and Bluetooth interfaces for everyday configuration.

---

## 9. Printers and Scanners

Connect the device and allow Fedora to detect it before installing manufacturer-specific software.

For printers, check:

**System Settings → Printers**

If a device is not detected, first verify:

* USB or network connectivity
* Device power
* Network visibility
* Required printing services
* Available Fedora packages

Install vendor-specific drivers only when the standard Fedora printing stack does not provide the required functionality.

Avoid downloading printer drivers from unofficial websites.

---

## 10. External Monitors

For external displays, use:

**System Settings → Display & Monitor**

Check:

* Resolution
* Refresh rate
* Scaling
* Display arrangement
* Primary display
* Orientation

For troubleshooting, identify the connected displays:

```bash
kscreen-doctor -o
```

If a display works at a lower resolution or refresh rate than expected, investigate the connection, cable, adapter, GPU driver, and monitor capabilities before changing system configuration.

---

## 11. Laptop Power Management

Fedora normally provides power management through the desktop environment and system services.

If `power-profiles-daemon` is installed, check the current power profile:

```bash
powerprofilesctl get
```

List available profiles:

```bash
powerprofilesctl list
```

Use KDE's power settings for normal laptop configuration.

Avoid installing multiple power-management frameworks that attempt to control the same hardware.

---

## 12. Storage Devices

Check detected storage devices:

```bash
lsblk
```

For filesystem information:

```bash
lsblk -f
```

For PCI storage hardware:

```bash
lspci | grep -Ei 'storage|nvme|sata'
```

Do not modify partitions or filesystems unless you understand the consequences.

For Btrfs systems, see [Backup & Recovery](12-backup-recovery.md) for snapshots and recovery practices.

---

## 13. USB Devices

List connected USB devices:

```bash
lsusb
```

If a USB device is not detected:

1. Try another USB port.
2. Check the cable or adapter.
3. Check `lsusb`.
4. Check system logs when necessary.
5. Check whether firmware or additional support is required.

Do not assume that a missing graphical device automatically means that a driver must be installed.

---

## 14. Hardware Troubleshooting

When hardware does not work, follow a structured process.

### 1. Identify the hardware

```bash
inxi -Fzxx
```

### 2. Check whether Linux detects it

Use appropriate tools such as:

```bash
lspci
lsusb
```

### 3. Check the kernel

```bash
uname -r
```

### 4. Check firmware

```bash
fwupdmgr get-devices
```

### 5. Check relevant services

For example:

```bash
systemctl status bluetooth
```

or:

```bash
systemctl --user status pipewire wireplumber
```

### 6. Check logs when necessary

```bash
journalctl -b -p warning
```

This approach is preferable to immediately installing third-party drivers or changing multiple components at once.

For broader troubleshooting procedures, see [Troubleshooting](15-troubleshooting.md).

---

## 15. Recommended Hardware Setup

A clean Fedora KDE hardware setup should generally include:

* Current Fedora kernel
* Firmware updates through `fwupd` when supported
* Standard Fedora graphics stack
* PipeWire for audio
* NetworkManager for networking
* KDE System Settings for desktop hardware configuration
* Only the additional drivers or firmware actually required by the hardware

The best approach is to **identify the hardware first, verify whether Fedora already supports it, and make the smallest necessary change**.

---

## 16. What to Avoid

Avoid:

* Installing drivers before identifying the hardware
* Replacing working Fedora drivers unnecessarily
* Installing multiple tools that manage the same hardware
* Downloading drivers from unofficial websites
* Installing custom kernels without a specific reason
* Changing graphics configuration without understanding the GPU setup
* Disabling security features simply to make a device work
* Making several hardware changes at once when troubleshooting

---

## Next Step

Continue with [Troubleshooting](15-troubleshooting.md) to review a structured approach for diagnosing common Fedora KDE problems.
