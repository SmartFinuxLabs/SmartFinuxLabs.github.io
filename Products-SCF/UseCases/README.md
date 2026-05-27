# Verity Use Cases

This folder contains one use case per file, derived from the consolidated Round001 ideation analysis and product specification.

Canonical source:

- `Products-SCF/Ideation/Round001/001 Draft of Specification.md`

Round002 has been merged into the canonical Round001 specification and deleted from the active ideation tree. The former upload-invoice draft is represented in Appendix A of the specification and in UC-001, UC-002, UC-003, UC-004, and UC-006.

## Use Case Index

| ID | Use case | Primary actor | Priority |
| --- | --- | --- | --- |
| UC-001 | Configure Invoice Mode Agreement | Buyer / Supplier | P0 |
| UC-002 | Supplier Creates Supplier-Issued Invoice | Supplier | P0 |
| UC-003 | Buyer Imports or Uploads Supplier Invoice | Buyer | P1 |
| UC-004 | Buyer Validates and Accepts Invoice | Buyer | P0 |
| UC-005 | Prevent Duplicate Invoice Financing | Platform | P0 |
| UC-006 | Buyer Creates Self-Billed Invoice | Buyer | P2 |
| UC-007 | Supplier Requests Factoring | Supplier | P0 |
| UC-008 | Investor Funds Receivable | Investor | P0 |
| UC-009 | Settle Invoice Through USDC Escrow | Buyer / Platform | P0 |
| UC-010 | Manage Factoring Risk Mode | Platform / Investor | P1 |
| UC-011 | Handle Buyer Default or Delinquency | Platform | P1 |
| UC-012 | Calculate and Display Yield | Investor | P0 |
| UC-013 | Manage Payment Mode | Buyer / Supplier / Investor | P2 |
| UC-014 | Build Verifiable Payment Profile | Platform | P1 |

## MVP Recommendation

The MVP should implement UC-001, UC-002, UC-004, UC-005, UC-007, UC-008, UC-009, and UC-012 first. UC-003, UC-010, UC-011, and UC-014 strengthen demo credibility. UC-006 and UC-013 are important extensions after the first complete supplier-issued invoice flow is stable.

Invoice-intake rule: structured invoice data is the workflow source of truth. PDF upload is an intake channel or evidence attachment, not a universal requirement.
