# Development Environment

Fedora provides a strong development environment without requiring a large collection of tools to be installed immediately after installation.

The recommended approach is to install a **small base toolset** and add project-specific dependencies only when required.

---

## 1. Recommended Base Setup

For a general development workstation, install:

```bash id="3r7v2m"
sudo dnf install git gcc gcc-c++ make
```

These provide the basic tools required for common development workflows.

Additional tools should be installed according to the requirements of the projects you work on.

---

## 2. Git

Git is useful for almost every development environment.

Verify the installation:

```bash id="6k9p3w"
git --version
```

Configure your identity before creating commits:

```bash id="8n4q1c"
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

SSH keys, GitHub configuration, and advanced Git workflows should be configured separately according to your requirements.

---

## 3. C and C++ Development

For C and C++ projects, the base compiler and build tools are:

```bash id="1f6m8x"
sudo dnf install gcc gcc-c++ make
```

For projects using CMake:

```bash id="5t2j7k"
sudo dnf install cmake
```

Do not install development libraries globally unless a project actually requires them.

---

## 4. Python

Check the installed Python version:

```bash id="9p3v6r"
python3 --version
```

For project dependencies, prefer isolated virtual environments.

Create a virtual environment inside a project directory:

```bash id="4c8m2n"
python3 -m venv .venv
```

Activate it:

```bash id="7x1q5d"
source .venv/bin/activate
```

When the project is finished, deactivate the environment with:

```bash id="2m6k8p"
deactivate
```

Avoid installing project-specific Python packages globally with:

```bash
sudo pip install ...
```

If a Python project requires additional packages for creating virtual environments, install the appropriate Fedora package rather than modifying the system Python installation.

---

## 5. Node.js

Install Node.js only when a project requires it.

Check whether it is already available:

```bash id="6w3n9q"
node --version
npm --version
```

Do not install Node.js simply because it is commonly used by developers.

Project-specific Node.js version management should follow the requirements of the project.

> **Recommendation:** Avoid maintaining multiple Node.js versions unless a project actually requires them.

---

## 6. Containers

For container-based development, Fedora provides **Podman**.

Install it when required:

```bash id="8r2m5v"
sudo dnf install podman
```

Verify the installation:

```bash id="3q7n1k"
podman --version
```

Containers can be useful for:

* Reproducible development environments
* Service dependencies
* Application testing
* Isolated development stacks

Do not install additional container platforms unless your workflow requires them.

---

## 7. Editors and IDEs

Fedora KDE does not require a specific development editor.

Common choices include:

* Kate
* VSCodium
* Visual Studio Code
* JetBrains IDEs
* Other editors appropriate for the project

Choose one primary editor when possible.

Avoid installing several IDEs unless different projects require them.

The choice of editor is primarily a workflow preference and does not need to change the underlying development environment.

---

## 8. Project Dependencies

Install dependencies according to the project's documentation.

Prefer:

* Fedora packages when system integration is required.
* Python virtual environments for Python projects.
* Project-specific package managers for languages such as Node.js.
* Containers when isolation is useful.

Avoid installing project dependencies globally when they can be safely isolated.

---

## 9. Development Libraries

Development libraries should be installed only when required by a project.

For example:

```bash id="5j8r2p"
sudo dnf install package-name-devel
```

`package-name-devel` is an example placeholder. Replace it with the actual package required by the project.

Do not install large collections of `*-devel` packages without a specific requirement.

---

## 10. Build Tools

Install additional build systems only when required.

Common examples include:

```text id="7c4m1x"
CMake
Meson
Ninja
Autotools
```

The required build system should normally be determined by the project's documentation or build files.

For example:

* `CMakeLists.txt` → CMake
* `meson.build` → Meson
* `Makefile` → Make
* `configure.ac` → Autotools

Do not install every build system simply because it is available.

---

## 11. Development Environment Separation

Keep project-specific tools and dependencies separated from the base operating system whenever practical.

Recommended:

```text id="9v2k6m"
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

## 12. What to Avoid

Avoid:

* Installing every development tool immediately after Fedora installation.
* Installing project dependencies globally when isolation is available.
* Using `sudo pip install` for project dependencies.
* Installing random development libraries without a requirement.
* Running untrusted installation scripts as root.
* Maintaining multiple versions of the same tool without a specific need.
* Mixing package-management methods without understanding where files are installed.
* Installing large development tool collections when only a few tools are required.

---

## Recommended Final State

A general Fedora KDE development workstation can start with:

```text id="4m7q2x"
Git
GCC / G++
Make
```

Then add tools according to actual project requirements:

```text id="6n3p8v"
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
