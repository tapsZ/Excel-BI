---
title: "Module 1 — Excel Interface & Workbook Basics"
---

# Module 1 — Excel Interface & Workbook Basics

**Day 1 · Morning · ~45 minutes**
*Topics: Getting Started, Creating & Opening Workbooks, Saving & Sharing, Cell Basics, Modifying Columns/Rows/Cells*

> **You need no prior Excel experience for this module.** If you have never opened Excel before, start here.

**📥 Practice files:** [Start workbook](d1_m1_interface_start.xlsx) · [Answer key](d1_m1_interface_answers.xlsx)

---

## Learn it

### What Excel actually is

Excel is a grid. That is the whole idea. Everything else is built on top of it.

- A **cell** is one box in the grid. It holds one thing — a number, some text, or a date.
- A cell has an **address** made of its column letter and row number: `A1`, `C7`, `H24`.
- A **worksheet** (or just "sheet") is one whole grid. Tabs at the bottom switch between them.
- A **workbook** is the file itself (`.xlsx`). One workbook holds many worksheets.

Think of it like a filing cabinet: the workbook is the drawer, each worksheet is a folder, each cell is a line on a page.

### The parts of the screen you must know

| What you see | Where it is | What it does |
|---|---|---|
| **Ribbon** | Across the top | All the buttons, grouped into tabs (Home, Insert, Data…) |
| **Name Box** | Top-left, above column A | Shows the address of the selected cell. Type an address here to jump to it |
| **Formula Bar** | Right of the Name Box | Shows what is *really* in the cell — this matters enormously later |
| **Sheet tabs** | Bottom-left | Switch between worksheets; the `+` adds a new one |
| **Status bar** | Very bottom | Quietly shows Count, Sum and Average of whatever you have selected |

> **The Formula Bar is the single most useful thing on the screen.** A cell can *display* `$1,000.00` while actually *containing* the number `1000`, or a formula. The cell shows you the result; the formula bar shows you the truth. Get into the habit of glancing at it.

### Typing into cells

Click a cell, type, then press:

- **Enter** — confirms and moves down
- **Tab** — confirms and moves right
- **Esc** — cancels, leaves the cell as it was

To change a cell you have already filled, you have three options: type over it (replaces everything), double-click it (edit inside the cell), or click it once and edit in the formula bar.

### Excel guesses what you typed

This trips up every beginner at least once. When you type, Excel silently decides whether it received a number, a date, or text:

- `1000` → a **number**, pushed to the right of the cell
- `29/08/2024` → a **date**, also pushed right (Excel stores dates as numbers)
- `MacBook Air M4` → **text**, pushed to the left

**Alignment is your free clue.** If a number you typed sits on the *left*, Excel thinks it is text, and no formula will add it up. That is the cause of a great many "Excel is broken" moments.

### Moving around without the mouse

| Keys | What happens |
|---|---|
| `Ctrl` + arrow key | Jump to the edge of the current block of data |
| `Ctrl` + `Home` | Back to A1 |
| `Ctrl` + `S` | Save |
| `Ctrl` + `Z` | Undo — your safety net, use it freely |

`Ctrl` + `Down Arrow` on a 10,000-row sheet takes you to the bottom instantly. It is worth learning today.

### Rows, columns and cells

- **Select** a whole row or column by clicking its number or letter.
- **Insert / Delete**: right-click the row number or column letter → Insert / Delete. Everything else shifts to make room.
- **Resize**: drag the line between two headers. Or **double-click** that line to auto-fit the column to its widest entry.
- Seeing `#####` in a cell simply means the column is too narrow. The data is fine — widen the column.

---

## See it — worked example

Open `d1_m1_interface_answers.xlsx` and look at the `Orders` sheet. This is what a tidy basic worksheet looks like, and it is the shape all our data will take:

- **Row 1 holds the headers** — Order ID, Order Date, Customer, and so on. One header per column, no blanks.
- **Every row below is one order.** One record per row, always.
- **Every column holds one kind of thing.** Dates in Order Date, money in Sales, and never the two mixed.
- Columns are wide enough to read.
- The header row is frozen, so it stays put when you scroll.

This layout — *one row per record, one column per field, headers on row 1* — is called a **flat table**. Keep to it. PivotTables need it, Power Query needs it, and Power BI needs it. Nearly every Excel problem people bring to a trainer starts with a spreadsheet that broke this rule.

Now click on cell `B2`. The cell shows `17/01/2026`. Look at the formula bar — it shows a date. Excel understands this is a point in time, not a piece of text, which is why it can later sort it, filter it by year, and chart it over time.

---

## Do it — practice

Open **`d1_m1_interface_start.xlsx`** and follow the `Instructions` sheet inside the workbook. In summary you will:

1. Explore the Name Box and Formula Bar, and jump around with `Ctrl` + arrows
2. Fix two columns that are too narrow to read
3. Delete a duplicate row at the bottom of the list
4. Type in a missing order (`O0008`) and re-sort the list
5. Save the file with `Ctrl` + `S`

**Check yourself:** open `d1_m1_interface_answers.xlsx`. You should have 12 orders, no duplicates, and every column readable.

### If you get stuck

| Problem | Why | Fix |
|---|---|---|
| Cell shows `#####` | Column too narrow | Double-click the line between the column headers |
| Your date shows as `45528` | Excel is showing the underlying number | Home tab → Number dropdown → Short Date |
| Your typed number sits on the left | Excel read it as text | Delete it and retype without spaces or stray characters |
| You deleted the wrong row | It happens to everyone | `Ctrl` + `Z` |

---

## Check your understanding

1. What is the difference between a worksheet and a workbook?
2. A cell displays `$1,000.00`. What might the formula bar show instead?
3. You type a number and it appears on the left of the cell. What has gone wrong, and why does it matter?
4. What does `Ctrl` + `Down Arrow` do, and when would you use it?

---

**Next:** [Module 2 — Formatting Cells & Numbers](../02_formatting_cells_and_numbers/LESSON.md)


---

*© 2026 Global Academy. Prepared for the ZIMASCO (Kwekwe) workshop. Facilitated by Tapiwa Zireva. Licensed to participants for personal learning — not for redistribution, resale, or reuse in other training without permission.*
