# Archive Support

Fedora KDE already provides most of the tools required to work with common archive and compression formats.

The goal is to provide a **clean and practical archive environment** without installing unnecessary utilities.

---

## 1. Recommended Setup

For a typical Fedora KDE Plasma workstation, install:

```bash
sudo dnf install ark 7zip
```

### Ark

**Ark** is the recommended graphical archive manager for KDE Plasma.

It integrates naturally with Dolphin and supports common archive formats.

**Recommendation:** Install.

### 7-Zip

**7-Zip** provides the `7z` command-line utility and supports a wide range of archive and compression formats.

**Recommendation:** Install.

---

## 2. Optional RAR Support

RAR files may still be encountered when exchanging files with Windows users.

Install RAR extraction support only if you need it:

```bash
sudo dnf install unrar-free
```

**Recommendation:** Optional.

Do not install multiple RAR utilities unless a specific compatibility requirement exists.

---

## 3. Standard Archive Tools

Fedora already provides standard tools for many common archive and compression formats.

| Tool            | Format / Purpose             |
| --------------- | ---------------------------- |
| `tar`           | Traditional Linux archives   |
| `gzip`          | `.gz` compression            |
| `bzip2`         | `.bz2` compression           |
| `xz`            | `.xz` compression            |
| `zstd`          | `.zst` compression           |
| `zip` / `unzip` | ZIP archives                 |
| `7z`            | 7-Zip and many other formats |

There is no reason to install all archive utilities manually.

Install an additional package only when a real requirement exists.

> **Note:** Package availability can vary between Fedora releases. Use DNF to check whether a particular tool is already installed or available.

---

## 4. Verify the Installation

Verify Ark and 7-Zip:

```bash
rpm -q ark 7zip
```

Check that the `7z` command is available:

```bash
command -v 7z
```

If RAR support was installed, verify it with:

```bash
rpm -q unrar-free
```

---

## 5. Using the Recommended Setup

### Graphical

For normal desktop usage, open archive files with **Ark**.

Ark can be used to:

* Open archives
* Extract files
* Create archives
* Add or remove archive contents

Dolphin integrates with Ark for normal archive operations.

### Terminal

Use `tar` for traditional Linux archives:

```bash
tar -tf archive.tar
tar -xf archive.tar
```

Use `7z` for 7-Zip archives and other formats supported by the tool:

```bash
7z l archive.7z
7z x archive.7z
```

For ZIP archives, if `unzip` is installed:

```bash
unzip -l archive.zip
unzip archive.zip
```

For RAR archives when `unrar-free` is installed:

```bash
unrar-free l archive.rar
unrar-free x archive.rar
```

---

## 6. Safe Archive Handling

For archives downloaded from the Internet:

1. Verify the source.
2. Check the checksum when one is provided.
3. Inspect the archive contents before extraction when appropriate.
4. Extract unfamiliar archives into a separate directory.
5. Do not execute scripts or binaries from an archive unless the source and contents are trusted.

To identify an unknown file:

```bash
file filename
```

---

## 7. What to Avoid

Avoid:

* Installing large collections of archive utilities.
* Installing multiple tools for the same format without a reason.
* Following old guides that recommend `p7zip` instead of Fedora's current `7zip` package.
* Downloading archive utilities from random websites.
* Installing RAR utilities if RAR files are never used.

Prefer Fedora packages and add optional tools only when a real requirement appears.

---

## 8. Recommended Configuration

For a clean Fedora KDE installation:

```text
Ark
  ↓
Primary graphical archive manager

7-Zip
  ↓
Additional command-line archive tool

tar / standard compression tools
  ↓
Existing Linux archive infrastructure

RAR support
  ↓
Optional — install only when required
```

This provides broad archive compatibility while keeping the system simple and maintainable.

---

## 9. Next Steps

Continue with:

* [KDE Setup](08-kde-setup.md)

Related:

* [Repositories](02-repositories.md)
* [Applications](10-applications.md)
