# 📋 Quick Setup & Troubleshooting Guide

This guide answers common questions and troubleshooting steps for setting up **CloudBox** with Firebase.

---

## ⚡ Quick Checklist

- [ ] Firebase project created in [Firebase Console](https://console.firebase.google.com/)
- [ ] Firebase Cloud Storage enabled
- [ ] Storage rules set to `allow read, write: if true;` and **Published**
- [ ] Web App registered and `firebaseConfig` copied
- [ ] Project running on a local web server (e.g. VS Code Live Server or `python -m http.server 3000`)
- [ ] Credentials saved in `js/firebase-config.js` or via the **Firebase Setup** button in the app

---

## ❓ Frequently Asked Questions & Troubleshooting

### 1. Why do I see a CORS error in the browser console when uploading to Firebase?
**Cause:** Firebase Cloud Storage restricts direct browser uploads unless CORS (Cross-Origin Resource Sharing) is configured or standard security rules are met.
**Fix:**
Ensure your Firebase Storage rules are published properly:
```javascript
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /{allPaths=**} {
      allow read, write: if true;
    }
  }
}
```
If you encounter strict domain CORS, you can set CORS for your bucket via `gsutil` or Google Cloud Shell:
```bash
# Create cors.json
echo '[{"origin": ["*"],"method": ["GET", "POST", "PUT", "DELETE", "HEAD"],"responseHeader": ["*"],"maxAgeSeconds": 3600}]' > cors.json
# Apply to bucket:
gsutil cors set cors.json gs://YOUR_STORAGE_BUCKET_NAME
```

### 2. Can I test uploading files without any Firebase setup?
**Yes!** CloudBox comes with a built-in **Interactive Demo Mode**. If no Firebase credentials are provided, it automatically utilizes browser **IndexedDB** to store file binary data locally. You can upload, view, download, and delete files with realistic progress animations immediately!

### 3. Why does opening `index.html` directly from file explorer show a CORS or module error?
**Explanation:** Modern browsers enforce security policies preventing `import` statements on `file://` URLs.
**Solution:** Always serve the project through a local development server such as:
- VS Code extension: **Live Server**
- Command line: `python -m http.server 3000`
- Command line: `npx serve .`

### 4. How do I switch back from Cloud mode to Demo mode?
Click on **"Firebase Setup"** in the top navigation bar, then click **"Switch to Demo Mode"**. This clears your cached credentials from `localStorage`.

---

## 🔐 Future Enhancements (For Student Projects & Portfolios)
- **User Authentication**: Add Firebase Authentication (Email/Password or Google Sign-In) to give each user their own private storage space (`uploads/{userId}/...`).
- **File Folders / Directories**: Allow creating virtual folders and hierarchical navigation.
- **File Sharing Permissions**: Create public and private share links with expiration timestamps.
- **Bulk Operations**: Multi-file selection for bulk download (zip) or bulk deletion.
