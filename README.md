# Sorted: UK Tax Check – iOS app

Free app with a Google AdMob banner, and a one-off 99p in-app purchase to remove ads.
Built with Capacitor, in the cloud with Codemagic.

- `www/index.html` – the app. The ads and purchase code is the last `<script>` block, with a `CONFIG` section at the top
- `capacitor.config.json` – app name (**Sorted** under the icon) and bundle ID `com.stevenwilliams.multijobtaxcheck`
- `codemagic.yaml` – builds the app, adds the AdMob settings, sends it to TestFlight
- `assets/` – icon and splash screen
- `privacy.html` – privacy policy covering ads and purchases. Host it online and add your email

---

## 1. Apple: get set up to sell
1. **App Store Connect → Business**: accept the **Paid Applications Agreement** and fill in bank and tax details (UK sellers fill in the W-8BEN form). In-app purchases won’t work until this says *Active*.
2. Join the **App Store Small Business Program** so Apple takes 15% instead of 30%.
3. Your app → **Monetization → In-App Purchases → +**
   - Type: **Non-Consumable**
   - Reference name: Remove ads
   - Product ID: `sorted_remove_ads` (must match exactly)
   - Price: **£0.99**
   - Display name: *Remove ads*. Description: *One-off payment that removes all ads.*
   - Review screenshot: a screenshot of the Remove ads section in the app
4. **Users and Access → Integrations → In-App Purchase** → generate a key, download the `.p8` file and note the Key ID and Issuer ID (RevenueCat needs these).

## 2. RevenueCat: checks who has paid
1. Sign up at revenuecat.com and create a project called *Sorted*.
2. Add an **App Store** app with bundle ID `com.stevenwilliams.multijobtaxcheck` and upload the In-App Purchase key from step 1.4.
3. **Products**: add `sorted_remove_ads`.
4. **Entitlements**: create `no_ads` and attach the product.
5. **API keys**: copy the Apple public key (starts `appl_`) into `CONFIG.revenueCatKey` in `www/index.html`.

## 3. Google AdMob: the ads
1. Sign up at admob.google.com and add payment details.
2. **Apps → Add app** → iOS → “not listed on an app store yet”. Copy the **App ID** (contains `~`) into `ADMOB_APP_ID` in `codemagic.yaml`.
3. **Ad units → Banner**. Copy the **ad unit ID** (contains `/`) into `CONFIG.bannerId` in `www/index.html`.
4. **Privacy & messaging**: create and publish a **European regulations (GDPR)** message and an **IDFA explainer** message for the app. Google requires the consent message for users in the UK and EEA, and the app shows it automatically.
5. After the app is live, link it to its App Store listing in AdMob. Adding an `app-ads.txt` file to your website (e.g. the same GitHub Pages site as the privacy policy) helps ads pay better.

## 4. Test with TestFlight
Leave `testAds: true` in `CONFIG`. Push to GitHub, run the Codemagic build, install from TestFlight and check:
- a **test ad** banner shows at the bottom and doesn’t cover anything
- the consent form and the “Allow tracking?” prompt appear
- **Remove ads – 99p** works (TestFlight uses Apple’s sandbox, so you aren’t charged) and the banner disappears
- delete and reinstall the app, then **Restore purchase** brings back the ad-free version

Never tap real ads on your own app: AdMob can close your account for it. That’s why test ads are on.

## 5. Release
1. In `www/index.html` set `testAds: false` and make sure `bannerId` is your real ad unit ID.
2. Push and build again.
3. In App Store Connect:
   - **Pricing**: Free
   - **App Privacy**: it’s no longer “Data Not Collected”. Answer the questions using Google’s guide (search “AdMob data disclosure”) and RevenueCat’s guide (search “RevenueCat App Privacy”). In short: Identifiers, Usage Data and Location are collected by Google for third-party advertising and may be used for tracking; Purchase History is collected for app functionality.
   - **Version page → In-App Purchases**: tick *Remove ads* so it’s reviewed with the app.
   - **Review notes**: “Free UK tax estimator. Banner ads from AdMob. Non-consumable ‘Remove ads’ purchase with a Restore button. All tax figures stay on the device. Not affiliated with HMRC.”
4. Submit for review.

## Updating later
Edit `www/index.html`, push, build. Each April add the new tax year to `YEARS`.
