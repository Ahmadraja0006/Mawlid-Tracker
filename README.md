# Eid Milad un Nabi Tracker

A shared, real-time community fund tracker for Eid Milad un Nabi (Mawlid). Friends contribute monthly, the admin records payments and withdrawals, and everyone can see the live numbers and share ideas for the celebration.

Built as a single HTML file with Firebase, with no build step or server.

## Features

- **Monthly contribution tracking** for 13 months (Aug 2026 to Aug 2027), with a paid/unpaid toggle and editable amount per person
- **Opens on the current month** automatically
- **Live stats:** total collected, contributors, this month's progress, amount collected in the selected month, fully paid months
- **Withdrawals:** the admin records money taken out with a note, and a **Balance in hand** figure is kept up to date
- **Overview grid:** see who has paid in which month at a glance
- **Manifesto page:** anyone can share an idea for the event, with no login needed
- **Real-time sync:** changes appear on every open device without refreshing
- **Admin-only editing:** viewers can read everything but cannot change the fund data

## Tech stack

- HTML, CSS and vanilla JavaScript (single file)
- Firebase Realtime Database for storage and live sync
- Firebase Authentication (email and password) for the admin
- Fraunces and Inter fonts from Google Fonts

## Setup

1. Create a project at [console.firebase.google.com](https://console.firebase.google.com).
2. Enable **Realtime Database** and **Authentication** (Email/Password), and create one admin user.
3. Put your own Firebase config in the `firebaseConfig` object inside `index.html`.
4. Set these Realtime Database rules:

```json
{
  "rules": {
    "fund": {
      ".read": true,
      ".write": "auth != null"
    },
    "ideas": {
      ".read": true,
      ".write": true,
      "$ideaId": {
        ".validate": "newData.hasChildren(['text', 'date']) && newData.child('text').isString() && newData.child('text').val().length <= 400 && newData.child('name').isString()"
      }
    }
  }
}
```

5. In **Authentication → Settings → Authorized domains**, add your GitHub Pages domain (`<username>.github.io`).

## Deploy with GitHub Pages

1. Upload the file to this repo as `index.html`.
2. Go to **Settings → Pages**.
3. Choose **Deploy from a branch**, pick `main` and `/ (root)`, then save.
4. Your site will be live at `https://<username>.github.io/<repo-name>/`.

## How permissions work

| Data | Who can read | Who can write |
|------|--------------|---------------|
| `fund` (contributors, payments, withdrawals) | Everyone | Signed-in admin only |
| `ideas` (manifesto) | Everyone | Everyone |

The database rules enforce this. Hiding the buttons in the UI is only for convenience.
