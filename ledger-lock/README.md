# LedgerLock user guide

LedgerLock is a private, offline expense tracker for India. It reads the transaction SMS your bank already sends, works out what you really spent, earned and saved, and keeps everything encrypted on your phone. It has **no internet permission**, no account, no ads and no analytics.

[![Download APK](https://img.shields.io/badge/Download-Ledger%20Lock%20APK-2ea44f?style=for-the-badge&logo=android&logoColor=white)](https://github.com/basiljose1/myapp-downloads/raw/master/ledger-lock/app-release.apk)

- [Getting started](#getting-started)
- [How the numbers work](#how-the-numbers-work)
- [Features](#features)
- [Free and Pro](#free-and-pro)
- [Privacy and security](#privacy-and-security)
- [About these screenshots](#about-these-screenshots)

## Getting started

First launch walks you through six steps. Everything is optional except creating the vault.

<table><tr><td align="center"><sub>Step 2</sub><br><img src="docs/screenshots/onboarding/step2.png" width="150" alt="Onboarding step 2"></td><td align="center"><sub>Step 3</sub><br><img src="docs/screenshots/onboarding/step3.png" width="150" alt="Onboarding step 3"></td><td align="center"><sub>Step 4</sub><br><img src="docs/screenshots/onboarding/step4.png" width="150" alt="Onboarding step 4"></td><td align="center"><sub>Step 5</sub><br><img src="docs/screenshots/onboarding/step5.png" width="150" alt="Onboarding step 5"></td><td align="center"><sub>Step 6</sub><br><img src="docs/screenshots/onboarding/step6.png" width="150" alt="Onboarding step 6"></td><td align="center"><sub>Step 7</sub><br><img src="docs/screenshots/onboarding/step7.png" width="150" alt="Onboarding step 7"></td></tr></table>

1. **Your money data never leaves this phone.** A short promise: no servers, no accounts, no analytics.
2. **Read bank SMS.** Grants SMS access so bank, card and UPI alerts can be found. Messages from people are skipped and never stored. Choose *Not now (manual mode)* to add transactions by hand instead.
3. **Payment app notifications (optional).** Reads Google Pay, PhonePe and Paytm notifications to catch UPI payments your bank texts late or not at all. Every other app is ignored. You can turn it on later in Settings.
4. **Create the vault.** Unlock with your fingerprint, face or screen lock; this creates the encrypted database that holds your data.
5. **Import your history.** Reads up to the last 12 months of messages. You can continue while it finishes in the background.
6. **Confirm your accounts, then pick an app-lock delay.** Mark which accounts and cards are yours (*Mine* / *Not mine*) so money moving between them is not counted as spending, then choose how long the app can sit idle before it locks (1, 2, 5 or 15 minutes).

## How the numbers work

Every message becomes a record that is never changed; only reconciled records reach the ledger. Each transaction has one type, and the headline figures come only from those types:

| Figure | Meaning |
| --- | --- |
| **Net spend** | Expenses + loan EMIs − refunds |
| **Core income** | Salary and other real income |
| **True savings** | Core income − net spend |

These are **never counted** as spending or income, and each shows why on its detail screen:

- Transfers between your own accounts
- Credit card bill payments (the swipe counts; paying the bill does not, so each rupee counts once)
- Investments (tracked separately as "invested")
- Anything uncertain, which waits in the **Review inbox** until you confirm it

Amounts use Indian grouping (lakh and crore), and money is stored in whole paise, never floating point.

## Features

### Home

<table><tr>
<td align="center"><sub>Light</sub><br><img src="docs/screenshots/light/home.png" width="250" alt="Home (light)"></td>
<td align="center"><sub>Dark</sub><br><img src="docs/screenshots/dark/home.png" width="250" alt="Home (dark)"></td>
</tr></table>

The month at a glance: **net spend this month** with a daily spend line against last month, a comparison with the same days last month, and **income, expenses and savings**. *Cash spent* and *Cash received* add a manual entry. Recent transactions sit below. The top bar has **Read new messages**, **Review inbox** and **Lock LedgerLock**. With Pro, a month-end balance forecast appears here too.

### Transactions

<table><tr>
<td align="center"><sub>Light</sub><br><img src="docs/screenshots/light/txns.png" width="250" alt="Transactions list (light)"></td>
<td align="center"><sub>Dark</sub><br><img src="docs/screenshots/dark/txns.png" width="250" alt="Transactions list (dark)"></td>
</tr></table>

Every transaction with its source badge (SMS, notification or manual), account and running balance when the bank reports one. Search by merchant, and filter by period, type, category and tag. A summary strip shows income, expense, credit and net for the current filter.

<table><tr><td align="center"><sub>Light</sub><br><img src="docs/screenshots/light/txn_detail.png" width="200" alt="Transaction detail (light)"></td><td align="center"><sub>Dark</sub><br><img src="docs/screenshots/dark/txn_detail.png" width="200" alt="Transaction detail (dark)"></td><td align="center"><sub>Light</sub><br><img src="docs/screenshots/light/add_cash.png" width="200" alt="Add transaction (light)"></td><td align="center"><sub>Dark</sub><br><img src="docs/screenshots/dark/add_cash.png" width="200" alt="Add transaction (dark)"></td></tr></table>

- **Transaction detail:** amount, merchant, date, source, whether it is *counted* and why, category, *Mark as transfer*, a note, tags and an attached receipt photo. The original SMS is always available as the source.
- **Add transaction:** record cash spent or received with amount, description, category, note, tags and a receipt photo.

### Budgets

<table><tr>
<td align="center"><sub>Light</sub><br><img src="docs/screenshots/light/budgets.png" width="250" alt="Budgets (light)"></td>
<td align="center"><sub>Dark</sub><br><img src="docs/screenshots/dark/budgets.png" width="250" alt="Budgets (dark)"></td>
</tr></table>

A monthly total plus limits per category or merchant, with progress bars, what is left and the previous/next month switcher. **Use smart defaults** proposes limits from your last three months of spending, rounded up to ₹500. Budgets support salary-day cycles, rollover and alerts (at your threshold, at 100%, and when you are on track to overspend).

<table><tr>
<td align="center"><sub>Light</sub><br><img src="docs/screenshots/light/budget_editor.png" width="250" alt="Budget editor (light)"></td>
<td align="center"><sub>Dark</sub><br><img src="docs/screenshots/dark/budget_editor.png" width="250" alt="Budget editor (dark)"></td>
</tr></table>

The editor sets the monthly total and a limit for each category; clear any you do not want.

### Insights tab

<table><tr>
<td align="center"><sub>Light</sub><br><img src="docs/screenshots/light/analytics.png" width="250" alt="Insights tab (light)"></td>
<td align="center"><sub>Dark</sub><br><img src="docs/screenshots/dark/analytics.png" width="250" alt="Insights tab (dark)"></td>
</tr></table>

Shortcuts to **Financial insights** and **Reports**, an **Ask** box for plain-language questions about your money (Pro), and a This month / Quarter / Year view with an all-categories filter showing total spent, transaction count and average per day, plus spending by category and merchant. Tapping a category lists its transactions.

### More

<table><tr>
<td align="center"><sub>Light</sub><br><img src="docs/screenshots/light/more.png" width="250" alt="More menu (light)"></td>
<td align="center"><sub>Dark</sub><br><img src="docs/screenshots/dark/more.png" width="250" alt="More menu (dark)"></td>
</tr></table>

Every screen that is not a tab lives here, grouped by what you are trying to do, with a search box at the top.

#### Accounts

<table><tr><td align="center"><sub>Light</sub><br><img src="docs/screenshots/light/accounts.png" width="200" alt="Accounts (light)"></td><td align="center"><sub>Dark</sub><br><img src="docs/screenshots/dark/accounts.png" width="200" alt="Accounts (dark)"></td><td align="center"><sub>Light</sub><br><img src="docs/screenshots/light/account_detail.png" width="200" alt="Account detail (light)"></td><td align="center"><sub>Dark</sub><br><img src="docs/screenshots/dark/account_detail.png" width="200" alt="Account detail (dark)"></td></tr></table>

Balances and card dues **as each bank last reported them**, with how old each figure is (never added up from transactions). LedgerLock flags **possible missing messages** when the balance changes by more than the transactions explain. On an account you can rename it, import a PDF statement (Pro), and mark it Mine, Household, Business, Hidden or Salary account.

#### Subscriptions and dues

<table><tr>
<td align="center"><sub>Light</sub><br><img src="docs/screenshots/light/subscriptions.png" width="250" alt="Subscriptions and dues (light)"></td>
<td align="center"><sub>Dark</sub><br><img src="docs/screenshots/dark/subscriptions.png" width="250" alt="Subscriptions and dues (dark)"></td>
</tr></table>

Recurring payments, EMIs and card bills, detected automatically or added by hand. Shows the monthly total, a yearly figure and the next 30 days. For each detected item you can *Confirm*, mark *Not recurring*, *I cancelled it*, or *Change type*.

#### Lending and borrowing

<table><tr>
<td align="center"><sub>Light</sub><br><img src="docs/screenshots/light/lending.png" width="250" alt="Lending and borrowing (light)"></td>
<td align="center"><sub>Dark</sub><br><img src="docs/screenshots/dark/lending.png" width="250" alt="Lending and borrowing (dark)"></td>
</tr></table>

Track money you lent or borrowed, with repayments, due dates and reminders. These records stay on the phone and never count as spending or income.

#### Financial calendar

<table><tr>
<td align="center"><sub>Light</sub><br><img src="docs/screenshots/light/calendar.png" width="250" alt="Financial calendar (light)"></td>
<td align="center"><sub>Dark</sub><br><img src="docs/screenshots/dark/calendar.png" width="250" alt="Financial calendar (dark)"></td>
</tr></table>

A month grid marking days with money in, spending, or a due or loan date, with the day's detail and everything due or overdue below.

#### Money in motion

<table><tr>
<td align="center"><sub>Light</sub><br><img src="docs/screenshots/light/motion.png" width="250" alt="Money in motion (light)"></td>
<td align="center"><sub>Dark</sub><br><img src="docs/screenshots/dark/motion.png" width="250" alt="Money in motion (dark)"></td>
</tr></table>

Transfers, card bill payments and investments this month, each shown from-account to to-account, so you can see exactly what was left out of your totals.

#### Financial insights

<table><tr>
<td align="center"><sub>Light</sub><br><img src="docs/screenshots/light/insights.png" width="250" alt="Financial insights (light)"></td>
<td align="center"><sub>Dark</sub><br><img src="docs/screenshots/dark/insights.png" width="250" alt="Financial insights (dark)"></td>
</tr></table>

What changed and why, for **this month, last month, this week or last week**: income, spending and net against the same days of the previous period, your savings rate, the largest category, and plain-language observations such as a category that moved by 20% or more.

#### Reports and export

<table><tr>
<td align="center"><sub>Light</sub><br><img src="docs/screenshots/light/reports.png" width="250" alt="Reports (light)"></td>
<td align="center"><sub>Dark</sub><br><img src="docs/screenshots/dark/reports.png" width="250" alt="Reports (dark)"></td>
</tr></table>

Income, Expense, Cash-flow, Category spending, Transaction, Lending, Borrowing and Repayment history reports, for this month, last month, last 3 months, this year or a custom range, filtered by category, exportable as CSV or PDF.

#### Review inbox

<table><tr>
<td align="center"><sub>Light</sub><br><img src="docs/screenshots/light/review.png" width="250" alt="Review inbox (light)"></td>
<td align="center"><sub>Dark</sub><br><img src="docs/screenshots/dark/review.png" width="250" alt="Review inbox (dark)"></td>
</tr></table>

Transactions and senders waiting for a check. Anything the parser is not sure about is held here and kept out of your totals until you decide; when there is nothing to check it says so.

#### Categories and Manage accounts

<table><tr><td align="center"><sub>Light</sub><br><img src="docs/screenshots/light/categories.png" width="200" alt="Categories (light)"></td><td align="center"><sub>Dark</sub><br><img src="docs/screenshots/dark/categories.png" width="200" alt="Categories (dark)"></td><td align="center"><sub>Light</sub><br><img src="docs/screenshots/light/manage-accounts.png" width="200" alt="Manage accounts (light)"></td><td align="center"><sub>Dark</sub><br><img src="docs/screenshots/dark/manage-accounts.png" width="200" alt="Manage accounts (dark)"></td></tr></table>

- **Categories:** built-in categories (Food, Groceries, Shopping, Travel, Fuel, Bills and Utilities, Subscriptions, Health, Entertainment, Education, Rent and more), set automatically from merchants and your corrections, plus your own.
- **Manage accounts:** ignore an account, set low-balance alerts, update a balance, or delete.

### Settings

<table><tr><td align="center"><sub>Light</sub><br><img src="docs/screenshots/light/settings.png" width="200" alt="Settings (light)"></td><td align="center"><sub>Dark</sub><br><img src="docs/screenshots/dark/settings.png" width="200" alt="Settings (dark)"></td><td align="center"><sub>Light</sub><br><img src="docs/screenshots/light/settings2.png" width="200" alt="Settings (backup and export) (light)"></td><td align="center"><sub>Dark</sub><br><img src="docs/screenshots/dark/settings2.png" width="200" alt="Settings (backup and export) (dark)"></td><td align="center"><sub>Light</sub><br><img src="docs/screenshots/light/settings3.png" width="200" alt="Settings (cash and app lock) (light)"></td><td align="center"><sub>Dark</sub><br><img src="docs/screenshots/dark/settings3.png" width="200" alt="Settings (cash and app lock) (dark)"></td></tr></table>

- **Plan:** your current plan and a link to LedgerLock Pro.
- **Appearance:** System, Light or Dark, optional pure black in dark mode, and wallpaper colours.
- **Notifications:** budget alerts, card dues and subscription reminders, monthly summary, unusual spending (Pro), low balance, and new transactions from bank SMS. All are created on the phone; nothing is sent anywhere.
- **Payment app notifications:** Google Pay, PhonePe and Paytm only.
- **Daily backups (Pro):** encrypted with a passphrase and written while the phone charges; keeps 7 daily, 4 weekly and 6 monthly copies in a folder you choose.
- **Export:** every transaction with type, category, account and source, as encrypted CSV (Pro) or unencrypted CSV.
- **Cash:** optionally move ATM withdrawals into a Cash account instead of counting them as spend.
- **Reprocess history:** read every stored message again with the current rules. Your own edits are kept.
- **App lock:** lock after 1, 2, 5 or 15 minutes idle.
- **Backup and restore:** an encrypted file protected by a passphrase you choose, including receipt photos. Restoring replaces everything on the phone, and the previous data is kept aside so a restore can be undone.
- **Help improve parsing:** report an unparsed message. Personal details are removed first and you can edit the text before sharing.
- **Privacy:** a plain statement that the app has no internet permission.

### App lock

<table><tr>
<td align="center"><sub>Light</sub><br><img src="docs/screenshots/light/lock.png" width="250" alt="Lock screen (light)"></td>
<td align="center"><sub>Dark</sub><br><img src="docs/screenshots/dark/lock.png" width="250" alt="Lock screen (dark)"></td>
</tr></table>

LedgerLock asks for your fingerprint, face or screen lock every time it opens and locks itself when idle. Lock it at any time with the lock icon in the top bar.

### LedgerLock Pro and About

<table><tr><td align="center"><sub>Light</sub><br><img src="docs/screenshots/light/paywall.png" width="200" alt="LedgerLock Pro (light)"></td><td align="center"><sub>Dark</sub><br><img src="docs/screenshots/dark/paywall.png" width="200" alt="LedgerLock Pro (dark)"></td><td align="center"><sub>Light</sub><br><img src="docs/screenshots/light/about.png" width="200" alt="About (light)"></td><td align="center"><sub>Dark</sub><br><img src="docs/screenshots/dark/about.png" width="200" alt="About (dark)"></td></tr></table>

The Pro screen shows your plan, what Pro adds and the offline license activation card. **About** shows the version, the full feature list and how to send feedback.

## Free and Pro

Free stays free. Pro unlocks the extras, and **going back to Free never hides or deletes anything**: every transaction, budget and rule you created stays, and only creating new Pro items is blocked. New installs include a **14-day Pro trial**.

| Feature | Free | Pro |
| --- | :---: | :---: |
| Bank, card and wallet SMS reading | ✓ | ✓ |
| Payment app notification reading | ✓ | ✓ |
| Transfer and card bill exclusion | ✓ | ✓ |
| Dashboards, insights, reports, calendar, lending, subscriptions | ✓ | ✓ |
| Manual encrypted backup and restore | ✓ | ✓ |
| Category limits in a monthly budget | 3 | Unlimited |
| Auto-categorise rules | 10 | Unlimited |
| Ask questions about your money | | ✓ |
| Month-end balance forecast | | ✓ |
| Unusual-spending alerts | | ✓ |
| PDF bank statement import | | ✓ |
| Daily encrypted backups to a folder you choose | | ✓ |
| Encrypted CSV export | | ✓ |
| On-device AI reading of messages the templates miss | | ✓ |

### Activating Pro

Pro uses an offline licence key. No Google Play, account or internet connection is involved.

1. Open **Settings → CardVault Pro**. You will see a six-character device code.
2. Tap **Copy request** or **Share request** and send the text to the developer.
3. Paste the licence key you receive into **License key** and tap **Activate**.

The section then shows *Pro · Activated on this device*, with the expiry date or "never expires". A licence only works on the device it was issued for.

## Privacy and security

- **No internet permission.** You can verify this under *App permissions*; the app cannot send your data anywhere.
- **No account, ads, analytics or trackers.**
- **Encrypted at rest.** The database is encrypted and opened only with your fingerprint, face or screen lock.
- **Only business senders are read.** Messages from people are skipped before their text is read; OTPs and promotions are ignored.
- **Screenshots and recents thumbnails are blocked** in the release app.
- **Backups and exports** are encrypted with your passphrase and go only where you choose.
- **Erase** your data from Settings, or simply uninstall.

## About these screenshots

Screenshots were taken from a debug build on an Android emulator with synthetic bank SMS only (fake names, accounts and UPI handles), in the app's Light and Dark themes. The debug build shows its title as "LedgerLock Dev" on the lock screen and the Pro trial banner, which a release build does not. The release app blocks screenshots, so these were taken with that protection disabled in a temporary local build only.
