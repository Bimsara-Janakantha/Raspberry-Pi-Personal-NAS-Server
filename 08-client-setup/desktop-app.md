# Desktop App Setup (Windows and Mac)

The Nextcloud desktop client gives you a folder on your PC that automatically syncs with your NAS vault — exactly like OneDrive or Google Drive.

---

## Step 1 — Download the Desktop Client

Go to the official download page and download the installer for your operating system:

```
https://nextcloud.com/install/#desktop-files
```

Install it and open the application.

---

## Step 2 — Connect to Your Server

1. When the app opens, click **Log in**.
2. Enter your server address:
   ```
   https://cloud.mynas.dev
   ```
3. Click **Next**. A browser window will open.
4. Log in with your Nextcloud username and an App Password (see the [Mobile App](mobile-app.md) guide for how to create one).
5. Click **Grant access**.

---

## Step 3 — Choose the Sync Method (Important)

After logging in, the app will ask how you want to sync your files. You will see two options:

| Option | What It Does |
|--------|-------------|
| Synchronize everything | Downloads all your NAS files to your local PC immediately |
| **Use virtual files instead of downloading content immediately** | Shows all files in Explorer but only downloads them when you open them |

**Always choose Virtual files.**

### Why Virtual Files Matters

Your NAS vault may hold several terabytes of data. Your laptop likely has far less free space than that. If you chose "Synchronize everything", the desktop app would try to download your entire NAS to your laptop and fill up the hard drive.

Virtual files give you the **OneDrive magic**: all your files appear in Windows Explorer as if they were local, they take up almost zero space, and a file is only downloaded to your actual PC when you double-click to open it. Once you are done, the local copy can be freed to save space again.

---

## Step 4 — Finish and Connect

Click **Connect** (or **Finish**). The Nextcloud folder will appear in Windows Explorer or Mac Finder within a few seconds.

```
This PC
├── Local Disk (C:)
├── Nextcloud (cloud.mynas.dev)   ← Your NAS vault
└── ...
```

Any file you place in this folder is synced to the NAS. Any file added to the NAS from another device appears here automatically.

---

## Finding the Server Address

If you need the server address at any point, find it in the web dashboard:

1. Log into `https://cloud.mynas.dev` in your browser.
2. Click **profile icon** → **Settings** → **Mobile & Desktop**.
3. The server address is shown there.

---

[← Mobile App](mobile-app.md) | [Back to Client Setup Overview →](README.md)
