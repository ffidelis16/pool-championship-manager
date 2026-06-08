# Pool Championship Manager

A single-page tournament manager for a local pool/snooker championship.

It includes a player roster, match calendar, realtime standings, playoff bracket, viewer mode, organizer mode, and Firebase Realtime Database sync.

> This is a public/template version. The active private tournament data, Firebase project, and organizer password are not included.

## What It Does

- Shows the full player list.
- Generates a two-leg round-robin match schedule.
- Lets organizers mark winners for each match.
- Calculates standings automatically.
- Shows semifinal, third-place, and final brackets.
- Syncs match results in realtime with Firebase Realtime Database.
- Allows viewers to follow the tournament without editing.
- Uses a simple password-gated organizer mode for casual/local use.

## Live Editing Model

The app has two modes:

- **Viewer mode:** users can see roster, calendar, matches, standings, and playoffs.
- **Organizer mode:** users enter the configured password and can update match winners.

The password is defined in `index.html`:

```js
const EDIT_PASSWORD = 'change-me-before-deploy';
```

Important: this password is only a UI-level gate. It is useful for small trusted groups, but it is not strong security because frontend code is visible to users.

For production or public tournaments, use Firebase Authentication and write rules that only allow authenticated organizers to update results.

## Firebase Setup

Create a Firebase project and enable **Realtime Database**.

Then replace the placeholder config in `index.html`:

```js
const firebaseConfig = {
  apiKey: "YOUR_FIREBASE_API_KEY",
  authDomain: "YOUR_PROJECT.firebaseapp.com",
  databaseURL: "https://YOUR_PROJECT-default-rtdb.firebaseio.com",
  projectId: "YOUR_PROJECT",
  storageBucket: "YOUR_PROJECT.firebasestorage.app",
  messagingSenderId: "YOUR_SENDER_ID",
  appId: "YOUR_APP_ID"
};
```

The app reads and writes one root key:

```txt
state
```

The saved structure is:

```json
{
  "state": {
    "results": {
      "m0": 1,
      "m1": 4
    },
    "playoffResults": {
      "sf1": "p1",
      "sf2": "p2",
      "final": "fw1",
      "third": "tl2"
    }
  }
}
```

## Security Rules

For a casual private group where everyone who knows the site can edit, open rules work but Firebase will warn you because anyone with the database URL can write to it.

Open rules:

```json
{
  "rules": {
    ".read": true,
    ".write": true
  }
}
```

Recommended rules for a public website with authenticated organizers:

```json
{
  "rules": {
    "state": {
      ".read": true,
      ".write": "auth != null"
    },
    ".read": false,
    ".write": false
  }
}
```

If you use the recommended rules, you must add Firebase Authentication to the frontend before organizers can edit results.

## Customizing The Tournament

Edit these constants in `index.html`.

### Players

```js
const PLAYERS = [
  'Player 1',
  'Player 2'
];
```

### Game Days

```js
const GAME_DAYS = [
  'Wed 08/04',
  'Fri 10/04'
];
```

### Match Schedule

The app uses `ROUNDS` to generate matches. Each match is a pair of player numbers:

```js
[1, 2]
```

Player numbers are 1-based, matching the order in `PLAYERS`.

## Organizer Manual

1. Open the site.
2. Choose **Quero marcar resultados de partidas**.
3. Enter the organizer password.
4. Go to **Partidas**.
5. Click a player name to mark that player as winner.
6. Click the selected winner again to clear the result.
7. Go to **Finais** and click players to mark playoff winners.

Every change is saved to Firebase and appears for all viewers in realtime.

## Deployment

This project is static. You can deploy it with:

- GitHub Pages
- Netlify
- Vercel
- Firebase Hosting
- any static hosting provider

For GitHub Pages:

1. Push the repository to GitHub.
2. Go to **Settings -> Pages**.
3. Select the main branch and root folder.
4. Open the generated Pages URL.

## Files

```txt
index.html   # full app: HTML, CSS, JS
favicon.ico
favicon.png
og-image.png
```

## Notes

This project was extracted from a real active local championship app and converted into a reusable public template. Real tournament data and credentials were intentionally removed.
