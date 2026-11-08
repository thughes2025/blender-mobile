# Blender iOS: TestFlight & Sideloading Guide

This repository includes an automated GitHub Actions CI/CD pipeline (`.github/workflows/ios-testflight.yml`) that compiles Blender for iPadOS/iOS on cloud Apple Silicon macOS runners and automatically deploys builds to **Apple TestFlight** or exports downloadable `.ipa` artifacts for **sideloading**.

---

## 1. Fast Track: Free Sideloading (No Paid Apple Developer Account Needed)

If you do not have a paid Apple Developer Account:

1. Push your changes to your GitHub fork:
   ```sh
   git push origin ios
   ```
2. Navigate to your repository on GitHub and open the **Actions** tab.
3. Click on the **iOS TestFlight & Sideload Build** workflow and trigger a build.
4. When the build completes, download the `blender-ios-<commit>` zip file under the **Artifacts** section at the bottom of the summary page.
5. Extract the `.ipa` file and install it directly on your iPad using:
   - **[SideStore](https://sidestore.io/)** (installs and refreshes over Wi-Fi directly on device)
   - **[AltStore](https://altstore.io/)**
   - **[TrollStore](https://github.com/opa334/TrollStore)** (if on a compatible iOS version with permanent signing)

---

## 2. TestFlight Automation (Apple Developer Program Members)

To have every `git push` automatically show up in the **TestFlight** app on your iPad or iPhone, configure the following secrets in your GitHub repository (**Settings > Secrets and variables > Actions > New repository secret**):

| Secret Name | Description | Where to Find It |
| :--- | :--- | :--- |
| `APPSTORE_ISSUER_ID` | App Store Connect Issuer UUID | App Store Connect → Users and Access → Integrations → Keys |
| `APPSTORE_KEY_ID` | App Store Connect Key ID | App Store Connect → Users and Access → Integrations → Keys |
| `APPSTORE_API_PRIVATE_KEY` | Private Key content (`.p8`) | Generated when creating an API Key in App Store Connect |
| `APPLE_DEVELOPMENT_TEAM` | 10-character Team ID | developer.apple.com → Membership details |
| `APPLE_CERTIFICATE_BASE64` | Base64-encoded `.p12` Distribution Cert | Export from Keychain Access, then `base64 -w 0 cert.p12` |
| `APPLE_CERTIFICATE_PASSWORD` | Password for your exported `.p12` | Set when exporting `.p12` |
| `APPLE_PROVISIONING_PROFILE_BASE64` | Base64-encoded App Store profile | Download `.mobileprovision` from developer.apple.com, then `base64 -w 0 profile.mobileprovision` |

---

## 3. Local Workspace & Development on Linux

You can write C++ and Python modifications, adapt touch gestures, or customize UI layouts on Linux.

- **Platform & Windowing Layer**: `intern/ghost/intern/GHOST_WindowIOS.mm`
- **Touch & Tablet Events**: `intern/ghost/intern/GHOST_ContextIOS.mm`
- **Window Manager Input**: `source/blender/windowmanager/intern/wm_event_system.cc`
- **Interface & Touch Widgets**: `source/blender/editors/interface/interface_handlers.cc`

When ready to test your changes on hardware, simply commit and push:
```sh
git commit -am "Improve touch navigation and gesture recognition"
git push origin ios
```
The GitHub Actions cloud runner will build the binary and update your iPad automatically.
