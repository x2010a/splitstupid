# 🧾 Splitbook

A Splitwise-style expense splitter in a single `index.html`. It needs no server, build step or account, and it runs on GitHub Pages.

## Features
- **Groups** with members (trips, flats, lunches)
- **Expenses** with a description, amount, currency, date, category and notes
- **Paid by** one person or several people
- **Split** equally (choose who's included), by exact amounts, by percentages or by shares
- **Multi-currency.** Each expense stays in its own currency (SGD, MYR, IDR, USD, THB, JPY and others). Balances are never converted, so each currency is settled separately. For example: *Alex owes Priya IDR 150,000 (≈ SGD 11.96)*
- An **≈ value in brackets** in your chosen home currency (SGD by default). It's for reference only and uses live rates from open.er-api.com. You can also edit the rates by hand
- **Balances** per person and per currency, with a **Simplify debts** option that gives the fewest payments
- **Settle up** to record payments (PayNow, cash and so on)
- **Stats**: totals by category, by month and per member
- **"You lent / you borrowed"** view once you set "Who are you?"
- Search and filters, plus edit and delete for any entry
- **Share link** to send a copy of the group to friends
- **Backup and restore** as JSON, and **CSV export** for Excel or Sheets
- Works on mobile and supports dark mode

## Deploy to GitHub Pages
1. Create a new repository on GitHub (e.g. `splitbook`).
2. Upload `index.html` (and this README) to the repo root, either with **Add file → Upload files** or with git:
   ```bash
   git init && git add . && git commit -m "Splitbook"
   git branch -M main
   git remote add origin https://github.com/<you>/splitbook.git
   git push -u origin main
   ```
3. In the repo, go to **Settings → Pages**. Under *Build and deployment*, choose **Deploy from a branch**, then **main** / **(root)**, and save.
4. After a minute the app is live at `https://<you>.github.io/splitbook/`.

## How data works
- Everything is saved in your browser's **localStorage**. Nothing is sent to a server, and the GitHub repo never sees your expenses.
- Each person's browser has its **own copy**. To sync, one person keeps the "master" copy and sends a fresh **Share link** after updates. Opening the link replaces the friend's copy of that group.
- Clearing browser data deletes your groups, so use **Backup** now and then.
- If you want everyone to edit the same live data, the next step would be adding a free backend such as Firebase or Supabase.
