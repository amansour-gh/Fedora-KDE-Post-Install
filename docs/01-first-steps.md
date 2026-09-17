# First Steps

After installing Fedora KDE Plasma, the first step is to bring the system fully up to date before installing additional software or making configuration changes.

Keeping the base system updated first helps ensure that the packages, dependencies, security updates, and system components are in a consistent state.

> **Important:** This guide targets the current stable Fedora KDE Plasma release. Commands and package names may change between Fedora releases, so always check the current Fedora documentation before applying instructions to a different release.

## 1. Update the System

Open **Konsole** and run:

```bash
sudo dnf upgrade --refresh
```

### What does this command do?

* `sudo` runs the command with administrative privileges.
* `dnf` is Fedora's package management tool.
* `upgrade` updates installed packages when newer versions are available.
* `--refresh` forces DNF to refresh repository metadata before checking for updates.

Review the packages that DNF proposes to update, then confirm when prompted.

### After the Update

If the update includes the kernel, system libraries, KDE Plasma components, or other core system components, reboot the computer before continuing:

```bash
systemctl reboot
```

A reboot is not required after every package update, but it is recommended after updates that affect components currently loaded by the running system.

## 2. Verify the System

After rebooting, confirm the Fedora release:

```bash
cat /etc/fedora-release
```

Then check the running kernel:

```bash
uname -r
```

Finally, check the KDE Plasma version:

```bash
plasmashell --version
```

These commands provide a quick snapshot of the system before continuing with the rest of the guide.

## Next Steps

After completing the initial system update, continue with:

* [Repositories](02-repositories.md)