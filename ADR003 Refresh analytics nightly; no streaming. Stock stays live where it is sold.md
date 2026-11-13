# **ADR-003: Refresh analytics nightly; no streaming. Stock stays live where it is sold**
- **Status:** Accepted

- **Date:**13-11-2026

- **Decision owner:** Head of Operations, Kloud Kart
## **Context**
Investors and a board member asked for "real-time dashboards." The constraints say stock counts can't go stale (that is how the company oversells), and everything else can refresh daily. The board meets quarterly. Volume is 4,000 orders a day, and the tables needed for analysis are likely a few GB. Customer deletions must reach the warehouse for ADR-004 to hold.

Stock does not go stale inside the database: checkout reads and writes it directly. Staleness comes from how stock is updated (deliveries, returns, other sales channels), which is a process and application problem, not a pipeline problem.
## **Decision**
Every night at 02:00, the key tables are fully reloaded from the replica into the warehouse and take dated snapshots of order status and stock levels. The customer-identity table is never snapshotted. "Right now" questions (orders today, items out of stock) are answered from the replica, seconds behind. Stock is never routed through analytics; it stays authoritative in the system that sells.

Overselling is diagnosed separately: the data engineer traces recent oversold orders on the replica, and the fix will most likely be in stock-handling processes and checkout.
## **Alternatives considered**
### **Alternative A: Managed change capture (e.g. a hosted service reading the database's change log)**
- **How it would work:** A hosted service reads every insert, update and delete from MySQL's change log and delivers each one to the warehouse within minutes.

- **Why it is attractive:** Full history, including changes during the day. Deletes are recorded with a timestamp, which is useful as proof. Light load on the database, little to operate, and a foundation for real-time later. It may even be less engineering than hand-written incremental loads.

- **Why rejected:** At this size, a full nightly reload gets the important benefits for free: deletes propagate because a deleted row simply isn't there the next night, and daily snapshots give the history forecasting needs. Change capture adds a per-volume fee and another vendor. It would also preserve every past change to customer records in the warehouse, so a deleted customer's details would persist in that history, working against ADR-004. Nobody has a need for intraday history.
## **Consequences**
- **What this buys:** A pipeline one engineer can own end to end. Predictable cost. Deletions reach the warehouse within 24 hours. An honest answer to the board: stock is live, the operations view is seconds behind, and board figures are daily.

- **What this costs:** Warehouse figures are up to a day old. Changes within a day are invisible (an order that goes pending → paid → cancelled on the same day shows only its end state). Full reloads grow with the data.

- **What it costs the client if I am wrong:** If a real-time need appears, managed change capture can be added on top of the replica and warehouse. Nothing built here is thrown away.

- **What would make me revisit this:** The nightly reload taking more than about an hour, or key tables reaching tens of GB (move to loading only changed rows); a need to act on events within minutes (fraud checks, courier tracking); clickstream capture.
## **What this survives**
- **Board pressure for "real-time":** stock is already live at the source, and operations has a seconds-behind view. Nothing a quarterly board decides needs fresher numbers.

- **Data doubling:** a full reload still fits comfortably in the night window.

- **Nigeria:** the same nightly job, with one more country's rows.

- **Not addressed here:** overselling itself, which needs process and checkout fixes rather than a faster pipeline.


