# Archives and Compression

Fedora provides the tools needed to work with common archive and compression formats.

On a KDE Plasma workstation, most users can handle archives through **Ark** and **Dolphin**, while terminal users can use standard command-line tools such as `tar`, `gzip`, `xz`, `zstd`, and `7z`.

The goal of this chapter is to explain:

* The difference between archiving and compression.
* Common archive formats.
* How to work with archives from the terminal.
* How to use Ark with KDE Plasma.
* When `7zip` is useful.
* How to handle RAR archives.
* How to inspect archives before extracting them.
* Safe extraction practices.
* Which packages are actually necessary.

> **Important:** Do not install every archive utility available. Fedora already provides many of the required tools, and Ark can use the available backend tools to handle many formats.

---

## 1. Archive vs Compression

An **archive** is a file containing one or more files and directories.

Compression reduces the size of data.

These are related but different operations.

For example:

```text
tar
```

primarily creates an archive containing multiple files.

Compression tools such as:

```text
gzip
bzip2
xz
zstd
```

compress data.

This is why you commonly see filenames such as:

```text
backup.tar
backup.tar.gz
backup.tar.bz2
backup.tar.xz
backup.tar.zst
```

A file such as:

```text
backup.tar.xz
```

means:

1. `tar` combined multiple files into an archive.
2. `xz` compressed the resulting archive.

Modern archive tools such as `7z` can combine archiving and compression in a single format.

---

## 2. Common Archive Formats

Some common formats encountered on Linux systems include:

| Format             | Typical use                       |
| ------------------ | --------------------------------- |
| `.tar`             | Archive without compression       |
| `.tar.gz` / `.tgz` | TAR + gzip                        |
| `.tar.bz2`         | TAR + bzip2                       |
| `.tar.xz`          | TAR + xz                          |
| `.tar.zst`         | TAR + zstd                        |
| `.zip`             | General-purpose archive           |
| `.7z`              | High-compression archive          |
| `.rar`             | Common proprietary archive format |

The correct tool depends on the format.

---

## 3. Check Installed Tools

Before installing anything, check what is already available.

For example:

```bash
command -v tar
```

```bash
command -v gzip
```

```bash
command -v xz
```

```bash
command -v zstd
```

```bash
command -v 7z
```

You can also check the versions:

```bash
tar --version
```

```bash
7z
```

If a command is missing, search Fedora before installing it:

```bash
dnf search package-name
```

---

## 4. TAR Archives

`tar` is one of the most important archive tools on Linux.

### Create a TAR archive

To archive a directory:

```bash
tar -cf archive.tar my-folder/
```

Options:

```text
-c    create
-f    specify the output file
```

### List the contents

```bash
tar -tf archive.tar
```

### Extract an archive

```bash
tar -xf archive.tar
```

### Extract to a specific directory

```bash
tar -xf archive.tar -C destination/
```

The destination directory must already exist.

For example:

```bash
mkdir extracted
tar -xf archive.tar -C extracted/
```

---

## 5. TAR + Gzip

Gzip is commonly used together with TAR.

### Create a compressed archive

```bash
tar -czf archive.tar.gz my-folder/
```

The `z` option tells `tar` to use gzip compression.

### List contents

```bash
tar -tzf archive.tar.gz
```

### Extract

```bash
tar -xzf archive.tar.gz
```

---

## 6. TAR + Bzip2

Bzip2 is another compression format.

### Create

```bash
tar -cjf archive.tar.bz2 my-folder/
```

### List

```bash
tar -tjf archive.tar.bz2
```

### Extract

```bash
tar -xjf archive.tar.bz2
```

Bzip2 is still encountered in older Linux software and source archives, but it does not need to be installed separately just because the format exists.

---

## 7. TAR + XZ

XZ provides strong compression and is commonly used for source code and Linux-related archives.

### Create

```bash
tar -cJf archive.tar.xz my-folder/
```

### List

```bash
tar -tJf archive.tar.xz
```

### Extract

```bash
tar -xJf archive.tar.xz
```

---

## 8. TAR + Zstandard

Zstandard (`zstd`) is a modern compression format designed for high performance.

A `.tar.zst` archive can be created with:

```bash
tar --zstd -cf archive.tar.zst my-folder/
```

List its contents:

```bash
tar --zstd -tf archive.tar.zst
```

Extract it:

```bash
tar --zstd -xf archive.tar.zst
```

Zstandard is particularly useful when speed is important.

---

## 9. ZIP Archives

ZIP is one of the most widely used archive formats, especially when exchanging files with Windows users.

### Create a ZIP archive

If `zip` is installed:

```bash
zip -r archive.zip my-folder/
```

### List contents

```bash
unzip -l archive.zip
```

### Extract

```bash
unzip archive.zip
```

### Extract to a directory

```bash
unzip archive.zip -d extracted/
```

If the `zip` or `unzip` commands are not available, search Fedora first:

```bash
dnf search zip
```

Do not install multiple ZIP implementations unnecessarily.

---

## 10. 7-Zip

7-Zip provides a modern archive format with strong compression.

Fedora 44 provides the `7zip` package. The package includes the `7z` command and supports creating and extracting several common formats.

Install it if you need 7z archives:

```bash
sudo dnf install 7zip
```

Check the command:

```bash
7z
```

### Create a 7z archive

```bash
7z a archive.7z my-folder/
```

### List contents

```bash
7z l archive.7z
```

### Extract

```bash
7z x archive.7z
```

### Extract to a specific directory

```bash
7z x archive.7z -oextracted/
```

The `-o` option specifies the output directory.

> **Note:** The current Fedora `7zip` package also supports packing and unpacking formats such as TAR, GZIP, BZIP2, XZ, ZIP, and WIM, in addition to its native 7z format.

---

## 11. Password-Protected Archives

7-Zip supports encrypted archives.

For example:

```bash
7z a -p archive.7z my-folder/
```

The command asks for the password interactively.

To encrypt filenames as well:

```bash
7z a -p -mhe=on archive.7z my-folder/
```

> **Security note:** Password-protected archives are useful for protecting data, but they are not a substitute for a proper backup strategy.

Avoid putting passwords directly into shell history when possible.

---

## 12. RAR Archives

RAR is commonly encountered when exchanging files with Windows users.

RAR support on Linux depends on the available extraction backend.

Fedora provides `unrar-free`, a free implementation capable of listing and extracting RAR archives. Fedora 44 currently provides version `0.3.3-2.fc44`.

Install it only if you need RAR extraction:

```bash
sudo dnf install unrar-free
```

Then:

```bash
unrar-free l archive.rar
```

To extract:

```bash
unrar-free x archive.rar
```

Fedora also provides an `unrar` wrapper package for the free implementation.

Another option is `unar`, which supports RARv5 and can handle encrypted and multi-volume RAR archives. Fedora 44 provides it as well.

For a normal workstation, do not install several RAR tools unless you actually need their different capabilities.

---

## 13. Ark and KDE Plasma

For KDE Plasma users, **Ark** is the main graphical archive manager.

You can install it with:

```bash
sudo dnf install ark
```

Ark integrates with KDE applications and supports many archive formats through its available backends.

Fedora 44's current Ark package depends on `7zip` and provides MIME handling for formats including:

* 7z
* ZIP
* RAR
* TAR
* GZIP
* BZIP2
* XZ
* Zstandard

among many others.

### Open an archive

From Dolphin:

1. Locate the archive.
2. Double-click it.
3. Ark should open the archive.
4. Inspect or extract the files.

You can also right-click an archive in Dolphin and use the available extraction actions.

---

## 14. Extracting Archives with Dolphin

For normal desktop use, Dolphin + Ark is usually easier than using the terminal.

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

For a simple archive, you usually do not need to open a terminal.

Use the terminal when:

* You are working with many archives.
* You need repeatable commands.
* You are working on a remote system.
* You need more control over extraction.
* You are following a software build or installation procedure.

---

## 15. Inspect an Archive Before Extracting

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

This allows you to see what the archive contains before writing files to your current directory.

For downloaded archives, this is a good habit.

---

## 16. Safe Extraction

Do not automatically extract an archive into an important directory.

For example, instead of:

```bash
tar -xf downloaded-file.tar.xz
```

inside a directory containing important files, create a dedicated extraction directory:

```bash
mkdir extracted
tar -xf downloaded-file.tar.xz -C extracted/
```

This keeps the extracted files isolated.

For ZIP:

```bash
mkdir extracted
unzip archive.zip -d extracted/
```

For 7z:

```bash
mkdir extracted
7z x archive.7z -oextracted/
```

---

## 17. Be Careful with Archives from the Internet

An archive can contain:

* Executable files
* Scripts
* Symbolic links
* Unexpected directory structures
* Files with misleading names

An archive itself does not automatically make its contents trustworthy.

Before using files from an unknown source:

1. Verify where the archive came from.
2. Check its checksum when the publisher provides one.
3. Inspect its contents.
4. Extract it into a dedicated directory.
5. Do not execute scripts simply because they were included in an archive.
6. Be especially careful with archives that contain installation scripts.

> **Important:** Extracting a file is not the same as executing it, but extracted scripts and binaries can still be dangerous if you run them.

---

## 18. Identifying an Archive

If you receive a file with an unclear extension, use:

```bash
file filename
```

For example:

```bash
file downloaded-file
```

The `file` command examines the file contents and attempts to identify its format.

This is more reliable than simply trusting the filename extension.

---

## 19. Common Commands at a Glance

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

## 20. Recommended Fedora KDE Setup

For a normal Fedora KDE workstation:

### Usually already available

The basic Linux archive and compression tools are normally provided by the system and its standard packages.

You do not need to install every compression utility manually.

### Recommended graphical tool

Install Ark if it is not already installed:

```bash
sudo dnf install ark
```

### Recommended additional tool

Install 7-Zip if you regularly work with `.7z` archives or want a versatile command-line archive tool:

```bash
sudo dnf install 7zip
```

### Optional RAR support

Install RAR extraction support only if you actually receive RAR archives:

```bash
sudo dnf install unrar-free
```

or consider `unar` when its additional RAR handling capabilities are useful:

```bash
sudo dnf install unar
```

Do not install both simply because they are available.

---

## 21. What Not to Install

Avoid old tutorials that recommend installing large collections of archive utilities without explaining why.

In particular:

* Do not install `p7zip` from an old guide just because it appears in older Linux documentation.
* Do not install multiple RAR implementations unless you have a specific requirement.
* Do not install every compression utility individually.
* Do not download archive programs from random websites when Fedora already provides them.
* Do not replace Fedora's packaged tools with manually installed binaries without a reason.

Fedora 44 provides the current `7zip` package, which also provides compatibility with the older `p7zip-plugins` interface.

---

## 22. Troubleshooting

### Ark does not open an archive

Check that Ark is installed:

```bash
rpm -q ark
```

If necessary:

```bash
sudo dnf install ark
```

Then try opening the archive again.

---

### 7z command is missing

Check:

```bash
command -v 7z
```

If nothing is returned:

```bash
sudo dnf install 7zip
```

---

### RAR archive cannot be extracted

First identify the archive:

```bash
file archive.rar
```

Then check whether RAR support is installed:

```bash
rpm -q unrar-free
```

If required:

```bash
sudo dnf install unrar-free
```

For more complex RAR archives, `unar` may provide better extraction support:

```bash
sudo dnf install unar
```

---

### Archive extraction fails

First inspect the archive:

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

* Whether the file was downloaded completely.
* Whether the archive is corrupted.
* Whether the archive is password protected.
* Whether the archive uses a format unsupported by the installed tool.
* Whether you have enough free disk space.

If the publisher provides a checksum, verify it before troubleshooting the archive further.

---

## 23. Recommended Workflow

For normal Fedora KDE usage:

```text
Archive received
       ↓
Identify the format
       ↓
Open with Ark / Dolphin
       ↓
Inspect contents
       ↓
Extract to a suitable directory
       ↓
Use the files
```

For terminal workflows:

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

This approach keeps archive handling simple and reduces unnecessary packages.

---

## 24. Summary

A clean Fedora KDE installation already has most of the infrastructure required for archive handling.

The recommended approach is:

1. Use **Ark + Dolphin** for normal graphical archive management.
2. Use `tar` for traditional Linux archives.
3. Use `gzip`, `bzip2`, `xz`, and `zstd` when working with their corresponding formats.
4. Install **7zip** when 7z archives or additional archive formats are needed.
5. Install RAR support only when RAR files are actually required.
6. Inspect archives before extracting them when they come from unknown or untrusted sources.
7. Extract downloaded archives into dedicated directories when appropriate.
8. Verify checksums when publishers provide them.
9. Prefer Fedora packages instead of downloading archive utilities from random websites.
10. Avoid installing multiple tools that provide the same functionality without a specific reason.

The goal is a simple, maintainable archive environment rather than installing every archive utility available.

---

## 25. Next Steps

After configuring archive support, continue with:

* [KDE Setup](08-kde-setup.md)

For applications that may require archive tools, see [Applications](10-applications.md).

For system packages and repositories, see [Repositories](02-repositories.md).
