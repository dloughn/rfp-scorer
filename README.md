# RFP Scorer — Tactical Marine Solutions

A single-file HTML app for scoring RFP evaluation spreadsheets question by question. Built for the Baffinland Steensby Inlet icebreaker procurement process.

**Live URL (iPad / Safari):** https://dloughn.github.io/rfp-scorer/

---

## What It Does

- Loads an `.xlsx` scoring spreadsheet
- Presents each question one at a time with Requirement and Deliverable text
- Lets you select a score (1–4 in 0.5 increments) and add an optional comment
- Auto-saves progress to browser localStorage after every question
- Offers to resume where you left off if you close and reopen
- Writes scores back into the DL column of the original spreadsheet on download
- Comments are written into a new column after the last used column on each sheet

---

## Files

| File | Purpose |
|------|---------|
| `rfp-scorer.html` | Source file — edit this one |
| `rfp-scorer-bundled.html` | Self-contained version with xlsx library inlined — use this on iPad |
| `bundle-scorer.sh` | Shell script to bundle the xlsx library into the HTML |

> `rfp-scorer-bundled.html` is generated locally and not tracked in Git. GitHub Pages serves `index.html` which is kept up to date manually.

---

## How to Use

### On Mac (Chrome)
1. Open `rfp-scorer-bundled.html` directly in Chrome
2. Click **Browse Files** and select your `.xlsx` scoring spreadsheet
3. Pick a sheet from the list
4. If the sheet has multiple proponent columns under DL (e.g. Horizon / Federal / TSS), pick which one you're scoring
5. Score each question — click a score button and optionally add a comment
6. Hit **Next →** to advance (progress auto-saves after every question)
7. Use **⬇ save** at any time to download a partial file mid-session
8. Hit **Finish ✓** on the last question to get to the download screen
9. Click **⬇ Download Scored Spreadsheet** — saves a new file, does not overwrite the original

### On iPad (Safari)
1. Go to https://dloughn.github.io/rfp-scorer/ in Safari
2. Tap **Browse Files** — navigate to your spreadsheet in Files / iCloud Drive / OneDrive
3. Same workflow as above
4. Download saves to iPad Downloads folder

---

## Resuming a Session

Progress is auto-saved to browser localStorage after every score. If you close the tab mid-session:

1. Reopen the app (same browser, same device)
2. Load the same spreadsheet
3. A **Resume session?** banner appears at the top showing sheet name, questions scored, and time last saved
4. Tap **Resume** to continue where you left off, or **Start Fresh** to discard and restart

> ⚠️ localStorage is device-specific — sessions do not sync between iPad and Mac. Pick one device per scoring session.

> ⚠️ Do not clear Chrome/Safari browsing data mid-session or you will lose unsaved progress. Use **⬇ save** regularly as a backup.

---

## Updating the App

When the source file (`rfp-scorer.html`) is updated:

### Step 1 — Download the new source file
Save the updated `rfp-scorer.html` to your Downloads folder, replacing the old one.

### Step 2 — Download the xlsx library (first time only)
Go to this URL in your browser and save the file (**File → Save Page As → Page Source**):
```
https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js
```
Save it to your Downloads folder as `xlsx.full.min.js`. You only need to do this once — keep it in Downloads permanently.

### Step 3 — Run the bundle script (Mac Studio via SSH)
```bash
~/Projects/bundle-scorer.sh
```
This produces `rfp-scorer-bundled.html` in your Downloads folder.

### Step 4 — Update GitHub Pages
```bash
cp ~/Downloads/rfp-scorer-bundled.html ~/Projects/rfp-scorer/index.html
cd ~/Projects/rfp-scorer
git add index.html
git commit -m "Update scorer"
git push origin main
```
GitHub Pages updates automatically within ~1 minute.

---

## Bundle Script

Located at `~/Projects/bundle-scorer.sh`. Reads:
- `~/Downloads/rfp-scorer.html` — source file
- `~/Downloads/xlsx.full.min.js` — xlsx library

Produces:
- `~/Downloads/rfp-scorer-bundled.html` — self-contained file safe for offline use

---

## Spreadsheet Structure

The app reads `.xlsx` files and looks for a `DL` header to identify the scoring column. It supports:

- **Single DL column** — goes straight to scoring
- **Multiple proponent columns under DL** (e.g. IB Gen sheet with Horizon / Federal / TSS) — shows a picker first

To add a new proponent column, simply add their name in the sub-row under the DL header in the spreadsheet. The picker will include them automatically.

Score output goes into the DL column. Comments go into the first empty column after the last used column on that sheet, with a `DL Comments` header.

---

## GitHub Setup

- **Repo:** https://github.com/dloughn/rfp-scorer (public — required for GitHub Pages)
- **GitHub Pages:** enabled on `main` branch, root folder
- **No tokens or environment variables required** — this app runs entirely in the browser

---

## Development Notes

- No build process — pure HTML/CSS/JS in a single file
- xlsx library (v0.18.5) is bundled inline for offline/iPad use
- localStorage key format: `rfp_scorer__{filename}__{sheetname}`
- Nothing is uploaded to any server — all data stays in the browser

---

*Tactical Marine Solutions — Steensby Inlet IB Procurement*
