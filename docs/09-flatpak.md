# Flatpak

Flatpak is a useful application format for Fedora KDE, especially for desktop applications that benefit from newer versions, sandboxing, or availability through Flathub.

The goal is to use Flatpak **selectively**, without creating duplicate installations or unnecessary configuration.

---

## 1. Recommended Setup

Fedora provides Flatpak support by default.

Check the configured remotes with:

```bash id="v9b5w4"
flatpak remotes
```

For a typical Fedora KDE installation, **Flathub** is the recommended additional source for Flatpak applications.

If Flathub is not already configured:

```bash id="7d1f3c"
flatpak remote-add --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo
```

After adding the remote, verify it with:

```bash id="j6x8tm"
flatpak remotes
```

> **Recommendation:** Use Flathub Stable for normal desktop applications.

---

## 2. Flatpak Sources

For normal use, keep the Flatpak configuration simple.

Recommended:

* Flathub Stable
* Fedora's existing Flatpak support

Avoid adding:

* Flathub Beta for normal use
* Random third-party Flatpak remotes
* Individual repositories without a specific requirement

The Flathub Beta repository is intended for testing and may contain pre-release or less stable versions.

---

## 3. When to Prefer Flatpak

Flatpak can be particularly useful for:

* Desktop applications
* Applications where a newer version is desirable
* Applications that are well maintained on Flathub
* Applications where sandboxing is useful
* Applications that are not available or are significantly older in Fedora repositories

Do not install an application as Flatpak simply because Flatpak is available.

> **Recommendation:** Choose the package format based on the application's requirements, integration, maintenance, and version availability.

---

## 4. Flatpak vs RPM

Use **DNF/RPM** primarily for:

* Fedora system components
* System libraries
* Hardware-related software
* Drivers
* Command-line utilities
* Software that integrates deeply with the operating system

Use **Flatpak** primarily for:

* Desktop applications
* Self-contained graphical applications
* Applications where Flathub provides a suitable maintained release

Both formats can coexist normally.

> **Important:** This is a guideline, not a strict rule. Some desktop applications may be better suited to Fedora packages, while others may work better as Flatpaks.

---

## 5. Avoid Duplicate Applications

Avoid installing the same application from both Fedora repositories and Flathub unless there is a specific reason.

For example, avoid keeping:

```text id="h4r1ps"
Application → RPM

Application → Flatpak
```

at the same time without a reason.

Choose the source that best fits the application's requirements and keep the installation simple.

Before installing an application, check whether another version is already installed:

```bash id="7l1q7c"
flatpak list
```

You can also check installed RPM packages with:

```bash id="2g0w8e"
dnf list --installed
```

---

## 6. Installing Applications

Search for an application:

```bash id="p0i6a4"
flatpak search application-name
```

Install an application from Flathub:

```bash id="8m8h9k"
flatpak install flathub application-id
```

When multiple results are available, verify:

* Application name
* Application ID
* Publisher
* Source
* Whether the application is maintained

Prefer the official application ID when the developer or publisher is clearly identified.

---

## 7. Updates

Update Flatpak applications with:

```bash id="5m2w6x"
flatpak update
```

Flatpak updates are separate from Fedora system updates.

A normal maintenance routine can therefore include:

```bash id="4b7j2n"
sudo dnf upgrade --refresh
flatpak update
```

For more information about the general update routine, see [System Updates](03-system-updates.md).

---

## 8. Unused Runtimes

Flatpak applications may use shared runtimes.

After removing applications, unused runtimes can be removed with:

```bash id="h4p8z1"
flatpak uninstall --unused
```

Review the proposed removals before confirming.

Do not manually delete Flatpak runtimes.

---

## 9. Permissions

Flatpak applications run with sandbox restrictions, but applications may request access to:

* Files and directories
* Devices
* Network resources
* Other system resources

Review permissions when an application requests access that seems broader than necessary.

KDE and other graphical tools may provide ways to review application permissions depending on the installed software and desktop integration.

> **Recommendation:** Grant additional permissions only when the application actually requires them.

---

## 10. Verified Applications

When available, the **Verified** status on Flathub can help identify applications whose developer or publisher has been verified by Flathub.

Verification does not mean that an application is automatically suitable for every use case.

Consider the application's publisher, maintenance status, permissions, and intended use before installing it.

---

## 11. Managing Flatpak Applications

List installed Flatpak applications:

```bash id="z2v4x9"
flatpak list --app
```

Show details about an installed application:

```bash id="8j5f1s"
flatpak info application-id
```

Remove an application:

```bash id="q6c8ra"
flatpak uninstall application-id
```

These commands are useful when troubleshooting duplicate installations or checking which Flatpak applications are currently installed.

---

## 12. Recommended Configuration

| Setting                        | Recommendation                 |
| ------------------------------ | ------------------------------ |
| Flatpak                        | Keep enabled                   |
| Flathub Stable                 | Recommended                    |
| Flathub Beta                   | Avoid for normal use           |
| Third-party remotes            | Avoid unless required          |
| Updates                        | Keep applications updated      |
| Unused runtimes                | Remove when no longer required |
| Permissions                    | Grant only when required       |
| Duplicate RPM/Flatpak installs | Avoid                          |
| Verified applications          | Prefer when available          |

---

## What to Avoid

Avoid:

* Installing every application as Flatpak.
* Keeping duplicate RPM and Flatpak versions of the same application.
* Adding multiple Flatpak remotes without a reason.
* Using Flathub Beta as a normal application source.
* Granting unnecessary filesystem or device permissions.
* Manually deleting Flatpak runtimes.
* Using unofficial installation scripts when a normal Flatpak package is available.

Prefer the simplest supported installation method that meets the application's requirements.

---

## Recommended Final State

A clean Fedora KDE installation can use both package formats:

```text id="v1x9az"
Fedora RPM repositories
        ↓
System packages and system integration

Flathub
        ↓
Selected desktop applications

Flatpak
        ↓
Applications + required runtimes
```

Use each package format where it provides an appropriate balance of integration, maintenance, version availability, and application requirements.

---

## Next Steps

Continue with [Applications](10-applications.md).
