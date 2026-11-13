# **ADR-004: Keep customer personal data in one governed place, with deletions we can prove**
- **Status:** Accepted

- **Date:** 13-11-2026

Decision owner: Head of Operations, Kloud Kart 
## **Context**
Kloud Kart must be defensible under Ghana's Data Protection Act, 2012 (Act 843), and has 90 days to respond to a Data Protection Commission complaint about a deleted customer's data. Today, the company cannot prove any deletion. Monday exports sit on laptops, Marketing exports its ~300,000-person customer list with no count of how many copies exist, and nobody owns personal data. The Nigeria pilot brings a second data-protection regime.

The 90-day deadline is shorter than a realistic hiring timeline, so the complaint response cannot wait for the data hires.
## **Decision**
1. Customer identity (name, phone, email, address) lives in one table in the warehouse, keyed by an internal customer ID. Every other table carries only the ID.

2. That table holds current data only and is never snapshotted, so a customer deleted in the shop disappears from the warehouse at the next nightly load.

3. CSV exports from production end. Marketing requests lists through a logged request, issued from the warehouse, with an expiry date.

4. A written deletion procedure covers the live database, the warehouse and its recovery window, backups (30-day rolling retention), and third-party marketing tools.

5. Within the first 60 days, the developer and the Head of Operations' team inventory the copies that already exist, to support the complaint response.
## **Alternatives considered**
### **Alternative A: Keep exports, add a written policy and an export register**
- **How it would work:** A data-handling policy staff sign, and a register logging every export: who, when, what, why.

- **Why it is attractive:** Cheapest option, no engineering, in place within a week. It is what many companies this size do, and it may satisfy a regulator for a first complaint.

- **Why rejected:** A register records that copies exist, not that they were destroyed. Proving a deletion happened is exactly what the complaint asks. It also relies on staff discipline holding across a growing team and a second country.
## **Consequences**
- **What this buys:** Deletions that can be proved; a known count of customer-data copies; a credible answer to the Commission; a design that extends to Nigeria.

- **What this costs:** Marketing loses self-serve exports, which adds friction. Developer and data engineer time. The Head of Operations' team must change Monday habits.

- **What it costs the client if I am wrong:** If counsel says a policy and register are enough, some engineering time was spent early. It is still needed for Nigeria and for scale.

- **What would make me revisit this:** Counsel's advice on in-country storage and backup retention; Nigeria's data-protection requirements; a new tool that holds customer data.
## **What this survives**
- **The complaint escalating:** there is a procedure, an inventory and an audit trail to show.

- **Hiring delays:** the complaint response (inventory, manual deletion, written procedure) does not depend on the warehouse or the new hires.

- **Growth and Nigeria:** one identity table scales; a new country adds rules, not new copies.

- **Not fully survived:** copies already sent to external tools or personal devices depend on people's cooperation to recover; the inventory documents them, it cannot guarantee them.


