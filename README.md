
# Weekly habit & task tracker

A self-contained, editable weekly habit + task tracker with real account sign-up/login (Firebase Auth + Firestore), so your data follows you across devices. Pure HTML/CSS/JS — no build step.

## Features
- Account system: sign up / log in with a unique username + password, plus a real "forgot password" email flow
- Distinctive full-screen login screen — animated gradient background, glassmorphism card, floating colour blobs
- Editing is locked behind login — no account, no edits
- Custom avatar: pick an emoji or upload your own photo
- Editable habits, daily tasks, weekly focus/reward/affirmation, monthly goals + monthly reward
- Customizable theme colour (picker + presets) — retints the whole app instantly
- Daily habits/tasks auto-reset each new calendar day; names/goals/theme never change
- Data synced via Firestore per account

## Required: Firestore security rules
Paste this in Firebase console → Firestore → Rules → Publish:
```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /usernames/{username} {
      allow read: if true;
      allow create: if request.auth != null && request.auth.uid == request.resource.data.uid;
      allow update, delete: if false;
    }
    match /users/{userId} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
  }
}
```

## Host on GitHub Pages
1. Create a new repo (any name) and add `index.html` to its root.
2. Go to **Settings → Pages**.
3. Under **Source**, choose **Deploy from a branch** → branch `main` → folder `/ (root)`.
4. Save. Live URL: `https://<your-username>.github.io/<repo-name>/` (takes a minute or two to go live).

## Notes
- Firebase config is already wired to a live project inside `index.html`.
- Login only works when hosted on a real URL or opened as a local file — it won't work inside Claude's own in-chat preview, since that sandboxes outside scripts.
- Usernames map internally to real emails (stored in a `usernames` collection) so username-based login and real password-reset emails can both work without a paid backend.
