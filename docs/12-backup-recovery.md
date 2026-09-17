# Backup & Recovery

A practical approach to system snapshots, file recovery, and backups on Fedora KDE.

This guide focuses on **Btrfs, Snapper, and backup practices** that can help protect the system from configuration mistakes, package changes, and data loss.

> **Important:** Snapshots are not backups. A snapshot stored on the same physical disk does not protect against disk failure, theft, or major hardware damage.

---

## 1. Check the Filesystem

First, check whether your system uses Btrfs:

```bash id="7v3k9p"
findmnt -t btrfs
```

If Btrfs is being used, Snapper can be used for filesystem snapshots.

If your system uses another filesystem, skip the Btrfs/Snapper sections and use a backup solution appropriate for your setup.

---

## 2. Verify Snapper

Install Snapper if it is not already installed:

```bash id="4m8q2x"
sudo dnf install snapper
```

Check the installed version:

```bash id="9c5n7r"
snapper --version
```

List the existing Snapper configurations:

```bash id="2k6p1w"
sudo snapper list-configs
```

Typical configurations may include:

```text id="8r3v5m"
Config  Subvolume

root    /
home    /home
```

The actual configurations depend on the filesystem layout and how Snapper was configured.

> **Recommendation:** Do not create a new Snapper configuration if the required configuration already exists.

---

## 3. Check Existing Snapshots

For a root configuration:

```bash id="6p2m9x"
sudo snapper -c root list
```

If a Home configuration exists:

```bash id="3n7q4v"
sudo snapper -c home list
```

Review the output before making any changes.

Depending on the configuration, Snapper may contain:

* Timeline snapshots
* Pre/post snapshots created around package operations
* Manually created snapshots

The presence of a Snapper configuration does **not** necessarily mean that automatic snapshots are enabled.

Likewise, a Home configuration may exist without containing automatic snapshots.

Always verify the actual snapshot list instead of assuming that a particular configuration is active.

---

## 4. Create a Manual Snapshot

Before making a significant system change, you can create a manual snapshot:

```bash id="5w8k2n"
sudo snapper -c root create --description "Before major system change"
```

For example, this can be useful before:

* Major system configuration changes
* Installing or changing system-level software
* Testing an unfamiliar configuration
* Making changes that may be difficult to undo manually

A snapshot provides a recovery point for the filesystem covered by that Snapper configuration.

> **Important:** A snapshot is useful only while the underlying storage remains accessible.

---

## 5. Btrfs Assistant

[Btrfs Assistant](https://gitlab.com/btrfs-assistant/btrfs-assistant) provides a graphical interface for managing Btrfs and Snapper.

Install it with:

```bash id="1q6v3p"
sudo dnf install btrfs-assistant
```

After installation, launch **Btrfs Assistant** from the KDE application menu.

Depending on the system configuration, it can be used to:

* View Btrfs filesystems and subvolumes
* View Snapper snapshots
* Create and remove snapshots
* Inspect snapshot information
* Perform supported management and recovery operations

The graphical interface is convenient, but understanding the underlying Snapper configuration remains important.

---

## 6. Review and Manage Snapshots

List snapshots:

```bash id="9m4r7x"
sudo snapper -c root list
```

If a Home configuration exists:

```bash id="2v8k5p"
sudo snapper -c home list
```

Snapshots consume disk space as the filesystem changes.

Remove an individual snapshot only when you are sure it is no longer needed:

```bash id="7q3n6m"
sudo snapper -c root delete <snapshot-number>
```

Replace `<snapshot-number>` with the actual snapshot ID.

Avoid deleting snapshots simply because there are many of them. If automatic snapshots are configured, reasonable retention policies are preferable to manually deleting snapshots without understanding their purpose.

---

## 7. Recover Files from a Snapshot

Snapshots can be useful when a file is accidentally deleted or modified.

A snapshot may contain an earlier version of the file, allowing you to recover it without restoring the entire system.

Use Snapper or Btrfs Assistant to identify the appropriate snapshot and locate the required file.

When possible, recover only the files you need rather than restoring the entire filesystem.

This reduces the risk of overwriting newer data.

> **Recommendation:** Prefer file-level recovery when only individual files are affected.

---

## 8. Review a Specific System Change

Snapper can compare two snapshots and show the changes between them.

For example:

```bash id="4x7m2q"
sudo snapper -c root status <pre-number>..<post-number>
```

Replace the placeholders with the actual snapshot numbers.

For changes that are appropriate to reverse, Snapper can generate the corresponding undo operation:

```bash id="8p3k6v"
sudo snapper -c root undochange <pre-number>..<post-number>
```

> **Warning:** Review the changes carefully before applying them. Undoing a snapshot difference is not the same as performing a complete system rollback, and it may not be appropriate if additional changes were made afterward.

For simple package changes, using DNF to explicitly install or remove the affected package is often safer than reverting a large snapshot difference.

---

## 9. Full System Rollback

A complete system rollback is more complex than restoring an individual file or undoing a specific change.

Whether a full rollback is appropriate depends on:

* The filesystem layout
* Snapper configuration
* Boot configuration
* Separate `/boot` or EFI partitions
* Bootloader configuration
* Encryption
* NVIDIA or other third-party drivers
* The exact type of change being reversed

Do not use a generic rollback command without first confirming that the system's layout and Snapper configuration support it.

For systems where a full rollback is appropriate, follow the documented Snapper/Btrfs recovery procedure for that filesystem layout and verify the available snapshots before proceeding.

> **Recommendation:** Treat full rollback as an advanced recovery procedure, not as a routine troubleshooting command.

Maintain a separate backup before attempting a major rollback whenever possible.

---

## 10. Keep Real Backups

Snapshots protect against certain filesystem and configuration problems, but they do not replace backups.

A proper backup should be stored on a **different physical device or system**.

Possible backup destinations include:

* External HDD or SSD
* NAS
* Another computer
* Cloud storage

For example, `rsync` can be used for file-level backups:

```bash id="6n2v8q"
rsync -avh --delete ~/Documents/ /path/to/backup/Documents/
```

> **Warning:** `--delete` removes files from the destination when they no longer exist in the source. Verify the source and destination paths carefully before using it.

For important data, consider maintaining multiple backup copies rather than relying on a single destination.

---

## 11. What Should Be Backed Up?

Prioritize data that would be difficult or impossible to recreate.

### Personal Data

* Documents
* Photos and videos
* Personal files
* Work files
* Projects

### Application Data

Depending on the application:

* Configuration files
* Profiles
* Databases
* Local application data

### Development Data

* Source code
* Project files
* Local configuration
* Environment configuration
* Important scripts

Keep secrets and credentials out of Git repositories.

If credentials or sensitive application data are stored in your Home directory, make sure your backup strategy protects them appropriately.

> **Security note:** Backup copies can contain sensitive information. Protect backup drives and storage locations with appropriate access controls and encryption when available.

---

## 12. Recommended Recovery Strategy

Use different tools for different problems:

| Problem                                     | Recommended approach                                              |
| ------------------------------------------- | ----------------------------------------------------------------- |
| Accidentally changed a system configuration | Review a Snapper snapshot                                         |
| Package operation caused a problem          | Review relevant pre/post snapshots                                |
| Accidentally deleted a file                 | Recover the file from a snapshot or backup                        |
| Need to undo a specific system change       | Review `snapper status` and `undochange`                          |
| Disk failure                                | Restore from an external/off-device backup                        |
| Lost or damaged personal files              | Restore from backup                                               |
| Major system failure                        | Reinstall if necessary and restore data/configuration from backup |

The key principle is:

> **Snapshots help you recover quickly. Backups help you recover when the system or storage itself is lost.**

---

## 13. Recommended Final State

A practical Fedora KDE recovery setup should have:

* Btrfs snapshots when supported and useful
* Snapper configured according to the system's filesystem layout
* Reasonable snapshot retention
* A separate backup of important personal data
* Backup copies stored independently from the main system
* A recovery procedure that has been tested before an emergency

Do not rely on snapshots as the only protection for important files.

---

## 14. What to Avoid

Avoid:

* Treating snapshots as a replacement for backups.
* Assuming Snapper is configured without checking.
* Creating duplicate Snapper configurations unnecessarily.
* Deleting snapshots without understanding their purpose.
* Performing a full rollback without checking the system layout.
* Keeping the only backup on the same physical disk.
* Using `rsync --delete` without verifying the destination.
* Storing sensitive backup data without appropriate protection.

---

## Next Steps

Continue with [Terminal & Shell Setup](13-terminal.md).
