# AI Compute Tycoon: Progressive Web App

This folder is a complete, static PWA. Upload it to a GitHub repository, turn on GitHub Pages, and PWABuilder can package it as an Android app (a Trusted Web Activity) for the Play Store.

```
index.html              the game (generated from src/ai-tycoon.html by tools/build-pwa.py)
manifest.webmanifest    name, colours, portrait standalone display, 11 icons (incl. maskable), 3 screenshots
sw.js                   service worker: offline play (page network-first, assets cache-first, fonts cached)
privacy.html            privacy policy (no data collected; saves stay on the device). The Play Store requires one.
icons/                  48–512 px PNGs, maskable 192/512, icon.svg
screenshots/            store screenshots (824×1800)
.nojekyll               makes GitHub Pages serve the .well-known folder
.well-known/assetlinks.json.example   template for removing the browser bar in the Android app
```

## 1. Put it on GitHub Pages
1. Create a repository and upload **the contents of this folder** to its root. `index.html` must sit at the top level. Include the hidden `.nojekyll` file and the `.well-known` folder.
2. Go to **Settings → Pages → Build and deployment**. Set Source to *Deploy from a branch*, choose branch `main` and folder `/ (root)`, and save.
3. After a minute, the game is live at `https://<user>.github.io/<repo>/`. Open it on your phone to check that it plays and offers **Install**.

> **For a clean full-screen Android app, choose the URL carefully.** Android checks `/.well-known/assetlinks.json` at the **root of the domain**.
> - A project site (`<user>.github.io/<repo>/`) serves that file under `/<repo>/`. Android won't find it, so the app shows a small browser bar at the top. It still works.
> - To remove the bar, either name the repository **`<user>.github.io`**, which serves it at the domain root, or attach a custom domain.

## 2. Build the Android app with PWABuilder
1. Go to **https://www.pwabuilder.com**, paste your Pages URL and click **Start**. The report should show the manifest, the service worker and the icons as present.
2. Click **Package for stores → Android → Generate**. Set a package ID, for example `com.yourname.aitycoon`. Keep *Signing key: Create new*.
3. Download the zip. It contains:
   - `.aab`: upload this to Play Console;
   - `.apk`: sideload it to test on a phone;
   - `signing.keystore` and `signing-key-info.txt`: **keep both safe**, because every future update needs them;
   - `assetlinks.json`.

## 3. Remove the browser bar (Digital Asset Links)
1. Copy the `assetlinks.json` that PWABuilder generated into `.well-known/assetlinks.json` in this repository, at the domain root as explained above.
2. If you publish through Google Play with Play App Signing, open **Play Console → your app → Setup → App integrity**. Add *that* SHA-256 fingerprint to the `sha256_cert_fingerprints` array as well.
3. Commit, wait for Pages to redeploy, then reopen the app.

## 4. Before publishing
- Edit `privacy.html` and replace the contact placeholder with your email.
- Play Console asks for:
  - the privacy-policy URL (`https://…/privacy.html`);
  - the screenshots from `screenshots/`;
  - a 512 px icon, `icons/icon-512.png`;
  - a 1024×500 feature graphic, which you'll need to make;
  - the content rating questionnaire.

## Updating the game later
Replace `index.html`, then change `VERSION` in `sw.js` (for example to `ai-tycoon-v2`) so that installed apps download the new build. The Android app loads from your site, so you don't need to rebuild the app for content updates.
