---
title: "Day 3 — Visualizing, Publishing & Automating Insights"
---

# Day 3 — Visualizing, Publishing & Automating Insights

**Theme: From data to decisions — advanced reporting and storytelling**

> **Prerequisite:** [Day 2](../day2_advanced_excel_and_power_bi/README.md). You need a working Power BI model to present — use [Data Modeling.pbix](../../powerbi_files/day2_advanced_excel_and_power_bi/Data%20Modeling.pbix) or your Day 2 homework file.

---

## The shape of the day

Days 1 and 2 were about making the numbers *correct*. Day 3 is about making them *matter*.

The morning covers visual design and narrative — how to choose charts that don't mislead, and how to turn a page of figures into an argument someone acts on. The afternoon covers getting it in front of people and keeping it fresh without you, then a two-hour capstone on genuinely messy data.

![Power BI workflow: Sources → 1 Power Query, 2 Modeling, 3 DAX, 4 Visuals, 5 Share → Managers.](../../images/powerbi-workflow.svg)

*Today you finish the last two stages — **Visuals** and **Share** — and put the whole pipeline in front of decision-makers.*

---

## Morning Session — visualisation & storytelling

| # | Module | Time | You will learn |
|---|---|---|---|
| 1 | [Visualization Best Practices](01_visualization_best_practices/LESSON.md) | 75 min | Choosing the right chart, the axis-at-zero rule, conditional formatting, custom and AI visuals, slicers, drill-through, **bookmarks**, layout, accessibility |
| 2 | [Storytelling with Data](02_storytelling_with_data/LESSON.md) | 45 min | The three-act structure, **titles that state findings**, giving numbers a comparison, writing the recommendation, presenting |

## Afternoon Session — publishing & capstone

| # | Module | Time | You will learn | Files |
|---|---|---|---|---|
| 3 | [Publishing, Sharing & Automating](03_publishing_and_sharing/LESSON.md) | 60 min | Power BI Service, workspaces vs apps, licensing, **scheduled refresh**, gateways, row-level security, export, Analyze in Excel, Teams & SharePoint | *uses your Day 2 model* |
| 4 | [**Capstone** — Executive Dashboard](04_capstone_executive_dashboard/LESSON.md) | 2 hrs + present | Build a complete dashboard from 23 sheets of raw data, then present it in two minutes | **[raw_tables.xlsx](04_capstone_executive_dashboard/raw_tables.xlsx)** |

**Capstone data:** [raw_tables.xlsx](04_capstone_executive_dashboard/raw_tables.xlsx) — 23 sheets, download this before the afternoon session
**Reference build:** [Project Clean Model.pbix](../../powerbi_files/day3_visualizing_publishing_automating/Project%20Clean%20Model.pbix) — *try Stages 1–3 yourself before opening this*

## Homework

**[Extend, Automate, Publish](homework/HOMEWORK.md)** — about 45 minutes. Add a measure and a slicer-driven visual to your capstone, make it robust with a refresh indicator and RLS, then publish it.

*No new files — you work in your own capstone build.*

---

## Expected outcome

You can design and present a fully functional Power BI dashboard integrating Excel data, demonstrating automation and insight delivery — and explain what should be done about what it shows.

---

## The capstone is the real assessment

`raw_tables.xlsx` is 23 sheets of deliberately unhelpful export data: a duplicated sheet, two order years with mismatched columns, category and subcategory jammed into one field, an inventory table in wide format, a comma-separated SKU list, geography split across three sheets, and text keys that need trimming before anything joins.

Every one of those maps to something taught on Day 2. Nothing in it is unfair — but nothing in it is tidy either, which is the point.

> **Deciding what to leave out is part of the exercise.** A good solution uses eight to ten tables, not 23.

---

## Where the marks are

| Area | Weight |
|---|---|
| Data model | 25% |
| Data cleaning | 20% |
| Measures | 20% |
| Visual design | 15% |
| Storytelling | 15% |
| Publishing | 5% |

Cleaning and modelling together are 45%. A beautiful dashboard on a broken model scores badly here — as it should, because in real life people believe it.

---

**Previous:** [Day 2](../day2_advanced_excel_and_power_bi/README.md) · **Course home:** [README](../../README.md)

---

*© 2026 Global Academy. Prepared for the ZIMASCO (Kwekwe) workshop. Facilitated by Tapiwa Zireva. Licensed to participants for personal learning — not for redistribution, resale, or reuse in other training without permission.*
