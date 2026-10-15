# Analyst Memo — Shopify App Store Insights

**Dashboard:** Shopify App Store Analysis  
**Reporting period:** Latest available data in the supplied `apps.csv` and `reviews.csv` files  
**Audience:** Business stakeholders and analysts

## Executive summary
This Power BI report examines the Shopify App Store catalog, user review activity, app ratings, category distribution, and developer responsiveness. It includes an **Overview** page for a high-level view and a **Trend Analysis** page for time-based comparisons.

## Key insight
**To be validated against the final report:** The Overview screenshot shows approximately 79 apps, 1K reviews, an average rating of 4.22, and a developer reply rate of 23.92%. These are preliminary screenshot readings, not verified calculations from the source files. The review trend appears to increase over the displayed period; validate this observation against the underlying dates and measures before treating it as a business finding.

## Business impact
Category and rating comparisons can identify segments with relatively high or low customer satisfaction. Review trends can reveal periods of changing engagement. Developer reply rate offers an indicator of responsiveness, but should not be interpreted as response quality or causation.

## Recommendations
1. Review categories with lower average ratings and sufficient review volume to prioritize further investigation.
2. Examine months with unusual review changes and check whether the pattern reflects actual customer activity or data coverage.
3. Monitor developer response rates alongside ratings and review volume to assess support engagement.
4. Revalidate all KPI definitions and date relationships before publishing.

## Report pages
### Overview
- KPI cards: Total Apps, Total Reviews, Average Rating, Developer Reply %
- Slicers: Category, Year, Has Free Plan
- Charts: Reviews by Month, Apps by Category, Average Rating by Category
- Page navigator for switching to Trend Analysis

### Trend Analysis
- Monthly review trends and prior-month/prior-year comparisons
- Month-over-month and year-over-year percentage change
- Month-to-date and year-to-date cumulative review measures
- Date/year filters and page navigation

## Data sources and model
- `data/apps.csv`: application metadata and attributes
- `data/reviews.csv`: review records, dates, and developer responses (depending on available columns)
- `dim_date`: continuous calendar date dimension created in Power BI

Expected relationships (adjust field names to actual source schema):
- `apps[app_id]` (one) → `reviews[app_id]` (many)
- `dim_date[Date]` (one) → `reviews[review_date]` (many)

Use a date-only review field if review timestamps include times. Mark `dim_date` as the date table. Use single-direction filtering from dimensions to the review fact table.

## DAX measure definitions
- **Total Apps:** distinct count of app IDs
- **Total Reviews:** count of review rows, provided one row represents one review
- **Average Rating:** arithmetic mean of app ratings (unweighted)
- **Developer Reply %:** share of review rows with a developer reply indicator
- **Reviews Previous Month:** `CALCULATE([Total Reviews], DATEADD(dim_date[Date], -1, MONTH))`
- **Reviews Previous Year:** `CALCULATE([Total Reviews], SAMEPERIODLASTYEAR(dim_date[Date]))`
- **Reviews YTD:** `TOTALYTD([Total Reviews], dim_date[Date])`
- **Reviews MTD:** `TOTALMTD([Total Reviews], dim_date[Date])`
- **Reviews MoM %:** difference from prior month divided by prior month
- **Reviews YoY %:** difference from prior year divided by prior year

DAX formulas must be adapted to actual column names and data types.

## Maintenance and extension
1. Update `apps.csv` and `reviews.csv` using the same schema.
2. Refresh the Power BI model and verify data types, relationships, and date coverage.
3. Check the KPI cards against manual totals and inspect slicer interactions.
4. Validate monthly and yearly comparison measures using sample periods.
5. Update this memo's reporting period, insights, and screenshots after refreshing.

## Limitations
- The report is descriptive; it does not establish causality.
- Ratings may be unweighted by number of reviews.
- Review dates and app launch dates are different concepts and should not be conflated.
- Blank prior-period values are expected where comparison data is absent.
- Any preliminary values inferred from screenshots should be verified against the CSV files.

## Repository contents
```
shopify-app-store-analysis/
├── README.md
├── report.pbix
├── data/
│   ├── apps.csv
│   └── reviews.csv
└── screenshots/
    ├── overview_page.png
    ├── trend_analysis_page.png
    └── model_view.png
```

## How to open
Install Power BI Desktop, download `report.pbix` and the `data/` files, open the report, update data source paths if prompted, and refresh. Use the page navigator to switch between Overview and Trend Analysis.
