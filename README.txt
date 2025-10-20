
# SolutionCraft.ai — Static Site (v1)

## Quick Start
1) Upload the contents of this folder to any static host (Netlify, Vercel, GitHub Pages, S3/CloudFront, etc.).
2) Replace the contact form action in `index.html` if you prefer a different provider.
3) To send assessment results to Google Sheets, deploy a Google Apps Script Web App and paste its URL into `SHEETS_ENDPOINT` in `index.html`.

## Google Sheets Integration
1) In Google Drive, create a new Google Sheet with headers:
   `timestamp, email, q1, q2, q3, q4, q5, q6, q7, q8, total, pct, tier`
2) From Drive, create **Apps Script** > add the following code:

   ```javascript
   const SHEET_NAME = 'Sheet1'; // change if needed
   function doPost(e){
     const ss = SpreadsheetApp.getActiveSpreadsheet().getSheetByName(SHEET_NAME);
     const data = JSON.parse(e.postData.contents);
     ss.appendRow([
       new Date(data.timestamp),
       data.email || '',
       data.q1, data.q2, data.q3, data.q4,
       data.q5, data.q6, data.q7, data.q8,
       data.total, data.pct, data.tier
     ]);
     return ContentService.createTextOutput(JSON.stringify({ok:true}))
                          .setMimeType(ContentService.MimeType.JSON);
   }
   ```

3) **Deploy** > **Manage deployments** > **New deployment** > **Web app**

   - Description: Assessment intake

   - Execute as: **Me**

   - Who has access: **Anyone** (or **Anyone with the link**)

   - Click **Deploy** and copy the Web App URL.

4) Open `index.html` and replace `REPLACE_WITH_YOUR_APPS_SCRIPT_URL` with your Web App URL.

5) Publish changes. New assessment submissions will be POSTed to your sheet as JSON.

## Assets
- `FullLogo.png` is referenced in the hero.
- `logo.png` is included as a spare asset if you want a smaller mark elsewhere.

## Notes
- The assessment runs fully client‑side. If the endpoint is not configured, users still see their score on page.
- To capture UTM parameters or page path with each submission, extend the payload in the `score()` function.
