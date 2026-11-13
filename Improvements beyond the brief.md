# **Improvement beyond the brief: Tested backups and a rehearsed restore**
## **The problem it solves**
Kloud Kart's website, orders, stock and customer accounts all live on one server. Nobody in the brief asked what happens if that server fails, is hacked, or someone accidentally deletes a table. The new report copy (ADR-001) does not protect against this: it copies every change within seconds, mistakes included, so a deleted table is deleted in both places. The brief says nothing about backups; we assume no tested backup exists.
## **What I built**
A backup and restore procedure, designed and ready for the developer to set up in Q1. It is not yet built: we have no access to the client's server.

1. **Nightly backup** of the full database, taken from the report copy so the website feels nothing.

2. **The database's record of changes kept between backups**, so it can be restored to any minute: for example, just before an accidental deletion at 3:12 p.m.

3. **Backups stored away from the server**, in cloud storage in a different location.

4. **Backups kept for 30 days, then deleted.** This limits how long a deleted customer's data survives, and is written into the deletion procedure (ADR-004). Retention period to be confirmed with counsel.

5. **A full restore rehearsed every quarter** onto a spare machine, timed, and written up as a one-page report for the Head of Operations.

**Targets**, to be confirmed in the first rehearsal: lose no more than 15 minutes of orders, and be selling again within 4 hours.

**What it does not do:** it gets the shop back after a failure; it does not keep it running through one. Automatic switch-over to the standby copy is a year-two decision.
## **What it is worth to Kloud Kart**
- **Sales protected:** one day offline is about 4,000 orders, roughly GHS 1.0M–2.4M in revenue depending on order value.

- **Records protected:** lost orders, payments owed and customer accounts cannot be priced; rebuilding them from emails and courier receipts would take weeks.

- **An answer for the board:** to "what happens if your server goes down?", the head can say "we're selling again within hours, and we rehearse it every quarter" instead of "we don't know."

- **Something she can check herself:** a one-page restore report every quarter, giving how long the restore took and how many minutes of orders would have been lost. No report means no tested backup.

- **A cleaner answer to the regulator:** a fixed backup period gives a fixed date by which a deleted customer is gone from every copy.
## **What it cost**
- **Storage:** under $10 a month.

- **Set-up:** a few days of the developer's time in Q1.

- **Running:** about half a day each quarter for the rehearsal, taken over by the data engineer from Q2.

- No new hire and no new vendor.


