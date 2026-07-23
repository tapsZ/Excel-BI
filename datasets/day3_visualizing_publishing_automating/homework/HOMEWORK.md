---
title: "Day 3 Homework — Extend, Automate, Publish"
---

# Day 3 Homework — Extend, Automate, Publish

**Time: about 45 minutes · The final piece of the course**

---

## The brief

Your capstone dashboard works. Now make it something the business can *rely on* — which means one more measure, one more way to interrogate it, and a delivery mechanism that doesn't depend on you being at your desk.

Work in your capstone file from [Module 4](../04_capstone_executive_dashboard/LESSON.md).

---

## Part 1 — Add a new DAX measure *(Day 2 M6)*

Add **one** measure your dashboard doesn't yet have. The first four work straight on the **Dashboard Dash** model; the last two need the **Boss Level** model (`raw_tables.xlsx`), which has the `shipments`, `payments` and `INVOICES` tables. Pick whichever is most useful for your story:

| Measure | DAX | Model |
|---|---|---|
| Rolling 3-month revenue | `CALCULATE([Total Revenue], DATESINPERIOD('Date'[Date], MAX('Date'[Date]), -3, MONTH))` | either |
| Margin vs last year | `[Gross Margin %] - CALCULATE([Gross Margin %], SAMEPERIODLASTYEAR('Date'[Date]))` | either |
| Revenue vs target | `[Total Revenue] - [Revenue Target]` | either |
| Share of category revenue | `DIVIDE([Total Revenue], CALCULATE([Total Revenue], ALL(PRODUCTS[Category])))` | either |
| Days to deliver | `AVERAGEX(shipments, DATEDIFF(shipments[ShipDate], shipments[DeliveryDate], DAY))` | Boss Level only |
| Days to pay | `AVERAGEX(payments, DATEDIFF(RELATED(INVOICES[InvoiceDate]), payments[PayDate], DAY))` | Boss Level only |

**Requirements:**

1. Format it properly (currency, percentage or whole number — not raw decimals)
2. **Verify it by hand** on a filtered subset before you trust it
3. Put it somewhere it earns its place — a card with a comparison, or a column in the matrix

## Part 2 — Add a slicer-driven visual *(Day 3 M1)*

4. Add **one new visual** that answers a question the dashboard currently can't
5. Add a **slicer** that drives it
6. Use **Edit interactions** so the slicer affects what it should and nothing else
7. Title the visual as a **finding**, not a label
8. Set its **alt text**

If the page is now crowded, **remove something**. A dashboard that grows without ever shrinking becomes unreadable.

## Part 3 — Make it robust *(Day 3 M3)*

9. Add a **"Last refreshed"** indicator so readers know how current the data is:
   - In Power Query: `= DateTime.LocalNow()` as a custom column on a one-row query, or
   - A measure: `Last Refresh = "Data as at " & FORMAT(MAX('Date'[Date]), "dd MMM yyyy")`
   - Put it in a small card or text box in a corner
10. Add a **contact** — your name or team — so people know who to ask
11. Apply **row-level security** and test it with **View as role**. On the Dashboard Dash model, filter on `branches[City]` — e.g. `[City] = "Mutare"` for a branch-manager role. (The Boss Level model has a ready-made `security` sheet mapping users to regions if you'd rather map roles to users.)

## Part 4 — Publish and deliver *(Day 3 M3)*

12. Publish to a **proper workspace** — not My Workspace
13. Configure **scheduled refresh**, and **turn on failure notifications**
14. Note which sources would need an **on-premises gateway**, and why
15. Export to **PDF** and check it still reads well as a static document
16. Try **Analyze in Excel** against your published model
17. Create an **app**, or add the report as a **Teams tab**

---

## If you don't have a Power BI account

Parts 1 and 2 work entirely in Desktop. For Parts 3 and 4, write a short **deployment plan** instead — it's the more valuable skill anyway:

- Which workspace, and who gets which role?
- Who are the consumers, and what licence does each need?
- Which sources need a gateway?
- What's the refresh schedule, and who gets the failure alerts?
- What are the RLS rules, and how do you verify the identity match works?
- App, direct share, or Teams — and why?

---

## Self-assessment

Tick these off honestly:

- [ ] The new measure is correct — I verified it by hand, not just by eye
- [ ] It's formatted, not showing raw decimals
- [ ] The new visual answers a real question
- [ ] Interactions are deliberate, not default
- [ ] Titles state findings
- [ ] Readers can see how fresh the data is and who to ask
- [ ] RLS tested **as a Viewer**, not as an Admin
- [ ] Refresh failure notifications are **on**
- [ ] I removed at least one thing to make room
- [ ] I can present the whole thing in two minutes

That last one is the real test. If you can't, the dashboard is still telling too many stories at once.

---

## And that's the course

You started with a grid of empty cells. You can now:

- Clean and structure data that arrives in whatever state it happens to arrive in
- Model it correctly, so the totals are actually true
- Calculate on it with DAX, including over time
- Present it so a busy person understands it in five seconds
- Publish it so it refreshes without you
- And say what should be done about it

The last one is what makes the rest worth having.

---

**Back to:** [Day 3 index](../README.md) · **Course home:** [README](../../../README.md)
