# **Situation Diagnosis and** **Data findings — Kloud Kart (brief)**
## **Summary**
Kloud Kart's visible problems (a slow site on Mondays, two revenue figures, uncounted marketing lists, a regulator complaint) come from four causes: customer data has no single home or owner; revenue has no agreed definition; reporting runs on the server that takes orders; and the whole business sits on one machine. The database is not too big (40 GB is small), and nothing here needs real-time technology. Overselling is real, but its cause is unknown and is a Q1 investigation.

Costs are in orders, and in cedis at three assumed order values (GHS 250 / 400 / 600), since none was given. They are revenue at risk, not profit.
## **Reconciliation**
How the Monday number drifts from a customer's click to a board slide (no dataset was provided, so stages replace row counts).

| **Stage** | **Today** | **Drift** | **Reason** |
| :---: | :---: | :---: | :---: |
| Generation | Order written on the website's server | None | Source data is sound |
| Ingestion | Monday CSV export from the live database | Site slows | Export competes with customers |
| Storage | CSVs on laptops and in Marketing | Untracked customer copies | Nobody owns or counts them |
| Transformation | Spreadsheet formulas | Finance ≠ Operations | Each team defines revenue differently |
| Serving | Figure goes into a slide | Board sees a disputed number | No agreed source |
## **Findings**
### **1: Customer data has no home or owner** 
- **What it is:** Customer data leaves as untracked CSVs, including Marketing's ~300,000-person list, so no deletion can be proved.

- **Source:** Stated in the brief, including the Commission complaint.

- **Scale:** 90-day legal deadline; possible enforcement under Act 843 (penalties: counsel to confirm); reputational damage in a funding year.

- **Effect on the client's number:** None on revenue; direct on the head's credibility.

- **Recommendation:** ADR-004.

- **Residual risk:** Copies already sent out can be documented, not guaranteed recovered.
### **2: No agreed definition of revenue** 
- **What it is:** Finance and Operations use one word for two numbers (likely different dates and treatment of returns, fees, VAT).

- **Source:** Mismatch stated in the brief; the rule differences are inferred.

- **Scale:** ~200–300 hours a year of the head's time on Monday rebuilds (assumed 4–6 h/week).

- **Effect on the client's number:** The board sees a figure Finance disputes. Ranked above F-03 because this is what damages the head in the boardroom.

- **Recommendation:** ADR-006, with ADR-002 and ADR-005.

- **Residual risk:** Decided, not solved, until both figures match for four Mondays running.
### **3: Reporting runs on the server that takes orders**
- **What it is:** Monday exports compete with checkout for the same machine and flush its memory cache.

- **Source:** Slowdowns stated (twice last month); mechanism inferred.

- **Scale:** ~170 orders lost per incident, ~340 a month: ~GHS 85k / 136k / 204k a month at risk.

- **Effect on the client's number:** Lower sales on export days.

- **Recommendation:** ADR-001 (cheap and pre-approved, hence ranked below F-02).

- **Residual risk:** If slowdowns persist, the website needs its own server.
### **4: The whole business runs on one machine**
- **What it is:** Website, orders, stock and customers on one server; no tested backup known.

- **Source:** Single server stated; absence of tested backups assumed.

- **Scale:** One day offline ≈ 4,000 orders (GHS 1.0M / 1.6M / 2.4M); lost records cannot be priced.

- **Effect on the client's number:** None until it fails; then all of them.

- **Recommendation:** ADR-001 (standby copy) and tested backups (improvement beyond the brief).

- **Residual risk:** A replica copies mistakes; only tested backups undo them.
### **5: Stock records lag the shelves, so the company oversells** 
- **What it is:** Stock is live in the database, so the lag is in how changes reach it: late delivery/return entries, sales outside the website, slow mobile-money payments, or simultaneous buyers.

- **Source:** Overselling stated; mechanism unconfirmed.

- **Scale:** Unknown; each case costs a refund, a delivery, possibly a customer.

- **Effect on the client's number:** Revenue on cancelled orders; lost repeat customers.

- **Recommendation:** ADR-003. The data engineer traces recent cases in Q1; Operations and the developer fix the cause. Track "orders cancelled for lack of stock" weekly.

- **Residual risk:** The data platform alone does not fix this.
### **6: One developer holds all knowledge; Nigeria adds a second currency and law** 
- **What it is:** One PHP developer runs the site and is the only one who knows the data. Nigeria brings naira and its own data-protection law.

- **Source:** Stated in the brief.

- **Scale:** Every roadmap step needs his time in the first 90 days.

- **Effect on the client's number:** Delays; revenue depends on the exchange rate used.

- **Recommendation:** ADR-005 (hires, contractor fallback); ADR-006 (currency in the definition); ADR-004 plus counsel (Nigerian data).

- **Residual risk:** His time is the plan's tightest constraint.
## **Looked wrong, is not wrong**


| **Apparent problem** | **Why it looks wrong** | **Why it is actually fine** |
| :---: | :---: | :---: |
| 40 GB is too big | The site slowed down | It's small; where the export ran was the problem |
| We need Spark | Data will grow | 100 GB is ~5–6 years away; warehouse SQL scales without a rewrite |
| We need real-time dashboards | A board member saw one | Stock is already live; operations gets a seconds-behind view; the board meets quarterly |
| We need ML now | Investors asked | No clean history yet; simple forecast in Q4, data scientist in year two |
| Someone's revenue is wrong | Figures never match | Both may be right about different things |
## **Could not verify**


| **Question we would have asked** | **What we assumed** | **What changes if wrong** |
| :---: | :---: | :---: |
| Average order value? | GHS 250–600 | Cedi figures only |
| How does overselling happen? | Late stock updates or offline sales | Who fixes it, and how urgently |
| What is the complaint about? | Data survived in an untracked copy | Scope of the 60-day inventory |
| How does each team define revenue? | Different dates and deductions | Content of the definition sessions only |
| Do tested backups exist? | No | The improvement shrinks to testing what exists |
| Is Nigeria on the same database? | Yes, in naira | Pipeline and revenue design |
| Must data stay in Ghana (Act 843)? | An African cloud region is acceptable | Warehouse location, possibly vendor |
| How large are the analysis tables? | A few GB | Nightly loads must handle only changed rows |
| How much developer time is available? | About a third, for 90 days | How soon a contractor is needed |

