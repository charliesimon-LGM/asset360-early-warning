# Asset360 Early Warning — hotel asset management intelligence

**A mobile-friendly, working demonstration built for GitHub Pages.**

The dashboard starts with **three fictional properties** and **12 months of synthetic monthly observations** (November 2025 through October 2026). It is fully operational without a server, API keys, account, or paid hosting. These figures are invented solely to show the workflow; **no actual STR, CoStar, Lodging Econometrics, PMS, or hotel data is included**.

## Publish to GitHub Pages

1. Create a new GitHub repository, e.g. `asset360-early-warning`.
2. Upload `index.html`, `styles.css`, `app.js`, and this `README.md` into the repository **root** (not the parent ZIP folder).
3. In GitHub, open **Settings → Pages**.
4. Under **Build and deployment**, choose **Deploy from a branch**; select `main` and `/(root)`; click **Save**.
5. Open the Pages URL GitHub supplies, typically `https://YOUR-USERNAME.github.io/asset360-early-warning/`.
6. On your phone, open that link in Safari or Chrome. Use **Add to Home Screen** for faster access.

No build process. No dependencies. No configuration needed. The initial screen is a live synthetic demonstration.

## Demo walkthrough

- Start with **Harbor Point Hotel**: a deteriorating property with warning signals in RevPAR market penetration, pace, labor productivity, negative reviews, receivable aging, competitive supply, and preventive maintenance.
- Switch to **Midtown Exchange**: mixed results; forward group and convention pace softening without broad service declines.
- Switch to **Seacliff Resort**: relatively stable, with only isolated pressure.
- Change the **Reporting month** to demonstrate early warning changes before the latest period.
- Visit **Signal monitor** to see actuals, favorable-direction-normalized variances versus budget, LY and underwriting, and trailing 3-month momentum.
- Visit **Trends & benchmarks** for historical data and side-by-side trend lines.
- Visit **Action register** to create, assign, update or close owner actions, including promotion of a flagged signal.
- Visit **Data & sources** to edit numbers in the monthly grid; changes immediately update the dashboard.
- Use **Data & export** to export a full JSON backup, CSV observations, or all scored signals as CSV.

## Data structure and import

One row per property, reporting month, and metric. CSV headers:

```csv
property,month,metric,actual,budget,ly,uw,source,notes
harbor,2026-10,rgi,88,104,103,105,STR report,RGI softness
harbor,2026-10,dirty,8,4,4,4,Review keyword audit,Review descriptions validated
```

**Property IDs:** `harbor`, `midtown`, `seacliff` (or IDs assigned to new properties).

**Metric IDs** are found in `app.js` in the `METRICS` definition and in CSV exports. `ly` means the same reporting period last year; `uw` means approved underwriting target, not necessarily the current budget. Monthly pace comparisons should always use matched as-of dates.

CSV imports update matching property/metric/month rows and add valid new months, leaving other observations intact. A JSON import fully restores a backup, including custom properties and owner actions. New property creation initializes observations to **zero** as missing placeholders; replace them with actual data before using its scores. Zero placeholders are not automatically distinguished from true zeros, so a newly created property should not be evaluated until its observation set is populated.

## Risk engine

- Signals can be **leading** (forward demand, group pace, labor productivity, review keyword incidence, AR aging, supply, PM compliance) or **lagging** (market share, realized RevPAR, profitability).
- Each metric is compared with **budget**, **last year**, and **underwriting**; direction is inverted for metrics where lower is better (labor hours per occupied room, receivables, complaints, supply impact).
- An additional **trailing 3-month versus preceding 3-month** trend helps surface persistence.
- Red/amber thresholds are illustrative, defined for each metric in `METRICS`, and can be tailored to asset type and materiality.
- A weighted, **heuristic** property risk index aggregates individual signals. **This is not a predictive model** or a statistically calibrated probability of loss. The user should validate each alert and override it with contextual judgment as appropriate.
- Financial metrics are illustrative and deliberately simplified. For live application, supplement with revenue mix, departmental profitability, flow-through/flex, wages versus hours, occupied rooms, expense inflation, cash/liquidity, and debt covenant headroom.

## Source licensing and confidentiality

**The app has no live integrations**. It does not log into STR/CoStar/Lodging Econometrics, parse arbitrarily formatted documents, ingest review sites automatically, or call AI. All preloaded content is synthetic. Obtain owner-approved and appropriately licensed report exports. Do not push confidential actual property data to a public GitHub repository: the published code is public, even though browser-entered data is not included in the repository. Data entered in the app is saved in **browser localStorage** on the device only, not synced between devices. Clearing the browser or using a different phone/browser can lose those records. Export JSON backups regularly. Do not regard localStorage as secure long-term storage for proprietary information.

## Next steps for production

1. Replace localStorage with authenticated, centralized per-user/per-property storage (e.g., Supabase/Firebase with row-level security) and access logs.
2. Build standardized import maps for owner P&L, balance sheet/AR, STR, PACE, GSS/QA, and review export formats.
3. Add review keyword normalization (language, context/negation, denominator, repeat-issue clustering), source provenance, and month-over-month alert explanations.
4. Add property-specific thresholds, regional supply threat overlays, operating action accountability, and an Asset360 integration contract.
5. Implement a source freshness check and distinguish missing values from zero; only then deploy against live portfolio data.

## Files

- `index.html` — application shell
- `styles.css` — responsive layouts and print styling
- `app.js` — embedded demo seed data, detection rules, charts, import/export, and actions
- `README.md` — publishing and operating instructions

All code is vanilla JavaScript, HTML, and CSS, with no external network requests.
