# Tally — shared bills for two

A small, private Splitwise-style app for you and your partner. Track shared
expenses, snap receipts, split bills however you like (equal, ratio, exact, or
one-person-pays), record settle-up payments, set recurring bills, and always
see who owes who.

It's a **single HTML file** (`index.html`) — no install, no app store.

---

## Two ways to run it

### A. Try it right now (local mode)
Just **double-click `index.html`** (or open it in any browser). During setup
choose **"Just this device."** Everything is saved on that device. Great for
kicking the tires, and you can move data with **Settings → Export/Import backup**.

> Local mode does **not** sync between your two phones — that's what cloud mode is for.

### B. Sync across both phones (cloud mode) — recommended
This uses **Firebase** (Google's free tier). One-time setup, ~10 minutes.

1. Go to **console.firebase.google.com** → **Add project** (any name, e.g.
   "tally"). You can skip Google Analytics.
2. Inside the project, click the **`</>`** (web) icon → **Add app** → register
   it. Firebase shows you a `firebaseConfig = { … }` block. **Copy that whole
   object.**
3. Left menu → **Build → Firestore Database → Create database** → **Start in
   production mode** → pick a location near you.
4. Left menu → **Build → Authentication → Get started → Sign-in method →
   Anonymous → Enable**.
5. Firestore → **Rules** tab → paste the contents of **`firestore.rules`**
   (included in this folder) → **Publish**.
6. Open `index.html`, choose **"Sync both phones"** in setup, paste the
   `firebaseConfig` object, and pick a **household code** (a long private
   phrase you both share, e.g. `aaron-and-anne-maple-4821`).
7. Send your partner **this same `index.html` file** and the **same household
   code**. They open it, choose "Sync both phones", paste the *same* config +
   code, and pick who they are. Done — edits now sync live. ✨

### Bonus: put it on your phones' home screens
- **iPhone (Safari):** Share → *Add to Home Screen*.
- **Android (Chrome):** ⋮ menu → *Add to Home screen*.

Even better, host it so you both get a real URL: in the same Firebase project
you can run `firebase deploy` with Firebase Hosting, or drop `index.html` on any
static host (Netlify, GitHub Pages, Vercel). Then just bookmark the URL on both
phones.

---

## How splitting & balances work
- Every expense records **who paid** and **each person's share**. Shares always
  add up to the exact total (odd cents handled).
- **Balance** nets everything: expenses you fronted, your shares, and
  settle-up payments. The home screen shows a single "you owe / owed to you".
- **Settle up** logs a payment from one of you to the other and brings the
  balance back toward zero. It keeps a history.
- **Recurring** bills auto-create their expense entries up to today's date,
  each month, on the day you choose. Toggle one off to pause it. Deleting a
  recurring rule leaves the expenses it already created.

## Notes & limits
- **Privacy:** the household code is the key to your data. Anyone with the app
  **and** your exact code can read/write it — so keep it long and private. This
  is a deliberate trade-off to keep setup dead simple for two people.
- **Receipts** are compressed on-device before saving (kept well under
  Firebase's per-record limit). Full images load on demand; a small thumbnail
  shows in lists.
- **Free tier** is comfortably enough for two people (Firestore's free quota is
  tens of thousands of reads/writes per day).
- **Backups:** Settings → Export downloads a full JSON (including receipts).

## Files in this folder
| File | What it is |
|------|-----------|
| `index.html` | The entire app. This is the thing you open/share. |
| `firestore.rules` | Security rules to paste into Firebase (cloud mode only). |
| `README.md` | This guide. |
