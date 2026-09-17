# System Updates

Keeping Fedora up to date is an important part of maintaining a secure, stable, and reliable system.

Fedora receives regular updates for the kernel, system libraries, KDE Plasma, applications, drivers, and security fixes.

This chapter focuses on **ongoing system updates** after the initial setup described in [First Steps](01-first-steps.md).

---

## 1. Check for Updates

To check whether updates are available:

```bash
dnf check-update
```

This checks the configured DNF repositories for newer package versions.

If updates are available, DNF lists the packages that can be updated. If no packages are listed, no updates are currently reported by the configured repositories.

> **Note:** `dnf check-update` only checks for available updates. It does not install them.

---

## 2. Update the System

The standard way to update Fedora is:

```bash
sudo dnf upgrade --refresh
```

### What does this command do?

* `sudo` runs the command with administrative privileges.
* `dnf` is Fedora's package manager.
* `upgrade` installs available updates for installed packages.
* `--refresh` forces DNF to refresh repository metadata before checking for updates.

Review the list of packages before confirming the transaction.

For a normal Fedora system, this is generally the preferred way to keep installed RPM packages up to date.

---

## 3. Refresh Repository Metadata

The `--refresh` option is useful when you want DNF to obtain fresh repository metadata before checking for updates.

For example:

```bash
sudo dnf upgrade --refresh
```

You normally do not need to manually delete DNF caches or repository metadata.

If you are troubleshooting repository problems, first inspect the repository configuration with:

```bash
dnf repolist
```

For more information about repositories, see [Repositories](02-repositories.md).

---

## 4. Understanding Kernel Updates

Fedora regularly provides updated Linux kernels.

After installing a new kernel, the currently running kernel does not change until the system is rebooted.

Check the running kernel with:

```bash
uname -r
```

You can also see installed kernels with:

```bash
rpm -q kernel
```

After a kernel update, rebooting allows Fedora to start the newly installed kernel.

```bash
systemctl reboot
```

### Do you need to reboot after every update?

No.

A reboot is generally most important after updates that affect the kernel or other core components that are currently loaded by the running system.

For routine application updates, a reboot is usually not required.

---

## 5. Check the System After an Update

After a significant update, especially one involving the kernel, graphics stack, KDE Plasma, or other core components, it can be useful to verify the system.

Check the running kernel:

```bash
uname -r
```

Check the Fedora release:

```bash
cat /etc/fedora-release
```

Check KDE Plasma:

```bash
plasmashell --version
```

If everything is working normally, no additional action is usually required.

---

## 6. Checking for Package Problems

If an update reports dependency or package problems, do not immediately start adding repositories or removing packages.

First inspect the error message carefully.

You can check the package database for problems with:

```bash
sudo dnf check
```

You can also inspect the repositories currently enabled:

```bash
dnf repolist
```

If the problem involves a specific package, inspect it with:

```bash
dnf info package-name
```

Repository configuration should be investigated before making large changes to the system.

---

## 7. Updating Flatpak Applications

Flatpak applications are updated separately from DNF packages.

To check for available Flatpak updates:

```bash
flatpak update
```

Flatpak will show the applications and runtimes that can be updated before asking for confirmation.

This means that keeping Fedora updated does not automatically mean that every Flatpak application is updated.

For detailed Flatpak and Flathub configuration, see [Flatpak and Flathub](09-flatpak.md).

---

## 8. Updating Firmware

Some hardware firmware updates are provided through the **Linux Vendor Firmware Service (LVFS)** and can be managed using `fwupdmgr`.

Check for available firmware updates:

```bash
fwupdmgr get-updates
```

To inspect supported devices and their firmware status:

```bash
fwupdmgr get-devices
```

Firmware updates are different from normal package updates. They may require a reboot or a special firmware/UEFI environment to complete.

If a firmware update fails, do not repeatedly force the update or modify firmware settings without understanding the error.

First:

1. Read the `fwupdmgr` output carefully.
2. Check whether the message identifies a known issue.
3. Consult the relevant `fwupd` or hardware-vendor documentation.
4. Avoid applying device-specific fixes to a system unless they are appropriate for that hardware.

Firmware availability and update behavior depend on the hardware vendor and device model.

> **Important:** A firmware update failure does not necessarily mean that the entire Fedora installation is damaged. Firmware updates can have hardware- or UEFI-specific requirements and failure conditions.

If no firmware updates are available, no further action is normally required.

---

## 9. Major Fedora Upgrades

Regular package updates are different from upgrading from one Fedora release to another.

For example:

```text
Fedora 44 → Fedora 45
```

is a **release upgrade**, not a normal package update.

Do not attempt a major Fedora upgrade by simply changing repository URLs or manually replacing the Fedora release number.

Before a release upgrade:

* Read the official Fedora upgrade documentation.
* Check that your current release is supported.
* Review known issues for the target release.
* Back up important data.
* Make sure the current system is fully updated.

Release upgrades should be treated as a separate maintenance task.

---

## 10. What to Avoid

Avoid the following practices:

* Running random update scripts from the internet.
* Disabling repositories simply because an update takes longer.
* Removing packages to solve an error without understanding the dependency problem.
* Mixing repositories from different Fedora releases.
* Using old repository configurations from previous Fedora versions.
* Interrupting a package transaction unless absolutely necessary.
* Performing a major Fedora upgrade without reading the current release documentation.

When an update fails, understanding the error is usually more useful than trying several unrelated commands.

---

## 11. Recommended Update Routine

For a typical Fedora KDE workstation, a simple maintenance routine is usually enough.

### Regularly

Run:

```bash
sudo dnf upgrade --refresh
```

### When needed

Check Flatpak updates:

```bash
flatpak update
```

Check firmware updates:

```bash
fwupdmgr get-updates
```

### After important system updates

If the kernel or other core system components were updated, consider rebooting:

```bash
systemctl reboot
```

There is generally no need to run multiple cleanup or optimization commands after every update.

---

## 12. Next Steps

After establishing a regular update routine, continue with:

* [Multimedia](04-multimedia.md)

Repository configuration is covered in [Repositories](02-repositories.md), while Flatpak configuration is covered separately in [Flatpak and Flathub](09-flatpak.md).
