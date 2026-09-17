# Terminal & Shell Setup

A practical approach to setting up the terminal and shell on Fedora KDE.

The goal is to provide a clean, reliable terminal environment without adding unnecessary customization or replacing the default shell without a clear reason.

## 1. Keep the Default Shell Unless You Need Another

Fedora provides a standard shell environment suitable for normal command-line use.

There is no need to change the default shell immediately after installation.

Keep the default shell if:

* You are comfortable with it.
* Your scripts depend on it.
* You do not need additional interactive shell features.

Choose another shell only when its features provide a practical benefit for your workflow.

---

## 2. Install a Terminal Emulator

Fedora KDE normally provides **Konsole**, the KDE terminal emulator.

If it is not installed:

```bash
sudo dnf install konsole
```

Konsole integrates well with KDE Plasma and provides the features needed for normal terminal work.

Avoid installing multiple terminal emulators unless you have a specific reason to use them.

---

## 3. Zsh

Zsh is an alternative interactive shell with features such as powerful completion and extensive customization options.

Install it with:

```bash
sudo dnf install zsh
```

Check the installed version:

```bash
zsh --version
```

You can start Zsh without changing your default shell:

```bash
zsh
```

This allows you to test it before making it your login shell.

> **Recommendation:** Do not change the default shell simply because Zsh is popular. Use it when its features are useful for your workflow.

---

## 4. Changing the Default Shell

If you decide to use Zsh as your login shell, first find its path:

```bash
command -v zsh
```

Then change the login shell:

```bash
chsh -s "$(command -v zsh)"
```

Log out and log back in for the change to take effect.

Verify the current shell with:

```bash
echo "$SHELL"
```

Changing the login shell is optional. It is not required to use Zsh for testing or individual sessions.

---

## 5. Shell Configuration

Shell configuration files are stored in the user's Home directory.

For Zsh, the main configuration file is:

```text
~/.zshrc
```

For Bash, the commonly used interactive configuration file is:

```text
~/.bashrc
```

Keep shell configuration simple and readable.

Prefer:

* Small, understandable changes
* User-level configuration
* Comments for non-obvious settings
* Reversible changes
* Avoiding unnecessary startup commands

Avoid copying large configuration files from random repositories without understanding what they do.

---

## 6. Command History

Shell history is useful for repeating commands and reviewing previous work.

Keep the default history behavior unless you have a specific requirement for changing it.

If you customize history, avoid storing sensitive information such as:

* Passwords
* API keys
* Access tokens
* Secrets passed directly as command arguments

Review commands before putting credentials directly into the terminal.

---

## 7. Command Completion and Suggestions

Shell completion can make command-line work faster and reduce typing errors.

For Zsh, additional completion and suggestion tools can be installed when needed.

For example:

```bash
sudo dnf install zsh-autosuggestions zsh-syntax-highlighting
```

These are optional.

Do not install shell plugins simply because they are commonly included in configuration frameworks.

Each additional plugin can affect startup time, behavior, or maintenance.

If installed, configure them through `~/.zshrc` and verify their configuration after updates.

---

## 8. Prompt Customization

A custom prompt can provide useful information such as:

* Current directory
* Git repository status
* Current branch
* Exit status
* Active environment

However, prompt customization is optional.

Keep the prompt readable and avoid displaying excessive information.

A simple prompt is generally easier to maintain than a large framework containing many unrelated features.

---

## 9. Git Integration

If Git is installed, shell integration can make repository work easier.

Basic Git information can be checked directly from the terminal:

```bash
git status
```

Prompt integrations are optional and should not replace the normal Git commands.

For development systems, prioritize a reliable Git installation and clear repository configuration over elaborate prompt customization.

---

## 10. Useful Terminal Utilities

Install additional command-line tools according to your actual workflow.

For example:

```bash
sudo dnf install curl wget unzip
```

Other utilities can be added when a specific application or workflow requires them.

Do not install large collections of command-line tools without a practical need.

---

## 11. Fast System Information

A system information tool can be useful for quickly checking the current environment.

For example:

```bash
sudo dnf install fastfetch
```

Run it with:

```bash
fastfetch
```

This is optional and has no effect on system functionality.

---

## 12. Shell Scripts and Compatibility

Do not assume that scripts written for one shell will work identically in another.

For scripts intended to use Bash, specify Bash explicitly when appropriate:

```bash
#!/usr/bin/env bash
```

For Zsh scripts:

```bash
#!/usr/bin/env zsh
```

For portable POSIX shell scripts:

```bash
#!/bin/sh
```

Keep system scripts and project scripts independent from your personal interactive shell configuration whenever possible.

This makes them easier to run on other Fedora systems.

---

## 13. Recommended Terminal Setup

A practical Fedora KDE terminal setup should include:

* Konsole or another preferred terminal emulator
* The default shell unless another shell is needed
* Git for development workflows
* Only the command-line utilities actually required
* Optional Zsh configuration for users who prefer it
* A simple and maintainable shell configuration

The goal is a **reliable working environment**, not maximum customization.

---

## 14. What to Avoid

Avoid:

* Changing the default shell without a reason
* Installing several terminal emulators without a need
* Copying large shell configurations blindly
* Installing many plugins that you do not use
* Putting passwords or secrets directly into shell commands
* Making scripts depend on your personal interactive shell configuration
* Using `sudo` unnecessarily for user-level configuration
* Treating prompt customization as a system requirement

---

## Next Step

Continue with [Hardware & Devices](14-hardware.md) to review hardware detection, firmware, drivers, peripherals, and hardware-specific configuration on Fedora KDE.
