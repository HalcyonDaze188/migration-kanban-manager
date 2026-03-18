# E2E Migration Kanban — Deploy Guide

## QUICK FIX: Greyed out options?
If the "Select a parent resource" options are greyed out, it means your work account lacks the "Project Creator" role in Google Cloud.

**The Fix:** Use a personal `@gmail.com` account for Steps 1 and 2. 
You can still deploy the code to your work GitHub later—the database and the website are independent of each other.

---

## STEP 1: Create Firebase Project (~3 mins)

1. Go to **https://console.firebase.google.com**
2. Click **"Add project"** → name it `nca-migration-kanban` → Continue
3. Disable Google Analytics (not needed) → **Create project**
4. Once created, click the **web icon** (`</>`) to add a web app
5. Name it `kanban` → **Register app**
6. Copy the `firebaseConfig` object — you'll need it in Step 3

> [!TIP]
> If you used a personal account, don't worry—this config will work for anyone using the link, even on work devices.

## STEP 2: Enable Realtime Database

1. In Firebase Console, go to **Build → Realtime Database**
   - *Can't find it?* Click **"All products"** at the bottom of the sidebar or use the search bar at the top and type **"Realtime Database"**.
2. Click **"Create Database"**
3. Choose location: **Australia (southeast1)** → Next
4. Select **"Start in test mode"** → Enable

> [!IMPORTANT]
> **To make the database permanent (so it doesn't expire in 30 days):**
> 1. Click the **"Rules"** tab at the top of the Realtime Database page.
> 2. Delete the existing code and paste this in:
> ```json
> {
>   "rules": {
>     ".read": true,
>     ".write": true
>   }
> }
> ```
> 3. Click **"Publish"**. (This allows your internal team to use the tool without needing individual accounts).

## STEP 3: Add Config to `index.html`

Open `index.html` and find the `FIREBASE_CONFIG` block near line 30. Replace the placeholder values:

```javascript
const FIREBASE_CONFIG = {
  apiKey: "AIzaSy...",              // from Firebase Console
  authDomain: "nca-migration-kanban.firebaseapp.com",
  databaseURL: "https://nca-migration-kanban-default-rtdb.firebaseio.com",
  projectId: "nca-migration-kanban",
  storageBucket: "nca-migration-kanban.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abc123"
};
```

## STEP 4: Create GitHub Repo & Deploy

1. Go to **https://github.com/new**
2. Name it `e2e-migration-kanban` → set to **Private** (or Public) → Create
3. Click **"uploading an existing file"** link
4. Drag and drop `index.html` into the upload area → Commit
5. Go to **Settings → Pages**
6. Under "Source", select **Deploy from a branch**
7. Branch: **main**, folder: **/ (root)** → Save
8. Wait ~60 seconds, then your site is live at:
   `https://YOUR-USERNAME.github.io/e2e-migration-kanban/`

## STEP 5: Share with Team

Send the GitHub Pages URL to your team. That's it — they open the link, and:
- All edits sync in real-time via Firebase
- No installs, no terminal, no accounts needed
- Works on desktop and mobile browsers

## Troubleshooting

**"Offline mode" banner showing?** → Firebase config not set. Check Step 3.

**"Disconnected" banner?** → Check Realtime Database rules in Firebase Console (Step 2).

**Data not syncing between people?** → Ensure everyone is using the GitHub Pages URL (not a local file).
