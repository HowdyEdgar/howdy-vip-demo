# vip.howdy.com

Phone-gated VIP events portal. Single static page + assets.

## How it works
- Gate: `POST https://www.howdy.com/api/vip/validate-phone` (Next.js API route in `howdycom/howdy-site-ui`, reads the **VIP List** tab of the "Howdy VIP Page 2026" Google Sheet via env feed URLs; CORS-locked to https://vip.howdy.com).
- Events list: `GET https://www.howdy.com/api/vip/events/moody` reads the **Events** tab. Row shape: `[status, date, formattedDate, artist, genre, soldOut, link, city]`.
- **Request Tickets** button (per row):
  - `soldOut == TRUE` → disabled "Sold out" pill.
  - `link` (column G) present → styled `<a>` opening the Blackthorn event page in a new tab. Registration happens on Blackthorn.
  - no `link` → legacy fallback: posts a row to the sheet's **Requests** tab via the bound Apps Script Web App.
- Rejected phone numbers are logged to the **Rejected Phones** tab via the same Apps Script.

## Updating events
Add rows in the Events tab of the sheet, tick the status checkbox (col A), and paste the Blackthorn event URL in the Link column (col G). Blackthorn event URLs come from the Howdy.com Events hub: https://events.blackthorn.io/en/Dn724Y37/g/Ht8NW0cfKm

## Deploy
The domain is Cloudflare-proxied. Update the hosting origin with the contents of this repo (index.html + assets/).
