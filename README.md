# 📸 GPhotos Custom Branding

Custom launcher branding (adaptive icons) and custom app name patches for **Google Photos**, built for the [Morphe](https://github.com/MorpheApp) patcher framework.

---

## ❓ About

This repository provides standalone launcher personalization patches for Google Photos (`com.google.android.apps.photos`):

- **🎨 Custom branding**: Replaces the default launcher icon with a custom Morphe adaptive pinwheel icon (violet, teal, indigo, and slate hand-drawn style) and supports Android 13+ themed/monochrome dynamic wallpaper tinting.
  - **Embedded assets**: All density icon assets (`mdpi` through `xxxhdpi`) are embedded directly in the `.mpp` bundle, so users never need to prepare or select external folders.
- **🏷️ Custom app name**: Sets a custom launcher and system label (e.g., `"Photos Morphe"` or your own custom name) to easily distinguish the patched app from stock Google Photos.
- **🔗 Modular & Multi-Bundle Ready**: Designed to be used alongside core feature bundles like [De-Vanced](https://github.com/RookieEnough/De-ReVanced). Keep feature patches and branding patches cleanly separated!

---

### How to Use in Morphe Manager

Click below to add these patches as a source in Morphe Manager:

👉 **[Add to Morphe Manager](https://morphe.software/add-source?github=mian196/GPhotos-custom-branding)**

*(Or manually add `mian196/GPhotos-custom-branding` under **Settings → Sources → Patch bundles**)*

---

## 🩹 Patches list

<!-- PATCHES_START EXPANDED -->

| Patch | Description | Options |
| --- | --- | --- |
| **Custom app name** | Changes the Google Photos app name in the launcher and system settings. | `appName`: Custom launcher label (default: `"Photos Morphe"`) |
| **Custom branding** | Adds custom Google Photos launcher branding with embedded adaptive icons (or optional custom folder override). | `customIcon` (optional): Path to alternative custom icon folder |

<!-- PATCHES_END -->

---

## 🛠️ Building Locally

1. Set your environment variables:
   ```powershell
   $env:GITHUB_ACTOR = "mian196"
   $env:GITHUB_TOKEN = (gh auth token)
   ```
2. Build the `.mpp` patch bundle:
   ```powershell
   .\gradlew.bat build generatePatchesList
   ```
   The output `.mpp` file will be generated in `patches/build/libs/patches-*.mpp`.

3. For comprehensive development, release, and maintenance instructions, see [gemini.md](gemini.md).

---

## 📜 License

This repository is licensed under the [GNU General Public License v3.0](LICENSE).

## 🤝 Credits & Acknowledgements

- **Custom Branding Logic & Original Implementation**:
  - Based on the Google Photos custom launcher branding patch originally contributed by [donny110](https://github.com/donny110) to [`rushiranpise/morphe-patches`](https://github.com/rushiranpise/morphe-patches) (licensed under GPLv3).
- **Custom Adaptive Icons & Vector Assets**:
  - Hand-drawn Morphe pinwheel adaptive icon foregrounds and monochrome themed vector assets fetched from [`Akash-Sriram/morphe-google-photos`](https://github.com/Akash-Sriram/morphe-google-photos).
- **Core Framework**:
  - Built with the [Morphe](https://github.com/MorpheApp) patcher and template ecosystem.

### License & Modification Rights
All upstream projects operate under the **GNU General Public License v3.0 (GPLv3)**. Under the terms of GPLv3, editing, modifying, and redistributing the code and assets are explicitly permitted, provided that:
1. The derivative work remains licensed under the GNU General Public License v3.0.
2. The original copyright and attribution notices are preserved.
3. Source code is made available when distributed.
