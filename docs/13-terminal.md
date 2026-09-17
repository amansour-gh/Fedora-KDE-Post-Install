# Terminal & Shell Setup

A practical approach to setting up the terminal and shell on Fedora KDE.

The goal is to provide a clean, reliable terminal environment without adding unnecessary customization or replacing the default shell without a clear reason.

---

## 1. Keep the Default Shell Unless You Need Another

Fedora normally provides **Bash** as the default user shell.

There is no need to change the default shell immediately after installation.

Keep Bash if:

* You are comfortable with it.
* Your scripts depend on it.
* You do not need additional interactive shell features.

Choose another shell only when its features provide a practical benefit for your workflow.

Check the configured login shell with:

```bash id="3m8v5q"
echo "$SHELL"
```

> **Recommendation:** Do not change the default shell simply because another shell is popular.

---

## 2. Terminal Emulator

Fedora KDE normally provides **Konsole**, the KDE terminal emulator.

If it is not installed:

```bash id="7n2k4p"
sudo dnf install konsole
```

Konsole integrates well with KDE Plasma and provides the features needed for normal terminal work.

Avoid installing multiple terminal emulators unless you have a specific reason to use them.

---

## 3. Zsh

Zsh is an alternative interactive shell with features such as advanced completion and extensive customization options.

Install it when required:

```bash id="5q9r2x"
sudo dnf install zsh
```

Check the installed version:

```bash id="8k3m6v"
zsh --version
```

You can start Zsh without changing your default shell:

```bash id="2p7w4n"
zsh
```

This allows you to test it before making it your login shell.

> **Recommendation:** Use Zsh when its features provide a practical benefit for your workflow. It is not required for normal Fedora KDE usage.

---

## 4. Changing the Default Shell

If you decide to use Zsh as your login shell, first find its path:

```bash id="6v1q8m"
command -v zsh
```

Then change the login shell:

```bash id="9r4k2p"
chsh -s "$(command -v zsh)"
```

Log out and log back in for the change to take effect.

Verify the configured login shell with:

```bash id="1m7x5q"
echo "$SHELL"
```

Changing the login shell is optional. It is not required to use Zsh for testing or individual sessions.

---

## 5. Shell Configuration

Shell configuration files are stored in the user's Home directory.

For Bash, the commonly used interactive configuration file is:

```text id="4q8n2m"
~/.bashrc
```

For Zsh, the main configuration file is:

```text id="7p3v6k"
~/.zshrc
```

Keep shell configuration simple and readable.

Prefer:

* Small, understandable changes
* User-level configuration
* Comments for non-obvious settings
* Reversible changes
* Minimal startup commands

Avoid copying large configuration files from random repositories without understanding what they do.

---

## 6. Command History

Shell history is useful for repeating commands and reviewing previous work.

Keep the default history behavior unless you have a specific requirement for changing it.

If you customize history, avoid storing sensitive information such as:

* Passwords
* API keys
* Access tokens
* Other secrets passed directly as command arguments

Review commands before putting credentials directly into the terminal.

> **Recommendation:** Prefer environment variables, configuration files with appropriate permissions, or dedicated credential-management tools when a command requires sensitive information.

---

## 7. Command Completion and Suggestions

Shell completion can make command-line work faster and reduce typing errors.

For Zsh, additional completion and suggestion tools can be installed when needed.

For example:

```bash id="3k6m9p"
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

If Git is installed, repository work can be performed directly from the terminal.

For example:

```bash id="8v4m1q"
git status
```

Prompt integrations can display Git information, but they are optional and should not replace normal Git commands.

For development systems, prioritize a reliable Git installation and clear repository configuration over elaborate prompt customization.

See [Development](11-development.md) for the recommended Git setup.

---

## 10. Useful Terminal Utilities

Install command-line utilities according to your actual workflow.

For example, a project or task may require:

```bash id="5m2q7x"
sudo dnf install curl wget unzip
```

Do not install large collections of command-line tools without a practical need.

> **Note:** Some utilities may already be installed as dependencies of other software. Check before installing additional packages.

---

## 11. Fast System Information

A system information tool can be useful for quickly checking the current environment.

For example:

```bash id="7x3n8m"
sudo dnf install fastfetch
```

Run it with:

```bash id="2q6v4p"
fastfetch
```

Fastfetch is optional and has no effect on core system functionality.

---

## 12. Shell Scripts and Compatibility

Do not assume that scripts written for one shell will work identically in another.

For scripts intended to use Bash, specify Bash explicitly when appropriate:

```bash id="4n8m2q"
#!/usr/bin/env bash
```

For Zsh scripts:

```bash id="6p1v7x"
#!/usr/bin/env zsh
```

For portable POSIX shell scripts:

```bash id="9m3k5q"
#!/bin/sh
```

Keep system scripts and project scripts independent from your personal interactive shell configuration whenever possible.

This makes them easier to run on other Fedora systems and reduces dependencies on personal configuration.

---

## 13. Recommended Terminal Setup

A practical Fedora KDE terminal setup should include:

* Konsole or another preferred terminal emulator
* Bash as the default shell unless another shell is needed
* Git for development workflows
* Only the command-line utilities actually required
* Optional Zsh configuration for users who prefer it
* A simple and maintainable shell configuration

The goal is a **reliable working environment**, not maximum customization.

---

## 14. What to Avoid

Avoid:

* Changing the default shell without a reason.
* Installing several terminal emulators without a need.
* Copying large shell configurations blindly.
* Installing many plugins that you do not use.
* Putting passwords or secrets directly into shell commands.
* Making scripts depend on your personal interactive shell configuration.
* Using `sudo` unnecessarily for user-level configuration.
* Treating prompt customization as a system requirement.

---

## Next Steps

Continue with [Hardware & Devices](14-hardware.md) to review hardware detection, firmware, drivers, peripherals, and hardware-specific configuration on Fedora KDE.
