# FlyBy Backend — Deployment Guide

## What you're deploying
Three Cloud Functions running on Google's servers:
- `autoCloseSessions` — nightly janitor that closes forgotten sessions
- `semesterReport`    — server-side calculation endpoint (faster than client)
- `notifyTardy`       — fires when a student is marked late (ready for email integration)

---

## Step 1 — Install Node.js (if not already installed)
Download from https://nodejs.org — get the LTS version.

Verify it works:
```bash
node --version   # should print v18.x or higher
npm --version    # should print 9.x or higher
```

---

## Step 2 — Install Firebase CLI
```bash
npm install -g firebase-tools
```

Verify:
```bash
firebase --version   # should print 12.x or higher
```

---

## Step 3 — Login to Firebase
```bash
firebase login
```
This opens a browser window. Sign in with the same Google account you used to create the Firebase project.

---

## Step 4 — Set up your project folder
Create a folder somewhere on your computer called `flyby-backend` and put these files inside it:
```
flyby-backend/
├── firebase.json
├── firestore.rules
└── functions/
    ├── index.js
    └── package.json
```

---

## Step 5 — Link to your Firebase project
Inside the `flyby-backend` folder, run:
```bash
firebase use mobiletracking-a53ac
```
(This tells Firebase CLI which project to deploy to.)

If that doesn't work, run:
```bash
firebase projects:list
```
Find your project ID and run `firebase use YOUR_PROJECT_ID`.

---

## Step 6 — Install function dependencies
```bash
cd functions
npm install
cd ..
```

---

## Step 7 — Enable Cloud Functions billing
Cloud Functions requires a "Blaze" (pay-as-you-go) plan, but the free tier is generous:
- 2 million function invocations/month free
- Your usage will be essentially $0

Go to: https://console.firebase.google.com → your project → Upgrade → Blaze

---

## Step 8 — Deploy everything
From inside the `flyby-backend` folder:
```bash
firebase deploy
```

You'll see output like:
```
✔  functions: autoCloseSessions deployed
✔  functions: semesterReport deployed
✔  functions: notifyTardy deployed
✔  firestore: rules deployed
```

---

## Step 9 — Get your semesterReport URL
After deploy, the CLI prints the function URL. It looks like:
```
https://us-central1-mobiletracking-a53ac.cloudfunctions.net/semesterReport
```

That URL is already hardcoded in your `attendance.html` file as `FUNCTIONS_URL`.
If it's different, update line ~18 of your HTML:
```js
const FUNCTIONS_URL = 'https://us-central1-mobiletracking-a53ac.cloudfunctions.net';
```

---

## That's it!
After deployment, the app automatically:
- Calls the Cloud Function when Semester Report is tapped
- Falls back to client-side calculation if the function is unavailable
- Auto-closes open sessions every night at 11:59 PM CT

---

## To add real email notifications (optional, later)
1. Sign up for SendGrid (free tier: 100 emails/day)
2. Run: `firebase functions:config:set sendgrid.key="YOUR_KEY"`
3. Uncomment the SendGrid block in `functions/index.js`
4. Run `npm install @sendgrid/mail` inside the functions folder
5. `firebase deploy --only functions`
