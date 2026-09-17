# Applications

Fedora KDE provides a solid base of applications, but a typical workstation may need additional desktop software.

The goal is to install **only the applications that provide a real benefit**, while choosing an appropriate and maintainable installation source.

---

## Recommended Installation Strategy

When installing an application, prefer sources in this order:

1. **Fedora repositories**
2. **Flathub**
3. **Official vendor repository**
4. **Other trusted sources only when required**

The best source depends on the type of application and how deeply it integrates with the system.

---

## Fedora Packages

Prefer Fedora RPM packages for:

* System utilities
* Command-line tools
* Libraries
* Hardware-related software
* Applications requiring strong system integration

Search before installing:

```bash id="h7o2jz"
dnf search application-name
```

Check package information:

```bash id="vl5n5p"
dnf info application-name
```

---

## Flatpak Applications

Flatpak is often a good choice for desktop applications, especially when:

* A current version is important.
* The application is well maintained on Flathub.
* Sandboxing is beneficial.
* The Fedora package is unavailable or not suitable.

See [Flatpak](09-flatpak.md) for the recommended Flatpak configuration.

---

## Official Vendor Packages

Some applications are best installed from their official vendor repository when the vendor provides and maintains one.

Examples may include:

* Web browsers
* Development tools
* Specialized commercial software

Prefer the vendor's official repository over downloading random RPM files from third-party websites.

---

## Recommended Application Categories

A typical Fedora KDE workstation may benefit from the following categories.

| Category               | Recommendation                                        |
| ---------------------- | ----------------------------------------------------- |
| Web Browser            | Install your preferred browser                        |
| Office                 | Install one primary office suite                      |
| PDF                    | Use a KDE-compatible PDF viewer                       |
| Media Player           | Install one capable media player                      |
| Archive Manager        | Ark + 7-Zip                                           |
| Password Manager       | Use a dedicated password manager                      |
| Communication          | Install only required applications                    |
| Development            | Install only required tools                           |
| Cloud Storage          | Install only services actually used                   |
| Screenshot / Recording | Use KDE tools unless additional features are required |

The exact applications should depend on the user's workflow rather than a fixed software collection.

---

## Office Applications

Choose **one primary office suite** unless compatibility requirements justify more than one.

Possible choices include:

* LibreOffice
* ONLYOFFICE

Avoid installing multiple office suites without a specific reason.

---

## PDF

KDE's PDF applications provide suitable support for normal desktop use.

Install an alternative PDF application only when a specific feature is required.

---

## Media

For normal multimedia playback, install a capable media player such as VLC when the default applications do not meet your requirements.

Avoid installing several media players that provide the same functionality unless there is a specific need.

---

## Password Management

Use a dedicated password manager for personal passwords.

Examples include:

* Bitwarden
* KeePassXC

KDE Wallet serves a different purpose and should not automatically be treated as a replacement for a dedicated password manager.

---

## Development Software

Development environments should be installed according to actual requirements.

Examples:

* Git
* GCC / G++
* CMake
* Python
* Containers
* IDEs and editors

Do not install a complete development stack unless it is actually needed.

Development setup is covered separately in [Development](11-development.md).

---

## Communication Applications

Install only the communication applications required for your workflow.

Prefer:

* Fedora packages when appropriate.
* Flathub for supported desktop applications.
* Official vendor repositories when they are the maintained distribution method.

Avoid unofficial packages when an official source is available.

---

## Cloud Storage

Install a cloud-storage client only when synchronization with a service is actually required.

Avoid running multiple synchronization clients for the same data.

---

## Application Sources

Before installing an application, check:

```text
Fedora repository
        ↓
Flathub
        ↓
Official vendor repository
        ↓
Other trusted source only when required
```

Do not use a random download website simply because it provides an RPM.

---

## Avoid Duplicate Installations

Do not normally install the same application from multiple sources.

For example:

```text
Firefox → Fedora RPM
Firefox → Flatpak
```

Choose one unless there is a specific reason to keep both.

This reduces:

* Duplicate files
* Conflicting defaults
* Maintenance overhead
* Confusion about updates

---

## Application Maintenance

Keep applications updated using their respective package system.

Fedora packages:

```bash id="8e8d5v"
sudo dnf upgrade --refresh
```

Flatpak applications:

```bash id="8q4tq8"
flatpak update
```

Vendor repositories are normally updated through DNF as well.

---

## What to Avoid

Avoid:

* Installing software just because it is popular.
* Installing multiple applications for the same purpose.
* Random RPM downloads.
* Unmaintained third-party repositories.
* Installation scripts from unknown websites.
* Keeping applications that are no longer used.
* Mixing multiple package formats without a reason.

---

## Recommended Final State

A clean Fedora KDE workstation should contain:

* Only the applications actually required.
* One primary application for each common task.
* Trusted and maintainable software sources.
* Regularly updated applications.
* Minimal duplicate software.

The goal is not to install everything available.

**Install what you need, from the most appropriate source, and keep the system maintainable.**

---

## Next Steps

Continue with [Development](11-development.md).
