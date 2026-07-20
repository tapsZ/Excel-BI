# Day 2 — Advanced Excel & Transitioning to Power BI

**Theme: Finishing Excel, then making the leap — data modelling, integration, and transformation**

> **Prerequisite:** [Day 1 — Excel Foundations](../day1_excel_foundations/README.md), or equivalent comfort with formulas, functions and PivotTables.

---

## The shape of the day

The morning **finishes Excel** — the advanced formula and data techniques a professional analyst actually uses. The afternoon **introduces Power BI**, and you'll find it far less alien than expected: Power Query and the Data Model you meet in the morning *are* the Power BI engine, shipped inside Excel.

By lunchtime you'll have built an automated dashboard in Excel. By the end of the day you'll have built the same kind of thing in Power BI, and understood why the second one scales and the first one doesn't.

---

## Morning Session — finalising Excel

| # | Module | Time | You will learn |
|---|---|---|---|
| 1 | [Advanced Formulas](01_advanced_formulas/LESSON.md) | 75 min | `XLOOKUP`, `INDEX`/`MATCH`, `IFS`, named ranges, Tables, dynamic arrays (`UNIQUE`, `SORT`, `FILTER`) |
| 2 | [Power Query & Power Pivot in Excel](02_excel_power_query_and_power_pivot/LESSON.md) | 90 min | Get & Transform, append vs merge, unpivot, folder combine, the Data Model, first DAX measures, **financial dashboard case study** |

**Files:** Module 1 has `practice_start.xlsx` / `practice_completed.xlsx`. Module 2 works from real files in `source_files/` plus `financial_dashboard_start.xlsx`.

## Afternoon Session — introducing Power BI

| # | Module | Time | You will learn |
|---|---|---|---|
| 3 | [The Power BI Ecosystem & Your First Dashboard](03_first_dashboard/LESSON.md) | 60 min | Desktop/Service/Mobile, connecting data, visuals, slicers, **cross-filtering** |
| 4 | [Connecting to Different Data Sources](04_connection_types/LESSON.md) | 40 min | Excel, CSV, folders, SQL, JSON, delimited text; Import vs DirectQuery |
| 5 | [Data Cleaning & Transformation in Power BI](05_power_query/LESSON.md) | 50 min | The same Power Query, plus column profiling; a full cleaning gauntlet; unpivot |
| 6 | [Data Modelling & Introduction to DAX](06_data_modeling/LESSON.md) | 90 min | Star schemas, relationships, date tables, role-playing dimensions, measures vs columns, **filter context**, time intelligence, KPIs |

**Finished Power BI files** live in `../../powerbi_files/day2_advanced_excel_and_power_bi/`:
`My First Dashboard.pbix` · `Data Cleaning (Power Query).pbix` · `Data Modeling.pbix`

> **Module 6 is the most important module of the day.** Visuals are easy; a correct model is what makes them true.

## Homework

**[Clean, Model, Measure](homework/HOMEWORK.md)** — about 45 minutes. Two messy quarterly exports from four African stores: clean them, append them, merge in lookups, model them, and write five measures. Exact checkpoint figures are given so you can verify every stage yourself.

---

## Expected outcome

You can connect to data wherever it lives, clean it repeatably, model it correctly, and calculate on it with DAX — ready for visualisation on Day 3.

---

## A note if you're on a Mac

Power BI Desktop is **Windows-only**. Options: a Windows VM (Parallels/UTM), Boot Camp, a cloud PC, or the Power BI Service's browser editing (reduced, but workable).

If none is available, the morning session runs perfectly on Mac Excel, and you can still read the afternoon lessons and inspect the provided `.pbix` files on a colleague's machine. The concepts — Power Query, the Data Model, DAX — are identical in both products, so nothing is wasted.

---

## What carries forward

| From Day 1 | Becomes on Day 2 |
|---|---|
| `SUMIF` / `COUNTIF` | `CALCULATE` with filter arguments |
| Absolute vs relative references | **Filter context** in DAX |
| PivotTable four zones | Power BI visual field wells |
| Excel slicers | Power BI slicers — same name, same behaviour |
| One flat table | A star schema of facts and dimensions |
| `XLOOKUP` between tables | A **relationship**, drawn once |

---

**Previous:** [Day 1 — Excel Foundations](../day1_excel_foundations/README.md) · **Next:** [Day 3 — Visualizing, Publishing & Automating](../day3_visualizing_publishing_automating/README.md) · **Course home:** [README](../../README.md)
