---
title: "Module 2 — Formatting Cells & Numbers"
---

# Module 2 — Formatting Cells & Numbers

**Day 1 · Morning · ~45 minutes**
*Topics: Formatting Cells, Understanding Number Formats, Find & Replace, Checking Spelling, Page Layout and Printing*

---

## Learn it

### The one idea that matters most

**Formatting changes how a value looks. It never changes the value itself.**

Type `1000` into a cell and apply Currency format. The cell now shows `$1,000.00`. But the value in that cell is still exactly `1000` — click it and check the formula bar. Every calculation still uses `1000`.

This matters because it works the other way too: making a cell *look* like a date does not make it a date, and typing a date as text does not become a real date just because you format it. Formatting is paint, not substance.

### Cell formatting (the look)

All on the **Home** tab:

| Group | What you get |
|---|---|
| Font | Bold, italic, size, colour, **Fill Colour** (cell background) |
| Alignment | Left/centre/right, top/middle/bottom, Wrap Text, Merge |
| Borders | Outline and internal gridlines that actually print |

Two habits worth forming immediately:

- **Give the header row a dark fill and white bold text.** It costs five seconds and instantly separates labels from data.
- **Use Wrap Text, not Merge Cells.** Merged cells look tidy and then quietly break sorting, filtering and PivotTables. Avoid merging inside a data table.

### Number formats (the meaning)

This is the important half of the module. The Number dropdown on the Home tab (it says "General" until you change it):

| Format | `1234.5` displays as | Use for |
|---|---|---|
| General | `1234.5` | the default; no opinion |
| Number | `1,234.50` | general quantities |
| Currency | `$1,234.50` | money |
| Accounting | `$ 1,234.50` | money in financial statements — symbols and decimals line up |
| Percentage | `123450.0%` | proportions — **see the warning below** |
| Short Date | `08/05/1903` | dates |
| Text | `1234.5` | forcing Excel to leave something alone |

> **The percentage trap.** Excel stores percentages as decimals: `0.15` *is* 15%. If you type `15` into a cell and then apply Percentage format, you get `1500%`. Either type `15%` directly, or type `0.15` and format it.

**Accounting vs Currency:** in Currency the `$` sits tight against the number; in Accounting it is pinned to the left edge of the cell so a column of figures lines up cleanly. Finance reports use Accounting. Now you know why.

### Find & Replace

`Ctrl` + `F` to find, `Ctrl` + `H` to find *and replace*.

Genuinely useful for cleaning: renaming a category across 5,000 rows, or stripping a stray character out of a column. Click **Options** for the extra controls — matching case, and "Look in: Values vs Formulas" (searching Values finds what you *see*; searching Formulas finds what is *underneath*).

> Always check what is selected before you click **Replace All**. If one cell is selected Excel searches the whole sheet; if a range is selected it only searches that range. And `Ctrl` + `Z` still works if it goes wrong.

### Getting it onto paper

The **Page Layout** tab, plus `Ctrl` + `P` to preview. Four settings solve nearly every printing complaint:

1. **Orientation → Landscape** — wide tables need wide paper
2. **Print Titles → Rows to repeat at top → `$1:$1`** — repeats your header on every printed page
3. **Scale to Fit → Width: 1 page** — stops one stray column spilling onto page 2
4. **Margins → Narrow** — buys you space for free

Always press `Ctrl` + `P` and *look* before printing. The preview is the truth.

---

## See it — worked example

Compare the two workbooks side by side.

`d1_m2_formatting_start.xlsx` holds correct data that is painful to read: everything the same size and colour, sales showing as bare numbers like `1000` and `120`, dates as raw serial-looking values, no borders.

`d1_m2_formatting_answers.xlsx` has exactly the same values, but:

- Header row: dark blue fill, white bold text, centred
- **Sales** column: Currency — `$1,000.00`, `$120.00`. Note that the `.00` and the thousands comma are added by the *format*; the stored value is still just the whole number `1000`
- **Order Date**: Short Date, consistent across every row
- **Quantity**: centred, because short numbers look better centred than ragged
- Borders around the whole table
- "Smart Home" renamed to "Smart Home Device" throughout by Find & Replace
- Landscape orientation with row 1 set to repeat when printed

Click any cell in the Sales column and check the formula bar. It still shows the plain number. The dollar sign lives in the format, not the data.

---

## Do it — practice

Open **`d1_m2_formatting_start.xlsx`** and follow the `Instructions` sheet. You will:

1. Style the header row (bold, dark fill, white text, centred)
2. Format Sales as Currency and Order Date as Short Date
3. Centre the Quantity column
4. Add All Borders to the table
5. Use `Ctrl` + `H` to replace "Smart Home" with "Smart Home Device"
6. Set up landscape printing with a repeating header row

**Check yourself:** compare against `d1_m2_formatting_answers.xlsx`.

### If you get stuck

| Problem | Why | Fix |
|---|---|---|
| Percentages are 100× too big | You typed `15` not `0.15` | Retype as `15%`, or divide by 100 |
| Date shows as a 5-digit number | Format got reset to General | Home → Number dropdown → Short Date |
| Replace All changed too much | A range was selected, or the term was too broad | `Ctrl` + `Z`, then use Options → Match entire cell contents |
| One column keeps printing on its own page | Sheet is wider than the paper | Page Layout → Scale to Fit → Width: 1 page |

---

## Check your understanding

1. You format a cell containing `1000` as Currency. What does `SUM` see when it reads that cell?
2. Why do finance reports tend to use Accounting format rather than Currency?
3. You type `15` and apply Percentage format. What appears, and why?
4. Which Page Layout setting makes your header row appear on every printed page?

---

**Previous:** [Module 1](../01_interface_and_workbook_basics/LESSON.md) · **Next:** [Module 3 — Multiple Worksheets & Navigation](../03_worksheets_and_navigation/LESSON.md)


---

*© 2026 Global Academy. Prepared for the ZIMASCO (Kwekwe) workshop. Facilitated by Tapiwa Zireva. Licensed to participants for personal learning — not for redistribution, resale, or reuse in other training without permission.*
