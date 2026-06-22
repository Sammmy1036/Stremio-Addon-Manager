# Stremio Addon Manager

A lightweight, self hosted tool for reordering, removing, and backing up your Stremio addons — with no server, no sign-up, and no third-party involvement. Your credentials go directly from your browser to Stremio's official API over HTTPS.

---

## Why does this exist?

Stremio has no built-in way to reorder installed addons. The only native workaround is uninstalling and reinstalling them in the order you want — which is tedious when you have a lot. This tool lets you drag-and-drop them into any order and save the result back to your account in seconds.

---

## Features

- **Drag-and-drop reorder** — grab any row and drop it where you want
- **Remove addons** — hover a row and click the ✕ to remove it
- **Backup** — download your full addon list as a JSON file
- **Restore order** — discard unsaved changes and revert to what's on your account
- **Two auth methods** — email/password login or manual auth key
- **No server** — everything runs in your browser; nothing is stored or transmitted anywhere except directly to `api.strem.io`
- **Single file** — the entire app is one `.html` file with no dependencies to install

---

## Usage

### Option 1 — Download and open

1. Download `StremioAddonManager.html`
2. Open it in your browser (double-click or drag onto a browser window)
3. Log in and start managing your addons

> **Note:** The file must be served over HTTP to make API calls. If login doesn't work when opening directly as a file (`file://`), use Option 2 below.

### Option 2 — Serve locally

If you hit any issues with `file://`, serve it over a local HTTP server instead. Pick whichever you have available:

**Python:**
```bash
python -m http.server 8080
```
Then open `http://localhost:8080/StremioAddonManager.html`

**Node.js:**
```bash
npx serve .
```

**VS Code:** Install the [Live Server](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer) extension, right-click the file → *Open with Live Server*

---

## Authentication

There are two ways to connect your Stremio account:

### Email & password
Enter the email and password you use to log into Stremio. This sends your credentials directly to `https://api.strem.io/api/login` over HTTPS — the same request Stremio's own apps make. Nothing is stored or intercepted.

> Facebook/Google login is not supported via this method. Use the auth key method instead.

### Auth key
If you log into Stremio via Google or Facebook, grab your auth key manually:

1. Go to [web.stremio.com](https://web.stremio.com) and log in
2. Open DevTools (`F12`) → Console tab
3. Paste the following and press Enter:
   ```js
   JSON.parse(localStorage.getItem("profile")).auth.key
   ```
4. Copy the output (without quotes) and paste it into the Auth Key field

---

## How it works

The app uses two undocumented but stable Stremio API endpoints:

| Endpoint | Purpose |
|---|---|
| `POST /api/login` | Authenticate and get an auth key |
| `POST /api/addonCollectionGet` | Fetch your current addon list |
| `POST /api/addonCollectionSet` | Save a new addon order back to your account |

After saving, **restart Stremio** (or reload the web app) to see the new order take effect.

---

## Privacy

- Your credentials are sent **only** to `api.strem.io` — the official Stremio API
- Nothing is logged, stored, or transmitted to any other server
- The app has no backend and is a static HTML file
- Closing the tab clears everything from memory

You can verify this yourself by inspecting the source.

---

## Disclaimer

This is an unofficial community tool and is not affiliated with or endorsed by Stremio. It uses Stremio's internal API as a workaround for missing native functionality. Use at your own risk! No warranty or support is provided by Stremio.

---

## License

MIT
