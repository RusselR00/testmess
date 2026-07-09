# Mess Admin — Setup & Deployment

Three parts, set up in this order:

```
[Supervisor browser] → GitHub Pages (docs/index.html) → Apps Script Web App → Google Sheet (data)
```

---

## Part 1 — Google Sheet + backend (Apps Script)

1. Create a new Google Sheet (this is your database).
2. **Extensions → Apps Script**. Delete the sample code, paste **all** of `Mess.gs`, Save.
3. Near the top of `Mess.gs`, change the passcode:
   ```js
   var PASSCODE = 'change-me-1234';   // pick your own — supervisors type this to log in
   ```
4. Reload the Google Sheet tab → a **🍽 Mess** menu appears →
   **Mess → Setup / Load initial data** (authorize on first run, then run it again).
   This creates the tabs and loads your 60 members + 160 UAQ employees.
5. Open the **Settings** tab and enter Breakfast / Lunch / Dinner prices (or set them later from the web UI).

## Part 2 — Deploy the backend as a Web App

1. In the Apps Script editor: **Deploy → New deployment**.
2. Gear icon → **Web app**.
3. Set:
   - **Execute as:** *Me*
   - **Who has access:** *Anyone*
4. **Deploy** → authorize → **copy the Web app URL** (ends in `/exec`).

> Whenever you change `Mess.gs` later, do **Deploy → Manage deployments → Edit → Version: New version → Deploy** so the URL keeps working.

## Part 3 — Front-end on GitHub Pages

1. Open `docs/index.html`, set line near the top:
   ```js
   const API_URL = 'PASTE_YOUR_APPS_SCRIPT_EXEC_URL_HERE';   // the /exec URL from Part 2
   ```
2. Push this repo to GitHub.
3. GitHub repo → **Settings → Pages** → Source: **Deploy from a branch**,
   Branch: `main`, Folder: **/docs** → Save.
4. Wait ~1 min → open the Pages URL → enter the **passcode** from Part 1. Done.

---

## Daily use
- **Today** tab — log exceptions (Skip / Add / whole-day leave).
- **Members** tab — add joiners (type the name to pull the Emp ID from the UAQ roster), edit defaults, set a **Leave date** when someone leaves (keeps old reports accurate).
- **Report** tab — pick a month (or a date range) → per-person bill + daily kitchen counts, with **Download CSV**.
- **Prices** tab — update meal prices.

## Notes
- The passcode in `index.html` login must match `PASSCODE` in `Mess.gs`. It is sent with every request; wrong passcode = access denied.
- This is basic protection suitable for an internal tool, not bank-grade security. Anyone with **both** the Pages URL and the passcode can edit data.
- The Google Sheet menu still works independently — you can always fall back to editing tabs directly.
