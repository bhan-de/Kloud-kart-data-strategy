# **ADR-002: Use a managed cloud warehouse, with all business logic in SQL**
- **Status:** Accepted

- **Date:** 13-11-2026

- **Decision owner:** Head of Operations, Kloud Kart 
## **Context**
Finance's numbers never match Operations' Monday numbers, because each definition of revenue lives in a different spreadsheet. Nobody can reproduce last week's figure, and nothing records how stock levels changed over time, which any future demand forecast will need. 
## **Decision**
The read replica (ADR-001) only holds the current state: when an order or stock count changes, the old value is overwritten. The numbers are small. The database is ~40 GB in total, and orders add ~1.5M rows a year. At 15–20% growth, it reaches 100 GB in roughly five to six years. Nobody at the company has run a warehouse or written Python professionally, and there are two data hires. Customer deletions must be provable to the Data Protection Commission. 

The replica is loaded nightly into Google BigQuery, in its Johannesburg region (pending legal confirmation on where data may be stored). All business definitions are written once, as tested SQL, and every report reads from them. Spending is capped with hard query quotas and budget alerts.  Spark or a lakehouse does not need to be adopted this year.

Snowflake would also work, but BigQuery is chosen over Snowflake because it charges per query with no always-on compute, has a free tier that likely covers this volume, and supports hard spending caps. 
## **Alternatives considered**
### **Alternative A: No warehouse: report straight from the replica**
- **How it would work:** Saved views on the replica, plus a free dashboard tool connected to it.

- **Why it is attractive:** No new cost or vendor. Uses MySQL, which the current developer already knows. Live data, and it could be running within weeks. It would likely be enough for year one.

- **Why rejected:** The replica is a mirror, not a diary. Last Monday's number can't be reproduced once orders change, and every day without stock snapshots is history permanently lost to future forecasting. A replica is read-only, so summary tables can't be built there, and definitions end up scattered across views and dashboards: the same problem that exists today.
### **Alternative B: Lakehouse (open table formats on cloud file storage)**
- **How it would work:** Data stored as open-format tables in cloud storage, queried by an engine of our choosing.

- **Why it is attractive:** The cheapest storage. Open formats avoid vendor lock-in. Built-in history. It is the best home for future website clickstream data and for a data scientist working in Python on raw history.

- **Why rejected:** Its strengths are year-two and year-three needs. Today it means the most moving parts (table formats, catalogue, file maintenance, engine choice) for two hires with no warehouse experience. Deleting a customer means rewriting files and expiring old versions, which is harder to prove within the 90-day complaint window than a SQL delete.
### **Alternative C: PySpark, to avoid a rewrite at 250–500 GB**
- **How it would work:** Pipelines written as Spark jobs on a managed cluster.

- **Why it is attractive:** It scales to terabytes, so the code would never need rewriting for volume.

- **Why rejected:** That volume is roughly a few years away. It requires cluster operations and Python skills nobody has yet. SQL in a warehouse already scales underneath without a rewrite, so the "future-proofing" is paid for now and may never be needed.
## **Consequences**
- **What this buys:** One place every number comes from. Reproducible history and daily stock snapshots. Access controls and logs on customer data. One place to add Nigeria, the payment provider and couriers.

- **What this costs:** Dependence on one vendor. A usage-based bill, capped by quota. A new tool for the team to learn. Data stored outside Ghana, subject to legal confirmation.

- **What it costs the client if I am wrong:** If costs exceed the cap or counsel requires in-country storage, the SQL definitions move to another database in weeks, not months. The definitions, which are the valuable part, are portable.

- **What would make me revisit this:** Website clickstream capture begins; a data scientist needs Python on raw history (add open-format tables alongside, not instead); the monthly bill approaches its cap; counsel requires in-country storage.
## **What this survives**
- **Data growing two to ten times:** routine for a warehouse; only multi-terabyte clickstream would change the decision.

- **Budget cuts:** quotas hold the bill; in the worst case, fall back to Alternative A temporarily and keep the SQL definitions.

- **The DPC complaint:** deletion is a SQL delete, plus a documented recovery window (ADR-004).

- **Investor "lakehouse" questions:** the warehouse can read open table formats later, so this is an extension point, not a dead end.


