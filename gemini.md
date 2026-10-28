# GPhotos-custom-branding Developer & Maintenance Guide

This document contains developer and AI assistant instructions on how to maintain, build, and release this repository.

---

## 📌 Repository Overview

This repository is a standalone Morphe patch bundle specifically providing **Custom Branding** (adaptive launcher icons) and **Custom App Name** patches for **Google Photos** (`com.google.android.apps.photos`).

It is designed to be used side-by-side with main patch bundles (such as [De-Vanced](https://github.com/RookieEnough/De-ReVanced)), so users can keep original feature patches clean while adding distinctive launcher branding without manually preparing or compiling icon folders.

---

## 🌿 Branching & Release Workflow

1. **Development Branch (`dev`)**:
   - Make all changes, new features, and fixes on the `dev` branch.
   - Do not commit directly to `main` for regular development.

2. **Semantic Commits**:
   - Every commit must follow conventional semantic commit format:
     - `feat: Added a new feature` (triggers a minor release / dev pre-release)
     - `fix: Some problem now fixed` (triggers a patch release / dev pre-release)
     - `chore: Internal change` (does not create a user-facing release)
   - Commits with `feat:` and `fix:` automatically generate pre-releases via GitHub Actions (`release.yml`). Users can test them in Morphe Manager by enabling `pre-release` on the patch source.

3. **Promoting to Stable (`main`)**:
   - When the `dev` branch is verified and ready for a stable release, merge `dev` into `main`:
     ```bash
     git checkout main
     git merge dev
     git push origin main
     ```
   - **Do NOT squash** — only merge! Squashing breaks semantic release version tracking.

4. **Automated Semantic Release (`release.yml`)**:
   - Releases are fully automated by `.github/workflows/release.yml` and `.releaserc`.
   - **Never manually edit or commit** generated files such as `patches-list.json`, `patches-bundle.json`, or `CHANGELOG.md` during release pipelines — the release workflow manages them.
   - **Never force-push (`git push --force`)** onto semantic release commits or tags, as this breaks subsequent automated releases. If a release fails, push a fix commit to trigger a new release.

---

## 🛠️ Local Build Instructions

### Prerequisites
- JDK 21+
- Android SDK installed (`local.properties` pointing to `sdk.dir`)

### Environment Variables
Due to Gradle plugin dependency evaluation, set `GITHUB_ACTOR` and `GITHUB_TOKEN` before invoking Gradle:
```powershell
$env:GITHUB_ACTOR = "mian196"
$env:GITHUB_TOKEN = (gh auth token) # or dummy token if using local Maven cache
```

### Build Commands
To compile the `.mpp` patch bundle and generate `patches-list.json`:
```powershell
.\gradlew.bat build generatePatchesList
```
The output `.mpp` file is generated at:
`patches/build/libs/patches-<version>.mpp`

---

## 🧩 Architecture Details

### Embedded Icons
All adaptive launcher icon assets are packaged directly into the `.mpp` archive inside:
`patches/src/main/resources/branding/`
- `mipmap-mdpi/`
- `mipmap-hdpi/`
- `mipmap-xhdpi/`
- `mipmap-xxhdpi/`
- `mipmap-xxxhdpi/`
  - `morphe_adaptive_background_custom.png`
  - `morphe_adaptive_foreground_custom.png`
- `drawable/morphe_adaptive_monochrome_custom.xml`

### Patches
1. **`CustomBrandingPatch.kt`**:
   - Copies embedded adaptive icons into the APK's `res/` directories.
   - Writes launcher XML layers (`mipmap-anydpi`, `mipmap-anydpi-v26`, `mipmap-anydpi-v33`).
   - Updates `android:icon` and `android:roundIcon` on `<application>` and launcher `<activity>` / `<activity-alias>` components.
   - Option `customIcon` is optional: if omitted, the embedded icon set is used automatically.
2. **`CustomAppNamePatch.kt`**:
   - Option `appName` (default: `"Photos Morphe"`).
   - Sets `<application>` and launcher component labels to `@string/morphe_google_photos_app_name`.
   - Injects the string value into `res/values/strings.xml`.

---

## 🔗 Multi-Bundle Patching Usage

### CLI Usage (Combined with De-Vanced)
```powershell
java -jar morphe-desktop.jar patch `
  --patches="De-Vanced.mpp" `
  --patches="GPhotos-custom-branding.mpp" `
  -e "Custom branding" `
  -e "Custom app name" `
  --keystore="Morphe.keystore" `
  ... `
  google-photos.apk
```

### Morphe Manager (Android)
Users can add both sources in **Settings → Sources → Patch bundles**:
- Source 1: `mian196/De-Vanced` (or upstream)
- Source 2: `mian196/GPhotos-custom-branding`
Both sets of patches will appear together for Google Photos.
