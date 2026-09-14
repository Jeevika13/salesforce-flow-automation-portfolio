# Salesforce Flow Automation Portfolio

Documented Salesforce Flow designs built and maintained for real estate CRM clients. Since Flows live inside a Salesforce org rather than in source code the way Apex does, this repo captures the **logic, triggers, and business impact** of each automation in plain documentation — the same way you'd hand off a design to another admin.

## 1. Lead Routing Flow

**Trigger:** New Lead record created or Lead Source updated
**Type:** Record-Triggered Flow (before/after save)

**Logic:**
1. Check Lead Source and Property Interest fields
2. Match against a routing table (Territory / Agent Assignment custom object)
3. Assign Owner via `Get Records` + `Update Records`
4. Fire an Email Alert to the assigned agent
5. Log the routing decision to a Lead History related list for audit

**Business impact:** Removed manual lead assignment, cutting average response time from ~4 hours to under 15 minutes.

---

## 2. Listing Status Update Flow

**Trigger:** Listing record field change (Status)

**Logic:**
1. Detect transition (e.g., Active → Under Offer → Sold)
2. Update related Opportunity and Deal records to match
3. Notify Property Owner contact via Email Alert
4. Create a follow-up Task for the assigned agent at 7 days if status is "Under Offer"

**Business impact:** Kept CRM listing status in sync with the public-facing property portal automatically, eliminating stale listings.

---

## 3. Two-Factor Task Assignment Flow

**Trigger:** Opportunity stage change to "Offer Received"

**Logic:**
1. Auto-create a review Task assigned to the sales manager
2. Set due date based on Offer Expiry Date field (minus 2 days)
3. Send reminder notification if Task remains open within 24 hours of due date

**Business impact:** Standardized offer review turnaround across all agents.

---

## Notes
- All Flows were built and tested in a sandbox before deployment, following UAT sign-off.
- Field and object names above are generalized/anonymized from real client implementations to respect confidentiality.
