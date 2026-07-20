# Module 4 — Formulas Fundamentals

**Day 1 · Morning · ~60 minutes**
*Based on GCFGlobal/LearnFree Excel: Intro to Formulas, Creating More Complex Formulas, Relative and Absolute Cell References*

> **This is the most important module of Day 1.** Everything that follows — functions, PivotTables, Power Query, even DAX in Power BI on Day 2 — is built on the ideas here. Take your time.

---

## Learn it

### Every formula starts with `=`

The equals sign is how you tell Excel "calculate this, don't just store it".

```
=10+5           gives 15
=A1+B1          adds whatever is in A1 and B1
```

Leave off the `=` and Excel stores the text `A1+B1`, which just sits there looking wrong.

### Use references, not numbers

This is the habit that separates a spreadsheet from a calculator.

```
=1000*0.15      works, but is a dead end
=E7*B4          reads the values from cells - alive
```

With the second version, change `B4` and the answer updates. With the first, you have to hunt down and retype every formula containing `0.15`. On a real report that is where errors come from.

**Rule: any number that could ever change belongs in its own cell, not inside a formula.**

### The operators

| Symbol | Does | Example |
|---|---|---|
| `+` `-` | add, subtract | `=A1+B1` |
| `*` `/` | multiply, divide | `=C7*D7` |
| `^` | power | `=A1^2` |
| `()` | force order | `=(A1+B1)*C1` |

Excel follows normal maths precedence: brackets first, then `^`, then `*` and `/`, then `+` and `-`.

This bites people:

```
=A1+B1*C1       multiplies FIRST, then adds
=(A1+B1)*C1     adds first - probably what you meant
```

When in doubt, add brackets. They cost nothing and make your intent obvious to whoever reads the file next.

### Building formulas by clicking

Faster and far less error-prone than typing addresses:

1. Type `=`
2. **Click** the first cell
3. Type the operator
4. **Click** the second cell
5. Press Enter

### Copying formulas down — the fill handle

Write a formula once, apply it to a thousand rows:

- Select the cell. A small square sits at its bottom-right corner.
- **Drag** it down, or **double-click** it to fill down as far as your data goes.

Double-clicking is the one to remember. On a 5,000-row sheet it is instant.

### Relative vs absolute references — the heart of the module

Write `=C7*D7` in `E7` and fill it down. Look at `E8`:

```
=C8*D8
```

Excel adjusted the row numbers. This is a **relative reference**, and it is the default. It is exactly what you want here — every row should multiply *its own* price by *its own* quantity.

Now suppose the discount rate lives in one single cell, `B3`. You write in `F7`:

```
=E7*B3          ← WRONG
```

Fill it down and row 8 becomes `=E8*B4`. But `B4` is the tax rate, not the discount rate. Row 9 looks at `B5`, which is empty — giving you zero. The formula slid off its target.

The fix is to **lock** the reference with dollar signs:

```
=E7*$B$3        ← RIGHT
```

`$B$3` is an **absolute reference**. Copy it anywhere and it still points at `B3`.

**The shortcut: type `B3`, then press `F4`.** Excel adds the dollar signs. (On some Macs you may need `fn` + `F4`.)

### Reading the dollar signs

The `$` locks whatever comes immediately after it.

| Written | Column | Row | Meaning |
|---|---|---|---|
| `B3` | moves | moves | Relative — the default |
| `$B$3` | locked | locked | Absolute — never moves |
| `$B3` | locked | moves | Mixed — stays in column B |
| `B$3` | moves | locked | Mixed — stays on row 3 |

Pressing `F4` repeatedly cycles through all four.

For now, learn the two that matter: **relative for row-by-row calculations, absolute for a single shared rate.** Mixed references become useful when you build a grid, and can wait.

### Errors and what they mean

| Error | Cause | Fix |
|---|---|---|
| `#DIV/0!` | Dividing by zero or an empty cell | Check the divisor; wrap in `IFERROR` (Module 5) |
| `#VALUE!` | Doing maths on text | A cell contains text that looks like a number |
| `#REF!` | A referenced cell was deleted | Rewrite the reference |
| `#NAME?` | Misspelled function name | Check the spelling |
| `#####` | Column too narrow | Widen it — not really an error |

Errors are Excel telling you precisely where the problem is. Read them rather than deleting them.

---

## See it — worked example

Open `practice_completed.xlsx`, sheet `Invoice Calc`.

At the top, in the yellow box, sit two input cells: **Discount Rate** in `B3` (10%) and **Tax Rate** in `B4` (15%). Every calculation below reads from these two cells. They are not typed into any formula.

Click through one row — row 7 — and read the formula bar each time:

| Cell | Formula | What it does |
|---|---|---|
| `E7` | `=C7*D7` | Unit price × quantity — **both relative** |
| `F7` | `=E7*$B$3` | Line total × discount rate — **`E7` relative, `$B$3` absolute** |
| `G7` | `=E7-F7` | Line total less the discount |
| `H7` | `=G7*$B$4` | Net × tax rate — absolute again |
| `I7` | `=G7+H7` | Net plus tax = total due |

Now click `F8` and `F20`. The first part changes with the row (`E8`, `E20`) while `$B$3` stays exactly as it is in every single row. That is relative and absolute working together — which is the whole point of the module.

**Now do the thing that proves it works.** Change `B3` from `10%` to `25%`. Every discount, every net, every tax figure, and the totals row all recalculate at once. Change it back.

That is a *model*, not a spreadsheet. One number in, and the whole thing responds. Build every workbook this way and you will rarely have to hunt for a hard-coded value again.

---

## Do it — practice

Open **`practice_start.xlsx`**. Columns E to I are empty. Follow the `Instructions` sheet to fill them in:

1. `E7` → `=C7*D7`, then **double-click the fill handle** to copy it down
2. `F7` → `=E7*$B$3` (type `B3` then press `F4`), copy down
3. `G7` → `=E7-F7`, copy down
4. `H7` → `=G7*$B$4`, copy down
5. `I7` → `=G7+H7`, copy down
6. Totals row: `E27` → `=SUM(E7:E26)`, copy across to `I27`
7. **Test it:** change `B3` to 25% and watch everything move

**Check yourself:** set the discount back to 10% and tax to 15%, then compare your Total Due column against `practice_completed.xlsx`.

### If you get stuck

| Problem | Why | Fix |
|---|---|---|
| Column F is right at the top, wrong below | You used `B3` instead of `$B$3` | Fix the top formula, copy it down again |
| Everything shows `0` further down | Same cause — the reference slid onto empty cells | As above |
| Formula shows as text | Missing `=` | Add it |
| `#VALUE!` | A price or quantity is stored as text | Check that number's alignment |
| Fill handle won't double-click | Gap in the adjacent column | Drag it down manually instead |

---

## Check your understanding

1. What does `=A1+B1*C1` calculate, and how would you make it add first?
2. Explain in one sentence why `$B$3` is needed instead of `B3` when referring to a shared rate.
3. You copy `=E7*$B$3` from `F7` to `F20`. What does it become?
4. What key adds dollar signs to a reference?
5. Why is putting a tax rate in its own cell better than typing `0.15` into every formula?

---

**Previous:** [Module 3](../03_worksheets_and_navigation/LESSON.md) · **Next:** [Module 5 — Functions & Working with Data](../05_functions_and_data_tips/LESSON.md)

*End of the morning session. After the break we move from writing your own arithmetic to using Excel's built-in functions.*
