# Like a Fish Foundry storefront

## Files
- `index.html` — responsive dark storefront page, styled around the HTML5 UP Directive project.
- `products.js` — product data for the cards. It starts empty intentionally, so the page will not invent products or prices.

## Deploy to AWS Amplify
1. Add/replace `index.html` at the root of the same GitHub repository Amplify currently deploys.
2. Add `products.js` beside `index.html` at the same root level.
3. Keep the existing `assets/` directory from HTML5 UP in the repository. The page has its own styling, but it still links to the original template stylesheet.
4. Commit and push to the connected branch. Amplify should rebuild automatically.
5. Test the site on desktop and mobile. The eBay buttons point to the store URL supplied by you.

## Populate product cards now (manual method)
Edit `products.js` and add an object for each actual listing. Each object supports:
- `title`: item title
- `category`: one of the three category names in the example
- `description`: short summary
- `price`: display price such as `$24.99`
- `image`: direct public image URL
- `url`: direct eBay item URL

Use actual current listing data. The page deliberately starts with no fake product cards. If the array is empty, visitors can still click through to the eBay store.

## Automate every six hours (later)
Recommended architecture:
1. An EventBridge Scheduler schedule runs every six hours.
2. A small AWS Lambda function retrieves current listings through an authorized eBay API (or another method permitted by eBay's current terms).
3. The function normalizes title, category, price, image URL, item URL, and availability into a `products.js` or JSON file.
4. The function publishes the updated data to a public S3 object or commits it to the site repository and triggers an Amplify build.

Important:
- Do not put eBay client secrets or refresh tokens in browser JavaScript or GitHub.
- Keep credentials in AWS Secrets Manager or encrypted Lambda environment configuration.
- The eBay store URL alone is not an API credential and does not reliably provide a structured feed for all listing fields.
- Before implementing the sync, confirm the appropriate eBay developer API and permissions for your account, then test pagination, ended/sold items, rate limits, and errors.
- If publishing through S3, the storefront will need to fetch that data file and the S3/CORS/cache settings must be configured. If committing to GitHub, use a narrowly scoped token stored server-side only.

## Notes
- The store and checkout remain on eBay. This page is a storefront and sends buyers to eBay for current listing details and checkout.
- The `safeUrl` check in `index.html` prevents arbitrary non-eBay links from being used as item destinations.
