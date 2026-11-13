# **ADR-005: Hire a data engineer first, then an analytics engineer**
- **Status:** Accepted

- **Date:** 13-11-2026

- **Decision owner:** Head of Operations, Kloud Kart
## **Context**
The board has budgeted two data hires this year. Nobody at Kloud Kart has warehouse or Python profeiciency. The first problems are engineering ones (replica cut-over, nightly pipeline, deletion procedure). The most visible problem to the board is a definitions problem: Finance and Operations produce different numbers. Investors asked about machine learning. The one PHP/MySQL developer also runs the website and is the only person who knows the database.
## **Decision**
- **Hire 1, data engineer**, starting as early as possible. First 90 days: take over the replica from the developer, build the nightly load into the warehouse, implement the deletion procedure, and trace the causes of overselling.

- **Hire 2, analytics engineer**, about three months later. First 90 days: run the revenue definition sessions with Finance and Operations, write the agreed definitions as tested SQL, and replace the Monday spreadsheet with a dashboard both teams use.

- If the analytics engineer title proves scarce, hire a strong SQL analyst and train them in version control, data modelling, automated tests and dbt within 90 days.
## **Alternatives considered**
### **Alternative A: Data engineer + data scientist**
- **How it would work:** The data engineer builds the platform while the data scientist builds demand forecasting.

- **Why it is attractive:** It answers the investors' ML question directly. Forecasting could reduce stock-outs and cash tied up in stock. It is a visible signal of progress to the board.

- **Why rejected:** There is no clean history and no agreed definitions to learn from. A data scientist would spend six months doing foundational work outside their speciality. A first forecast at this volume is a seasonal baseline the analytics engineer can produce in Q4. It does nothing to close the Finance–Operations gap the board will see first.
### **Alternative B: Data engineer + data analyst**
- **How it would work:** The analyst answers the Head of Operations' questions and builds dashboards.

- **Why it is attractive:** Cheaper, easier to hire, and faster answers in month one.

- **Why rejected:** An analyst produces more answers, not the same answer. Definitions would drift back into dashboards and spreadsheets: the problem the company has today.
## **Consequences**
- **What this buys:** A working foundation, plus one agreed set of numbers, built by the two people whose work everything later depends on.

- **What this costs:** No machine learning this year, which must be said plainly to investors. Hiring risk on a rare job title. The data engineer depends on the developer's time in the first 90 days, while he is also running the website.

- **What it costs the client if I am wrong:** If forecasting is more urgent than it appears (for example, stock-outs are very costly), a year of forecasting value is lost. Mitigated by a baseline forecast in Q4.

- **What would make me revisit this:** Twelve months of clean history and stock snapshots in the warehouse (hire the data scientist); the data engineer hire slipping past Q1 (bring in a contractor for the replica and pipeline).
## **What this survives**
- **Board pressure for ML:** a baseline forecast in Q4, and a data scientist in year two, once there is history worth learning from.

- **Budget cut to one hire:** the data engineer alone, taking on analytics engineering more slowly; the analytics engineer is the one deferred.

- **A slow analytics engineer search:** the hire-and-train path for a strong SQL analyst.


