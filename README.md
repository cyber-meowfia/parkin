# PARKIN — GitHub Pages Manual Deployment Bundle

This folder contains the complete, pre-compiled static web build of **PARKIN** ready to upload and publish to GitHub Pages.

---

## What is Inside This Folder

* **`index.html`**: The main entry page configured for GitHub Pages.
* **`.nojekyll`**: Tells GitHub Pages to bypass Jekyll processing (required for Flutter web assets).
* **`404.html`**: Route fallback handler to prevent blank screen 404 errors when refreshing.
* **`main.dart.js`**: The compiled Flutter application logic.
* **`flutter.js` & `flutter_bootstrap.js`**: Flutter web engine loaders.
* **`assets/` & `canvaskit/`**: WebAssembly engine, icons, and fonts.

---

## How to Manually Upload & Publish

### Option 1: Drag-and-Drop via GitHub Web Browser (No Git Commands Needed)

1. Create a new repository on [GitHub](https://github.com/new) named `parkin` (or any name you choose).
2. On the repository page, click **"uploading an existing file"**.
3. Drag and drop all the files and folders from this folder into the GitHub upload area.
4. Click **Commit changes**.
5. In your GitHub repository:
   * Go to **Settings** $\rightarrow$ **Pages**.
   * Under **Build and deployment** $\rightarrow$ **Source**, choose **Deploy from a branch**.
   * Branch: **`main`**, Folder: **`/(root)`**.
   * Click **Save**.

---

### Option 2: Using Terminal inside this Desktop folder

Open Terminal and run:

```bash
cd ~/Desktop/parkin_github_pages
git init
git add .
git commit -m "Deploy PARKIN to GitHub Pages"
git branch -M main
git remote add origin https://github.com/<YOUR_GITHUB_USERNAME>/<YOUR_REPO_NAME>.git
git push -u origin main --force
```

Then in GitHub $\rightarrow$ **Settings** $\rightarrow$ **Pages** $\rightarrow$ select **Branch: main** and **Folder: / (root)**.

---

### Important: Firebase Domain Authorization

In the [Firebase Console](https://console.firebase.google.com/):
1. Go to **Authentication** $\rightarrow$ **Settings** tab.
2. Click **Authorized domains** $\rightarrow$ **Add domain**.
3. Add `<YOUR_GITHUB_USERNAME>.github.io` so users can sign in on the web.
