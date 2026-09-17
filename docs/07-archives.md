# Archive Support

Fedora KDE already provides most of the tools required to work with common archive and compression formats.

The goal of this setup is to provide a **clean and practical archive environment** without installing unnecessary utilities.

---

## Recommended Setup

For a typical Fedora KDE Plasma workstation, install:

```bash
sudo dnf install ark 7zip
```

### Ark

**Ark** is the recommended graphical archive manager for KDE Plasma.

It integrates naturally with Dolphin and provides support for common archive formats.

**Recommendation:** Install.

### 7-Zip

**7-Zip** provides the `7z` command-line utility and supports a wide range of archive and compression formats.

**Recommendation:** Install.

---

## Optional RAR Support

RAR files are less common but may still be encountered when exchanging files with Windows users.

Install RAR extraction support only if you need it:

```bash
sudo dnf install unrar-free
```

**Recommendation:** Optional.

Do not install multiple RAR utilities unless a specific compatibility requirement exists.

---

## Archive Tools Already Provided by Fedora

The system already provides or uses standard tools for common formats, including:

| Tool            | Format / Purpose              | Recommendation           |
| --------------- | ----------------------------- | ------------------------ |
| `tar`           | Linux archives                | Keep                     |
| `gzip`          | `.gz`                         | Keep / use when required |
| `bzip2`         | `.bz2`                        | Use when required        |
| `xz`            | `.xz`                         | Keep / use when required |
| `zstd`          | `.zst`                        | Keep / use when required |
| `zip` / `unzip` | `.zip`                        | Install only if needed   |
| `7zip`          | `.7z` and many other formats  | **Recommended**          |
| `unrar-free`    | RAR extraction                | Optional                 |
| `unar`          | Additional archive extraction | Optional                 |

There is no reason to install all of these manually.

---

## Recommended Installation

Install the standard Fedora KDE archive setup:

```bash
sudo dnf install ark 7zip
```

If RAR support is required:

```bash
sudo dnf install unrar-free
```

Verify the installation:

```bash
rpm -q ark 7zip
```

And:

```bash
command -v 7z
```

---

## Using the Recommended Setup

### Graphical

For normal desktop usage:

```text
Dolphin → Archive → Ark
```

Ark should be the primary graphical tool for:

* Opening archives
* Extracting files
* Creating archives
* Managing archive contents

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

For ZIP archives:

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

## Safe Archive Handling

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

## What to Avoid

Avoid:

* Installing large collections of archive utilities.
* Installing multiple tools for the same format without a reason.
* Following old guides that recommend `p7zip` instead of Fedora's current `7zip` package.
* Downloading archive utilities from random websites.
* Installing RAR utilities if RAR files are never used.

Prefer Fedora packages and add optional tools only when a real requirement appears.

---

## Recommended Configuration

For a clean Fedora KDE installation:

```text
Ark
  ↓
Primary graphical archive manager

7-Zip
  ↓
Primary additional command-line archive tool

tar / standard compression tools
  ↓
Existing Linux archive infrastructure

RAR support
  ↓
Optional — install only when required
```

This provides broad archive compatibility while keeping the system simple and maintainable.

---

## Next Steps

Continue with:

* [KDE Setup](08-kde-setup.md)

Related:

* [Repositories](02-repositories.md)
* [Applications](10-applications.md)
