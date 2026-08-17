# 🚴‍♂️ Bike Revenue Insights

An interactive Power BI dashboard analyzing bike-share revenue, profitability, and rider behavior across 2025–2026, built on hourly rental data joined with cost/pricing information.

![Bike Revenue Insights Dashboard](Screenshot%202026-08-18%20000831.png)

## 📌 Project Overview

This project explores hourly bike-share rental data to answer a core business question: **"When are we making money?"**

The dashboard combines two years of rider data (2025 & 2026) with cost data to calculate revenue, COGS, and profit at an hourly grain, then visualizes the results through KPIs, trend lines, and seasonal/demographic breakdowns.

## 📊 Dashboard Highlights

- **KPI Cards:** 3M total riders, 0.45 profit margin, $15M sum of revenue, $10.45M sum of profit
- **When Are We Making Money?** — Hourly revenue table showing midday (10–15h) and early evening as the most profitable windows, with Wednesday and Friday outperforming other days
- **KPI Over Time** — Monthly trend of riders, average profit, and average revenue from 2021 to 2022
- **Revenue by Season** — Bar chart comparing revenue across all four seasons
- **Rider Demographic** — Donut chart showing the split between rider types (81.17% vs. 18.83%)

## 🗂️ Repository Contents

| File | Description |
|---|---|
| `bike_share_yr_0.csv` | Hourly bike-share rental data for year 2025 |
| `bike_share_yr_1.csv` | Hourly bike-share rental data for year 2026 |
| `cost_table.csv` | COGS and pricing reference table by year |
| `Screenshot 2026-08-18 000831.png` | Dashboard preview image |
| `README.md` | Project documentation |

## 🧮 Data Modeling (SQL)

A unified analysis view was created by combining both years of rental data and joining it against the cost table to calculate revenue and profit per record:

```sql
CREATE OR ALTER VIEW v_bike_analysis AS

-- Step 1: Combine both years of rental data into one dataset
WITH cte AS (
    SELECT * FROM bike_share_yr_0   -- Year 2025
    UNION ALL
    SELECT * FROM bike_share_yr_1   -- Year 2026
)

-- Step 2: Select final columns and calculate revenue & profit
SELECT
    dteday,
    season,

    -- Convert raw yr flag (0/1) into actual calendar years
    CASE
        WHEN a.yr = 0 THEN 2025
        WHEN a.yr = 1 THEN 2026
        ELSE a.yr
    END AS yr,

    [weekday],
    hr,
    rider_type,
    riders,
    price,
    COGS,

    -- Derived financial metrics
    riders * price                     AS revenue,
    riders * price - COGS * riders     AS profit

FROM cte a
LEFT JOIN cost_table b
    ON a.yr = b.yr;   -- Join cost data by year
```

This view standardizes the year labels, unions both datasets, and derives `revenue` and `profit` at the row level — forming the base table for all dashboard visuals.

## 💡 Recommended Strategy

Based on the profitability and demand patterns uncovered in the analysis, the following pricing strategy is recommended:

1. **Market Analysis** — Conduct further market research to understand customer satisfaction, potential competitive changes, and the overall economic environment. This can guide whether to lean toward the lower or higher end of any suggested price increase.
2. **Segmented Pricing Strategy** — Consider different pricing structures for casual vs. registered users, since they exhibit distinct price sensitivities and usage behaviors.
3. **Monitor and Adjust** — Roll out new price points gradually while continuously tracking customer feedback and sales velocity, allowing the strategy to be fine-tuned without overcommitting.

## 🛠️ Tools & Technologies

- **SQL** (Views, CTEs, Joins) — data modeling and transformation
- **Power BI** — dashboard design and DAX-driven KPIs
- **CSV / Excel** — raw data source files

## 🔗 Links

- **LinkedIn:** [linkedin.com/in/manar-abdeen](https://www.linkedin.com/in/manar-abdeen/)
- **GitHub Repository:** [github.com/ManarAbdeen/Bike-Revenue-Insights](https://github.com/ManarAbdeen/Bike-Revenue-Insights)
