# Maintenance Reports — phone app (app shell)

These files make the Daily Maintenance Reports web app **installable on phones and PCs**: home-screen icon, opens full screen with no address bar.

They are only the frame. All reports, photos, PDFs and users stay in Google Drive and the MRP Data Google Sheet. Nothing is stored or sent through this site.

| File | What it is |
|---|---|
| `index.html` | The page that shows the Apps Script app full screen, plus the "Install the app" prompt |
| `manifest.webmanifest` | App name, colours and icons (this is what makes it installable) |
| `sw.js` | Lets the app open offline with a "No internet connection" screen |
| `icons/` | App icons |

## 1. Allow the app to be shown inside the frame (Apps Script)

In **Api.gs**, change the `doGet` function's last line from

```javascript
    .setXFrameOptionsMode(HtmlService.XFrameOptionsMode.DEFAULT);
```

to

```javascript
    .setXFrameOptionsMode(HtmlService.XFrameOptionsMode.ALLOWALL);
```

Save, then **Deploy → Manage deployments → ✏️ → Version: New version → Deploy**.

Note: this lets any website display the app inside its page. The PIN login still protects the data. Check with IT that this is acceptable.

## 2. Put in your Web app URL

1. In Apps Script, go to **Deploy → Manage deployments** and copy the **Web app URL** (it ends in `/exec`).
2. Open `index.html`, find `var APP_URL = 'PASTE_YOUR_WEB_APP_URL_HERE';`, and paste the URL between the quotes.

You only do this once: the URL stays the same when you deploy new versions.

## 3. Publish on GitHub Pages (free)

1. Sign in at **github.com** (create a free account if needed; a company or team account is better than a personal one).
2. Click **+ → New repository**. Name it, for example, `tmi-maintenance-reports`. Set it to **Public** (free GitHub Pages needs a public repository; it holds only these frame files, not data). Click **Create repository**.
3. Click **uploading an existing file**. Drag in **everything inside this folder**, including the `icons` folder, so `index.html` is at the top level. Click **Commit changes**.
4. Go to **Settings → Pages**. Under *Build and deployment*, set **Source: Deploy from a branch**, **Branch: main**, folder **/ (root)**, and click **Save**.
5. After about a minute, the page shows the address, like `https://<your-account>.github.io/tmi-maintenance-reports/`. **This is the new link for everyone.**

## 4. Install on phones

- **Android (Chrome):** open the link. Tap **Install** on the prompt at the bottom, or ⋮ → **Install app** (or *Add to Home screen*).
- **iPhone (Safari):** open the link in **Safari**. Tap **Share ⬆︎ → Add to Home Screen → Add**.
- **PC (Chrome / Edge):** click the install icon in the address bar.

Users sign in once inside the installed app; it stays signed in for 30 days (`SESSION_DAYS`).

Tell users not to open the link from inside Viber or Messenger: those built-in browsers cannot install apps or download images. Open it in Chrome or Safari first.

## Updating

- **App changes (Apps Script):** deploy a new version as usual. The installed app picks them up the next time it opens. Nothing to change here.
- **Icon or name:** replace the files in the GitHub repository. Phones may take a day to update the icon.
- The old `script.google.com/…/exec` link keeps working too.
