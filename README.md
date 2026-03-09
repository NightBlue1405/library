# 🔮 Arcanum — Secret Study Vault

A hidden study app disguised as a classical library. One book holds a secret entrance.

## ✦ Features

- **Library UI** — Clickable book spines with real study content
- **Hidden entrance** — Click the cat's nose on the dark *Codex Obscura* tome
- **Dark lamp login** — A moody, elegant authentication screen
- **Email + Google auth** — Firebase Authentication
- **Real-time chat** — One-on-one messaging via Firestore
- **Ephemeral messages** — Disappear 10 seconds after being read
- **Fully responsive** — Mobile, tablet, desktop

---

## 🛠 Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | React 18 + Vite |
| Auth | Firebase Authentication |
| Database | Cloud Firestore |
| Hosting | Firebase Hosting |
| Fonts | Google Fonts (Cinzel, Crimson Text, IM Fell English) |

---

## 🚀 Local Setup

### 1. Clone & Install

```bash
git clone <your-repo-url>
cd arcanum
npm install
```

### 2. Create a Firebase Project

1. Go to [Firebase Console](https://console.firebase.google.com)
2. Click **Add project** → name it (e.g. `arcanum-vault`)
3. Enable **Google Analytics** (optional)
4. Click **Create project**

### 3. Enable Firebase Services

**Authentication:**
1. In Firebase Console → **Authentication** → **Get started**
2. Enable **Email/Password** provider
3. Enable **Google** provider (add your support email)

**Firestore:**
1. In Firebase Console → **Firestore Database** → **Create database**
2. Select **Start in test mode** (we'll add proper rules later)
3. Choose a region close to your users

### 4. Get Your Config Keys

1. Firebase Console → **Project Settings** (gear icon)
2. Scroll to **Your apps** → click **</>** (Web app)
3. Register app → copy the `firebaseConfig` object

### 5. Set Up Environment Variables

```bash
cp .env.example .env.local
```

Edit `.env.local` and fill in your Firebase config values:

```
VITE_FIREBASE_API_KEY=AIzaSy...
VITE_FIREBASE_AUTH_DOMAIN=your-project.firebaseapp.com
VITE_FIREBASE_PROJECT_ID=your-project-id
VITE_FIREBASE_STORAGE_BUCKET=your-project.appspot.com
VITE_FIREBASE_MESSAGING_SENDER_ID=123456789
VITE_FIREBASE_APP_ID=1:123456789:web:abc123
```

### 6. Deploy Firestore Security Rules

```bash
npm install -g firebase-tools
firebase login
firebase init firestore  # Select your project
firebase deploy --only firestore:rules
```

### 7. Run Locally

```bash
npm run dev
```

Open [http://localhost:5173](http://localhost:5173)

---

## 🔒 Firestore Security Rules

The `firestore.rules` file enforces:
- Users can only read/write their own profile
- Chat room access is restricted to the two participants
- Messages can only be created by the sender
- Both participants can update messages (read receipts, ephemeral flags)
- No hard deletes (soft delete with `deleted: true`)

Deploy rules:
```bash
firebase deploy --only firestore:rules
```

---

## 🌐 Deploy to Firebase Hosting

```bash
# Build the production app
npm run build

# Deploy hosting + rules
firebase deploy
```

Your app will be live at `https://your-project-id.web.app`

---

## 📁 Project Structure

```
arcanum/
├── src/
│   ├── components/
│   │   ├── Library.jsx         # Main bookshelf
│   │   ├── Library.css
│   │   ├── BookSpine.jsx       # Individual book (+ secret cat book)
│   │   ├── BookSpine.css
│   │   ├── BookContent.jsx     # Study content modal
│   │   ├── BookContent.css
│   │   ├── LoginPage.jsx       # Dark lamp login
│   │   ├── LoginPage.css
│   │   ├── ChatApp.jsx         # Chat shell with sidebar
│   │   ├── ChatApp.css
│   │   ├── UserList.jsx        # List of other scholars
│   │   ├── UserList.css
│   │   ├── ChatRoom.jsx        # Message thread
│   │   ├── ChatRoom.css
│   │   ├── ChatMessage.jsx     # Individual message + ephemeral timer
│   │   ├── ChatMessage.css
│   │   ├── LoadingScreen.jsx   # Candle loading state
│   │   └── LoadingScreen.css
│   ├── firebase/
│   │   ├── config.js           # Firebase initialization
│   │   ├── auth.js             # Auth helpers
│   │   └── chat.js             # Firestore chat + ephemeral logic
│   ├── hooks/
│   │   ├── useAuth.js          # Auth state hook
│   │   └── useChat.js          # Chat + ephemeral timer hook
│   ├── styles/
│   │   └── global.css          # CSS variables, animations
│   ├── App.jsx                 # Root component + routing
│   └── main.jsx
├── firestore.rules             # Security rules
├── firebase.json               # Hosting + rules config
├── .env.example                # Environment variable template
├── vite.config.js
└── package.json
```

---

## 🕰 How Ephemeral Messages Work

1. User A sends a message → stored in Firestore with `readBy: []`
2. User B opens the chat → message is marked `readBy: [B_uid]`
3. First read triggers `ephemeralStart` timestamp on the message
4. Both clients subscribe to the timestamp and count down 10 seconds
5. At 0 seconds, the message is soft-deleted (`deleted: true`)
6. The UI shows "✦ Message self-destructed ✦" briefly, then fades

---

## 🎨 Customization

- **Add books**: Edit the `BOOKS` array in `Library.jsx`
- **Change ephemeral timer**: Edit `EPHEMERAL_DELAY_MS` in `hooks/useChat.js`
- **Change colors**: Edit CSS variables in `styles/global.css`
- **Secret trigger**: The cat nose in `BookSpine.jsx` (look for `cat-nose` class)

---

## 🐱 Finding the Secret

Look for the darkest book on the shelf. It has a small cat silhouette on its spine.  
Click the cat's pink nose.  
The lamp lights up.

*"Not all who wander the library are there for study."*
