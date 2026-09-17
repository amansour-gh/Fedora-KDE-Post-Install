# KDE Plasma Setup

Fedora KDE Plasma provides sensible defaults for most desktop settings.

After installation, only a few areas usually need to be reviewed or adjusted. The goal is to keep Plasma **stable, clean, and close to its supported defaults**.

---

## 1. Keep Plasma Updated

Before making configuration changes, make sure the system is fully updated:

```bash
sudo dnf upgrade --refresh
```

Reboot if a kernel or major system component was updated.

For the general update procedure, see [System Updates](03-system-updates.md).

---

## 2. Display Configuration

Open:

**System Settings → Display & Monitor**

Recommended:

* Use the monitor's native resolution.
* Select the highest stable refresh rate supported by the monitor.
* Use the default scaling when it provides a comfortable size.
* Use fractional scaling only when necessary.
* For multiple monitors, verify their arrangement and primary display.

> **Recommendation:** Do not change display settings without a specific reason.

---

## 3. Wayland

Wayland is the default and recommended session for current Fedora KDE installations.

Check the active session with:

```bash
echo $XDG_SESSION_TYPE
```

A Wayland session should return:

```text
wayland
```

Use X11 when a specific application, workflow, or hardware requirement makes it necessary.

> **Note:** Some applications may still have better compatibility under X11. Session choice should be based on actual requirements rather than changing it unnecessarily.

---

## 4. Appearance

Open:

**System Settings → Colors & Themes**

Recommended:

* Keep the default KDE theme or a standard Breeze variant.
* Avoid unnecessary third-party themes.
* Avoid customization scripts that modify multiple KDE components.

The recommended setup should remain easy to reproduce after reinstalling Fedora.

---

## 5. Fonts

Review:

**System Settings → Appearance → Fonts**

Recommended:

* Use a clear, readable interface font.
* Keep font sizes close to the defaults unless a different size is needed.
* Avoid installing duplicate versions of the same fonts.

Additional font installation is covered in [Fonts](06-fonts.md).

---

## 6. Default Applications

Open:

**System Settings → Apps & Windows → Default Applications**

Review the applications associated with:

* Web browsing
* File management
* PDF files
* Images
* Audio
* Video
* Email

Set these according to the applications actually installed on the system.

> **Recommendation:** Configure defaults after installing your main applications rather than changing them unnecessarily during the initial setup.

---

## 7. Dolphin

Dolphin is the recommended file manager for KDE Plasma.

Recommended:

* Keep Dolphin as the default file manager.
* Enable file previews when useful.
* Keep the Places sidebar limited to frequently used locations.
* Avoid unnecessary plugins and service menus.

No major customization is required for a normal workstation.

---

## 8. Power Management

Especially on laptops, review:

**System Settings → Power Management**

Recommended:

* Keep automatic screen locking enabled.
* Configure screen timeout according to your usage.
* Keep suspend enabled unless there is a specific reason not to.
* Use KDE's built-in power profiles.
* Avoid disabling power management globally.

> **Recommendation:** Prefer KDE's built-in power management instead of installing additional power-management utilities.

---

## 9. Mouse and Touchpad

Open:

**System Settings → Input Devices**

Review:

* Pointer speed
* Touchpad scrolling
* Tap-to-click
* Natural scrolling
* Touchpad behavior while typing, when available

Only change settings that match your hardware and workflow.

---

## 10. Night Light

KDE provides a built-in Night Light feature.

Open:

**System Settings → Display & Monitor → Night Light**

**Recommendation:** Optional.

Enable it if it is useful for your evening usage.

A separate application is not required.

---

## 11. Notifications

Open:

**System Settings → Notifications**

Recommended:

* Keep important system notifications enabled.
* Disable notifications from applications that are not useful.
* Avoid disabling notifications globally.

Review notification settings after installing your main applications.

---

## 12. KDE Services

### Baloo

Baloo provides KDE's file indexing and search functionality.

For a normal desktop installation, leave Baloo enabled unless there is a specific reason to change it.

If a large directory does not need to be indexed, such as a development build directory or large data collection, exclude that directory instead of disabling Baloo globally.

### KDE Wallet

KDE Wallet provides secure storage for credentials and other secrets used by applications.

Leave KDE Wallet enabled when KDE applications or other installed software depend on it.

If no application requires it, there is normally no need to configure or modify it.

> **Note:** KDE Wallet is a system credential store. It is separate from password-manager applications such as Bitwarden or KeePassXC.

---

## 13. Screen Lock

Review:

**System Settings → Screen Locking**

Recommended:

* Keep automatic screen locking enabled.
* Use a reasonable inactivity timeout.
* Require authentication when unlocking the session.

Screen locking is especially important on laptops and shared workstations.

---

## 14. Desktop Effects

Keep KDE's default desktop effects.

Disable individual effects only when they cause:

* Performance problems
* Rendering issues
* Compatibility problems

There is normally no need to install additional effects.

---

## 15. KDE Discover

Discover can be useful for graphical management of applications and Flatpaks.

Recommended approach:

* Use **DNF** for Fedora system packages and system updates.
* Use **Flatpak** for Flatpak applications.
* Use **Discover** when a graphical interface is preferred.

Avoid managing the same application through multiple package sources without a reason.

For Flatpak configuration, see [Flatpak](09-flatpak.md).

---

## Recommended Final State

| Area                 | Recommendation                                  |
| -------------------- | ----------------------------------------------- |
| Plasma               | Keep updated                                    |
| Display              | Native resolution / appropriate refresh rate    |
| Scaling              | Default unless adjustment is needed             |
| Session              | Wayland unless a specific requirement needs X11 |
| Theme                | Breeze / standard KDE theme                     |
| Fonts                | Simple and readable                             |
| Default Applications | Review after installing applications            |
| Dolphin              | Keep default with minimal adjustments           |
| Power Management     | Keep enabled                                    |
| Mouse / Touchpad     | Adjust to hardware                              |
| Night Light          | Optional                                        |
| Notifications        | Keep useful notifications                       |
| Baloo                | Keep enabled unless there is a specific reason  |
| KDE Wallet           | Leave enabled when required                     |
| Screen Lock          | Keep enabled                                    |
| Desktop Effects      | Keep defaults                                   |
| Discover             | Optional graphical interface                    |

---

## What to Avoid

Avoid:

* Excessive KDE customization.
* Large collections of widgets.
* Unmaintained third-party themes.
* Random KDE configuration scripts.
* Disabling KDE services without a specific reason.
* Installing additional utilities when KDE already provides the required functionality.
* Changing settings simply because they can be changed.

The recommended Fedora KDE installation should remain **stable, maintainable, and easy to reproduce**.

---

## Next Steps

Continue with [Flatpak](09-flatpak.md).
