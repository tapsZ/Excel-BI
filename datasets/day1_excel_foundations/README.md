---
title: "Day 1 — Excel Foundations"
---

# Day 1 — Excel Foundations

**Theme: Getting comfortable and productive in Excel before going advanced**

> **No prior Excel experience needed.** This day starts at "what is a cell" and finishes at PivotTables and Goal Seek.

---

## How each module works

Every module follows the same three-part shape, so you always know where you are:

| Part | What it is |
|---|---|
| **Learn it** | A short plain-language explanation (2–3 minutes of reading) |
| **See it** | A worked example on the finished workbook — open it and click through |
| **Do it** | A practice task in the starter workbook, with an answer key to check against |

Each folder contains:

- **`LESSON.md`** — the guide. Start here.
- **a `..._start.xlsx` workbook** — your working file. It has an `Instructions` sheet inside with the exact click-by-click steps.
- **a `..._answers.xlsx` workbook** — the completed answer key. Open it, click cells, read the formula bar.

(Each file is named for its day and module — e.g. `d1_m3_worksheets_start.xlsx` — so downloads never get mixed up.)

All eight modules use the **same dataset** — NovaTech Retail's order log (laptops, smartphones, tablets, accessories and smart home devices sold across Zimbabwe and neighbouring African countries — South Africa, Zambia, Botswana and Kenya). You will watch it grow from a plain list into a summarised, charted, interactive report. That same data continues into Day 2's Power BI work.

---

## Morning Session — the mechanics of a spreadsheet

| # | Module | Time | You will learn | Files |
|---|---|---|---|---|
| 1 | [Excel Interface & Workbook Basics](01_interface_and_workbook_basics/LESSON.md) | 45 min | Cells, sheets, workbooks; the Formula Bar; navigating; inserting, deleting and resizing | [Start](01_interface_and_workbook_basics/d1_m1_interface_start.xlsx) · [Answers](01_interface_and_workbook_basics/d1_m1_interface_answers.xlsx) |
| 2 | [Formatting Cells & Numbers](02_formatting_cells_and_numbers/LESSON.md) | 45 min | Fonts, fills, borders; Currency vs Accounting; the percentage trap; Find & Replace; printing | [Start](02_formatting_cells_and_numbers/d1_m2_formatting_start.xlsx) · [Answers](02_formatting_cells_and_numbers/d1_m2_formatting_answers.xlsx) |
| 3 | [Multiple Worksheets & Navigation](03_worksheets_and_navigation/LESSON.md) | 40 min | Managing sheets; cross-sheet formulas like `=SUM(Laptop!F:F)`; freezing panes | [Start](03_worksheets_and_navigation/d1_m3_worksheets_start.xlsx) · [Answers](03_worksheets_and_navigation/d1_m3_worksheets_answers.xlsx) |
| 4 | [Formulas Fundamentals](04_formulas_fundamentals/LESSON.md) | 60 min | Writing formulas; operators and brackets; the fill handle; **relative vs absolute references** | [Start](04_formulas_fundamentals/d1_m4_formulas_start.xlsx) · [Answers](04_formulas_fundamentals/d1_m4_formulas_answers.xlsx) |

> **Module 4 is the most important module of the day.** Everything afterwards — functions, PivotTables, Power Query, even DAX in Power BI — rests on it. Don't rush it.

## Afternoon Session — turning a spreadsheet into an answer

| # | Module | Time | You will learn | Files |
|---|---|---|---|---|
| 5 | [Functions & Working with Data](05_functions_and_data_tips/LESSON.md) | 60 min | `SUM`, `AVERAGE`, `MAX`, `MIN`, `COUNT`; **`COUNTIF`, `SUMIF`**; nested `IF`; `IFERROR` | [Start](05_functions_and_data_tips/d1_m5_functions_start.xlsx) · [Answers](05_functions_and_data_tips/d1_m5_functions_answers.xlsx) |
| 6 | [Sorting, Filtering & Organizing Data](06_sorting_filtering_organizing/LESSON.md) | 45 min | Multi-level sorting; filters; automatic Subtotals; why `SUBTOTAL` isn't `SUM` | [Start](06_sorting_filtering_organizing/d1_m6_sorting_start.xlsx) · [Answers](06_sorting_filtering_organizing/d1_m6_sorting_answers.xlsx) |
| 7 | [Tables, Conditional Formatting & Charts](07_tables_conditional_formatting_charts/LESSON.md) | 55 min | Excel Tables and structured references; data bars and colour scales; choosing the right chart | [Start](07_tables_conditional_formatting_charts/d1_m7_tables_charts_start.xlsx) · [Answers](07_tables_conditional_formatting_charts/d1_m7_tables_charts_answers.xlsx) |
| 8 | [PivotTables & What-If Analysis](08_pivottables_and_what_if_analysis/LESSON.md) | 60 min | Building PivotTables; slicers; PivotCharts; **Goal Seek** and Scenario Manager | [Start](08_pivottables_and_what_if_analysis/d1_m8_pivottables_start.xlsx) · [Answers](08_pivottables_and_what_if_analysis/d1_m8_pivottables_answers.xlsx) |

## Homework

**[The Branch Report](homework/HOMEWORK.md)** — about 30 minutes. A fresh 35-order dataset to format, calculate, summarise and chart. Answer key included, with every figure as a live formula you can inspect.

**Files:** [d1_homework_start.xlsx](homework/d1_homework_start.xlsx) · [d1_homework_answers.xlsx](homework/d1_homework_answers.xlsx)

---

## Expected outcome

By the end of Day 1 you can confidently navigate Excel, write your own formulas, use conditional functions, and summarise a hundred records into a decision-ready report with a PivotTable.

---

## What carries into Day 2 and 3

Four ideas from today reappear almost unchanged in Power BI. Notice them as you go:

| Today | Where it returns |
|---|---|
| **Flat tables** — one row per record, one column per field | Every Power Query and Power BI model depends on it |
| **Absolute vs relative references** (Module 4) | The same "what is this calculation looking at?" thinking becomes DAX filter context |
| **PivotTables** (Module 8) | Power BI visuals are PivotTables that draw themselves |
| **Slicers** (Module 8) | Identical name, identical behaviour, in Power BI |

---

**Next:** [Day 2 — Advanced Excel & Transitioning to Power BI](../day2_advanced_excel_and_power_bi/README.md) · **Course home:** [README](../../README.md)

---

*© 2026 Tapiwa Zireva. Prepared for the ZIMASCO (Kwekwe) workshop. Licensed to participants for personal learning — not for redistribution, resale, or reuse in other training without permission.*
