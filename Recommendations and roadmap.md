# **Kloud Kart — Recommendations and roadmap**
## **Recommendation**
Build the smallest set of things that makes the numbers trustworthy, keeps the shop fast, and proves customer data is handled lawfully, with the two approved hires and a few hundred dollars a month in tools. Kloud Kart takes about three orders a minute; this plan is sized for that, not for Amazon.

**What** **to** **build**

1. **A second copy of the database for reports** (pre-approved). Reports can no longer slow the shop, and the copy is a live standby. *(ADR-001)*

2. **One place where every number is calculated**, refreshed nightly (Google BigQuery). Revenue is defined once and owned by Finance, so everyone reads the same figure. It keeps a daily record of stock for future forecasting. *(ADR-002, 003, 006)*

3. **Control of customer data.** One copy, no exports to laptops, logged list requests, provable deletions. *(ADR-004)*

4. **Tested backups**, rehearsed every quarter. *(Improvement beyond the brief)*

**What will not** **be** **built** **this year**

- **Real-time dashboards:** stock is already live in the system that sells; Operations sees today's orders within seconds; board figures can refresh nightly.

- **A** **lakehouse** **or** **Spark:** built for data hundreds of times our size; the business might reach 100 GB in about five to six years, and can extend later without starting over.

- **Machine learning:** no year of clean history yet. A simple forecast in Q4; a data scientist in year two.

**Roughly what it costs** (estimates; check current prices before the board) Hires: within the existing budget · Report server: similar to the current one, pre-approved · Warehouse: likely $0–50/month, capped at $100 · Dashboards: free · Backups: under $10/month · One-off: legal advice, and a contractor only if the first hire is late.

**The two hires**

- **Data engineer, first (Q1).** First 90 days: take over the report copy, load the warehouse nightly, run the deletion procedure, set up backups, find why the company oversells.

- **Analytics engineer, ~3 months later (Q2).** First 90 days: get Finance and Operations to agree definitions, write them into the warehouse with automatic checks, replace the Monday spreadsheet with a shared dashboard.

Chosen over a data scientist (whose work depends on these two) and a data analyst (more numbers, not one agreed number).

**Hard investor questions**

- *Real-time?* Stock is live; Operations is seconds behind; the board needs daily, not by the minute.

- *Are the numbers right?* One definition, owned by Finance; dashboard and books match four Mondays running.

- *AI?* A forecast in Q4; a data scientist once there's a year of history.

- *Data protection?* One copy of customer data, logged exports, provable deletions; the complaint answered within 90 days.

**Not verified:** order value, existence of backups, Nigeria's setup, data-location law, and the cause of overselling, which the data team will find; Operations and the developer will fix it. Details: `diagnosis.md`.

<div style="page-break-after: always;"></div> 
## **12-month roadmap**
Quarters count from approval (Q1 = December–February). Every outcome can be checked by the Head of Operations without asking an engineer.

| **Quarter** | **What gets done** | **How the head of Ops** **checks it** |
| :---: | :---: | :---: |
| **Q1**<br>Protect the shop, answer the regulator | Report copy running; exports from the live database switched off. Every copy of customer data found; complainant's data deleted. Backups started; first restore rehearsed. Data engineer hired. Overselling investigated. | Mondays pass with no slowdown. Complaint answered before day 90, with proof of deletion. A restore report states how long it took. A written finding names why stock goes wrong; "orders cancelled for lack of stock" counted weekly. |
| **Q2**<br>One place for the numbers | Warehouse loading nightly, with a daily stock record. Revenue, orders and returns defined in writing, signed by Finance and Operations. First shared dashboard. Analytics engineer hired. Marketing lists only via logged requests. | Dashboard shows yesterday's figures by 7 a.m. A signed definitions page exists. The request log shows every list issued, to whom, and when it expires. |
| **Q3**<br>The Monday spreadsheet retires | Monday numbers come from the dashboard. Nigeria in the same reports, in both currencies. Overselling fix in place. Second restore rehearsal. | Finance's revenue and the dashboard match four Mondays running. No time spent rebuilding figures. Cancellations for lack of stock are falling. |
| **Q4**<br>Ready for year two | First demand forecast by category, compared with actual sales. Board pack built from the dashboard. Third restore rehearsal. Year-two plan, including whether to hire a data scientist. | Each month's forecast sits beside actual sales. The board pack needs no spreadsheet. A written year-two proposal is ready. |
**Dependencies:** until the data engineer starts, Q1 relies on the developer or a contractor; the complaint cannot wait for hiring. The Q3 overselling fix depends on Q1's finding.

**If something slips, protect in this order:** the complaint deadline, the report copy, backups, matching revenue. The forecast can move to year two without harming anything else.


