# Multimedia

A fresh Fedora KDE installation provides a solid multimedia foundation, but some additional codecs and multimedia components may be required for broader support of audio and video formats.

This chapter explains how to configure multimedia support while keeping the system maintainable and avoiding unnecessary packages.

> **Important:** Multimedia packages can depend on the Fedora release and the repositories enabled on the system. Always use packages appropriate for the current Fedora release.

---

## 1. Understand Fedora Multimedia Support

Fedora includes many multimedia components by default, but some codecs and formats cannot be distributed directly in Fedora's official repositories because of licensing or other distribution restrictions.

For this reason, additional multimedia support may be provided through **RPM Fusion**.

If additional multimedia support is required, enable the appropriate RPM Fusion repositories as described in the [Repositories](02-repositories.md) chapter.

> **Note:** RPM Fusion is an optional third-party repository. Do not enable additional repositories simply because they exist. Use them when the software they provide solves a specific requirement.

---

## 2. Install Multimedia Support

If you need additional codecs or multimedia libraries, Fedora provides a multimedia package group that can install commonly used multimedia components.

Run:

```bash
sudo dnf group install multimedia
```

Review the packages that DNF proposes before confirming the transaction.

The exact packages installed by a group can change between Fedora releases.

If your existing audio and video playback already works correctly, you may not need to install additional multimedia packages.

> **Important:** Do not copy package lists from old Fedora guides without checking whether they are still appropriate for the current release.

---

## 3. Audio Support with PipeWire

Fedora KDE uses **PipeWire** as the modern multimedia and audio framework.

You can check the PipeWire version with:

```bash
pipewire --version
```

Check the PipeWire service with:

```bash
systemctl --user status pipewire
```

You can also check the PulseAudio-compatible PipeWire service:

```bash
systemctl --user status pipewire-pulse
```

A normally functioning Fedora KDE installation should show these services as:

```text
Active: active (running)
```

> **Note:** The service may appear as `disabled` in the `Loaded` line while still being `active (running)`. This can be normal because PipeWire services can be started through socket activation.

### Check Available Audio Devices

When troubleshooting audio, you can inspect the available devices with:

```bash
wpctl status
```

This is useful for identifying:

* Speakers
* Headphones
* HDMI/DisplayPort audio
* USB audio devices
* Microphones
* Bluetooth audio devices

---

## 4. Test Audio

Before installing additional audio packages, first verify that the existing system works correctly.

Use KDE's audio settings to check:

* Output device
* Input device
* Volume
* Application-specific audio
* Default audio device

If audio works correctly, there is normally no reason to install additional audio servers or replace PipeWire.

> **Recommendation:** Avoid installing PulseAudio manually on a modern Fedora KDE installation. Fedora uses PipeWire with a PulseAudio-compatible interface for applications that expect PulseAudio.

---

## 5. Video Playback

For local video playback, applications such as VLC or MPV can be installed according to your preference.

For example:

```bash
sudo dnf install vlc
```

or:

```bash
sudo dnf install mpv
```

You do not need to install multiple media players unless you have a specific reason to use them.

KDE's default applications may already provide enough functionality for many users.

---

## 6. Browser Multimedia and DRM

Modern web browsers use their own multimedia components and may require additional configuration for some formats or protected content.

If a website reports that a required media component is missing, first check the browser's documentation and settings before installing random codec packages.

For DRM-protected content, browsers may use **Widevine** or another supported DRM component.

DRM support is separate from installing system-wide multimedia codecs.

> **Important:** Installing additional system codecs does not automatically enable every DRM-protected streaming service.

---

## 7. Hardware-Accelerated Video

Hardware acceleration can reduce CPU usage when playing high-resolution video.

Support depends on:

* GPU hardware
* Graphics drivers
* Kernel
* Mesa or vendor graphics stack
* Application
* Video codec

Before changing anything, identify the graphics hardware:

```bash
lspci | grep -Ei 'vga|3d|display'
```

You can also check whether the Mesa DRI drivers package is installed:

```bash
rpm -q mesa-dri-drivers
```

Do not install graphics drivers or multimedia packages blindly.

Hardware acceleration should be configured according to the GPU and driver in use.

Detailed graphics configuration is covered separately in [Graphics](05-graphics.md).

---

## 8. Verifying the Multimedia Stack

Instead of testing individual codecs with a single GStreamer element name, first verify that GStreamer itself is installed correctly:

```bash
gst-inspect-1.0 --version
```

You can also inspect the available GStreamer plugins:

```bash
gst-inspect-1.0 | less
```

This can help identify whether the required multimedia plugins are available.

> **Note:** Not every application uses GStreamer directly. VLC, MPV, browsers, and other applications may use different multimedia frameworks.

---

## 9. Troubleshooting Multimedia Problems

When audio or video does not work, avoid immediately installing large collections of codecs or replacing working system components.

Start by identifying the problem.

### Audio

Check PipeWire:

```bash
systemctl --user status pipewire
```

Check available audio devices:

```bash
wpctl status
```

### Video

Identify the GPU:

```bash
lspci | grep -Ei 'vga|3d|display'
```

Check the installed graphics stack before changing drivers.

For more detailed graphics diagnostics, see [Graphics](05-graphics.md).

### Applications

If only one application has a multimedia problem, check that application's configuration and documentation first.

A problem limited to one application does not necessarily indicate a system-wide codec problem.

---

## 10. What to Avoid

Avoid the following practices:

* Installing large unofficial codec bundles from random websites.
* Mixing multimedia packages from different Fedora releases.
* Replacing PipeWire with older audio servers without a specific reason.
* Installing multiple conflicting media frameworks.
* Installing graphics drivers without identifying the GPU.
* Copying commands from old Fedora releases without checking their current relevance.
* Removing system multimedia packages simply because one application has a problem.

A smaller and well-understood multimedia setup is generally easier to maintain.

---

## 11. Recommended Multimedia Setup

For a typical Fedora KDE workstation:

1. Keep the official Fedora repositories enabled.
2. Enable RPM Fusion only when required.
3. Install the multimedia group when additional codecs or multimedia libraries are needed.
4. Keep PipeWire as the default audio system.
5. Use `wpctl` when troubleshooting audio devices.
6. Install a media player such as VLC or MPV if needed.
7. Configure hardware acceleration based on the actual GPU and driver.
8. Troubleshoot the specific application before changing the entire multimedia stack.

The goal is to provide broad multimedia support without unnecessary system modifications.

---

## 12. Next Steps

After configuring multimedia support, continue with:

* [Graphics](05-graphics.md)

Repository configuration is covered in [Repositories](02-repositories.md), while firmware and general update procedures are covered in [System Updates](03-system-updates.md).
