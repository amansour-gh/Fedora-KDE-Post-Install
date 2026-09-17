# Troubleshooting

This chapter provides a practical approach to diagnosing common problems on Fedora KDE Plasma.

The goal is not to provide a solution for every possible problem, but to provide a reliable starting point for identifying the cause before making system changes.

> **Important:** Diagnose the problem first. Avoid changing multiple settings or removing packages without understanding what they affect.

---

## First Checks

When something is not working as expected, start with a few basic checks.

### Fedora and Kernel

```bash
cat /etc/fedora-release
uname -r
```

### Failed Services

```bash
systemctl --failed
```

If there are no failed units, systemd is not currently reporting failed services.

### Disk Space

```bash
df -h
```

A full filesystem can cause unexpected problems with applications, updates, and system services.

### Memory

```bash
free -h
```

On Fedora, some memory usage is expected because Linux uses available memory for caching.

---

## Package and DNF Problems

If the problem involves installing, removing, or updating software, first check the package manager.

### Check for Package Problems

```bash
sudo dnf check
```

### Review Recent Transactions

```bash
dnf history
```

To inspect a specific transaction:

```bash
dnf history info <ID>
```

This is useful when a problem appears immediately after installing or removing software.

### Review Before Removing Packages

Avoid blindly running:

```bash
sudo dnf autoremove
```

Always review the packages DNF proposes to remove before confirming.

A package may appear unused while still being useful to another application or workflow.

---

## Systemd and Service Problems

If a system service is not working, start by checking its status.

```bash
systemctl status <service>
```

List failed services:

```bash
systemctl --failed
```

For errors from the current boot:

```bash
journalctl -b -p err
```

Do not restart or disable a service simply because it appears unfamiliar. First determine what the service does and whether it is actually related to the problem.

---

## Graphics Problems

Graphics problems can involve the GPU, kernel driver, Mesa, Wayland, X11, or an application.

### Identify the Graphics Stack

```bash
inxi -Gxx
```

For OpenGL information:

```bash
glxinfo -B
```

If `glxinfo` is not available, install the package that provides it only when needed for diagnosis.

For Intel and AMD systems, the standard kernel and Mesa drivers are normally preferred.

For NVIDIA systems, use the recommended Fedora-compatible driver source documented in the graphics section of this guide.

### Wayland

Check the current session:

```bash
echo $XDG_SESSION_TYPE
```

Expected output on a Wayland session:

```text
wayland
```

If a problem occurs only in one application, test the application separately before changing the entire desktop graphics configuration.

---

## Audio Problems

Fedora KDE uses PipeWire and WirePlumber for modern audio management.

Check available audio devices:

```bash
wpctl status
```

Check the user audio services:

```bash
systemctl --user status pipewire pipewire-pulse wireplumber
```

If the services are active but the wrong output or input device is selected, check the KDE audio settings before changing PipeWire configuration files.

Avoid creating custom PipeWire configuration unless the default setup does not solve the problem.

---

## Network Problems

### NetworkManager

Check device status:

```bash
nmcli device
```

For a lower-level view:

```bash
ip link
```

Check the NetworkManager service:

```bash
systemctl status NetworkManager
```

For Wi-Fi problems, first determine whether the wireless device is detected before troubleshooting the connection itself.

For example:

```bash
nmcli device
```

If the device is not detected, investigate the hardware or driver before changing NetworkManager settings.

---

## Bluetooth Problems

Check the Bluetooth service:

```bash
systemctl status bluetooth
```

Check the Bluetooth controller:

```bash
bluetoothctl show
```

If the controller is available but a device will not connect, remove and pair the device again before making system-level changes.

---

## Display and KDE Plasma Problems

For display problems, first check the KDE display settings.

Useful information can also be obtained with:

```bash
kscreen-doctor -o
```

This shows connected displays, resolutions, refresh rates, and their current state.

When troubleshooting KDE Plasma:

1. Determine whether the problem affects the entire desktop or one application.
2. Check whether the problem occurs after a reboot.
3. Check whether it is specific to Wayland or X11.
4. Avoid changing several Plasma settings at once.

If a Plasma configuration file needs to be modified, make a backup before changing it.

---

## Storage Problems

Check disks and mount points:

```bash
lsblk
```

Check filesystem usage:

```bash
df -h
```

Check mounted filesystems:

```bash
findmnt
```

For Btrfs systems, remember that snapshots are not backups.

A snapshot stored on the same physical disk does not protect against disk failure.

Before performing filesystem recovery or rollback operations, identify the affected filesystem and understand the recovery procedure. Do not use a rollback command blindly.

---

## Logs and Journal

The system journal is one of the most useful sources of diagnostic information.

### Current Boot

```bash
journalctl -b
```

### Errors from Current Boot

```bash
journalctl -b -p err
```

### Previous Boot

```bash
journalctl -b -1
```

When reporting a problem, include the relevant error rather than copying the entire journal.

For a specific service:

```bash
journalctl -u <service>
```

For a user service:

```bash
journalctl --user -u <service>
```

---

## Application Problems

When an application does not start correctly, first determine how it was installed.

Common sources include:

* Fedora RPM packages
* Flatpak
* Vendor repositories
* AppImage

For RPM packages, check the package:

```bash
dnf list installed <package>
```

For Flatpak applications:

```bash
flatpak list
```

If an application fails to start, running it from the terminal can reveal useful error messages.

Before deleting application configuration, make a backup of the relevant configuration directory or file.

Do not immediately reinstall an application. First determine whether the problem is related to:

* the application itself
* its configuration
* missing dependencies
* permissions
* the desktop environment
* the graphics or audio stack

---

## Safe Recovery Principles

When troubleshooting, follow a few basic rules:

* Change one thing at a time.
* Prefer reversible changes.
* Make backups before modifying configuration files.
* Do not remove packages simply because they appear unfamiliar.
* Do not disable services without understanding their purpose.
* Do not copy commands from old guides without checking whether they apply to the current Fedora release.
* Avoid system-wide configuration changes when an application-specific solution is sufficient.
* Record what was changed so it can be reverted if necessary.

For major system changes, make sure a current backup is available before proceeding.

---

## When to Search for a Solution

If the basic checks do not identify the problem, use reliable sources.

Prefer:

* Fedora documentation
* KDE documentation
* Official project documentation
* Fedora Bugzilla
* The application's official issue tracker

When evaluating a solution, check:

1. The Fedora version it targets.
2. Whether it applies to KDE Plasma.
3. Whether it applies to Wayland or X11.
4. Whether the package or configuration mentioned still exists.
5. Whether the proposed change is reversible.

Avoid applying old commands simply because they appear frequently in search results.

---

## Reporting a Problem

When asking for help, provide enough information to reproduce or diagnose the problem.

Useful information may include:

```bash
cat /etc/fedora-release
uname -r
inxi -Fzxx
systemctl --failed
journalctl -b -p err
```

Do not share sensitive information from logs or configuration files.

If the problem affects a specific component, provide the relevant command output instead of unrelated system information.

---

## Summary

A reliable troubleshooting process is:

1. Identify exactly what is not working.
2. Check the basic system state.
3. Identify whether the problem is hardware, system, desktop, or application related.
4. Check relevant logs.
5. Make one controlled change.
6. Test the result.
7. Revert the change if it does not help.
8. Search official or current documentation when deeper investigation is required.

The goal is to **diagnose before modifying** and to keep the system stable and reproducible.