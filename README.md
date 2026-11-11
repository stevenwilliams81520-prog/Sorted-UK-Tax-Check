# Sorted: UK Tax Check – iOS app

The tax calculator wrapped as an iPhone app with Capacitor, built in the cloud with Codemagic.

- `www/index.html` – the app itself (works offline, saves kept on the phone)
- `capacitor.config.json` – app name and bundle ID `com.stevenwilliams.multijobtaxcheck`
- `codemagic.yaml` – builds the app and sends it to TestFlight
- `assets/` – app icon and splash screen (icons are made automatically during the build)
- `privacy.html` – privacy policy to host online (Apple needs a link to one)

## 1. Apple Developer website
1. Certificates, Identifiers & Profiles → Identifiers → **+** → App IDs → App.
2. Bundle ID (Explicit): `com.stevenwilliams.multijobtaxcheck`. No extra capabilities needed.

## 2. App Store Connect
1. Apps → **+** → New App → iOS.
2. Name: **Sorted: UK Tax Check**, subtitle **Multiple jobs & self-employed**, language English (UK), bundle ID from above, SKU e.g. `sortedtaxcheck`.
   The name under the icon on the iPhone is **Sorted** (set in `capacitor.config.json`).
3. Copy the **Apple ID** number from App Information.

## 3. GitHub
Create a new repo and upload everything in this folder (keep the folder structure).

## 4. Codemagic
1. Add the repo as a new app and choose **codemagic.yaml** as the configuration.
2. In `codemagic.yaml` change:
   - `CODEMAGIC_ASC_KEY` → the name of your App Store Connect API key in Codemagic (Team settings → Integrations)
   - `APP_STORE_APPLE_ID: 0000000000` → the Apple ID number from step 2
3. Start a build of **ios-release**. When it finishes the build appears in TestFlight after Apple’s processing.

## 5. Submit for review
In App Store Connect fill in:
- **Privacy Policy URL** – host `privacy.html` (e.g. GitHub Pages) and add your email to it first
- **App Privacy** → *Data Not Collected*
- **Category** – Finance. **Age rating** – 4+
- **Screenshots** – 6.9" or 6.7" iPhone screenshots (take them from the TestFlight build)
- **Review notes** – “A UK income tax estimator for people with more than one job. All data stays on the device. Not affiliated with HMRC.”
Then pick the build and **Submit for Review**.

## Updating the app
Edit `www/index.html`, push to GitHub and run the Codemagic build again. The build number goes up automatically.
Each April, add the new tax year to `YEARS` in `www/index.html`.
