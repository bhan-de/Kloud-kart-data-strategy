# **ADR-001: Move all reporting and exports onto the approved read replica**
- **Status:** Accepted

- **Date:** 13-11-2026

- **Decision owner:** Head of Operations, Kloud Kart
## **Context**
The live website and the ~40 GB MySQL database (orders, inventory, customer accounts) share one server. Twice last month, the Monday CSV export slowed the site to a crawl.
## **Decision**
 Use the pre-approved machine as a MySQL read replica. All reporting, exports and the future warehouse feed read from the replica, and CSV exports from the production database are switched off. The website and the main database stay on the current server.
## **Alternatives considered**
### **Alternative A: Move the website onto its own server**
- **How it would work:** The new machine runs the website; the existing machine runs only the database.

- **Why it is attractive:** It's the classic first scaling step. Web and database stop competing for CPU, the database gets all the memory, and it makes room for Nigeria traffic.

- **Why rejected:** It doesn't fix the incident. The Monday export would still hammer the same database the website depends on, so the site would still slow down. It also leaves all company data on a single machine, while the replica gives a second, live copy.
### **Alternative B: Upgrade to a bigger server**
- **How it would work:** Replace the box with one that has more memory, CPU and faster disks.

- **Why it is attractive:** No new moving parts. The developer manages one machine as today.

- **Why rejected:** It raises the threshold rather than removing the conflict: a bigger export on a bigger month collides again. It isn't the pre-approved spend, and it still leaves one copy of everything.
## **Consequences**
- **What this buys:** Reporting can no longer slow the website. A live standby copy of the database. An "orders so far today / items out of stock" view that is seconds behind, with no new infrastructure.

- **What this costs:** Running cost of a second server, which should roughly match the main one or it falls behind. Replication delay must be monitored. A few days of the developer's time to set up, before the data engineer arrives.

- **What it costs the client if I am wrong:** If slowdowns continue after exports move (meaning website traffic, not reporting, is the bottleneck), the website will need its own server as well. Regret is low: the replica still provides the standby copy and the reporting source.

- **What would make me revisit this:** Main server CPU staying high at peak after exports are removed; Nigeria traffic growing beyond the pilot; replication delay regularly exceeding a few minutes.
## **What this survives**
- **Faster growth or a Nigeria scale-up:** the separation still holds; moving the website to its own server becomes the next step, and the replica is unchanged.

- **Budget pressure:** this is the cheapest fix available and is already approved.

- **Hiring delays:** the developer, or a short-term contractor, can set it up without the data hires.

- **Not survived on its own:** a bad delete or a corrupted database is copied to the replica within seconds. A replica is not a backup; tested backups are covered separately, as the improvement beyond the brief.


