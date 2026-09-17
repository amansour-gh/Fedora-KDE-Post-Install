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
* How to refresh the font cache when necessary.

> **Important:** Avoid installing large collections of fonts unless you actually need them. Too many fonts can make font selection more difficult and may create unnecessary duplicates.

---

## 1. Check Installed Fonts

Before installing additional fonts, check which fonts are already available.

You can list installed fonts with:

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

You can also use:

```bash
fc-match sans-serif
```

This shows which font Fontconfig selects for the generic `sans-serif` family.

---

## 2. Use Fedora Font Packages When Possible

For fonts that are available in Fedora repositories, installing the packaged version is generally preferable to downloading the font manually.

Advantages include:

* Package management
* Easier updates
* Clean removal
* Dependency management
* Integration with the Fedora system

Search for available fonts with:

```bash
dnf search fonts
```

You can also search for a specific font:

```bash
dnf search noto
```

Then inspect a package before installing it:

```bash
dnf info package-name
```

---

## 3. Useful Font Families

A typical Fedora KDE installation already includes a number of useful fonts.

Additional font families may be useful depending on your requirements.

### Noto Fonts

Noto is a large font family designed to provide broad Unicode and language coverage.

Fedora provides Noto fonts through packages such as:

```text
google-noto-fonts
google-noto-fonts-all
```

The `google-noto-fonts-all` package contains a very large collection of Noto font families.

Do not install the complete collection unless you specifically need extensive language coverage.

For many users, installing only the required Noto family is a better approach.

For example, search for available Noto packages:

```bash
dnf search google-noto
```

Then install only the package you need.

### Liberation Fonts

Liberation fonts are useful for compatibility with documents that use common Microsoft fonts.

They provide replacements for:

* Arial → Liberation Sans
* Times New Roman → Liberation Serif
* Courier New → Liberation Mono

Fedora provides these through packages such as:

```text
liberation-sans-fonts
liberation-serif-fonts
liberation-mono-fonts
```

You can install the complete Liberation collection with:

```bash
sudo dnf install liberation-fonts-all
```

For a normal desktop system, installing only the required families is also sufficient.

---

## 4. Arabic Fonts

Fedora provides fonts with Arabic support, but the exact fonts available depend on the installed packages and language requirements.

Before installing additional Arabic fonts, check what is already available:

```bash
fc-list :lang=ar
```

You can search Fedora packages for Arabic-related fonts with:

```bash
dnf search arabic fonts
```

When choosing an Arabic font, consider:

* Arabic glyph quality
* Latin character support
* Readability
* Weight and style availability
* Compatibility with the applications you use

Noto and other Unicode fonts can provide broad Arabic and multilingual coverage.

Do not install several Arabic font families simply because they are available.

---

## 5. Install a Font from Fedora

Once you have identified the package you need, install it with DNF.

For example:

```bash
sudo dnf install liberation-sans-fonts
```

Or another required font package:

```bash
sudo dnf install package-name
```

After installation, the font should become available to applications through the system font configuration.

You can verify it with:

```bash
fc-match "Liberation Sans"
```

---

## 6. Microsoft Fonts

Some documents and websites are designed around Microsoft fonts such as:

* Arial
* Times New Roman
* Courier New
* Georgia
* Verdana
* Trebuchet
* Impact

For most users, Fedora's Liberation fonts provide useful replacements:

```text
Arial           → Liberation Sans
Times New Roman → Liberation Serif
Courier New     → Liberation Mono
```

However, replacement fonts do not always have identical font metrics.

If exact compatibility with an original Microsoft font is required, the original font files must be obtained from a legitimate source and according to the applicable license.

> **Important:** Microsoft Core Fonts are not provided as an official Fedora font package.

### Installing a Legally Obtained Font

If you have legally obtained the required `.ttf` or `.otf` files, install them for your user account.

Create the personal font directory if necessary:

```bash
mkdir -p ~/.local/share/fonts
```

Copy the font files into it:

```bash
cp ~/Downloads/*.ttf ~/.local/share/fonts/
```

For OpenType fonts:

```bash
cp ~/Downloads/*.otf ~/.local/share/fonts/
```

Then rebuild the font cache:

```bash
fc-cache -f
```

Verify the font:

```bash
fc-match "Arial"
```

Replace `Arial` with the actual font family you installed.

> **Note:** Installing fonts under `~/.local/share/fonts` does not require `sudo` and keeps user-installed fonts separate from Fedora-managed system packages.

### Avoid Old Third-Party Installers

Older third-party Microsoft font installers may still be available online, but they should not automatically be considered suitable for a current Fedora installation.

Do not bypass RPM security checks with options such as:

```bash
--nodigest
```

or:

```bash
--nosignature
```

A package that fails modern RPM verification should not be forced into the system simply to install a font.

---

## 7. Installing Downloaded Fonts

Sometimes a required font is not available through Fedora repositories.

Common font file formats include:

```text
.ttf
.otf
.ttc
```

For fonts intended only for your user account, install them in your personal font directory:

```bash
mkdir -p ~/.local/share/fonts
```

Then copy the font files into it.

For example:

```bash
cp ~/Downloads/MyFont.ttf ~/.local/share/fonts/
```

For multiple font files:

```bash
cp ~/Downloads/MyFont*.ttf ~/.local/share/fonts/
```

After copying the fonts, rebuild the font cache:

```bash
fc-cache
```

Fedora documentation recommends using a personal font directory for manually downloaded fonts rather than placing them directly into system directories.

> **Tip:** Installing personal fonts under `~/.local/share/fonts` keeps them separate from Fedora-managed system fonts and does not require `sudo`.

---

## 8. System-Wide Fonts

A font may sometimes need to be available to all users on the system.

System-wide fonts can be installed through Fedora packages whenever possible.

For manually installed fonts, a system font directory can be used, but this should normally be unnecessary for a personal workstation.

Prefer:

```text
Fedora package
        ↓
system-managed font
```

or:

```text
~/.local/share/fonts/
        ↓
user-specific font
```

Avoid copying downloaded fonts directly into arbitrary system directories.

---

## 9. Refresh the Font Cache

Most applications should detect newly installed fonts automatically.

If a newly installed font does not appear, rebuild the font cache:

```bash
fc-cache
```

You can also rebuild the cache more explicitly:

```bash
fc-cache -f
```

Then restart the application that should use the font.

For example, if a font was installed while LibreOffice was already running, close and reopen LibreOffice.

---

## 10. Verify a Font

Use `fc-match` to determine which font Fontconfig selects.

For example:

```bash
fc-match "Noto Sans"
```

Or:

```bash
fc-match "Liberation Sans"
```

To see the actual file being used:

```bash
fc-match -f '%{family}\n%{file}\n' "Noto Sans"
```

This is useful when an application appears to be using a different font than expected.

---

## 11. Check Arabic Font Matching

For Arabic text, Fontconfig can be queried using the Arabic language tag:

```bash
fc-match :lang=ar
```

You can also inspect the available Arabic fonts:

```bash
fc-list :lang=ar family
```

If Arabic text appears incorrectly, check:

1. Whether a suitable Arabic font is installed.
2. Whether the application supports Arabic shaping correctly.
3. Whether the selected font contains Arabic glyphs.
4. Whether another font is being selected as a fallback.

Do not assume that a font with good Latin support also provides good Arabic support.

---

## 12. Fonts in KDE Plasma

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

This keeps KDE's configuration easy to understand and reproduce.

---

## 13. Programming and Terminal Fonts

Developers may want a dedicated monospace font for terminals and code editors.

Common choices include:

* Noto Sans Mono
* Liberation Mono
* DejaVu Sans Mono
* Other programming-oriented monospace fonts

Before installing another font, check whether a suitable monospace font is already installed:

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

## 14. Microsoft Fonts and Document Compatibility

When working with Microsoft Office documents, exact font availability can affect document layout.

For example, a document created using Arial may use a different font if Arial is unavailable.

Liberation fonts can provide useful substitutes:

```text
Arial           → Liberation Sans
Times New Roman → Liberation Serif
Courier New     → Liberation Mono
```

However, a substitute font does not guarantee identical text layout.

Documents that depend on exact font metrics may still display differently if the original font is not installed.

If exact compatibility is required, use the original font when you are legally permitted to obtain and install it.

---

## 15. Avoid Font Problems

Avoid the following practices:

* Installing hundreds of fonts without a reason.
* Installing the same font from multiple sources.
* Mixing manually downloaded copies with Fedora packages unnecessarily.
* Installing outdated third-party font packages without understanding their maintenance status.
* Copying fonts directly into `/usr/share/fonts` when a user-specific installation is sufficient.
* Installing a complete language collection when only one font family is needed.
* Keeping multiple versions of the same font family.
* Changing system font configuration files without understanding Fontconfig.
* Bypassing RPM package verification to install an old third-party package.

If a font does not work, first verify whether the system actually sees it.

Useful commands include:

```bash
fc-list
```

```bash
fc-match "Font Name"
```

```bash
fc-cache -f
```

---

## 16. Recommended Font Setup

For a typical Fedora KDE workstation:

1. Use the fonts already provided by Fedora.
2. Install additional fonts only when there is a specific requirement.
3. Prefer Fedora packages when the required font is available.
4. Use Noto fonts when broad language or Unicode coverage is needed.
5. Use Liberation fonts for compatibility with common Microsoft document fonts.
6. Install original Microsoft fonts only when they are specifically required and have been obtained legitimately.
7. Install manually downloaded fonts under `~/.local/share/fonts` when they are only needed for your user account.
8. Use `fc-match` and `fc-list` to verify font availability.
9. Run `fc-cache -f` only when necessary.
10. Restart applications after installing fonts if they do not detect them automatically.
11. Avoid large font collections unless they are actually required.

The goal is to maintain a clean font environment while providing the language, document, and application compatibility that the system actually needs.

---

## 17. Next Steps

After configuring fonts, continue with:

* [Archives](07-archives.md)

For system updates, see [System Updates](03-system-updates.md).

For applications that may require specific fonts, see [Applications](10-applications.md).
