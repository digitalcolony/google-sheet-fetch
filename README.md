# Google Sheet Fetch

[![Netlify Status](https://api.netlify.com/api/v1/badges/d3083442-5421-479d-8dcc-64549309a621/deploy-status)](https://app.netlify.com/sites/google-sheet-fetch/deploys)

For this example, I have a Google Sheet of coffee roasters that I've ordered from online.

[Coffee Roasters - Mail Order](https://docs.google.com/spreadsheets/d/1h-oqlqJ_G3UXuDSkdFHuEaCVuOXQOb68y2sduXQRTn4/edit?usp=sharing)

## Create a function to read and return Sheet data

Select Extensions and then App Script. Rename the function to doGet(). Save and deploy as a Web App.

For a detailed tutorial check out: [How to Use Google Sheets as a Simple CMS](https://youtu.be/15y1D1mGKdE?si=e1juk1TUDqtExBs1) by Coding in Public.

```javascript
function doGet() {
	const doc = SpreadsheetApp.getActiveSpreadsheet();
	const sheet = doc.getSheetByName("Ordered");
	const lastRow = sheet.getLastRow();
	const values = sheet.getRange(2, 2, lastRow - 1, 3).getDisplayValues();
	const result = values.map((r) => ({
		roaster: r[0],
		location: r[1],
		state: r[2],
	}));
	return ContentService.createTextOutput(JSON.stringify({ data: result })).setMimeType(
		ContentService.MimeType.JSON
	);
}
```

## Server-Side Render with Netlify

Astro will build a static site by default. This is fine if the data in the spreadsheet rarely or never changes. However, if you want the website to see the latest data on the spreadsheet with each page load, you will want to SSR (server side render). I'm using Netlify to host this project, so they will manage the server requests.

```console
npx astro add netlify
```

Astro has a list of other host adapters [here](https://docs.astro.build/en/guides/server-side-rendering/#adding-an-adapter).
