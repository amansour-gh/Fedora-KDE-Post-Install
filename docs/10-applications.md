# Applications

Fedora KDE provides a solid base of applications, but a typical workstation may need additional desktop software.

The goal is to install **only applications that provide a real benefit**, while choosing an appropriate and maintainable installation source.

---

## 1. Choosing an Installation Source

When installing an application, consider the following sources:

1. **Fedora repositories**
2. **Flathub**
3. **Official vendor repository**
4. **Other trusted sources when specifically required**

There is no single source that is always best for every application.

Choose based on:

* Application type
* System integration
* Version availability
* Maintenance
* Security
* Update mechanism

> **Recommendation:** Prefer an official and actively maintained source over an unofficial package simply because it is easier to download.

---

## 2. Fedora Packages

Fedora RPM packages are generally appropriate for:

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

Use DNF for installation:

```bash id="n2w7fd"
sudo dnf install package-name
```

---

## 3. Flatpak Applications

Flatpak is often a good choice for desktop applications, especially when:

* A newer version is desirable.
* The application is well maintained on Flathub.
* Sandboxing is useful.
* The Fedora package is unavailable or not suitable.

See [Flatpak](09-flatpak.md) for the recommended Flatpak configuration.

---

## 4. Official Vendor Packages

Some applications are best installed from their official vendor repository when the vendor provides and maintains one.

Examples may include:

* Web browsers
* Development tools
* Specialized commercial software

Prefer the vendor's official distribution method over downloading random RPM files from third-party websites.

> **Recommendation:** If a vendor provides an official repository, verify that it is documented and maintained by the vendor before adding it.

---

## 5. Recommended Application Categories

A typical Fedora KDE workstation may benefit from the following categories:

| Category               | Recommendation                                        |
| ---------------------- | ----------------------------------------------------- |
| Web Browser            | Install your preferred browser                        |
| Office                 | Install one primary office suite                      |
| PDF                    | Use a suitable PDF viewer                             |
| Media Player           | Install one capable media player                      |
| Archive Manager        | Ark + 7-Zip                                           |
| Password Manager       | Use a dedicated password manager                      |
| Communication          | Install only required applications                    |
| Development            | Install only required tools                           |
| Cloud Storage          | Install only services actually used                   |
| Screenshot / Recording | Use KDE tools unless additional features are required |

The exact applications should depend on the user's workflow rather than a fixed software collection.

---

## 6. Office Applications

Choose **one primary office suite** unless compatibility requirements justify more than one.

Common choices include:

* LibreOffice
* ONLYOFFICE

Avoid installing multiple office suites without a specific reason.

Consider:

* Document compatibility
* Microsoft Office file support
* Required features
* Performance
* Maintenance

---

## 7. PDF Applications

KDE provides suitable PDF support for normal desktop use.

Use the default KDE PDF application when it meets your requirements.

Install an alternative PDF application only when a specific feature is required.

Avoid installing several PDF viewers without a reason.

---

## 8. Media Applications

For normal multimedia playback, the applications already provided by Fedora KDE may be sufficient.

Install a capable media player such as VLC when the existing applications do not meet your requirements.

Avoid installing several media players that provide the same functionality unless there is a specific need.

For multimedia and codec configuration, see [Multimedia](04-multimedia.md).

---

## 9. Password Management

Use a dedicated password manager for personal passwords.

Examples include:

* Bitwarden
* KeePassXC

KDE Wallet serves a different purpose and should not automatically be treated as a replacement for a dedicated password manager.

For KDE Wallet configuration, see [KDE Setup](08-kde-setup.md).

---

## 10. Communication Applications

Install only the communication applications required for your workflow.

Prefer:

* Fedora packages when appropriate.
* Flathub for supported desktop applications.
* Official vendor repositories when they are the maintained distribution method.

Avoid unofficial packages when an official source is available.

---

## 11. Cloud Storage

Install a cloud-storage client only when synchronization with a service is actually required.

Before installing one, consider whether the service already provides:

* A web interface
* A supported desktop client
* A KDE-compatible integration method

Avoid running multiple synchronization clients for the same data.

---

## 12. Development Software

Development environments should be installed according to actual requirements.

Common tools include:

* Git
* GCC / G++
* CMake
* Python
* Containers
* IDEs and editors

Do not install a complete development stack unless it is actually needed.

Development setup is covered separately in [Development](11-development.md).

---

## 13. Avoid Duplicate Installations

Do not normally install the same application from multiple sources.

For example:

```text id="e2y4cq"
Firefox → Fedora RPM

Firefox → Flatpak
```

Choose one unless there is a specific reason to keep both.

This helps reduce:

* Duplicate installations
* Conflicting defaults
* Maintenance overhead
* Confusion about updates

Before installing an application, check whether another version is already installed.

For Flatpak applications:

```bash id="j7r5p1"
flatpak list --app
```

For RPM packages, use:

```bash id="m4x8q2"
dnf list --installed
```

---

## 14. Application Maintenance

Keep applications updated through their respective package systems.

Fedora packages:

```bash id="8e8d5v"
sudo dnf upgrade --refresh
```

Flatpak applications:

```bash id="8q4tq8"
flatpak update
```

Applications installed through vendor repositories are normally updated through DNF as well.

For the complete update procedure, see [System Updates](03-system-updates.md).

---

## 15. What to Avoid

Avoid:

* Installing software simply because it is popular.
* Installing multiple applications for the same purpose without a reason.
* Downloading random RPM files.
* Adding unmaintained third-party repositories.
* Running installation scripts from unknown websites.
* Keeping applications that are no longer used.
* Mixing multiple package formats without a reason.

Prefer the simplest supported installation method that meets the application's requirements.

---

## Recommended Final State

A clean Fedora KDE workstation should contain:

* Only the applications actually required.
* A sensible primary application for each common task.
* Trusted and maintainable software sources.
* Regularly updated applications.
* Minimal duplicate software.

The goal is not to install everything available.

> **Install what you need, from the most appropriate source, and keep the system maintainable.**

---

## Next Steps

Continue with [Development](11-development.md).
