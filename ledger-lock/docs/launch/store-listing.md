# Play store listing (en-IN)

Lead with SMS-based money management: Play judges whether SMS is core partly from the listing
([SMS policy](https://support.google.com/googleplay/android-developer/answer/9047303): the description
should "prominently document and promote" the core feature). Avoid "best", "#1", rankings, emoji or prices
in title and icon (Play metadata policy; verify before submission). Every privacy claim below can be checked
by a user in "About this app → App permissions".

## Title (29/30)

```
Ledgerlock: SMS Money Tracker
```

## Short description (75/80)

```
Expense tracker that reads bank SMS on your phone. No internet. No account.
```

## Full description (~2,300/4,000)

```
Ledgerlock turns the transaction SMS your bank already sends you into a clear picture of your money. It reads alerts from your banks, credit cards and wallets, works out what you spent, earned and saved, and does it all on your phone, without internet access.

SMS-BASED MONEY MANAGEMENT
• Reads transaction SMS from 85+ Indian banks, card issuers and wallets: UPI, IMPS, NEFT, debit and credit cards, ATM withdrawals, EMIs, salary credits and refunds.
• Imports up to 12 months of past SMS in one go, then records new alerts as they arrive.
• Shows the SMS behind every transaction, so you can always check the source.

NUMBERS YOU CAN TRUST
• Transfers between your own accounts are not counted as spending or income.
• Credit card bills are not counted twice: the swipe counts, the bill payment does not.
• Net spend, core income and true savings, with a clear "why it's not counted" on every excluded item.
• Anything uncertain goes to a review inbox instead of quietly changing your totals.

SEE WHERE IT GOES
• Spending by category and merchant, month by month.
• Account balances from your SMS, with how old each balance is.
• Card outstanding and limits.
• Budgets and rules to auto-categorise merchants.

PRIVATE BY DESIGN
• No internet permission. Check it yourself under "App permissions": the app cannot send your data anywhere.
• No account, no sign-up, no ads, no analytics, no trackers.
• Only messages from business senders are read. Messages from people are skipped before their text is read. OTPs and offers are ignored.
• Your data is stored in an encrypted database on your phone, protected by your screen lock or fingerprint.
• Backups and exports are encrypted with your password and go only where you choose.
• Erase everything from Settings at any time, or simply uninstall.

LEDGERLOCK PRO
Free: SMS tracking, transfer and card-bill handling, dashboards, 3 budgets and 10 rules.
Pro adds on-device AI help, unlimited budgets and rules, PDF statement import, scheduled encrypted backups, alerts and forecasts. Includes a 14-day free trial of Pro; your data stays yours if you don't buy.

Built for India: ₹ amounts in lakh and crore format, Indian banks and UPI.

Ledgerlock needs SMS permission to work: it is how your transactions get in. Messages never leave your phone.
```

Before publishing, remove any feature not in the release build (Pro features, PDF import, AI assistant,
85+ institutions count, notification parsing). Overstated features are a metadata violation and can also
weaken the SMS declaration.

## Feature bullets (for the website / sideload page)

- Automatic expense tracking from bank, card and wallet SMS
- Transfers and card bills never double-counted
- Net spend, income and true savings at a glance
- No internet permission, no account, no ads
- Encrypted on your phone; encrypted backups you control

## Screenshot captions (6, phone, portrait)

| # | Screen | Caption |
| --- | --- | --- |
| 1 | Home: net spend hero, income, savings | Your spending, built from bank SMS |
| 2 | Transactions list with source badges | Every UPI, card and bank alert, tracked automatically |
| 3 | Transaction detail: both legs + "why it's not counted" | Transfers and card bills never counted twice |
| 4 | Analytics: categories by month | See where your money goes |
| 5 | Accounts: balances with as-of age, card outstanding | All your accounts and cards in one place |
| 6 | Onboarding promise / SMS disclosure screen | No internet. No account. Stays on your phone. |

Use anonymised data only (fake names, accounts, VPAs) in every screenshot and the feature graphic.

## Keywords (India)

Play has no keyword field; ranking comes from the title, short and full description. Use these naturally
in the text above (already mostly covered), not as a list:

expense tracker, SMS expense tracker, money manager, budget planner, spending tracker, UPI expense tracker,
bank SMS tracker, credit card tracker, personal finance, offline expense tracker, daily expense, kharcha,
hisab, salary tracker, EMI tracker, savings tracker, no internet, privacy.

## Category and tags

Category: **Finance** (verify the closest tags in Console). Contains ads: **No**. In-app purchases: **Yes**
(once Play Billing ships; the current code has no billing dependency yet).
