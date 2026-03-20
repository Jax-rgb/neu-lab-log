# 🔬 NEU Laboratory Usage Log

A web & mobile-compatible app for logging professor room usage at Nueva Ecija University of Science and Technology.

---

## ✨ Features

| Feature | Details |
|---|---|
| QR Code Check-in | Scan professor ID QR code to log room usage |
| Google Sign-in | Institutional email `@neu.edu.ph` login |
| Success Message | "Thank you for using Room [Room Number]" |
| Admin Dashboard | Stats cards, charts, full logs table |
| Search & Filter | By professor, room, daily/weekly/monthly/custom date |
| CSV Export | Download filtered logs as spreadsheet |
| Block Professor | Toggle to block/unblock access |
| PWA | Installable on mobile like a native app |
| Firebase Security | Firestore rules prevent tampering |

---

## 🚀 Setup Guide (No Installation Needed — All in Browser)

### Step 1: Create Firebase Project

1. Go to [https://console.firebase.google.com](https://console.firebase.google.com)
2. Click **"Add Project"** → Name it `neu-lab-log` → Click through setup
3. Enable **Google Analytics** (optional)

### Step 2: Enable Services

**Authentication:**
1. In Firebase Console → **Authentication** → **Get Started**
2. Click **Google** → Enable → Set support email → Save
3. Go to **Settings** → **Authorized domains** → Add your GitHub Pages or Firebase Hosting domain

**Firestore Database:**
1. **Firestore Database** → **Create Database**
2. Choose **Production mode** → Select region (e.g., `asia-southeast1` for Philippines)
3. Click **Enable**

### Step 3: Get Your Firebase Config

1. Firebase Console → **Project Settings** (gear icon)
2. Under **"Your apps"** → Click **"</ >"** (Web) → Register app
3. Copy the `firebaseConfig` object — it looks like:

```javascript
const firebaseConfig = {
  apiKey: "AIzaSy...",
  authDomain: "your-project.firebaseapp.com",
  projectId: "your-project-id",
  storageBucket: "your-project.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abc123"
};
```

### Step 4: Put Config in the App

1. Open `public/index.html`
2. Find this section (around line 370):

```javascript
const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_PROJECT_ID.firebaseapp.com",
  ...
};
```

3. Replace with your real config values

### Step 5: Create Your First Admin

In Firebase Console → **Firestore** → **Start collection**:
- Collection ID: `admins`
- Document ID: *(your Google account UID — find it in Authentication → Users)*
- Add field: `email` → your @neu.edu.ph email

### Step 6: Add Professors

In Firestore → `professors` collection → Add document:
```
name: "Juan Dela Cruz"
email: "Juan.DelaCruz@neu.edu.ph"
qr_id: "NEU-PROF-001"   ← must match what's encoded in their QR code
status: "active"
```

### Step 7: Deploy Security Rules

In Firebase Console → **Firestore** → **Rules** tab → Paste contents of `firestore.rules` → Publish

---

## 📱 GitHub + Firebase Hosting (Auto-Deploy)

### Push to GitHub

```bash
# In your project folder:
git init
git add .
git commit -m "Initial commit: NEU Lab Usage Log"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/neu-lab-app.git
git push -u origin main
```

### Connect Firebase Hosting

1. Firebase Console → **Hosting** → **Get Started**
2. Follow the setup (skip the CLI steps — we use GitHub Actions)
3. Go to **Hosting** → **GitHub** tab → Connect your repository

### Set GitHub Secrets

In your GitHub repo → **Settings** → **Secrets and variables** → **Actions**:

| Secret Name | Value |
|---|---|
| `FIREBASE_SERVICE_ACCOUNT` | Download from Firebase → Project Settings → Service accounts → Generate new private key |
| `FIREBASE_PROJECT_ID` | Your Firebase project ID (e.g., `neu-lab-log`) |

Now every push to `main` auto-deploys! 🎉

---

## 📲 QR Code Setup for Professors

Each professor needs a QR code that encodes their `qr_id` value from Firestore.

**Generate QR codes for free:**
1. Go to [https://www.qr-code-generator.com](https://www.qr-code-generator.com)
2. Select **Text** type
3. Enter the professor's ID (e.g., `NEU-PROF-001`)
4. Download and print for their ID card

---

## 🔒 Security Model

| Role | Can Do |
|---|---|
| Professor | Create log entries only (no edit/delete) |
| Admin | Read all logs, manage professors, block accounts |
| Blocked Professor | Cannot create new log entries |
| Anyone | Cannot access dashboard data |

---

## 🧪 Demo Mode

If you open the app without configuring Firebase (keeps `YOUR_API_KEY`), it runs in **Demo Mode** with sample data. Great for testing and presentations!

**Demo QR codes to test:** `NEU-PROF-001` through `NEU-PROF-005`

---

## 📁 Project Structure

```
neu-lab-app/
├── public/
│   ├── index.html          ← Entire app (one file!)
│   └── manifest.json       ← PWA manifest
├── .github/
│   └── workflows/
│       └── deploy.yml      ← Auto-deploy to Firebase
├── firebase.json           ← Firebase Hosting config
├── firestore.rules         ← Security rules
├── firestore.indexes.json  ← Firestore query indexes
└── README.md
```

---

## 💡 Tips

- **Mobile camera for QR:** Works best in Chrome on Android. On iOS, use Safari.
- **Add to Home Screen:** Users can install the PWA from browser menu for a native-app feel.
- **Multiple rooms:** Admin can set up a tablet per room pre-set to that room's number.
- **Offline:** Add a service worker for full offline support if needed.
