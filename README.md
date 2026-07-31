# 💸 Peso AI — Personal Debt Tracker

> A mobile-first debt management web app built for Filipino users. Track all your debts, follow a cash-flow snowball strategy, and work your way to financial freedom — one payment at a time.

![Peso AI](https://img.shields.io/badge/Peso_AI-Debt_Tracker-1A2744?style=for-the-badge&logo=firebase&logoColor=white)
![Firebase](https://img.shields.io/badge/Firebase-Firestore-FF6F00?style=for-the-badge&logo=firebase&logoColor=white)
![HTML](https://img.shields.io/badge/Single_File-HTML-E34F26?style=for-the-badge&logo=html5&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)

---

## ✨ Features

| Feature | Description |
|---|---|
| 🔐 **Auth** | Email/password login & registration via Firebase Auth |
| 📋 **Debt list** | Track up to unlimited debts with tier, phase, due date, monthly payment |
| 📊 **Dashboard** | Live KPIs — total debt, paid off, progress bar, phase breakdown |
| ⛄ **Snowball tracker** | 21-step payoff chain — freed cash rolls into the next debt automatically |
| 💰 **Budget view** | Income vs known payments with remaining cash calculator |
| ➕ **Add / Edit / Delete** | Full CRUD for every debt entry |
| ✅ **Mark paid** | Toggle any debt as paid and watch your progress update live |
| 🔥 **Firestore sync** | All data saved per user in Firebase — works on any device |
| 📱 **Mobile-first** | Designed for phone use, max-width 430px, sticky nav |
| 🇵🇭 **PH context** | Peso (₱) formatting, Filipino greetings, local lender names |

---

## 📸 App Screens

```
┌─────────────────────┐   ┌─────────────────────┐   ┌─────────────────────┐
│  💸 Peso AI         │   │  📋 All Debts        │   │  ⛄ Snowball         │
│─────────────────────│   │─────────────────────│   │─────────────────────│
│ Magandang umaga!    │   │ Filter: All | T1 …  │   │ 1. Atome Loan →     │
│                     │   │                     │   │    Frees ₱1,270/mo  │
│ Total debt          │   │ 🔴 Anchoridge        │   │                     │
│ ₱452,960            │   │ ₱40,278 · 28mo left │   │ 2. Salmon Loan →    │
│                     │   │                     │   │    Frees ₱2,990/mo  │
│ Paid off  ₱0        │   │ 🟠 Salmon Loan       │   │                     │
│ Active    21        │   │ ₱5,980 · PAY NOW ✓  │   │ ...                 │
│ Finishing 3         │   │                     │   │ 21. San Jose Koop   │
│                     │   │ 🔵 Maya Credit Card  │   │     → DEBT FREE 🎉  │
│ ████░░░░ 0%         │   │ ₱51,970 · FREEZE    │   │     Feb 2029        │
└─────────────────────┘   └─────────────────────┘   └─────────────────────┘
```

---

## 🚀 Quick Start

### 1. Clone or download

```bash
git clone https://github.com/YOUR-USERNAME/peso-ai.git
cd peso-ai
```

Or just [download the ZIP](../../archive/refs/heads/main.zip) and open `index.html` in your browser — it works in demo mode without any setup.

### 2. Open in browser

```bash
# No build step needed — just open the file
open index.html
```

The app runs in **demo mode** with sample data until you connect Firebase.

---

## 🔥 Firebase Setup (for cloud sync)

### Step 1 — Create a Firebase project

1. Go to [console.firebase.google.com](https://console.firebase.google.com)
2. Click **Add project** → name it (e.g. `peso-ai`)
3. Disable Google Analytics if not needed → **Create project**

### Step 2 — Enable Authentication

1. Left sidebar → **Build → Authentication**
2. Click **Get started**
3. Enable **Email/Password** provider → Save

### Step 3 — Create Firestore Database

1. Left sidebar → **Build → Firestore Database**
2. Click **Create database**
3. Choose **Start in test mode** → select your region → Done

### Step 4 — Register a Web App

1. Project overview → click the **Web** icon (`</>`)
2. Register app (name it `peso-ai-web`)
3. Copy the `firebaseConfig` object — it looks like this:

```javascript
const firebaseConfig = {
  apiKey: "AIzaSy...",
  authDomain: "peso-ai.firebaseapp.com",
  projectId: "peso-ai",
  storageBucket: "peso-ai.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abc123"
};
```

### Step 5 — Paste config into the app

Open `index.html`, find the config block near the top of the `<script>` tag, and replace it:

```javascript
// ══ PASTE YOUR FIREBASE CONFIG HERE ══════════════════════════════
const firebaseConfig = {
  apiKey: "YOUR_ACTUAL_KEY",        // ← replace this
  authDomain: "YOUR_PROJECT.firebaseapp.com",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_PROJECT.appspot.com",
  messagingSenderId: "YOUR_SENDER_ID",
  appId: "YOUR_APP_ID"
};
// ═════════════════════════════════════════════════════════════════
```

### Step 6 — Set Firestore security rules

In Firebase Console → Firestore → **Rules** tab, replace the default rules with:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{userId}/{document=**} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
  }
}
```

Click **Publish**. This ensures each user can only access their own data.

---

## 🌐 Deploy to GitHub Pages

### Option A — Manual upload (easiest)

1. Go to your GitHub repo → **Add file → Upload files**
2. Upload `index.html`
3. Go to **Settings → Pages**
4. Source: **Deploy from a branch** → `main` / `/(root)`
5. Click **Save**

Your app will be live at:
```
https://YOUR-USERNAME.github.io/peso-ai/
```

### Option B — Git command line

```bash
git init
git add index.html README.md
git commit -m "Initial deploy: Peso AI Debt Tracker"
git branch -M main
git remote add origin https://github.com/YOUR-USERNAME/peso-ai.git
git push -u origin main
```

Then enable Pages in repo Settings as above.

---

## 🗂️ Data Structure (Firestore)

Each user's debts are stored at `/users/{uid}/debts/{debtId}`:

```json
{
  "name": "Maya Credit Card",
  "balance": 51970.31,
  "monthlyPay": null,
  "monthsLeft": null,
  "due": "Jul 6",
  "rate": "HIGH",
  "tier": 3,
  "phase": 3,
  "action": "STOP USING",
  "notes": "High interest – freeze card",
  "secured": false,
  "status": "current",
  "createdAt": "2026-07-31T00:00:00Z"
}
```

### Tier system

| Tier | Color | Meaning |
|---|---|---|
| 1 | 🔴 Red | Must stay current — secured/asset-linked |
| 2 | 🟠 Amber | Finish fast — installment loans close to done |
| 3 | 🔵 Blue | Revolving debt — high interest, freeze & attack |
| 4 | 🟢 Green | Long-term installments — maintain minimum |

### Phase system

| Phase | Timeline | Strategy |
|---|---|---|
| 1 | Now → Month 4 | Kill shortest loans first, free up cash |
| 2 | Month 4 → 10 | Roll freed cash into JuanHand cluster |
| 3 | Month 10+ | Attack revolving debt (Maya CC, Atome Card) |
| 4 | Throughout | Keep long-term loans current |

---

## 💡 The Cash-Flow Snowball Strategy

This app uses a **cash-flow snowball** — not the traditional debt snowball (smallest balance first) or avalanche (highest interest first), but instead prioritizing **debts that free up the most monthly cash the fastest**.

```
Month 1–2:  Pay off Atome Loan short → frees ₱1,270/mo
            Pay off Salmon Loan      → frees ₱2,990/mo
Month 3–4:  Roll both into CIMB Loan → frees ₱990/mo
...and so on until San Jose Koop in Feb 2029 → 🎉 DEBT FREE
```

The goal is to reduce the **number of active monthly obligations** as fast as possible, which prevents the cash-flow squeeze that comes from juggling too many due dates.

---

## 🛠️ Tech Stack

- **Frontend:** Vanilla HTML, CSS, JavaScript — zero dependencies, zero build tools
- **Auth:** Firebase Authentication (Email/Password)
- **Database:** Cloud Firestore (NoSQL, real-time)
- **Hosting:** GitHub Pages (free)
- **AI assistance:** Built with Claude (Anthropic)

---

## 📁 File Structure

```
peso-ai/
├── index.html      ← entire app (HTML + CSS + JS in one file)
└── README.md       ← this file
```

---

## 🔒 Privacy & Security

- All debt data is stored in **your own** Firebase project — not shared with anyone
- Firestore rules ensure each user can **only access their own data**
- No analytics, no ads, no third-party tracking
- The app never transmits your financial data anywhere except your own Firestore database

---

## 🤝 Contributing

Pull requests welcome. To suggest a feature or report a bug, open an [issue](../../issues).

---

## 📄 License

MIT License — free to use, modify, and distribute.

---

## 🙏 Acknowledgements

Built as a personal finance tool for Filipino users managing multiple consumer debts. Inspired by real debt payoff strategies adapted to the Philippine lending landscape (GCash, Maya, JuanHand, Atome, CIMB, etc.).

---

<div align="center">
  <strong>Peso AI</strong> · Built with ❤️ and Claude · 
  <a href="https://YOUR-USERNAME.github.io/peso-ai/">Live Demo</a>
</div>
