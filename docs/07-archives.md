# Archives and Compression

Fedora provides a wide range of tools for working with compressed files and archives.

For a Fedora KDE Plasma workstation, the recommended approach is to use **Ark** for graphical archive management and **7-Zip** when command-line access or broader format support is needed.

This chapter covers:

* Archive and compression concepts.
* Archive tools available in Fedora.
* Recommended packages for Fedora KDE.
* Installation and verification.
* Graphical archive management with Ark and Dolphin.
* Terminal usage with `tar`, `7z`, and other tools.
* ZIP, TAR, 7z, RAR, and common compression formats.
* Safe archive extraction.
* Troubleshooting.

> **Important:** Do not install every archive utility available. Start with the recommended tools and install additional packages only when you actually need them.

---

## 1. Archive vs Compression

An **archive** combines one or more files and directories into a single file.

**Compression** reduces the amount of space required to store data.

These are different operations, although they are often used together.

For example:

```text
backup.tar
```

is an archive created with `tar`.

While:

```text
backup.tar.xz
```

means:

1. `tar` created the archive.
2. `xz` compressed it.

This is why Linux archives commonly have names such as:

```text
.tar
.tar.gz
.tar.bz2
.tar.xz
.tar.zst
```

---

## 2. Archive Tools Available in Fedora

Fedora provides packages for many common archive formats.

| Package / Tool  | Purpose                                   | Recommended                          |
| --------------- | ----------------------------------------- | ------------------------------------ |
| `ark`           | KDE graphical archive manager             | **Yes**                              |
| `7zip`          | 7-Zip command-line archive tool           | **Yes**                              |
| `tar`           | Traditional Linux archive tool            | **Yes / normally already available** |
| `gzip`          | gzip compression                          | **Use when required**                |
| `bzip2`         | bzip2 compression                         | **Use when required**                |
| `xz`            | xz compression                            | **Use when required**                |
| `zstd`          | Zstandard compression                     | **Use when required**                |
| `zip` / `unzip` | ZIP archives                              | **Use when required**                |
| `unrar-free`    | Free RAR extraction support               | **Optional**                         |
| `unar`          | Additional archive/RAR extraction support | **Optional**                         |

Fedora's `7zip` package supports creating and extracting formats including 7z, XZ, BZIP2, GZIP, TAR, ZIP, and WIM, with additional formats available for extraction.

---

## 3. Recommended Fedora KDE Setup

For a normal Fedora KDE workstation, the recommended baseline is:

### Ark

**Ark** is the natural graphical archive manager for KDE Plasma.

It integrates with the KDE desktop and can open, create, extract, and manage many archive formats. Fedora 44 currently provides Ark 26.08.1.

Install it with:

```bash
sudo dnf install ark
```

### 7-Zip

**7-Zip** is recommended as the main additional command-line archive utility.

Fedora 44 currently provides `7zip` 26.02-1.fc44. The package provides the `7z` command and also provides compatibility with the `p7zip-plugins` interface.

Install it with:

```bash
sudo dnf install 7zip
```

### Recommended baseline

If you want the standard archive setup for a Fedora KDE workstation:

```bash
sudo dnf install ark 7zip
```

This gives you:

```text
Dolphin
   ↓
Ark
   ↓
Graphical archive management

and

Terminal
   ↓
7z
   ↓
Command-line archive management
```

---

## 4. Optional RAR Support

RAR archives are still common when exchanging files with other systems.

Fedora provides `unrar-free`, a free implementation that can list and extract RAR archives. Fedora 44 currently provides version `0.3.3-2.fc44`.

Install it only if you actually need RAR support:

```bash
sudo dnf install unrar-free
```

Fedora also provides an `unrar` wrapper package for the same free implementation.

For additional archive extraction capabilities, Fedora also provides `unar`.

Do not install several RAR tools simply because they are available.

Start with:

```bash
sudo dnf install unrar-free
```

and add another tool only if a particular archive requires it.

---

## 5. Check What Is Already Installed

Before installing anything, check the current system.

For Ark:

```bash
rpm -q ark
```

For 7-Zip:

```bash
rpm -q 7zip
```

For the `7z` command:

```bash
command -v 7z
```

For TAR:

```bash
command -v tar
```

For RAR support:

```bash
rpm -q unrar-free
```

If a package is already installed, there is no reason to install it again.

---

## 6. Install the Recommended Tools

For the normal Fedora KDE setup:

```bash
sudo dnf install ark 7zip
```

Verify:

```bash
rpm -q ark 7zip
```

Then verify the 7-Zip command:

```bash
7z
```

You should see the 7-Zip command-line help.

Check TAR:

```bash
tar --version
```

If you need RAR files:

```bash
sudo dnf install unrar-free
```

Then:

```bash
rpm -q unrar-free
```

---

## 7. Why We Do Not Install Every Compression Package

It may be tempting to install:

```text
gzip
bzip2
xz
zstd
zip
unzip
7zip
unrar
unar
```

all at once.

This is usually unnecessary.

Fedora already uses many of these tools as part of the normal system environment, and applications can depend on them when required.

The recommended approach is:

```text
Install the KDE archive manager
        ↓
Install 7-Zip
        ↓
Use existing system tools
        ↓
Add optional tools only when needed
```

This keeps the workstation simpler and avoids unnecessary packages.

---

# 8. Using Ark with Dolphin

For normal desktop usage, **Dolphin + Ark** is the easiest approach.

Typical workflow:

```text
Dolphin
   ↓
Select archive
   ↓
Open with Ark
   ↓
Inspect contents
   ↓
Extract
```

For example, when you double-click:

```text
backup.tar.xz
```

Dolphin can open it with Ark.

From Ark you can:

* Browse the archive.
* Extract files.
* Extract the complete archive.
* Create new archives.
* Add files.
* Remove files.
* Work with supported encrypted archives.

For most desktop users, this is all that is required.

---

## 9. Creating an Archive with Ark

In Ark, you can create a new archive through the graphical interface.

The exact menu layout may vary slightly between KDE Plasma releases.

The general workflow is:

```text
Ark
 ↓
New Archive
 ↓
Choose archive name and format
 ↓
Add files/directories
 ↓
Save
```

For normal desktop usage, Ark avoids the need to remember command-line options.

---

# 10. TAR Archives

`tar` is one of the most important archive tools on Linux.

### Create a TAR archive

```bash
tar -cf archive.tar my-folder/
```

Where:

```text
-c    create
-f    specify the output file
```

### List contents

```bash
tar -tf archive.tar
```

### Extract

```bash
tar -xf archive.tar
```

### Extract to a specific directory

Create the destination first:

```bash
mkdir extracted
```

Then:

```bash
tar -xf archive.tar -C extracted/
```

---

# 11. TAR + Gzip

A common Linux archive is:

```text
.tar.gz
```

Create one:

```bash
tar -czf archive.tar.gz my-folder/
```

List its contents:

```bash
tar -tzf archive.tar.gz
```

Extract it:

```bash
tar -xzf archive.tar.gz
```

The `z` option tells `tar` to use gzip.

---

# 12. TAR + Bzip2

For `.tar.bz2` archives:

Create:

```bash
tar -cjf archive.tar.bz2 my-folder/
```

List:

```bash
tar -tjf archive.tar.bz2
```

Extract:

```bash
tar -xjf archive.tar.bz2
```

Bzip2 is less common on modern systems than some newer compression formats, but it is still encountered in older software and source archives.

---

# 13. TAR + XZ

For `.tar.xz` archives:

Create:

```bash
tar -cJf archive.tar.xz my-folder/
```

List:

```bash
tar -tJf archive.tar.xz
```

Extract:

```bash
tar -xJf archive.tar.xz
```

XZ is frequently encountered in Linux source archives and software distributions.

---

# 14. TAR + Zstandard

Zstandard is a modern compression format designed for good compression and high speed.

For `.tar.zst`:

Create:

```bash
tar --zstd -cf archive.tar.zst my-folder/
```

List:

```bash
tar --zstd -tf archive.tar.zst
```

Extract:

```bash
tar --zstd -xf archive.tar.zst
```

---

# 15. ZIP Archives

ZIP is one of the most common formats when exchanging files with Windows users.

If the `zip` command is available:

Create:

```bash
zip -r archive.zip my-folder/
```

List:

```bash
unzip -l archive.zip
```

Extract:

```bash
unzip archive.zip
```

Extract to a specific directory:

```bash
mkdir extracted
unzip archive.zip -d extracted/
```

If the commands are missing, search Fedora before installing packages:

```bash
dnf search zip
```

---

# 16. 7-Zip Archives

The native 7-Zip format is:

```text
.7z
```

Create an archive:

```bash
7z a archive.7z my-folder/
```

List its contents:

```bash
7z l archive.7z
```

Extract:

```bash
7z x archive.7z
```

Extract to a specific directory:

```bash
mkdir extracted
7z x archive.7z -oextracted/
```

The `7z` command can also work with many other archive formats. Fedora's current `7zip` package supports packing/unpacking 7z, XZ, BZIP2, GZIP, TAR, ZIP and WIM, and can unpack a wider range of formats.

---

# 17. Password-Protected 7z Archives

7-Zip can create encrypted archives.

For example:

```bash
7z a -p archive.7z my-folder/
```

It will ask for the password.

To encrypt the filenames as well:

```bash
7z a -p -mhe=on archive.7z my-folder/
```

Avoid putting passwords directly into commands when possible because shell history may record them.

Password-protected archives are useful for protecting individual archives, but they are not a replacement for backups.

---

# 18. RAR Archives

If you receive a RAR archive and installed `unrar-free`, you can inspect it with:

```bash
unrar-free l archive.rar
```

Extract it with:

```bash
unrar-free x archive.rar
```

If a particular RAR archive cannot be handled correctly, check whether another Fedora-supported extraction tool such as `unar` is more appropriate.

The important point is that **RAR support is optional**. There is no need to install RAR utilities on every Fedora KDE system.

---

# 19. Inspect an Archive Before Extracting

It is often useful to inspect an archive before extracting it.

For TAR:

```bash
tar -tf archive.tar
```

For ZIP:

```bash
unzip -l archive.zip
```

For 7z:

```bash
7z l archive.7z
```

For RAR:

```bash
unrar-free l archive.rar
```

This allows you to see:

* What files are included.
* The directory structure.
* Whether unexpected files are present.
* Approximately how much data will be extracted.

This is especially useful for archives downloaded from the Internet.

---

# 20. Safe Extraction

Avoid extracting an unknown archive directly into an important directory.

Instead, create a dedicated directory:

```bash
mkdir extracted
```

Then extract into it.

For TAR:

```bash
tar -xf archive.tar -C extracted/
```

For ZIP:

```bash
unzip archive.zip -d extracted/
```

For 7z:

```bash
7z x archive.7z -oextracted/
```

This makes it easier to inspect and remove the extracted contents if necessary.

---

# 21. Archives from the Internet

An archive can contain:

* Executable files.
* Shell scripts.
* Symbolic links.
* Configuration files.
* Unexpected directory structures.
* Files designed to trick the user into executing them.

Before using an archive from an unknown source:

1. Verify the source.
2. Check the checksum if the publisher provides one.
3. Inspect the archive contents.
4. Extract it into a suitable directory.
5. Do not execute scripts simply because they were included in the archive.
6. Read installation instructions before running commands.

> **Important:** Extracting an archive does not automatically execute its contents. The risk comes when you subsequently open, execute, or install untrusted files.

---

# 22. Identify an Unknown File

If a downloaded file has an unclear extension, use:

```bash
file filename
```

For example:

```bash
file downloaded-file
```

The `file` command examines the file contents and attempts to identify its actual format.

This is more useful than relying only on the filename extension.

---

# 23. Common Terminal Commands

### TAR

```bash
tar -cf archive.tar folder/
tar -tf archive.tar
tar -xf archive.tar
```

### TAR + gzip

```bash
tar -czf archive.tar.gz folder/
tar -tzf archive.tar.gz
tar -xzf archive.tar.gz
```

### TAR + bzip2

```bash
tar -cjf archive.tar.bz2 folder/
tar -tjf archive.tar.bz2
tar -xjf archive.tar.bz2
```

### TAR + xz

```bash
tar -cJf archive.tar.xz folder/
tar -tJf archive.tar.xz
tar -xJf archive.tar.xz
```

### TAR + zstd

```bash
tar --zstd -cf archive.tar.zst folder/
tar --zstd -tf archive.tar.zst
tar --zstd -xf archive.tar.zst
```

### ZIP

```bash
zip -r archive.zip folder/
unzip -l archive.zip
unzip archive.zip
```

### 7z

```bash
7z a archive.7z folder/
7z l archive.7z
7z x archive.7z
```

---

# 24. Verify the Installation

After installing the recommended packages:

```bash
rpm -q ark
```

```bash
rpm -q 7zip
```

Then:

```bash
command -v 7z
```

And:

```bash
tar --version
```

For optional RAR support:

```bash
rpm -q unrar-free
```

A successful package query confirms that the package is installed.

---

# 25. Troubleshooting

## Ark is not installed

Check:

```bash
rpm -q ark
```

If it is missing:

```bash
sudo dnf install ark
```

---

## 7z is not available

Check:

```bash
command -v 7z
```

If nothing is returned:

```bash
sudo dnf install 7zip
```

Then verify:

```bash
7z
```

---

## RAR extraction does not work

Check:

```bash
rpm -q unrar-free
```

If necessary:

```bash
sudo dnf install unrar-free
```

Then inspect the archive:

```bash
unrar-free l archive.rar
```

If the archive still cannot be handled, consider another Fedora-supported extraction tool such as `unar`.

---

## Archive extraction fails

First inspect the archive rather than immediately trying to extract it.

For example:

```bash
7z l archive.7z
```

or:

```bash
tar -tf archive.tar
```

or:

```bash
unzip -l archive.zip
```

Then check:

* Whether the download completed successfully.
* Whether the archive is corrupted.
* Whether it requires a password.
* Whether the format is supported.
* Whether enough disk space is available.

If the publisher provides a checksum, verify the downloaded file.

---

# 26. Recommended Setup

For most Fedora KDE Plasma installations, the recommended setup is:

### Essential graphical tool

```bash
sudo dnf install ark
```

**Use for:**

* Opening archives from Dolphin.
* Extracting files.
* Creating archives.
* Normal desktop archive management.

### Recommended command-line tool

```bash
sudo dnf install 7zip
```

**Use for:**

* `.7z` archives.
* Command-line archive operations.
* Working with many common archive formats.
* More flexible terminal workflows.

### Optional RAR support

```bash
sudo dnf install unrar-free
```

**Use only if:**

* You regularly receive RAR archives.
* Ark or 7-Zip cannot handle a particular RAR archive.

### Other tools

Use `tar`, `gzip`, `xz`, `zstd`, `zip`, and `unzip` when the format or workflow requires them.

Do not install every archive package just because it exists.

---

# 27. Recommended Workflow

For normal desktop use:

```text
Archive received
       ↓
Open with Dolphin
       ↓
Ark
       ↓
Inspect contents
       ↓
Extract
       ↓
Use the files
```

For terminal work:

```text
Identify format
       ↓
List archive contents
       ↓
Create extraction directory
       ↓
Extract
       ↓
Verify the extracted files
```

This provides a simple and reproducible archive workflow without unnecessary software.

---

# 28. Summary

For a typical Fedora KDE Plasma workstation:

1. **Ark** is the recommended graphical archive manager.
2. **7zip** is the recommended additional command-line archive tool.
3. `tar` remains an important standard Linux archive utility.
4. `gzip`, `bzip2`, `xz`, and `zstd` are used according to the archive format.
5. ZIP tools are useful when exchanging files with other operating systems.
6. RAR support should be installed only when required.
7. Inspect archives before extracting them when they come from unknown sources.
8. Extract downloaded archives into dedicated directories when appropriate.
9. Verify checksums when publishers provide them.
10. Prefer Fedora packages over random downloads.
11. Avoid installing multiple tools that provide the same functionality without a specific reason.

The goal is not to install every archive utility available. The goal is to have the right tools for a clean and practical Fedora KDE workstation.

---

# 29. Next Steps

After configuring archive support, continue with:

* [KDE Setup](08-kde-setup.md)

For system packages and repositories, see [Repositories](02-repositories.md).

For applications that may require archive tools, see [Applications](10-applications.md).
