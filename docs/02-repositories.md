# Repositories

Fedora uses software repositories to provide packages and updates for the system.

Understanding repositories is important before installing additional software because different repositories have different maintainers, release policies, and levels of integration with Fedora.

The general rule for this guide is:

> **Prefer official Fedora repositories first, and add third-party repositories only when there is a clear reason to do so.**

---

## 1. Fedora Official Repositories

A standard Fedora installation uses official Fedora repositories for the base system and regular updates.

You can see the currently enabled repositories with:

```bash
dnf repolist
```

To include disabled repositories:

```bash
dnf repolist --all
```

Typical Fedora repositories include:

* `fedora` — packages provided with the Fedora release
* `updates` — updated packages released after the Fedora release
* `fedora-cisco-openh264` — OpenH264 packages provided through Fedora's Cisco arrangement

The exact repository list can vary depending on the Fedora release and system configuration.

---

## 2. Finding Packages

Before adding another repository, check whether the package is already available from the repositories you have.

Search for a package:

```bash
dnf search package-name
```

For example:

```bash
dnf search firefox
```

To inspect a package:

```bash
dnf info package-name
```

You can also check which repository provides a package:

```bash
dnf info package-name
```

Look for the **Repository** field in the output.

### Why check first?

Adding an external repository for a package that Fedora already provides can create unnecessary complexity.

It is usually better to use the Fedora package when it meets your requirements.

---

## 3. Inspecting Repository Information

For more detailed information about a specific repository:

```bash
dnf repoinfo repository-id
```

For example:

```bash
dnf repoinfo fedora
```

This can show information such as:

* Repository name
* Repository ID
* Repository URL
* Enabled/disabled status
* Package count
* Metadata information

Use this when troubleshooting repositories or verifying where packages are coming from.

---

## 4. When Should You Add a Third-Party Repository?

A third-party repository can be useful when:

* Fedora does not provide the software you need.
* The vendor officially provides an RPM repository.
* A trusted project provides software that is intentionally outside Fedora's repositories.
* A specific feature or package is unavailable from the standard Fedora repositories.

However, adding repositories should not be treated as a routine post-installation requirement.

Every additional repository introduces another software source that can affect package updates and dependencies.

A good decision process is:

```text
Is the package available in Fedora?
        │
        ├── Yes → Use the Fedora package if it meets your needs.
        │
        └── No
             │
             ├── Is there a Flatpak?
             │      └── Consider Flatpak.
             │
             ├── Is it available through RPM Fusion?
             │      └── Consider RPM Fusion.
             │
             └── Does the vendor provide an official RPM repository?
                    └── Consider the vendor repository.
```

The appropriate choice depends on the application.

---

## 5. RPM Fusion

[RPM Fusion](https://rpmfusion.org/) provides additional packages for Fedora that are not included in the standard Fedora repositories.

It is divided mainly into:

* **Free**
* **Nonfree**

RPM Fusion is not part of the Fedora project itself.

It can provide software and packages that Fedora does not distribute through its official repositories because of licensing, legal, patent, or Fedora policy considerations.

### Important

RPM Fusion is **optional**.

Do not install it simply because it is commonly mentioned in Fedora post-installation guides.

Install it when you actually need packages that it provides.

---

## 6. Installing RPM Fusion

If you decide that RPM Fusion is required, the release packages can be installed using the current Fedora release automatically:

```bash
sudo dnf install \
  https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm \
  https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
```

The command uses:

```bash
rpm -E %fedora
```

to detect the installed Fedora release.

This avoids hard-coding a specific Fedora version into the command.

After installation, verify the repositories:

```bash
dnf repolist
```

You should see RPM Fusion repositories if the installation was successful.

---

## 7. RPM Fusion Additional Repositories

RPM Fusion can also provide additional repositories for specific purposes.

For example, a system may have repositories related to:

* NVIDIA drivers
* Steam
* Other specialized packages

These should be enabled only when they are actually needed.

A repository being available does not mean that it should automatically be enabled.

---

## 8. Third-Party Application Repositories

Some applications provide their own RPM repositories.

Examples may include:

* Web browsers
* Development tools
* Cloud storage applications
* Proprietary applications

These repositories should generally be configured according to the application's official documentation.

For example, if an application vendor provides an official Fedora/RPM repository, prefer the vendor's documented installation method over copying a repository configuration from an unrelated website.

Application-specific repositories and installation instructions are documented later in:

* `10-applications.md`
* `11-development.md`

This keeps this chapter focused on **repository management** rather than individual applications.

---

## 9. COPR Repositories

Fedora also provides **COPR**, a service that allows developers and projects to build packages for Fedora.

COPR repositories can be useful for software that is not available in Fedora's official repositories.

However, COPR repositories are independently maintained.

Before enabling one, consider:

* Who maintains the repository?
* Is it actively maintained?
* Does it support your Fedora release?
* Why is the repository needed?
* Is there an official Fedora package or Flatpak alternative?

For a stable workstation, avoid adding COPR repositories without a specific reason.

---

## 10. Flatpak Is Different

Flatpak repositories are separate from DNF/RPM repositories.

For example:

```bash
flatpak remotes
```

shows configured Flatpak remotes.

While:

```bash
dnf repolist
```

shows DNF repositories.

These are different software distribution systems and should not be confused.

A graphical application may therefore be available as:

* Fedora RPM
* RPM Fusion package
* Vendor RPM
* Flatpak

The best choice depends on the application and the user's requirements.

For detailed Flatpak configuration and Flathub setup, see:

* [Flatpak and Flathub](09-flatpak.md)

---

## 11. Temporarily Disabling a Repository

If a repository is causing a problem, you can temporarily disable it for a single DNF command.

For example:

```bash
sudo dnf update --disablerepo=repository-id
```

This does not permanently change the repository configuration.

It is useful when troubleshooting package conflicts or temporarily avoiding a problematic repository.

---

## 12. Removing or Disabling an Unwanted Repository

Before removing a repository, first identify how it was installed and which packages depend on it.

You can inspect the repository with:

```bash
dnf repoinfo repository-id
```

For repositories installed through a release package, you can check the package:

```bash
rpm -q package-name
```

Do not remove a repository simply because it is not part of the default Fedora installation.

Third-party repositories may be intentionally required for software already installed on the system.

If a repository is no longer needed, follow the documentation provided by its maintainer for removing it.

---

## 13. Repository Verification

After adding or changing repositories, verify the configuration:

```bash
dnf repolist
```

Then check that DNF can access the repositories:

```bash
sudo dnf check-update
```

If DNF reports package or dependency problems, do not immediately add more repositories.

First determine which repository introduced the conflicting package.

Useful commands include:

```bash
dnf info package-name
```

and:

```bash
dnf repoinfo repository-id
```

---

## 14. Recommended Repository Strategy

For a typical Fedora KDE workstation, the recommended approach is:

### 1. Fedora repositories

Use the official Fedora repositories whenever possible.

### 2. Flatpak

Consider Flatpak for desktop applications where it provides a better fit.

### 3. RPM Fusion

Add RPM Fusion when you specifically need software or packages provided by it.

### 4. Official vendor repositories

Use an application's official repository when the vendor provides one and there is a clear reason to prefer it.

### 5. Other third-party repositories

Use them only when necessary and after checking their maintenance status and Fedora compatibility.

---

## 15. What Not to Do

Avoid:

* Adding large numbers of repositories without a specific reason.
* Following old Fedora guides without checking the Fedora release they target.
* Adding a repository just because a package is available there.
* Mixing multiple repositories that provide the same software without understanding the consequences.
* Using random repository files from unofficial websites.
* Keeping obsolete repositories enabled after they are no longer needed.

A smaller and well-understood repository configuration is generally easier to maintain and troubleshoot.

---

## 16. Quick Verification Checklist

After configuring repositories:

```bash
dnf repolist
```

Confirm that:

* Fedora repositories are enabled.
* Required third-party repositories are present.
* Unnecessary repositories have not been added.
* Repositories support the current Fedora release.
* DNF can refresh repository metadata successfully.

For a clean Fedora KDE installation, the goal is not to have the largest possible number of repositories.

The goal is to have **the right repositories for the software you actually use**.

## Next Steps

After reviewing repository configuration, continue with:

* [System Updates](03-system-updates.md)
