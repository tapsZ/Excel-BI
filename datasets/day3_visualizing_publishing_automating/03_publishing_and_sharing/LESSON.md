---
title: "Day 3 · Module 3 — Publishing, Sharing & Automating"
---

# Day 3 · Module 3 — Publishing, Sharing & Automating

**Afternoon · ~60 minutes**
*Topics: Power BI Service, workspaces, apps, permissions, scheduled refresh, gateways, row-level security, export and embed, and integration with Excel, Teams and SharePoint*

---

## Learn it

### From your machine to everyone else's

A `.pbix` on your laptop helps nobody. Publishing is what turns your work into something the business uses.

**Home → Publish** → choose a workspace. That is the whole action. Everything below is about doing it *properly*.

### Where things live in the Service

| Item | What it is |
|---|---|
| **Semantic model** (formerly dataset) | Your data + model + measures |
| **Report** | The pages of visuals, built on a model |
| **Dashboard** | A single-page pinned summary — Service-only, distinct from a report |
| **Workspace** | A container for the above |
| **App** | A polished, packaged workspace published to consumers |

> **"Report" and "dashboard" mean specific, different things in Power BI.** A report is multi-page and interactive, built in Desktop. A dashboard is a single page of tiles pinned from one or more reports, and exists only in the Service. People use the words loosely; the Service does not.

### Workspaces

**My Workspace** is personal. Nothing shared with a team should live there — when that person leaves or is on holiday, the report goes with them. It is the most common Power BI governance mistake.

Create a proper workspace instead, with roles:

| Role | Can |
|---|---|
| **Admin** | Everything, including delete the workspace and manage access |
| **Member** | Publish, edit, share |
| **Contributor** | Publish and edit; cannot share |
| **Viewer** | Read only |

**Give people the least access that lets them do their job.** Most consumers should be Viewers of an app, not members of a workspace.

### Apps

A workspace is a workbench; an **app** is the finished product.

Create App from the workspace, choose which reports to include, set navigation and branding, and publish to named people, groups, or the whole organisation.

**Apps are the right way to distribute to a wide audience.** Consumers get a clean, curated experience and can't see your works-in-progress.

### Licensing — the part that surprises people

| Licence | You can |
|---|---|
| **Free** | Build in Desktop; publish to My Workspace only; **cannot share** |
| **Pro** | Publish, share and consume shared content — the standard |
| **Premium Per User (PPU)** | Pro plus larger models, more frequent refresh, extra features |
| **Fabric / Premium capacity** | Organisation-wide capacity; **free users can view** content in it |

**To share a report, both you and the recipient normally need Pro** — unless the workspace sits on Premium/Fabric capacity, in which case free users can view.

Check this before promising a report to fifty people. It is a frequent and awkward surprise.

### Refresh

Published data is a **snapshot** as of the last refresh. It does not update itself until you tell it to.

Semantic model → **Settings** → **Scheduled refresh**: set a time zone, add up to 8 daily refreshes on Pro (48 on PPU/Premium), and — importantly — turn on **failure notifications**.

> **Turn on refresh failure emails.** A silently broken refresh means people making decisions on stale numbers while the report looks perfectly healthy. It's the most damaging failure mode in BI, and it's one checkbox.

### Gateways

Cloud sources (SharePoint Online, SQL Azure, web) refresh without help. **On-premises sources — a file on your network, a local SQL Server — need an On-premises Data Gateway.**

Install it on a machine that stays on, sign in, and map the data source's credentials. Then scheduled refresh can reach it.

**This is the single most common reason a refresh works in Desktop and fails in the Service.** Desktop reaches your local files; the Service cannot, without a gateway.

Standard mode (shared, for teams) is what you want; Personal mode is for individual use only.

### Row-level security

Restrict what each person sees *within* one report.

**In Desktop:** Modeling → **Manage roles** → create a role with a DAX filter:

```
[Region] = "East Africa"
```

Dynamic, using the signed-in user's identity — far more maintainable:

```
[UserEmail] = USERPRINCIPALNAME()
```

Test with **View as role** in Desktop. Then in the Service, assign people to the role under the semantic model's **Security**.

> **Two things that bite.** RLS applies to Viewers, **not** to workspace Admins/Members — they see everything, so test with a genuine Viewer account. And dynamic RLS depends on the email in your security table exactly matching the sign-in identity. (Day 2's model had precisely that mismatch — `@arka.com` versus `@retailco.com` — which would silently show nothing.)

### Exporting

| Format | Notes |
|---|---|
| **PDF** | Static snapshot; good for circulating |
| **PowerPoint** | Each page becomes a slide |
| **Excel** | Export data from a visual; row limits apply |
| **Analyze in Excel** | **Live PivotTable connected to the published model** |
| **Paginated report** | Pixel-perfect, print-ready multi-page |

**Analyze in Excel deserves attention.** It gives Excel users a live PivotTable against your governed model and measures — one version of the truth, in the tool they already like. It is often the most effective way to get finance teams to adopt Power BI: they keep Excel, you keep control of the numbers.

### Embedding

- **Publish to web** — generates a public link. **It is genuinely public: no authentication, indexable by search engines.** Never use it for internal or personal data. Many organisations disable it outright, and rightly.
- **Embed in SharePoint** — the SharePoint page part; respects permissions
- **Embed in Teams** — add a report as a tab
- **Embed for customers** — Power BI Embedded, a developer route

### Integration

**Excel:** Analyze in Excel, as above. Excel can also connect directly to a published semantic model via Data → Get Data → From Power BI.

**Teams:** add a report as a tab in a channel so it sits where the conversation happens. Reports are also shareable into chat. This drives adoption more than anything else — people check Teams; they don't visit a BI portal.

**SharePoint:** embed via the Power BI web part, and store source files in SharePoint/OneDrive so cloud refresh works without a gateway.

**Power Automate & Alerts:** set a data alert on a dashboard tile (e.g. sales below target), then trigger a Power Automate flow to email or post to Teams. This is the "automating insights" part — the report reaches out to people instead of waiting to be visited.

### Deployment pipelines *(Premium/PPU)*

Dev → Test → Production stages, so changes are tested before reaching consumers. Useful once a report matters enough that breaking it would be a problem.

### A sensible checklist before you share anything

1. Data is correct and the refresh succeeds
2. Scheduled refresh set, **with failure notifications on**
3. Gateway configured if any source is on-premises
4. RLS applied and tested **as a Viewer**
5. Published to a **proper workspace**, not My Workspace
6. Distributed via an **app** for wide audiences
7. Least-privilege permissions
8. Sensitivity label applied if your organisation uses them
9. A contact and a "last refreshed" indicator on the report itself
10. Mobile layout checked if anyone will read it on a phone

That "last refreshed" timestamp is worth adding to every report. A visible `LastRefresh = MAX('Date'[Date])` measure, or a Power Query timestamp column, tells readers how much to trust what they're seeing.

---

## See it — worked example

Walk through the full cycle with a report you've already built.

1. Open your Day 2 model or Module 1 dashboard
2. **Home → Publish** → pick a workspace
3. In the Service, open the report. Note the **semantic model** listed separately
4. Pin a visual to a **new dashboard** — see how it differs from the report
5. Semantic model → Settings → **Scheduled refresh**. Try to configure it. If your source is a local file, you'll be told you need a **gateway** — that message is the lesson
6. Try **Export → PDF** and **Analyze in Excel**
7. Create an **app** from the workspace and view it as a consumer would
8. Add the report as a **tab in a Teams channel**

Note where you hit licensing or admin walls. Knowing the boundaries is part of knowing the tool.

---

## Do it — practice

**If you have a Power BI account:**

1. Publish to a proper workspace (create one if needed)
2. Explore the workspace: model, report, and a dashboard you pin
3. Configure scheduled refresh; **enable failure notifications**
4. Add RLS: a role, a DAX filter, tested with **View as role**
5. Create and publish an app
6. Export to PDF and PowerPoint
7. Try **Analyze in Excel**
8. Add a "Last refreshed" indicator to the report
9. Add it as a Teams tab
10. Set a data alert on a dashboard tile

**If you don't have an account** (or are on a Mac without Desktop) — this module is still examinable knowledge, and you can:

- Write out the deployment checklist for your own organisation's data
- Decide which sources would need a gateway and why
- Draft the RLS rules your data would need, and identify where identity matching might fail
- Work out which licences the intended audience would require
- Decide app vs direct share, and justify it

Those decisions are the actual professional skill; the clicking is easy.

### If you get stuck

| Problem | Why | Fix |
|---|---|---|
| Refresh works in Desktop, fails in Service | Local source, no gateway | Install/configure the on-premises gateway |
| Can't share | Free licence, or My Workspace | Get Pro; publish to a real workspace |
| Recipients can't open it | They lack Pro, workspace not on Premium | Licences, or Premium capacity |
| RLS shows everything | Tested as Admin/Member | Test as a Viewer; RLS doesn't apply to workspace admins |
| Dynamic RLS shows nothing | Email mismatch with sign-in identity | Align the security table with real UPNs |
| Refresh silently stale | Notifications off | Turn on failure emails |
| Custom visual missing in export | Not supported in that format | Use a built-in visual |
| Publish to web unavailable | Disabled by admin — usually correctly | Use an app or SharePoint embed |

---

## Check your understanding

1. What's the difference between a report and a dashboard in the Service?
2. Why shouldn't team content live in My Workspace?
3. When do you need a gateway?
4. Why should you test RLS as a Viewer rather than an Admin?
5. What does Analyze in Excel give a finance team, and why does that matter for adoption?
6. Why is "Publish to web" dangerous?
7. Name three things on the pre-share checklist and why each matters.
8. Which licence do you and your readers need for ordinary sharing?

---

**Previous:** [Module 2](../02_storytelling_with_data/LESSON.md) · **Next:** [Module 4 — Capstone Project](../04_capstone_executive_dashboard/LESSON.md) · **Day 3 index:** [README](../README.md)


---

*© 2026 Tapiwa Zireva. Prepared for the ZIMASCO (Kwekwe) workshop. Licensed to participants for personal learning — not for redistribution, resale, or reuse in other training without permission.*
