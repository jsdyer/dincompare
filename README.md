# DIN Compare

Search Health Canada's Drug Product Database by an eight-digit DIN or brand name and compare up to three official product records. No API key, Worker API handler, database or scheduled collector is needed.

<!-- coolcode:guide -->
Keep leading zeros when entering a DIN. Search a brand name or exact DIN, choose a status and refine returned products by company or class. Add products to compare ingredients and strengths, dosage forms, administration routes, company and status. Open the official record for available monographs. Searches go directly to Health Canada; nothing is saved locally unless you explicitly export a comparison. Matching fields do not establish interchangeability, suitable treatment, dosage equivalence, retail price or pharmacy stock.
<!-- /coolcode:guide -->

## Sources and privacy

The [official DPD API](https://health-products.canada.ca/api/documentation/dpd-documentation-en.html) supports cross-origin browser requests. Queries use `drugproduct` with `din` or `brandname`, plus the selected `status`. Choosing a product retrieves `activeingredient`, `form`, `route` and `status` by drug code. API schema responses can be either arrays or single objects; both are handled. Missing data and partial detail failures remain visible. Each selected record has a retrieval time; source `last_update_date` is not a complete change log. The [DPD information page](https://www.canada.ca/en/health-canada/services/drugs-health-products/drug-products/drug-product-database.html) explains its coverage.

Queries are sent only on submit, directly to `health-products.canada.ca`; detail calls are sequential. A five-minute in-memory cache avoids duplicates. There is no localStorage, URL query state, third-party search logging or persistent saved health history. The standard CoolCode tool counter receives the tool ID only. Exported JSON contains selected product records, not a user's medical history.

## Expand or customize

Edit `config/settings.json`, then mirror its reviewed defaults into `web/settings.json` (the builder copies the config during build). Result display limits, comparison limits, timeouts and cache duration are tunable. The API host is intentionally fixed in `web/model.mjs`; changing configuration cannot turn this into an arbitrary proxy. Visitors may search any valid brand or DIN beyond the example. For ingredient-based advanced search, the tool links to Health Canada's full search interface.

## Test and publish

Run `node --test tests/*.test.mjs`, `python3 -m unittest discover -s tests -v`, `python3 scripts/build.py`. Browser tests intercept only their own test-page API requests with isolated fixtures, verifying single-object responses, missing details, hostile names, comparison and mobile layout. Published code contains no mock records. Live API requests were separately verified for DIN 00326925 and brand search.

Successful main builds deploy with GitHub Pages Actions. No refresh guard or watchdog target is applicable because each search uses the live source. Data is under the [Open Government Licence](https://open.canada.ca/en/open-government-licence-canada).
