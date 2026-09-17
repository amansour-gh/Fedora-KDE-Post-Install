# Backup & Recovery

A practical backup and recovery setup for Fedora KDE, with special consideration for systems using Btrfs and Snapper.

The goal is to protect both the operating system and personal data while keeping recovery simple and reversible.

---

## 1. Check the Filesystem

Before configuring Btrfs-specific recovery tools, verify that the system uses Btrfs:

```bash
findmnt -t btrfs
```

If `/` is mounted as `btrfs`, Snapper can be used for system snapshots.

> Snapper is useful for system recovery, but snapshots are **not a replacement for an external backup**.

---

## 2. Snapper

Snapper manages Btrfs snapshots and is particularly useful for recovering from system changes and package updates.

Install it if necessary:

```bash
sudo dnf install snapper
```

Check the installed version:

```bash
snapper --version
```

### Check Existing Configurations

Do not create a new configuration if one already exists.

```bash
sudo snapper list-configs
```

Typical configurations may include:

```text
Config   Subvolume
root     /
home     /home
```

If the required configuration already exists, no additional setup is necessary.

---

## 3. Automatic Snapshots

On a correctly configured Fedora system, Snapper may already create automatic timeline snapshots and snapshots around package operations.

Check the root snapshots:

```bash
sudo snapper -c root list
```

Look for entries such as:

* Timeline snapshots
* Pre/Post snapshots around package operations

Automatic snapshot policies should normally be left enabled unless there is a specific reason to change them.

---

## 4. Create a Manual Snapshot

Before making a significant system change, a manual snapshot can be useful.

For the root filesystem:

```bash
sudo snapper -c root create --description "Before major system change"
```

Check the result:

```bash
sudo snapper -c root list
```

For the Home configuration, if available:

```bash
sudo snapper -c home create --description "Before major user-data change"
```

---

## 5. Btrfs Assistant

**Btrfs Assistant** provides a graphical interface for managing Btrfs and Snapper.

Install it when using a Btrfs-based system:

```bash
sudo dnf install btrfs-assistant
```

Launch it from the KDE application menu.

It can be used to:

* View Btrfs filesystems and subvolumes
* Manage Snapper configurations
* View snapshots
* Create and delete snapshots
* Inspect snapshot differences
* Perform supported restore operations

For KDE users, Btrfs Assistant is a convenient graphical alternative to using Snapper entirely from the terminal.

> Keep Snapper CLI available even when using Btrfs Assistant. Recovery should not depend on a single graphical application.

---

## 6. Review Snapshots

List root snapshots:

```bash
sudo snapper -c root list
```

List Home snapshots:

```bash
sudo snapper -c home list
```

Before deleting a snapshot, make sure it is no longer needed.

Delete a specific snapshot:

```bash
sudo snapper -c root delete SNAPSHOT_NUMBER
```

Replace `SNAPSHOT_NUMBER` with the actual snapshot number.

Avoid deleting large groups of snapshots without checking the retention policy and available disk space first.

---

## 7. Recover Files from a Snapshot

Snapshots can be useful when a file was accidentally modified or deleted.

First identify the required snapshot:

```bash
sudo snapper -c home list
```

Btrfs Assistant can also be used to browse available snapshots and perform supported recovery operations.

For individual files, prefer restoring only the required files instead of performing a complete system rollback.

---

## 8. Undo a System Change

When a specific package or configuration change caused a problem, Snapper can compare two snapshots.

For example:

```bash
sudo snapper -c root status SNAPSHOT1..SNAPSHOT2
```

This helps identify files changed between the two snapshots.

For supported changes, Snapper can undo those changes:

```bash
sudo snapper -c root undochange SNAPSHOT1..SNAPSHOT2
```

Review the proposed changes carefully before applying them.

> `undochange` is intended for reverting changes between snapshots. It is different from a full system rollback.

---

## 9. System Rollback

A complete system rollback should be treated as a recovery operation, not a normal maintenance command.

Before performing one:

1. Confirm the correct snapshot.
2. Make sure important personal data is backed up separately.
3. Understand that `/boot` may be on a separate filesystem and is not necessarily included in a Btrfs snapshot.
4. Prefer Btrfs Assistant or the Fedora/Snapper documentation for the exact rollback procedure for the current filesystem layout.

Do not use a rollback command blindly on an unfamiliar Btrfs layout.

---

## 10. Real Backups

Snapshots protect against unwanted changes, but they do **not** protect against:

* Disk failure
* Loss or theft of the computer
* Filesystem corruption affecting the entire disk
* Accidental deletion of all snapshots
* Physical damage

For important personal data, keep a separate backup on another storage device or system.

A simple file backup can use `rsync`.

Example:

```bash
rsync -a --info=progress2 ~/Documents/ /run/media/$USER/Backup/Documents/
```

To restore:

```bash
rsync -a --info=progress2 /run/media/$USER/Backup/Documents/ ~/Documents/
```

Adjust the source and destination paths to match the actual backup device.

> Do not copy the example path blindly. Verify the mounted backup location first.

---

## 11. What Should Be Backed Up?

At minimum, consider backing up:

* Documents
* Pictures
* Videos
* Downloads if needed
* Work/project directories
* Browser profiles when required
* Application data
* SSH keys
* Important configuration files
* Password-manager data or vault exports when appropriate

For KDE systems using KWallet, its data belongs to the protected Home backup and should **never** be committed to Git or uploaded to GitHub.

---

## 12. Recommended Recovery Strategy

Use different tools for different problems:

| Problem                               | Recommended solution                     |
| ------------------------------------- | ---------------------------------------- |
| Accidentally changed a system setting | Snapper                                  |
| Problem after package update          | Snapper pre/post snapshots               |
| Deleted/modified personal file        | Home snapshot or backup                  |
| Need to browse snapshots graphically  | Btrfs Assistant                          |
| SSD/HDD failure                       | External backup                          |
| New installation                      | Restore personal data from backup        |
| Complete system recovery              | Snapper/Btrfs recovery + external backup |

---

## 13. Recommended Final State

A Fedora KDE Btrfs system should ideally have:

* Btrfs for the system filesystem
* Snapper configured for required subvolumes
* Automatic snapshots enabled
* Btrfs Assistant available for graphical management
* Important personal data backed up separately
* Recovery procedures tested before they are actually needed

### Important

**Snapshots are not backups.**

Use Snapper for fast local recovery from system and file changes, and use a separate backup destination for protection against hardware failure or complete system loss.

---

## What to Avoid

* Treating Snapper snapshots as the only backup
* Disabling automatic snapshots without a reason
* Deleting snapshots without checking what they contain
* Performing a full rollback when only one file needs recovery
* Storing the only backup on the same physical disk
* Backing up sensitive credentials to Git/GitHub
* Using untested third-party backup scripts as the primary recovery method
* Assuming a snapshot includes separate filesystems such as `/boot` or external storage

## Next Step

Continue with [Terminal & Shell Setup](13-terminal.md) to review the recommended terminal and shell configuration for Fedora KDE.
