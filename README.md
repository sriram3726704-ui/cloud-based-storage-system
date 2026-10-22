# ☁️ CloudBox - Cloud-Based File Storage System

A modern, student-friendly, full-featured **Cloud-Based File Storage System** built with **HTML5, CSS3, Vanilla JavaScript (ES Modules)**, and **Google Firebase Cloud Storage**.

---

## 🌟 Key Features

- 📤 **Drag & Drop Uploads**: Multi-file drag and drop with live progress bars, percent counters, and cancellation.
- 👁️ **Instant Previews**: Rich built-in preview modal for Images, PDFs, Videos, Audio tracks, and Code/Text files.
- 📥 **Direct Download**: One-click file downloading with clean, preserved filenames.
- 🔗 **Shareable Links**: Copy public/signed download URLs to clipboard with a single click.
- 🗑️ **Safe Deletion**: Deletes files directly from Google Cloud Storage with a confirmation dialog.
- 🔍 **Real-Time Search & Filters**: Search by file name; filter by categories (*Images, Documents, Media, Archives, Other*); sort by date, name, or size.
- 📊 **Storage Overview**: Real-time storage consumption meter and file count statistics.
- ⚡ **Zero-Setup Demo Mode**: Runs immediately out of the box using browser IndexedDB so students can test all features offline before connecting their Firebase project.
- ⚙️ **In-App Config Manager**: Connect Firebase either by editing `js/firebase-config.js` or by pasting your config directly in the application UI.

---

## 📁 Project Structure

```
cloud-based-file-storage-system/
│
├── index.html               # Main user interface & modals (semantic HTML5)
├── README.md                # Project documentation and guide
├── SETUP_GUIDE.md           # Detailed Firebase step-by-step tutorial
│
├── css/
│   └── style.css            # Modern glassmorphism UI, variables, animations
│
└── js/
    ├── firebase-config.js   # Firebase credentials and configuration helpers
    ├── storage-service.js   # Unified storage layer (Firebase Storage + IndexedDB fallback)
    └── app.js               # Application logic, drag & drop, rendering, modals, toasts
```

---

## 🚀 How to Run the Project

Because the project uses modern **JavaScript ES Modules (`import/export`)**, it must be served through a local web server (rather than opening `file:///index.html` directly):

### Option 1: VS Code Live Server (Easiest for Students)
1. Open this project folder in **Visual Studio Code**.
2. Install the **Live Server** extension by *Ritwick Dey* (if not installed).
3. Right-click on `index.html` and select **"Open with Live Server"**.
4. The application will open automatically at `http://127.0.0.1:5500`.

### Option 2: Python (Built into Windows/macOS/Linux)
Open a terminal in the project directory and run:
```bash
python -m http.server 3000
```
Open your browser and navigate to: `http://localhost:3000`

### Option 3: Node.js / npx
If you have Node.js installed, run:
```bash
npx serve .
```
Navigate to the URL shown in your terminal (usually `http://localhost:3000`).

---

## 🔧 Step-by-Step Firebase Storage Setup Guide

Follow these 5 steps to connect your own Google Cloud Storage bucket:

### Step 1: Create a Firebase Project
1. Open the [Firebase Console](https://console.firebase.google.com/) and sign in with your Google account.
2. Click **"Add project"** (or "Create a project").
3. Enter a project name (e.g., `my-cloudbox-storage`).
4. (Optional) Disable Google Analytics to simplify setup, then click **"Create Project"**.

---

### Step 2: Enable Firebase Cloud Storage
1. In the left navigation menu of Firebase Console, click on **"Build"** ➔ **"Storage"**.
2. Click **"Get started"**.
3. A security rules prompt will appear: select **"Start in test mode"** and click **Next**.
4. Choose a Cloud Storage location close to you (e.g., `us-central1`, `asia-south1`, etc.) and click **"Done"**.

---

### Step 3: Configure Storage Security Rules
To allow your web application to upload, view, and delete files, update your storage rules:

1. In the Storage dashboard, click on the **"Rules"** tab.
2. Replace the contents with the following rules:

```javascript
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /{allPaths=**} {
      // Allows anyone with your Web App config to read, write, and delete files
      allow read, write: if true;
    }
  }
}
```

3. Click the **"Publish"** button to apply the rules.

> 💡 *Note for production: When adding user authentication in the future, you can restrict access so each user can only read/write their own folder: `allow read, write: if request.auth != null;`.*

---

### Step 4: Register Web App & Get Credentials
1. In Firebase Console, click the ⚙️ **Project settings** icon (gear icon in the top left sidebar).
2. Under the **"General"** tab, scroll down to the **"Your apps"** section.
3. Click the **Web** icon (`</>`) to register a web application.
4. Name your app (e.g., `CloudBox Web`) and click **"Register app"**.
5. You will see a `firebaseConfig` object like this:

```javascript
const firebaseConfig = {
  apiKey: "AIzaSyBxxxxxxx-xxxxxxx",
  authDomain: "my-cloudbox.firebaseapp.com",
  projectId: "my-cloudbox",
  storageBucket: "my-cloudbox.appspot.com",
  messagingSenderId: "123456789012",
  appId: "1:123456789012:web:abcdef123456"
};
```

---

### Step 5: Connect Credentials to the Project

You can connect your credentials in **either** of these two ways:

#### Way A: In-App UI (No code editing required!)
1. Launch the app in your browser (`http://localhost:3000`).
2. Click the **"Firebase Setup"** button in the top navigation bar.
3. Paste the values into the fields (or paste the entire `firebaseConfig` block into the *"Paste Config Object"* tab).
4. Click **"Save & Connect"**. The app will immediately connect to your live Firebase Cloud Storage!

#### Way B: Editing `js/firebase-config.js`
Open `js/firebase-config.js` in your code editor and replace the placeholder values:

```javascript
export const firebaseConfig = {
  apiKey: "AIzaSyBxxxxxxx-xxxxxxx",
  authDomain: "my-cloudbox.firebaseapp.com",
  projectId: "my-cloudbox",
  storageBucket: "my-cloudbox.appspot.com",
  messagingSenderId: "123456789012",
  appId: "1:123456789012:web:abcdef123456"
};
```

---

## 🧠 Educational Concepts Demonstrated

1. **Cloud Storage Architecture**: Understanding bucket references (`ref`), resumable file uploads (`uploadBytesResumable`), download URL generation (`getDownloadURL`), metadata extraction (`getMetadata`), and deletion (`deleteObject`).
2. **Asynchronous JavaScript**: Using Promises, `async/await`, and upload progress event observers (`uploadTask.on('state_changed', ...)`).
3. **Responsive Glassmorphism Design**: Using CSS custom properties (variables), `backdrop-filter: blur()`, CSS Grid auto-fill layouts, and CSS keyframe animations.
4. **Resilient Architecture**: Automatic fallback to browser IndexedDB when offline or unconfigured, preventing application crashes.

---

## 🛡️ License
This project is open-source and free to use for learning, classroom demonstrations, and student portfolios.
