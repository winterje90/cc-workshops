# Landing Page Setup Guide

## Step 1: Create the Google Apps Script (form → Google Sheet)

This captures registration form submissions into a Google Sheet.

### 1.1 Create a Google Sheet
1. Go to [Google Sheets](https://sheets.google.com) and create a new spreadsheet
2. Name it **"CC Workshop Registrations"**
3. In Row 1, add these headers: `Timestamp | First Name | Last Name | Email | Company | Role`

### 1.2 Create the Apps Script
1. In the spreadsheet, go to **Extensions → Apps Script**
2. Delete any existing code and paste this:

```javascript
function doPost(e) {
  var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
  var data = JSON.parse(e.postData.contents);

  sheet.appendRow([
    new Date(),
    data.firstName,
    data.lastName,
    data.email,
    data.company,
    data.role
  ]);

  return ContentService
    .createTextOutput(JSON.stringify({ status: 'success' }))
    .setMimeType(ContentService.MimeType.JSON);
}
```

3. Click **Deploy → New deployment**
4. Select type: **Web app**
5. Set:
   - Description: "Workshop Registration"
   - Execute as: **Me**
   - Who has access: **Anyone**
6. Click **Deploy**
7. Copy the **Web app URL** (looks like `https://script.google.com/macros/s/ABC.../exec`)

### 1.3 Update the landing page
In `site/index.html`, find this line:
```
const FORM_URL = 'https://script.google.com/macros/s/PLACEHOLDER/exec';
```
Replace `PLACEHOLDER` with your actual deployment ID from step 1.2.

---

## Step 2: Add Google Calendar Links

Once you have the workshop dates/times, update the "Add to Google Calendar" section in `index.html`. The calendar link format is:

```
https://calendar.google.com/calendar/render?action=TEMPLATE&text=Workshop+Title&dates=20260215T180000Z/20260215T193000Z&details=Description&location=Zoom+Link
```

Replace the dates and details for each workshop.

---

## Step 3: Deploy to Vercel

### Option A: Vercel CLI (recommended)
1. Install: `npm i -g vercel`
2. From the repo root: `cd site && vercel`
3. Follow the prompts (link to your Vercel account, confirm settings)
4. Vercel auto-deploys on every push to `main`

### Option B: Vercel Dashboard
1. Go to [vercel.com](https://vercel.com) and sign up / log in
2. Click **Add New → Project**
3. Import the `winterje90/cc-workshops` GitHub repo
4. Set **Root Directory** to `site`
5. Framework Preset: **Other**
6. Click **Deploy**

Your site will be live at `cc-workshops.vercel.app` (or a custom domain if you configure one).

---

## Step 4: Add Joe & Eric as Collaborators

Once you have their GitHub usernames, run:
```bash
gh api repos/winterje90/cc-workshops/collaborators/USERNAME -X PUT
```

They'll get an email invitation to accept.
