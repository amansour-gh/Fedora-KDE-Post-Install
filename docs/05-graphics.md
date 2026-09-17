# Graphics

Fedora KDE provides a modern graphics stack based on the Linux kernel, Mesa, Wayland, and the graphics drivers appropriate for the installed hardware.

Most systems do not require manual graphics configuration after installation. The first step should always be to identify the GPU and verify that the current graphics stack is working correctly.

This chapter focuses on **identifying graphics hardware, verifying drivers, understanding Intel/AMD/NVIDIA configurations, and troubleshooting common graphics problems**.

> **Important:** Graphics configuration is hardware-dependent. Do not install a graphics driver or apply hardware-specific configuration without first identifying the GPU and understanding which driver the system is already using.

---

## 1. Understand the Graphics Stack

A Linux graphics stack consists of several components working together.

The most important ones include:

* Linux kernel
* GPU driver
* Mesa graphics libraries
* Wayland or X11
* KDE Plasma compositor
* Hardware-acceleration libraries
* Applications and their graphics APIs

Different GPUs use different drivers.

For example:

* Intel integrated graphics normally use the kernel's Intel graphics driver together with Mesa.
* AMD graphics normally use the kernel's AMDGPU driver together with Mesa.
* NVIDIA graphics can use either the open-source Nouveau driver or an NVIDIA driver, depending on the hardware and requirements.

Because these components are interconnected, replacing one part of the graphics stack without understanding the system can create additional problems.

---

## 2. Identify the GPU

Before changing anything, identify the installed graphics hardware:

```bash
lspci | grep -Ei 'vga|3d|display'
```

A laptop may show more than one GPU.

For example, a system may contain:

```text
Intel integrated graphics
NVIDIA discrete graphics
```

This is commonly called a **hybrid graphics** configuration.

You should identify all GPUs before installing or changing drivers.

---

## 3. Check the Kernel Graphics Driver

The `lspci` command can also show which kernel driver is currently associated with each GPU.

Run:

```bash
lspci -k | grep -EA3 'VGA|3D|Display'
```

Look for lines such as:

```text
Kernel driver in use:
Kernel modules:
```

This is useful for determining whether the system is currently using the expected kernel graphics driver.

Do not assume that the presence of a GPU means that a proprietary driver is required.

---

## 4. Intel Graphics

Modern Intel integrated graphics are normally supported by the Linux kernel and Mesa.

For a typical Intel system, no additional graphics driver installation is required.

First identify the GPU using [Identify the GPU](#2-identify-the-gpu).

Then check the active kernel driver using [Check the Kernel Graphics Driver](#3-check-the-kernel-graphics-driver).

If the Intel GPU is working correctly, avoid installing additional graphics drivers simply because a separate driver package is available.

### Verify Mesa

You can check the installed Mesa packages with:

```bash
rpm -q mesa-dri-drivers
```

Additional Mesa packages may already be installed as dependencies of the desktop environment and applications.

---

## 5. AMD Graphics

AMD graphics support on modern Fedora systems is generally provided by the kernel's AMDGPU driver together with Mesa.

Identify the GPU using [Identify the GPU](#2-identify-the-gpu).

Then check the active kernel driver using [Check the Kernel Graphics Driver](#3-check-the-kernel-graphics-driver).

For a supported AMD GPU, you normally do not need to install a separate proprietary graphics driver.

Keep the Fedora kernel and Mesa packages updated through the normal system update process.

For additional graphics troubleshooting, inspect the kernel driver before making any changes.

---

## 6. NVIDIA Graphics

NVIDIA systems require more attention because the appropriate driver depends on the GPU generation and the user's requirements.

First identify the GPU using [Identify the GPU](#2-identify-the-gpu).

Then check the active kernel driver using [Check the Kernel Graphics Driver](#3-check-the-kernel-graphics-driver).

### Do Not Install a Driver Automatically

A common mistake is to install an NVIDIA driver immediately after seeing an NVIDIA GPU.

This is not always necessary.

Before installing anything, determine:

1. Which NVIDIA GPU is installed.
2. Which driver is currently active.
3. Whether the current setup already provides the required functionality.
4. Whether the system needs CUDA, compute support, gaming performance, or another NVIDIA-specific feature.

If an NVIDIA driver is required, use a maintained Fedora-compatible source and follow the current instructions for the specific Fedora release.

RPM Fusion provides NVIDIA driver packages for supported Fedora releases. The exact package names and supported driver branches can change over time.

Do not copy an NVIDIA installation command from an old Fedora guide without checking that it applies to the current release.

---

## 7. Hybrid Graphics on Laptops

Many laptops contain both integrated and discrete GPUs.

For example:

```text
Integrated GPU
      +
Discrete GPU
```

The integrated GPU may be used for normal desktop operation while the discrete GPU is used for applications that require additional graphics performance.

Before changing hybrid graphics configuration, identify both GPUs using [Identify the GPU](#2-identify-the-gpu).

Then inspect the active drivers using [Check the Kernel Graphics Driver](#3-check-the-kernel-graphics-driver).

Do not disable the integrated GPU or force all applications to use the discrete GPU without a specific reason.

Using the integrated GPU for normal desktop workloads can be useful for reducing power consumption on laptops.

---

## 8. Verify 3D Acceleration

After installing Fedora KDE, it is useful to verify that hardware acceleration is available.

If `glxinfo` is available, run:

```bash
glxinfo -B
```

Look at the following information:

```text
OpenGL vendor string
OpenGL renderer string
OpenGL version string
```

The renderer should normally identify the GPU or graphics driver being used.

If `glxinfo` is not installed, it is provided by the `mesa-demos` package:

```bash
sudo dnf install mesa-demos
```

Then run:

```bash
glxinfo -B
```

> **Note:** The exact output depends on the GPU, driver, session type, and installed graphics stack.

---

## 9. Wayland and KDE Plasma

Fedora KDE uses Wayland as the modern default graphics session.

You can check the current session type with:

```bash
echo $XDG_SESSION_TYPE
```

A Wayland session normally reports:

```text
wayland
```

KDE Plasma's compositor is responsible for displaying windows, desktop effects, animations, and other graphical elements.

If Plasma is functioning normally, there is usually no reason to replace or manually reconfigure the compositor.

---

## 10. Hardware-Accelerated Video

Graphics acceleration is also used for video playback.

Support depends on:

* GPU hardware
* Kernel driver
* Mesa or vendor driver
* VA-API or other acceleration interfaces
* Video codec
* Application

If `vainfo` is available, run:

```bash
vainfo
```

If it is not installed, the utility is provided by the `libva-utils` package:

```bash
sudo dnf install libva-utils
```

The exact output depends on the GPU and driver.

Multimedia configuration is covered separately in [Multimedia](04-multimedia.md).

---

## 11. Check for Graphics Problems

If the desktop is working normally, there is usually no need to perform extensive graphics configuration.

If you experience problems such as:

* Screen flickering
* Missing hardware acceleration
* Very low graphical performance
* External monitor problems
* Incorrect resolution
* Applications using the wrong GPU
* GPU-related crashes

start by collecting information about the current system.

### GPU

Use [Identify the GPU](#2-identify-the-gpu).

### Active Driver

Use [Check the Kernel Graphics Driver](#3-check-the-kernel-graphics-driver).

### Session Type

```bash
echo $XDG_SESSION_TYPE
```

### OpenGL

If `glxinfo` is available:

```bash
glxinfo -B
```

### Kernel Messages

If the problem appears to be driver-related, inspect recent kernel messages:

```bash
journalctl -b -k
```

Avoid changing drivers before identifying the actual problem.

---

## 12. External Monitors

KDE Plasma provides display configuration through:

**System Settings → Display & Monitor**

From there you can configure:

* Resolution
* Refresh rate
* Display arrangement
* Primary display
* Scaling
* Orientation

For most users, KDE's graphical configuration is preferable to manually editing X11 configuration files.

If an external monitor is not detected correctly, first check:

1. Cable and adapter
2. Monitor input
3. Display settings
4. GPU driver
5. Kernel messages

Do not create manual display configuration files unless they are actually required.

---

## 13. What to Avoid

Avoid the following practices:

* Installing proprietary drivers without identifying the GPU.
* Mixing graphics drivers from unrelated repositories.
* Downloading NVIDIA installers directly from random websites.
* Copying old NVIDIA commands from previous Fedora releases.
* Disabling integrated graphics without a specific reason.
* Manually modifying X11 configuration files on a Wayland system without understanding the consequences.
* Removing Mesa packages to troubleshoot an unrelated problem.
* Installing multiple competing graphics stacks.
* Changing several graphics components at once.

When troubleshooting graphics, change one thing at a time and verify the result.

---

## 14. Recommended Graphics Setup

For a typical Fedora KDE workstation:

1. Identify the GPU after installation.
2. Check which kernel driver is currently active.
3. Keep the Fedora kernel and graphics stack updated.
4. Use Mesa for supported Intel and AMD graphics.
5. Use an appropriate NVIDIA driver only when required.
6. Keep hybrid graphics configurations unchanged unless there is a specific need to modify them.
7. Use KDE System Settings for display configuration.
8. Verify hardware acceleration when necessary.
9. Troubleshoot the actual problem before replacing drivers.
10. Avoid hardware-specific commands in a general-purpose setup.

The goal is to keep the graphics stack as close as possible to the supported Fedora configuration while making only the changes that the hardware actually requires.

---

## 15. Next Steps

After verifying the graphics configuration, continue with:

* [Fonts](06-fonts.md)

For multimedia and video acceleration, see [Multimedia](04-multimedia.md).

For general system updates, see [System Updates](03-system-updates.md).
