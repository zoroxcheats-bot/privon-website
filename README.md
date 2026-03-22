# PRIVON — Private Messenger

> No phone number. No ads. Just privacy.

A full-featured private messaging web app with Moments (stories), real-time chat, and fine-grained privacy controls — built with vanilla HTML/CSS/JS and Firebase.

---

## 📁 Files

| File | Description |
|------|-------------|
| `privon-app.html` | The full chat application |
| `privon-landing.html` | Marketing landing page |

---

## 🚀 Features

### 💬 Chat
- Real-time messaging with Firebase Firestore
- Photo & video sharing via Firebase Storage
- Friends-only messaging system
- Friend requests (send, accept, reject)

### ✨ Moments (Stories)
- Post **Text/Mood**, **Photo**, **Video**, or **Anonymous** thoughts
- Auto-expire after **24 hours**
- **Stories bar** at top of Convos + dedicated **Moments tab**
- **Emoji reactions** (private, per viewer)
- **Reply** to a Moment → opens DM instantly
- **Moment insights** — view count + reactions breakdown
- **Privacy per post:**
  - 👥 All Friends
  - ⭐ Close Friends
  - 🎯 Custom pick (allowlist)
  - 🚫 Hidden from specific people
- **Quiet mode** — post without notifying anyone

### 🔐 Privacy Controls
- Toggle **Last Seen** visibility
- Toggle **Read Receipts**
- Username-based identity (no phone number ever)

### ⚙️ Settings
- Update username anytime
- Delete account permanently

---

## 🛠️ Setup

### 1. Clone the repo
```bash
git clone https://github.com/yourusername/privon.git
cd privon
```

### 2. Firebase Setup
This project uses your existing Firebase project. The config is already embedded in `privon-app.html`.

If you want to use your own Firebase project:
1. Go to [Firebase Console](https://console.firebase.google.com)
2. Create a new project
3. Enable **Authentication** (Email/Password + Google)
4. Enable **Firestore Database**
5. Enable **Storage**
6. Replace the `firebaseConfig` object in `privon-app.html` with your own

### 3. Firestore Rules
Set these rules in your Firebase Console → Firestore → Rules:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{userId} {
      allow read: if request.auth != null;
      allow write: if request.auth.uid == userId;
    }
    match /friendRequests/{requestId} {
      allow read, write: if request.auth != null;
    }
    match /chats/{chatId}/messages/{messageId} {
      allow read, write: if request.auth != null;
    }
    match /moments/{momentId} {
      allow read, write: if request.auth != null;
    }
  }
}
```

### 4. Storage Rules
```
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /media/{allPaths=**} {
      allow read, write: if request.auth != null;
    }
  }
}
```

### 5. Run
Just open the HTML files directly in a browser, or deploy to any static host:

```bash
# Using VS Code Live Server, or:
npx serve .
```

---

## 🌐 Deploy to GitHub Pages

1. Push both HTML files to your repo
2. Go to **Settings → Pages**
3. Set source to `main` branch, `/ (root)`
4. Your site will be live at `https://yourusername.github.io/privon/`

> **Tip:** Set `privon-landing.html` as `index.html` so the landing page is the homepage, and the app is at `/privon-app.html`

---

## 📦 Tech Stack

- **Frontend:** Vanilla HTML, CSS, JavaScript (ES Modules)
- **Backend:** Firebase (Auth, Firestore, Storage)
- **Fonts:** DM Sans, DM Mono (Google Fonts)
- **No build step** — just open and go

---

## 🗂️ Firestore Data Structure

```
users/
  {userId}/
    username, email, friends[], closeFriends[], privacy{}

friendRequests/
  {fromId_toId}/
    fromUserId, fromUsername, toUserId, toUsername, status, createdAt

chats/
  {chatId}/
    messages/
      {messageId}/
        text, mediaUrl, mediaType, senderId, senderName, timestamp

moments/
  {momentId}/
    type, text, bg, mediaUrl, authorId, authorName,
    privacy, allowList[], hideFrom[], quiet,
    views[], reactions{}, createdAt, expiresAt
```

---

## 📄 License

MIT — feel free to use, modify, and build on this.
