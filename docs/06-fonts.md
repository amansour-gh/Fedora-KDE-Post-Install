# Fonts

Fedora provides a good selection of fonts for normal desktop use, documents, web browsing, programming, and many international languages.

In most cases, you do not need to install a large collection of fonts after installing Fedora KDE.

The goal of this chapter is to explain:

* Which fonts are useful for a typical Fedora KDE installation.
* When additional fonts are actually needed.
* How to install fonts using Fedora packages.
* How to handle Microsoft fonts when exact compatibility is required.
* How to install personal fonts downloaded from external sources.
* How to verify that a font is available.

> **Important:** Avoid installing large collections of fonts unless you actually need them. Too many fonts can make font selection more difficult and may create unnecessary duplicates.

---

## 1. Check Installed Fonts

Before installing additional fonts, check which fonts are already available.

List installed fonts with:

```bash
fc-list
```

To search for a specific font:

```bash
fc-list | grep -i "font name"
```

For example:

```bash
fc-list | grep -i "noto"
```

You can also check which font Fontconfig selects for a generic font family:

```bash
fc-match sans-serif
```

This is useful when you want to know whether a suitable font is already available before installing another one.

---

## 2. Prefer Fedora Font Packages

When a required font is available through Fedora repositories, prefer the packaged version over downloading it manually.

Advantages include:

* Package management
* Easier updates
* Clean removal
* Dependency management
* Integration with the Fedora system

Search for available font packages with:

```bash
dnf search fonts
```

You can also search for a specific family:

```bash
dnf search noto
```

Inspect a package before installing it:

```bash
dnf info package-name
```

Replace `package-name` with the actual package name.

> **Recommendation:** Use Fedora packages whenever the font you need is available there.

---

## 3. Useful Font Families

A typical Fedora KDE installation already includes useful fonts. Additional families should be installed only when there is a specific requirement.

### Noto Fonts

Noto is a large font family designed to provide broad Unicode and language coverage.

Fedora provides Noto fonts through multiple packages. Search for available packages with:

```bash
dnf search google-noto
```

Install only the family or language coverage you actually need.

Avoid installing the complete Noto collection unless extensive multilingual coverage is required.

### Liberation Fonts

Liberation fonts are useful substitutes for common Microsoft document fonts.

They provide replacements for:

```text
Arial           → Liberation Sans
Times New Roman → Liberation Serif
Courier New     → Liberation Mono
```

Fedora provides individual packages such as:

```text
liberation-sans-fonts
liberation-serif-fonts
liberation-mono-fonts
```

For example:

```bash
sudo dnf install liberation-sans-fonts
```

You can also install the complete Liberation collection when required:

```bash
sudo dnf install liberation-fonts-all
```

For most users, installing only the required families is sufficient.

---

## 4. Arabic Fonts

Fedora provides fonts with Arabic support, but the available families depend on the installed packages and language requirements.

Before installing additional Arabic fonts, check what is already available:

```bash
fc-list :lang=ar
```

You can search for Arabic-related font packages with:

```bash
dnf search arabic fonts
```

When choosing an Arabic font, consider:

* Arabic glyph quality
* Latin character support
* Readability
* Available weights and styles
* Compatibility with the applications you use

Noto and other Unicode fonts can provide broad Arabic and multilingual coverage.

> **Recommendation:** Do not install several Arabic font families simply because they are available. Install another family when you have a specific readability, language, or compatibility requirement.

---

## 5. Installing a Font from Fedora

Once you have identified the required package, install it with DNF.

For example:

```bash
sudo dnf install liberation-sans-fonts
```

After installation, verify that Fontconfig can find the font:

```bash
fc-match "Liberation Sans"
```

If the package is installed but an application does not see the font, see the verification and troubleshooting sections below.

---

## 6. Microsoft Fonts and Document Compatibility

Some documents and websites are designed around Microsoft fonts such as:

* Arial
* Times New Roman
* Courier New
* Georgia
* Verdana
* Trebuchet
* Impact

For many documents, Fedora's Liberation fonts provide useful substitutes:

```text
Arial           → Liberation Sans
Times New Roman → Liberation Serif
Courier New     → Liberation Mono
```

However, substitute fonts do not always have identical font metrics.

This can affect document layout, line wrapping, page breaks, and other formatting.

If exact compatibility with an original Microsoft font is required, obtain the original font files from a legitimate source and install them according to the applicable license.

> **Important:** Do not assume that a substitute font will produce identical document layout.

---

## 7. Installing Downloaded Fonts

Sometimes a required font is not available through Fedora repositories.

Common font formats include:

```text
.ttf
.otf
.ttc
```

For fonts needed only by your user account, use the personal font directory:

```bash
mkdir -p ~/.local/share/fonts
```

Copy the required font files into it. For example:

```bash
cp ~/Downloads/MyFont.ttf ~/.local/share/fonts/
```

For OpenType fonts:

```bash
cp ~/Downloads/MyFont.otf ~/.local/share/fonts/
```

You can also copy multiple files when necessary:

```bash
cp ~/Downloads/MyFont*.ttf ~/.local/share/fonts/
```

User-installed fonts do not require `sudo` and remain separate from Fedora-managed system packages.

> **Recommendation:** Prefer `~/.local/share/fonts` for manually downloaded fonts on a personal workstation.

---

## 8. Refreshing the Font Cache

Most applications should detect newly installed fonts automatically.

If a newly installed font does not appear, rebuild the font cache:

```bash
fc-cache -f
```

Then restart the application that should use the font.

For example, if a font was installed while LibreOffice was already running, close and reopen LibreOffice.

> **Note:** You normally do not need to run `fc-cache` after every font installation. Use it when the newly installed font is not detected.

---

## 9. Verifying Font Availability

Use `fc-match` to determine which font Fontconfig selects.

For example:

```bash
fc-match "Noto Sans"
```

Or:

```bash
fc-match "Liberation Sans"
```

To see the selected font family and actual file:

```bash
fc-match -f '%{family}\n%{file}\n' "Noto Sans"
```

This is useful when an application appears to be using a different font than expected.

For Arabic text, you can check language-based font matching:

```bash
fc-match :lang=ar
```

You can also list fonts that provide Arabic language coverage:

```bash
fc-list :lang=ar family
```

If Arabic text does not display correctly, check:

1. Whether a suitable Arabic font is installed.
2. Whether the selected font contains Arabic glyphs.
3. Whether the application supports Arabic shaping correctly.
4. Whether another font is being selected as fallback.

Do not assume that a font with good Latin support also provides good Arabic support.

---

## 10. Fonts in KDE Plasma

KDE Plasma provides font settings through:

**System Settings → Appearance → Fonts**

Depending on the Plasma version, you can configure fonts used by different parts of the desktop, such as:

* General
* Fixed width
* Small
* Toolbar
* Menu
* Window title

When changing KDE fonts, prefer the graphical settings instead of manually editing font configuration files.

This keeps KDE's configuration easier to understand and reproduce.

> **Recommendation:** Avoid changing KDE font settings unless the default appearance or readability does not meet your needs.

---

## 11. Programming and Terminal Fonts

Developers may want a dedicated monospace font for terminals and code editors.

Common choices include:

* Noto Sans Mono
* Liberation Mono
* DejaVu Sans Mono
* Other programming-oriented monospace fonts

Before installing another font, check whether a suitable monospace font is already available:

```bash
fc-match monospace
```

A programming font should ideally provide:

* Clear distinction between `0` and `O`
* Clear distinction between `1`, `l`, and `I`
* Good punctuation visibility
* Consistent character width
* Good Unicode support

The best choice is largely a matter of readability and personal preference.

---

## 12. Avoid Common Font Problems

Avoid the following practices:

* Installing hundreds of fonts without a reason.
* Installing the same font from multiple sources.
* Mixing manually downloaded copies with Fedora packages unnecessarily.
* Installing outdated third-party font packages without understanding their maintenance status.
* Copying fonts directly into system directories when a user-specific installation is sufficient.
* Installing a complete language collection when only one font family is needed.
* Keeping multiple versions of the same font family.
* Changing Fontconfig configuration files without understanding their purpose.
* Bypassing RPM package verification to install an old third-party package.

If a font does not work, first verify whether the system actually sees it:

```bash
fc-match "Font Name"
```

If the font was manually installed and is not detected:

```bash
fc-cache -f
```

Then restart the affected application.

---

## 13. Recommended Font Setup

For a typical Fedora KDE workstation:

1. Use the fonts already provided by Fedora.
2. Install additional fonts only when there is a specific requirement.
3. Prefer Fedora packages when the required font is available.
4. Use Noto when broad language or Unicode coverage is needed.
5. Use Liberation fonts for compatibility with common Microsoft document fonts.
6. Install original Microsoft fonts only when they are specifically required and have been obtained legitimately.
7. Install manually downloaded fonts under `~/.local/share/fonts` when they are only needed for your user account.
8. Use `fc-match` and `fc-list` to verify font availability.
9. Run `fc-cache -f` only when necessary.
10. Restart applications after installing fonts if they do not detect them automatically.
11. Avoid large font collections unless they are actually required.

The goal is to maintain a clean font environment while providing the language, document, and application compatibility that the system actually needs.

---

## 14. Next Steps

After configuring fonts, continue with:

* [Archives](07-archives.md)

For system updates, see [System Updates](03-system-updates.md).

For applications that may require specific fonts, see [Applications](10-applications.md).
