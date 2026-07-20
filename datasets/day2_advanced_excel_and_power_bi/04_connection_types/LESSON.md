---
title: "Day 2 · Module 4 — Connecting to Different Data Sources"
---

# Day 2 · Module 4 — Connecting to Different Data Sources

**Afternoon · ~40 minutes**
*Topics: files, folders, databases, JSON, delimited text — and the difference between Import and DirectQuery*

**Data:** this folder

---

## Learn it

### Real data is scattered

Sales sit in a database. The finance team sends spreadsheets. The web team's API returns JSON. Someone drops a CSV into a shared folder every month. A system writes a log file.

Power BI connects to all of it and treats it identically once loaded. That is most of its value.

### The connectors you'll actually meet

| Source | Get Data → | Notes |
|---|---|---|
| **Excel** | Excel Workbook | Pick sheets or named tables — **named tables are far more reliable** |
| **CSV / text** | Text/CSV | Check the delimiter and encoding |
| **Folder** | Folder | Combines every file inside — the monthly-file solution |
| **SQL Server** | SQL Server database | Server + database name; Windows or SQL auth |
| **JSON** | JSON | Arrives nested — needs expanding |
| **Web** | Web | A URL, or an API endpoint |
| **SharePoint** | SharePoint folder | Same idea as Folder, in the cloud |

### Import vs DirectQuery

When connecting to a database Power BI asks which mode you want. It matters.

| | **Import** | **DirectQuery** |
|---|---|---|
| Where the data lives | Copied into the `.pbix` | Stays in the source |
| Speed | Very fast | Depends on the source |
| Freshness | As of last refresh | Live |
| Full Power Query & DAX | Yes | Restricted |
| Size limit | Yes (compressed, but real) | None |

**Use Import unless you have a specific reason not to.** It is faster, more capable, and simpler. Reach for DirectQuery only when data is genuinely too large to import or must be up-to-the-second.

### Named tables beat sheet references

When connecting to Excel, the navigator shows both worksheets and named tables (tables show a different icon).

**Always choose the named table.** A sheet reference grabs whatever is in the grid, including stray notes and blank rows. A named table has defined boundaries that grow properly with the data.

This is exactly why Module 1 insisted on naming your tables.

### Combining files from a folder

The most useful connector.

**Get Data → Folder** → browse → **Combine & Transform**.

Power BI reads every file, uses the first as a template, appends the rest, and adds a `Source.Name` column so you know where each row came from.

Next month, drop the new file in the folder and hit Refresh. It is included automatically.

> **Requirement:** every file must share the same structure — same columns, same order. One file with an extra column, or headers on row 3 instead of row 1, will break the combine. Check before you trust it.

### JSON

JSON is nested — objects inside objects, lists inside those. Power Query shows `Record` and `List` values you must **expand** by clicking the arrows on the column header until you reach a flat table.

It takes a few clicks and looks alarming the first time. The pattern is always the same: expand, expand again, then set types.

### Delimited text files

Not every text file is a comma-separated one. Files may use semicolons, tabs, pipes, or — as here — ` ; ` with spaces around it.

Power Query lets you set a **custom delimiter** on import. If your data arrives all in one column, that is the setting to fix. Trim the results afterwards to remove the stray spaces.

### Credentials and privacy

On first connection Power BI asks how to authenticate (Anonymous, Windows, Database, Organizational account) and for a **privacy level** (Public / Organizational / Private).

Privacy levels control whether Power BI is allowed to send data from one source to another while querying. If you hit a "privacy levels" error when merging two sources, that setting is the cause — File → Options → Privacy.

---

## See it — worked example

This folder is deliberately a mess of formats, because that is realistic.

| File | Format | The lesson |
|---|---|---|
| `masterdata_extended_1.xlsx` | Excel | Connect via the **named table**, not the sheet |
| `MonthlySales/` | **12 CSVs**, `Sales_Jan` … `Sales_Dec` | The **folder combine** pattern |
| `sql_server_database.sql` | SQL script | A relational source — see below |
| `product_new_prices.json` | JSON | Nested; needs expanding |
| `events_log.txt` | Delimited text | Custom ` ; ` delimiter |

### `MonthlySales/` — the important one

Twelve files, one per month, identical structure:

```
OrderID,ProductID,CustomerID,Quantity,Sales,OrderDate
100,105,5,1,38,2025-01-01
```

The wrong way: open all twelve, copy, paste, repeat next January.
The right way: connect to the **folder** once. All twelve load. Next year's file joins automatically.

Try both if you have time — the contrast makes the point better than any explanation.

### `product_new_prices.json`

```json
{
  "lastUpdated": "2025-01-15T09:00:00Z",
  "currency": "USD",
  "priceUpdates": [
    { "ProductID": 101, "Product": "Bottle", "OldPrice": 10, "NewPrice": 12, ... }
  ]
}
```

The useful data is inside `priceUpdates`. On import you get a Record; drill into `priceUpdates`, convert the List **To Table**, then expand the columns.

### `events_log.txt`

```
OrderID ; EventType ; EventTimestamp ; Description
1 ; CREATED ; 2025-01-01T09:12:45 ; Order successfully created
```

Semicolons **with spaces around them**. Import with a custom delimiter of `;` then **Trim** every text column, or the values keep their padding and every later join silently fails to match.

That trailing-space trap is one of the most common real-world data bugs, and this file exists to let you meet it in a safe setting.

### `sql_server_database.sql`

A script defining a `SalesDB` with `Customers`, `Employees`, `Products`, `Orders` and `OrdersArchive`.

**You do not need SQL Server installed to learn from this.** Open it in a text editor and read the `CREATE TABLE` statements — you can see the tables, their key columns, and how they relate. That structure is exactly the star-schema thinking of Module 6.

If you *do* have SQL Server available: run the script, then Get Data → SQL Server, choose **Import**, and select the tables. Notice that Power BI often detects the relationships for you from the database's foreign keys.

---

## Do it — practice

Work through as many as time allows. **Exercise 2 is the one that matters most.**

**1. Excel (~5 min)** — Get Data → Excel → `masterdata_extended_1.xlsx`. In the navigator, note which entries are sheets and which are named tables. Load a named table.

**2. Folder — do this one (~10 min)** — Get Data → Folder → `MonthlySales` → **Combine & Transform**. Confirm you have all twelve months in one table. Find the `Source.Name` column. Then set `OrderDate` to a date type and load.

**3. JSON (~10 min)** — Get Data → JSON → `product_new_prices.json`. Drill into `priceUpdates`, convert To Table, expand to a flat table with `ProductID`, `Product`, `OldPrice`, `NewPrice`, `EffectiveDate`.

**4. Text (~10 min)** — Get Data → Text/CSV → `events_log.txt`. Set the delimiter to `;`. **Trim every text column.** Set `EventTimestamp` to Date/Time.

**5. SQL (optional)** — read the script; run and connect if you have an instance.

**6. Compare (~5 min)** — you now have several queries in one file, from four different formats. Open the Model view. They are all just tables now. That uniformity is the point of this module.

### If you get stuck

| Problem | Why | Fix |
|---|---|---|
| Everything in one column | Wrong delimiter | Re-import; set the correct one |
| Folder combine fails | Files differ in structure | Check for an odd file out; remove it and retry |
| JSON shows `Record`/`List` | Not yet expanded | Click the expand arrows; convert Lists To Table |
| Text values won't match on join | Hidden leading/trailing spaces | Transform → Format → **Trim** |
| Credentials keep prompting | Cached wrong credential | File → Options → Data source settings → Clear permissions |
| "Privacy levels" error on merge | Sources on incompatible levels | File → Options → Privacy → adjust |

---

## Check your understanding

1. When would you choose DirectQuery over Import — and why is Import usually right?
2. Why connect to a named table rather than a worksheet?
3. What does From Folder give you that opening twelve files by hand does not, and what must be true of the files?
4. `Record` appears in a JSON column. What next?
5. Two tables won't join even though the IDs look identical. What is the most likely cause?
6. What is `Source.Name` and why is it useful?

---

**Previous:** [Module 3](../03_first_dashboard/LESSON.md) · **Next:** [Module 5 — Power Query Editor in Power BI](../05_power_query/LESSON.md) · **Day 2 index:** [README](../README.md)
