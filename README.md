---
title: "Excel to Power BI — A 3-Day Practical Workshop"
---

# Excel to Power BI — A 3-Day Practical Workshop

**Advanced Analytics and Reporting: from your first cell to a published executive dashboard**

*Workshop prepared for ZIMASCO, Kwekwe.*

📖 **Learners: read this course online at [tapsz.github.io/Excel-BI](https://tapsz.github.io/Excel-BI/)** — no GitHub account needed.

---

## Who this is for

**Anyone.** The course starts at "what is a cell" and finishes with a governed, refreshable Power BI model published to the cloud.

If you already know Excel well, start at Day 2. If you don't, Day 1 will get you there — no prior experience assumed.

---

## How it works

Every module follows the same three-part shape, so you always know where you are:

| Part | What it is |
|---|---|
| **Learn it** | A short plain-language explanation |
| **See it** | A worked example on a finished file — open it and click through |
| **Do it** | A hands-on task, with an answer key to check yourself against |

**Every day ends with a small practical homework** (30–45 minutes) with verifiable answers, so you can confirm the skills stuck before moving on.

A single running dataset — **NovaTech Retail**, a consumer electronics business — carries through the whole course. You watch the same data grow from a plain list into a formatted report, a PivotTable, a Power BI model, and finally an executive dashboard.

---

## The three days

### [Day 1 — Excel Foundations](datasets/day1_excel_foundations/README.md)
*Getting comfortable and productive in Excel before going advanced*

**Morning — the mechanics of a spreadsheet**
1. [Excel Interface & Workbook Basics](datasets/day1_excel_foundations/01_interface_and_workbook_basics/LESSON.md)
2. [Formatting Cells & Numbers](datasets/day1_excel_foundations/02_formatting_cells_and_numbers/LESSON.md)
3. [Multiple Worksheets & Navigation](datasets/day1_excel_foundations/03_worksheets_and_navigation/LESSON.md)
4. [Formulas Fundamentals](datasets/day1_excel_foundations/04_formulas_fundamentals/LESSON.md) — *relative vs absolute references*

**Afternoon — turning a spreadsheet into an answer**

5. [Functions & Working with Data](datasets/day1_excel_foundations/05_functions_and_data_tips/LESSON.md) — `SUMIF`, `COUNTIF`, nested `IF`
6. [Sorting, Filtering & Organizing Data](datasets/day1_excel_foundations/06_sorting_filtering_organizing/LESSON.md)
7. [Tables, Conditional Formatting & Charts](datasets/day1_excel_foundations/07_tables_conditional_formatting_charts/LESSON.md)
8. [PivotTables & What-If Analysis](datasets/day1_excel_foundations/08_pivottables_and_what_if_analysis/LESSON.md)

**Homework:** [The Branch Report](datasets/day1_excel_foundations/homework/HOMEWORK.md)
**Outcome:** confidently navigate Excel, write your own formulas, and summarise a hundred records into a decision-ready report.

---

### [Day 2 — Advanced Excel & Transitioning to Power BI](datasets/day2_advanced_excel_and_power_bi/README.md)
*Finishing Excel, then making the leap — data modelling, integration, and transformation*

**Morning — finalising Excel**
1. [Advanced Formulas](datasets/day2_advanced_excel_and_power_bi/01_advanced_formulas/LESSON.md) — `XLOOKUP`, `INDEX`/`MATCH`, `IFS`, named ranges, dynamic arrays
2. [Power Query & Power Pivot in Excel](datasets/day2_advanced_excel_and_power_bi/02_excel_power_query_and_power_pivot/LESSON.md) — Get & Transform, the Data Model, first DAX, **financial dashboard case study**

**Afternoon — introducing Power BI**

3. [The Power BI Ecosystem & Your First Dashboard](datasets/day2_advanced_excel_and_power_bi/03_first_dashboard/LESSON.md)
4. [Connecting to Different Data Sources](datasets/day2_advanced_excel_and_power_bi/04_connection_types/LESSON.md) — files, folders, SQL, JSON
5. [Data Cleaning & Transformation in Power BI](datasets/day2_advanced_excel_and_power_bi/05_power_query/LESSON.md)
6. [Data Modelling & Introduction to DAX](datasets/day2_advanced_excel_and_power_bi/06_data_modeling/LESSON.md) — star schemas, relationships, **filter context**, time intelligence

**Homework:** [Clean, Model, Measure](datasets/day2_advanced_excel_and_power_bi/homework/HOMEWORK.md)
**Outcome:** connect, clean, model and calculate on data from anywhere — ready for visualisation.

---

### [Day 3 — Visualizing, Publishing & Automating Insights](datasets/day3_visualizing_publishing_automating/README.md)
*From data to decisions — advanced reporting and storytelling*

**Morning — visualisation & storytelling**
1. [Visualization Best Practices](datasets/day3_visualizing_publishing_automating/01_visualization_best_practices/LESSON.md)
2. [Storytelling with Data](datasets/day3_visualizing_publishing_automating/02_storytelling_with_data/LESSON.md)

**Afternoon — publishing & capstone**

3. [Publishing, Sharing & Automating](datasets/day3_visualizing_publishing_automating/03_publishing_and_sharing/LESSON.md) — Service, workspaces, refresh, gateways, RLS
4. [**Capstone** — Build and Present an Executive Dashboard](datasets/day3_visualizing_publishing_automating/04_capstone_executive_dashboard/LESSON.md)

**Homework:** [Extend, Automate, Publish](datasets/day3_visualizing_publishing_automating/homework/HOMEWORK.md)
**Outcome:** design, present and publish a full Power BI dashboard — and say what should be done about what it shows.

---

## Repository layout

```
datasets/
  day1_excel_foundations/              8 modules + homework
    0N_topic/  LESSON.md, dN_mN_<topic>_start.xlsx, dN_mN_<topic>_answers.xlsx
  day2_advanced_excel_and_power_bi/    6 modules + homework
  day3_visualizing_publishing_automating/  4 modules + homework

powerbi_files/
  day2_advanced_excel_and_power_bi/    My First Dashboard · Data Cleaning (Power Query) · Data Modeling
  day3_visualizing_publishing_automating/  Project Clean Model
```

Each practice workbook has an **`Instructions` sheet inside it** with click-by-click steps, so you can work from the file alone if you prefer.

---

## What you need

| Day | Requirement |
|---|---|
| 1 | Excel (any recent version — Windows, Mac or web) |
| 2 morning | Excel with Power Query. **Power Pivot is Windows-only** |
| 2 afternoon, 3 | **Power BI Desktop — Windows only** (free) |
| 3 publishing | A Power BI account; Pro to share |

> **On a Mac?** Day 1 and Day 2's morning run fine. For Power BI Desktop you'll need a Windows VM (Parallels/UTM), Boot Camp, or a cloud PC. Failing that, the lessons and provided `.pbix` files still teach the concepts, and Power Query/DAX are identical in both products — nothing is wasted.
>
> Some newer functions (`XLOOKUP`, `IFS`, `FILTER`, `UNIQUE`, `SORT`) need Microsoft 365 or Excel 2021+. Where they appear, the lessons give an `INDEX`/`MATCH` fallback that works everywhere.

---

## Expected outcomes

By the end you will:

- Demonstrate advanced analytical capability across Excel and Power BI
- Produce dynamic, automated dashboards for financial and operational reporting
- Clean and model data so the totals are actually true
- Communicate findings so they change a decision

---

## The three things worth remembering

Tools change. These don't:

1. **Clean data first.** Everything downstream inherits its problems.
2. **Model it properly.** Facts and dimensions, a star schema, every time.
3. **Say what it means.** A number without a comparison and a recommendation changes nothing.

