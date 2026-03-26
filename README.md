# 🚀 Nexus Portfolio — Setup Guide

A PWA for tracking crypto, stocks & gold — with Firebase, Google Login, and GitHub Pages.

---

## Step 1 — Create Firebase Project

1. Go to https://console.firebase.google.com/
2. **Add project** → name it `nexus-portfolio` (or anything you want)
3. Disable Google Analytics (optional) → **Create project**

---

## Step 2 — Enable Google Sign-In

1. In Firebase Console → **Authentication** → **Get started**
2. **Sign-in method** tab → Enable **Google**
3. Set your **Project support email** → Save

---

## Step 3 — Create Firestore Database

1. **Firestore Database** → **Create database**
2. Choose **Start in production mode** → Select region (e.g., `asia-southeast2` for Indonesia)
3. **Enable**

### Set Security Rules:
Go to **Rules** tab and paste:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /assets/{assetId} {
      allow read, write: if request.auth != null && request.auth.uid == resource.data.uid;
      allow create: if request.auth != null && request.auth.uid == request.resource.data.uid;
    }
  }
}
```

---

## Step 4 — Get Firebase Config

1. Firebase Console → **Project Settings** (gear icon)
2. Scroll to **Your apps** → click **</>** (Web)
3. Register app name → Copy the `firebaseConfig` object

Paste it into `index.html` replacing the placeholder:

```js
const firebaseConfig = {
  apiKey: "AIza...",
  authDomain: "your-project.firebaseapp.com",
  projectId: "your-project-id",
  storageBucket: "your-project.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123:web:abc..."
};
```

---

## Step 5 — Deploy to GitHub Pages

1. Create a new GitHub repo (e.g. `nexus-portfolio`)
2. Upload all files: `index.html`, `manifest.json`, `sw.js`
3. Go to **Settings** → **Pages**
4. Source: **Deploy from branch** → `main` → `/ (root)` → **Save**
5. Your URL: `https://yourusername.github.io/nexus-portfolio/`

### Add GitHub Pages URL as Authorized Domain in Firebase:
1. Firebase Console → **Authentication** → **Settings** → **Authorized domains**
2. Add `yourusername.github.io`

---

## Step 6 — (Optional) Live Crypto Prices

The app fetches live prices from CoinGecko's free API automatically for:
`BTC, ETH, BNB, SOL, ADA, DOGE, XRP, MATIC, AVAX, DOT`

No API key needed. Refresh button in Live Prices tab.

---

## Features

- ✅ Google Sign-In (per-user data isolation)
- ✅ Track Crypto, Stocks, Gold
- ✅ Total portfolio value dashboard
- ✅ Allocation donut chart
- ✅ 7-day trend line chart
- ✅ Live crypto prices (CoinGecko)
- ✅ P&L % vs buy price
- ✅ PWA — installable on Android/iOS
- ✅ Offline capable (service worker)
- ✅ IDR currency formatting

---

## Adding Assets

- Go to **Assets** tab → **+ ADD ASSET**
- Type: Crypto / Stock / Gold
- Symbol: `BTC`, `BBRI`, `ANTM`, `LQ45`, `GOLD`, etc.
- Qty: number of units / lots / grams
- Buy Price: your purchase price in IDR per unit
- Current Price: today's price in IDR (crypto auto-updates)
