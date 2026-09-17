# Development Environment

Fedora provides a strong development environment without requiring a large collection of tools to be installed immediately after installation.

The recommended approach is to install a **small base toolset** and add project-specific dependencies only when required.

---

## Recommended Base Setup

For a general development workstation, install:

```bash
sudo dnf install git gcc gcc-c++ make
```

These provide the basic tools required for common development workflows.

---

## Git

Git is recommended for almost every development environment.

Verify the installation:

```bash
git --version
```

Configure your identity before creating commits:

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

SSH keys, GitHub configuration, and advanced Git workflows should be configured separately according to the user's requirements.

---

## C and C++ Development

For C and C++ projects, install the required compiler and build tools.

Basic setup:

```bash
sudo dnf install gcc gcc-c++ make
```

For projects using CMake:

```bash
sudo dnf install cmake
```

Do not install development libraries globally unless a project actually requires them.

---

## Python

Fedora provides Python as part of the standard software ecosystem.

Check the installed version:

```bash
python3 --version
```

For project dependencies, prefer isolated virtual environments:

```bash
python3 -m venv .venv
```

Activate the environment:

```bash
source .venv/bin/activate
```

Avoid installing project-specific Python packages globally with `sudo pip`.

---

## Node.js

Install Node.js only when a project requires it.

Check availability:

```bash
node --version
npm --version
```

Do not install Node.js simply because it is commonly used by developers.

Project-specific Node.js version management should be handled according to the requirements of the project.

---

## Containers

For container-based development, Fedora's recommended container tool is **Podman**.

Install it when required:

```bash
sudo dnf install podman
```

Verify:

```bash
podman --version
```

Containers should be used when they provide a practical benefit, such as:

* Reproducible development environments
* Service dependencies
* Application testing
* Isolated development stacks

Do not install a complete container platform unless your workflow requires it.

---

## Editors and IDEs

Fedora KDE does not require a specific development editor.

Common choices include:

* Kate
* VSCodium
* Visual Studio Code
* JetBrains IDEs
* Other editors appropriate for the project

Choose one primary editor when possible.

Avoid installing several IDEs unless different projects require them.

---

## Project Dependencies

Install dependencies according to the project's documentation.

Prefer:

* Fedora packages when system integration is required.
* Project-specific virtual environments for Python.
* Project-specific package managers for languages such as Node.js.
* Containers when isolation is useful.

Avoid installing project dependencies globally when they can be isolated safely.

---

## Development Libraries

Development libraries should be installed only when required by a project.

For example:

```bash
sudo dnf install package-name-devel
```

Do not install large collections of `*-devel` packages without a specific requirement.

---

## Build Tools

Install additional build systems only when required.

Common examples include:

```text
CMake
Meson
Ninja
Autotools
```

The required build system should normally be determined by the project itself.

---

## Development Environment Separation

Keep project-specific tools and dependencies separated from the base operating system whenever practical.

Recommended:

```text
System
 ├── Git
 ├── Compiler / basic build tools
 └── Required system libraries

Project
 ├── Python virtual environment
 ├── Node.js dependencies
 ├── Container environment
 └── Project-specific tools
```

This reduces conflicts between unrelated projects and makes development environments easier to reproduce.

---

## What to Avoid

Avoid:

* Installing every development tool immediately after Fedora installation.
* Installing project dependencies globally when isolation is available.
* Using `sudo pip install` for project dependencies.
* Installing random development libraries without a requirement.
* Running untrusted installation scripts as root.
* Maintaining multiple versions of the same tool without a specific need.
* Mixing package-management methods without understanding where files are installed.

---

## Recommended Final State

A general Fedora KDE development workstation should normally start with:

```text
Git
GCC / G++
Make
```

Then add:

```text
CMake        → when required
Python       → when required
Node.js      → when required
Podman       → when required
IDE / Editor → according to workflow
```

The objective is to keep the base system **small, stable, and reproducible**, while allowing each development project to define its own dependencies.

---

## Next Steps

Continue with [Backup and Recovery](12-backup-recovery.md).
