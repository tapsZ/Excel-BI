---
title: "Day 1 Homework — The Branch Report"
---

# Day 1 Homework — The Branch Report

**Time: about 30 minutes · Do this before Day 2**

**📥 Practice files:** [Start workbook](d1_homework_start.xlsx) · [Answer key](d1_homework_answers.xlsx)

---

## The brief

You have just been sent the order log for one of NovaTech Retail's branches — 12 orders, exported straight from the system and completely unformatted.

Your regional manager wants **a one-page summary they can actually read**, and answers to a few questions, by tomorrow morning.

Everything you need was covered in today's eight modules. Nothing new.

**Open `d1_homework_start.xlsx` and work on the `Branch Orders` sheet.**

---

## Part 1 — Make it presentable *(Modules 1, 2, 7)*

1. Widen every column so nothing is cut off. Columns A and E are too narrow.
2. Format the header row: bold, dark fill, white text, centred.
3. Format **Sales** as Currency and **Order Date** as Short Date.
4. Centre the **Quantity** column.
5. Add All Borders to the table.
6. Freeze the header row so it stays put when you scroll.
7. Add **Data Bars** to the Sales column.

## Part 2 — Add a calculated column *(Module 5)*

8. Column I, **Order Size**, is empty. Fill it with a nested `IF` that labels each order:
   - `Large` if Sales are **$1,000 or more**
   - `Medium` if Sales are **$300 or more**
   - `Small` otherwise

   Copy it all the way down.

## Part 3 — Answer these seven questions *(Modules 5, 6)*

Put your answers somewhere sensible — a clear block to the right of the data, or a separate sheet. **Use formulas, not a calculator.** The point is that the answers update if the data changes.

| # | Question | Hint |
|---|---|---|
| Q1 | What are total sales for the branch? | `SUM` |
| Q2 | How many orders are there? | `COUNT` |
| Q3 | What is the average order value? | `AVERAGE` |
| Q4 | What was the largest single order? | `MAX` |
| Q5 | **Which category earned the most?** | see Part 4 |
| Q6 | How many orders were over $1,000? | `COUNTIF` with `">1000"` |
| Q7 | What were total sales **outside** Zimbabwe? | `SUMIF` with `"<>Zimbabwe"` |

## Part 4 — Summarise and chart it *(Modules 7, 8)*

9. Build a **PivotTable**: `Category` in Rows, `Sales` in Values. Add a second Values field set to **Count** so you can see both money and order volume.
10. Format the values as Currency.
11. Add a **PivotChart** — a column chart — and give it a proper title.
12. Use it to answer **Q5**.

---

## Stretch goals *(optional — only if you have time)*

- Turn the data into an Excel **Table** (`Ctrl` + `T`) and name it `BranchOrders`
- Add a **Slicer** on Country and watch the pivot respond
- Sort by Category then Sales descending, and add **Subtotals** by category
- Set the sheet up to print on one landscape page with the header repeating

---

## Checking your work

Open **`d1_homework_answers.xlsx`**.

The `Answers` sheet has every figure as a **live formula** — click any cell and read the formula bar to see exactly how it was worked out. There is also a category summary table (built with `SUMIF` and `COUNTIF`) showing what your PivotTable should produce, plus the finished chart.

The `Branch Orders` sheet shows one reasonable way to have formatted the data.

**If a number doesn't match:** don't just copy the answer. Click your cell, click the key's cell, and compare the two formulas. The difference between them is the thing you actually need to learn.

### Common slips

| Symptom | Likely cause |
|---|---|
| Q7 returns `0` | The `"<>Zimbabwe"` criteria needs the `<>` **inside** the quotes |
| Everything labelled `Medium` | Your nested `IF` tests 300 before 1000 — reverse them |
| Pivot shows Count where you wanted Sum | Right-click the value → Summarize Values By → Sum |
| Q1 doesn't match the key | Your `SUM` range missed a row — it should cover rows 2 to 13 |
| Answers didn't change when you edited data | The pivot needs a manual **Refresh** |

---

## What you should be able to say afterwards

If you finished this without needing the answer key for most of it, you are ready for Day 2. You can take a raw export, make it readable, calculate on it, summarise it, and answer real questions with it.

**Day 2** picks up right here: `XLOOKUP` and `INDEX`/`MATCH` for joining data across tables, dynamic arrays, Power Query for cleaning at scale, and Power Pivot — then the move across to Power BI.

---

**Back to:** [Day 1 index](../README.md) · **Last module:** [Module 8](../08_pivottables_and_what_if_analysis/LESSON.md)
