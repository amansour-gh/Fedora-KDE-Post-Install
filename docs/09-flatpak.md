# Flatpak

Flatpak is a recommended application format for Fedora KDE, especially for desktop applications that benefit from newer versions, sandboxing, or availability through Flathub.

The goal is to use Flatpak **selectively**, without creating duplicate installations or unnecessary runtimes.

---

## Recommended Setup

Fedora already provides Flatpak support.

First, check the configured remotes:

```bash
flatpak remotes
```

For a typical Fedora KDE installation, **Flathub** is the recommended additional source for Flatpak applications.

If Flathub is not already configured:

```bash
flatpak remote-add --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo
```

This is the official Flathub setup method.

---

## Recommended Source

Use:

**Flathub Stable**

Avoid adding:

* Flathub Beta
* Random third-party Flatpak remotes
* Individual repositories without a specific requirement

The Flathub Beta repository is intended for testing and may contain unstable or experimental versions.

---

## When to Prefer Flatpak

Flatpak is particularly useful for:

* Desktop applications
* Applications where a newer version is desirable
* Applications that are well maintained on Flathub
* Applications where sandboxing is beneficial
* Applications that are not available or are outdated in Fedora repositories

Do not install an application as Flatpak simply because Flatpak is available.

---

## Flatpak vs RPM

Use **DNF/RPM** for:

* Fedora system components
* System libraries
* Hardware-related software
* Drivers
* Command-line utilities
* Software that integrates deeply with the operating system

Use **Flatpak** primarily for:

* Desktop applications
* Self-contained graphical applications
* Applications where Flathub provides a better maintained or newer release

The two formats can coexist normally.

---

## Avoid Duplicate Applications

Avoid installing the same application from both Fedora RPM repositories and Flathub unless there is a specific reason.

For example, do not keep:

```text
Application → RPM
Application → Flatpak
```

at the same time without a reason.

Choose the source that best fits the application's requirements and keep the installation simple.

---

## Installing Applications

Search Flathub:

```bash
flatpak search application-name
```

Install from Flathub:

```bash
flatpak install flathub application-id
```

Prefer the **official application ID** and verify the application publisher before installation.

---

## Updates

Update Flatpak applications with:

```bash
flatpak update
```

Flatpak updates are separate from Fedora system updates.

A normal maintenance routine can therefore include:

```bash
sudo dnf upgrade --refresh
flatpak update
```

---

## Unused Runtimes

Flatpak applications may use shared runtimes.

After removing applications, unused runtimes can be removed with:

```bash
flatpak uninstall --unused
```

Review the packages before confirming removal.

Do not manually remove runtimes that are still required by installed applications.

---

## Permissions

Flatpak applications run with sandbox restrictions, but applications may request access to files, devices, networks, or other system resources.

Review permissions when an application requests access that seems broader than necessary.

KDE users can also manage Flatpak applications through graphical tools that expose application permissions when supported.

Do not grant additional permissions unless the application actually requires them.

---

## Verified Applications

When available, prefer applications with the **Verified** status on Flathub.

Verification helps identify applications whose developer or publisher has been verified by Flathub.

It does not mean that every verified application is automatically suitable for every use case.

---

## Flatpak Configuration

Recommended:

| Setting                        | Recommendation            |
| ------------------------------ | ------------------------- |
| Flatpak                        | Keep enabled              |
| Flathub                        | Recommended               |
| Flathub Beta                   | Avoid for normal use      |
| Third-party remotes            | Avoid unless required     |
| Updates                        | Keep applications updated |
| Unused runtimes                | Remove periodically       |
| Permissions                    | Grant only when required  |
| Duplicate RPM/Flatpak installs | Avoid                     |
| Verified applications          | Prefer when available     |

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

---

## Recommended Final State

A clean Fedora KDE installation should normally have:

```text
Fedora RPM repositories
        ↓
System packages and system integration

Flathub
        ↓
Selected desktop applications

Flatpak
        ↓
Updated applications + required runtimes
```

Use each package format where it provides the most appropriate integration and maintenance model.

---

## Next Steps

Continue with [Applications](10-applications.md).
