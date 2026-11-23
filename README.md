# 🧾 Splitstupid

A Splitwise-style app for splitting expenses with friends. It's a single `index.html`, hosted free on GitHub Pages, with optional **live sync** through Firebase so everyone in a group can add expenses from their own phone.

## Features
- **Groups** with members (trips, flats, lunches). Share an invite link once and everyone stays in sync.
- **Expenses** with a description, amount, currency, date, category and notes.
- **Paid by** one person or several people.
- **Split** equally (choose who's in), by exact amounts, by percentages or by shares.
- **Multi-currency.** Each expense stays in its own currency (SGD, MYR, IDR and more), and each currency is settled separately: *Alex owes Priya IDR 150,000 (≈ SGD 11.96)*. The bracket is for reference only and uses live rates, which you can also edit.
- **Balances** per person and per currency, with **Simplify debts** for the fewest payments.
- **Settle up** to record payments (PayNow, cash and so on).
- **Stats**: totals by category, month and member.
- **You lent / you borrowed** view (each person picks who they are on their own phone).
- Works **offline**. Changes sync when you're back online.
- CSV export and JSON backup and restore. Works on mobile and supports dark mode.

---

## 1. Put it on GitHub Pages
1. Create a new GitHub repository, e.g. `splitstupid`.
2. Upload `index.html`, `firestore.rules` and this `README.md` with **Add file → Upload files**, then commit.
3. Go to **Settings → Pages → Build and deployment**, choose **Deploy from a branch**, then **main** / **(root)**, and save.
4. After a minute or so the app is live at `https://<your-username>.github.io/splitstupid/`.

At this point the app works, but each group stays on one device. Step 2 turns on live sync.

## 2. Turn on live sync (Firebase, about 5 minutes, free)
1. Go to **https://console.firebase.google.com**, sign in with a Google account and click **Create a project**. Name it `splitstupid`. You can turn off Google Analytics.
2. In the left menu open **Build → Firestore Database** and click **Create database**.
   - Location: **asia-southeast1 (Singapore)**. This can't be changed later.
   - Start in **production mode**.
3. Open the **Rules** tab, delete what's there, paste in the contents of `firestore.rules` and click **Publish**.
4. Click the **gear icon → Project settings**. Under **Your apps**, click the **web icon `</>`**, give it a nickname (e.g. `web`) and click **Register app**. Leave "Firebase Hosting" unticked.
5. Firebase shows a `firebaseConfig = { ... }` block. Copy the values into the **FIREBASE SETTINGS** section near the top of the script in `index.html`:
   ```js
   const FIREBASE_CONFIG = {
     apiKey: "AIza...",
     authDomain: "splitstupid-xxxx.firebaseapp.com",
     projectId: "splitstupid-xxxx",
     storageBucket: "splitstupid-xxxx.appspot.com",
     messagingSenderId: "1234567890",
     appId: "1:1234567890:web:abc123",
     measurementId: "G-XXXXXXXXXX"   // only if Analytics is on (optional)
   };
   ```
   You can edit the file directly on GitHub: open `index.html`, click the ✏️ pencil, paste and commit. These values are **not secret**; Firebase expects them to be public.
6. Wait a minute for GitHub Pages to update, then reload the app. New groups now show **● Live sync**.

### Using it
- **New group:** keep "Sync online" ticked. Then go to **Group settings → Invite link** and send it on WhatsApp. That's the only time you need to share anything.
- **Friends** open the link, tap their name, and they're in. Anyone can add, edit or delete expenses, and everyone sees changes within a second or two.
- **Existing local group:** go to **Group settings → Start syncing this group**.
- **Offline**, e.g. on a ferry: keep adding expenses. They upload automatically when you're back online.

### Privacy and security
- Each group's ID is a long random code inside the invite link. Only people with the link can open the group, and nobody can browse other groups. Anyone with the link has full access to that group, so only share it with the group.
- "Who are you" and the ≈ currency are saved on each phone only. They are not uploaded.
- Firebase may email you that your rules allow access without signing in. That's expected for invite-link access. The rules still block listing groups and reject malformed data.
- The free Spark plan covers about 50,000 reads and 20,000 writes per day. A friend group won't come close.

### Analytics (optional)
If `measurementId` is filled in, the app sends usage events to Google Analytics. It records **only actions, never names, amounts or descriptions**:

| Event | Extra info |
|---|---|
| `group_created` | synced or not, default currency, member count |
| `group_joined`, `sync_started`, `demo_opened` | — |
| `expense_added` / `expense_edited` / `expense_deleted` | currency, split mode, category, multiple payers or not |
| `payment_recorded` / `payment_edited` | currency |
| `invite_shared` | whatsapp or copy |
| `tab_viewed` | expenses / balances / stats / settings |
| `csv_exported`, `backup_exported` | — |

Find them in the Firebase console under **Analytics → Realtime** (within minutes) or **Analytics → Events** (after up to 24 hours). Leave `measurementId` empty to turn tracking off.

---

## Without Firebase
If `projectId` is left empty, the app runs in **this device only** mode. You can still send a one-off copy of a group with **Send a copy**, but later changes won't sync.
