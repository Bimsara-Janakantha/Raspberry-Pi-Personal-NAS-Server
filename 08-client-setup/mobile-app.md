# Mobile App Setup

The official Nextcloud mobile app gives access to your files on the go and can automatically back up photos from your phone.

---

## Step 1 — Download the App

| Platform | Link |
|----------|------|
| iOS (iPhone/iPad) | [App Store — Nextcloud](https://apps.apple.com/app/nextcloud/id1125420102) |
| Android | [Google Play — Nextcloud](https://play.google.com/store/apps/details?id=com.nextcloud.client) |

---

## Step 2 — Add Your Account

1. Open the app and tap **Add account** (or the **+** button).
2. Enter your server address:
   ```
   https://cloud.mynas.dev
   ```
3. The app will open a browser window for you to log in.
4. Enter your Nextcloud username and password.
5. Tap **Grant access** to allow the app to connect.

---

## Step 3 — Use an App Password (Recommended)

For better security, use an **App Password** instead of your main Nextcloud password. This way, if your phone is ever compromised, you can revoke that specific password without changing your main one.

**Create an App Password:**

1. Log into Nextcloud via the web browser.
2. Click the **profile icon** → **Settings** → **Security**.
3. Scroll down to **Devices & Sessions**.
4. Type a name for the device (e.g. `Alice's iPhone`) and click **Create new app password**.
5. Copy the generated password — you will only see it once.

Use this app password when logging into the mobile app instead of your main password.

---

## Enable Auto Photo Backup (Optional)

1. In the app, tap the **three lines** (menu) in the top left.
2. Tap **Auto upload**.
3. Enable **Auto upload photos** and/or **Auto upload videos**.
4. The app will automatically upload new photos to your Nextcloud vault in the background.

---

[← Web Browser](web-browser.md) | [Next: Desktop App →](desktop-app.md)
