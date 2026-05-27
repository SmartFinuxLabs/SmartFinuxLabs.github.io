# Verity MVP Implementation Roadmap

## 1. Planning Basis

This plan uses the consolidated Round001 product specification as the executable MVP roadmap basis for **Verity**, the on-chain supply chain finance platform. Round002 has been merged into Round001 and removed from the active ideation tree.

Primary source documents:

- `Products-SCF/Ideation/Round001/001 Draft of Specification.md`
- `Products-SCF/Ideation/Round001/001 TBD Key Points.md`
- `Products-SCF/Ideation/Round001/001 Risk Analysis on Buyer Default.md`
- `Products-SCF/Ideation/Round001/001 Yield Model.md`
- `Products-SCF/Ideation/Round001/Frontend_TRD.md`

Note: `Products-SCF/Ideation/Round001/001 Draft of Specification.md` is now the canonical product specification. It contains the merged Round002 product specification and Appendix A for invoice upload, buyer validation, and the decision to treat the upload-invoice draft as a workflow module rather than a separate product spec.

## 1.1 SDLC Map

Verity's planning flow follows this SDLC path:

```text
Ideation -> Use Cases -> Roadmap of Impl Plan
```

| SDLC stage | Folder | Purpose | Output |
| --- | --- | --- | --- |
| Ideation | `Products-SCF/Ideation/` | Research, product analysis, value model, risk analysis, and consolidated product specification. | Consolidated Round001 product specification and supporting analysis. |
| Use Cases | `Products-SCF/UseCases/` | Convert product concepts into actor-centered behaviors and acceptance criteria. | UC files and fine-grained sub-use cases. |
| Roadmap of Impl Plan | `Products-SCF/ImplPlan/` | Schedule selected use cases into implementation phases, sprints, priorities, and delivery checkpoints. | MVP implementation roadmap. |

The roadmap should remain traceable back to the use cases, and each use case should remain traceable back to the ideation and product specification documents.

## 2. MVP Definition

### 2.1 MVP Goal

Deliver a working Verity MVP where:

1. Supplier creates a structured supplier-issued invoice.
2. Buyer reviews, validates, and accepts the invoice as a payable.
3. Platform creates a deterministic invoice hash and blocks duplicates.
4. Supplier requests factoring for accepted invoice value.
5. Investor funds the accepted receivable.
6. Supplier receives USDC advance.
7. Buyer settles at maturity.
8. Escrow distributes principal, yield, fees, and residual balances.

### 2.2 MVP Product Principle

```
Only buyer-accepted Due Value is financeable.
Submitted invoice data is not enough.
```

### 2.3 MVP In Scope

- Supplier-issued invoice mode.
- Manual supplier invoice creation without PDF requirement.
- Buyer review, matching, acceptance, dispute, rejection, and hold.
- Buyer-side invoice import placeholder/API-ready interface.
- Deterministic invoice hash and duplicate prevention.
- Accepted invoice registry.
- Factoring request for accepted invoices.
- Investor marketplace and funding flow.
- Simple discount and annualized yield display.
- USDC escrow settlement path.
- Basic buyer/supplier/investor profiles.
- Basic buyer payment history and delinquency flag.

### 2.4 MVP Out of Scope

- Full ERP connector marketplace.
- Production-grade legal assignment engine.
- Multi-jurisdiction compliance automation.
- Fully automated PDF/OCR extraction.
- AI-driven near-duplicate detection.
- Broad non-recourse underwriting.
- Secondary market trading.
- Full fiat-only settlement.
- Insurance underwriting.

## 3. Priority Model

| Priority | Meaning | Rule |
| --- | --- | --- |
| P0 | MVP-critical | Required for the end-to-end demo and product thesis. |
| P1 | MVP-important | Strongly improves credibility, risk control, or user flow. |
| P2 | Post-MVP extension | Useful after the first complete working system. |
| P3 | Future scale feature | Important for production or market expansion, but not needed for MVP. |

## 4. Phase Overview

| Phase | Name | Duration | Main outcome |
| --- | --- | --- | --- |
| Phase 0 | Product and Architecture Alignment | 1 sprint | Finalize decisions, schemas, UX paths, and technical foundation. |
| Phase 1 | Invoice Truth and Buyer Acceptance | 2 sprints | Supplier-issued invoice can become buyer-accepted Due Value. |
| Phase 2 | Factoring Marketplace and Yield | 2 sprints | Supplier can request factoring; investor can fund accepted invoice. |
| Phase 3 | USDC Escrow and Settlement | 2 sprints | Advance, retention, repayment, yield, and settlement ledger work end to end. |
| Phase 4 | Risk, Default, and Audit Hardening | 1 sprint | Credit ceilings, delinquency trigger, audit trail, and profile signals. |
| Phase 5 | MVP Polish and Demo Readiness | 1 sprint | End-to-end demo, QA, documentation, and deployment preparation. |

Recommended sprint length: **1 week** for hackathon-style delivery or **2 weeks** for normal product delivery.

## 4.1 Use Case Coverage by Phase

| Phase | Sprint | Use cases |
| --- | --- | --- |
| Phase 0 | Sprint 0.1 | UC-001, UC-010, UC-013 |
| Phase 1 | Sprint 1.1 | UC-001, UC-002, UC-003 |
| Phase 1 | Sprint 1.2 | UC-004 |
| Phase 2 | Sprint 2.1 | UC-005, UC-010 |
| Phase 2 | Sprint 2.2 | UC-007, UC-008, UC-012 |
| Phase 3 | Sprint 3.1 | UC-008, UC-009, UC-012 |
| Phase 3 | Sprint 3.2 | UC-009, UC-014 |
| Phase 4 | Sprint 4.1 | UC-010, UC-011, UC-014 |
| Phase 5 | Sprint 5.1 | UC-001 through UC-014 validation, with UC-006 and UC-013 as documented extension paths |

## 4.2 Selected MVP Use Cases

The MVP implementation should prioritize these use cases:

| Use case | Priority | MVP decision |
| --- | --- | --- |
| UC-001 Configure Invoice Mode Agreement | P0 | Implement supplier-issued default; store mode agreement. |
| UC-002 Supplier Creates Supplier-Issued Invoice | P0 | Implement manual structured invoice creation. |
| UC-003 Buyer Imports or Uploads Supplier Invoice | P1 | Implement import/upload placeholder with manual confirmation. |
| UC-004 Buyer Validates and Accepts Invoice | P0 | Implement complete buyer review and acceptance. |
| UC-005 Prevent Duplicate Invoice Financing | P0 | Implement deterministic hash and exact duplicate block. |
| UC-007 Supplier Requests Factoring | P0 | Implement accepted-invoice factoring request. |
| UC-008 Investor Funds Receivable | P0 | Implement investor funding flow. |
| UC-009 Settle Invoice Through USDC Escrow | P0 | Implement demo USDC escrow or ledger-backed equivalent. |
| UC-010 Manage Factoring Risk Mode | P1 | Implement default representation/warranty recourse display. |
| UC-011 Handle Buyer Default or Delinquency | P1 | Implement basic delinquency trigger and buyer freeze. |
| UC-012 Calculate and Display Yield | P0 | Implement simple discount and annualized yield. |
| UC-014 Build Verifiable Payment Profile | P1 | Implement basic profile event updates. |

Deferred use cases:

| Use case | Reason |
| --- | --- |
| UC-006 Buyer Creates Self-Billed Invoice | Post-MVP because supplier-issued invoice should prove the core Due Value flow first. |
| UC-013 Manage Payment Mode | Model fields in MVP; automate hybrid/fiat modes after USDC demo flow works. |

## 5. Phase 0: Product and Architecture Alignment

### Goal

Convert the consolidated Round001 product specification into implementation-ready decisions.

### Sprint 0.1: Foundation Planning

Mapped use cases: **UC-001, UC-010, UC-013**

| Task | Priority | Owner | Deliverable |
| --- | --- | --- | --- |
| Confirm MVP flow: supplier-issued invoice only | P0 | Product | Frozen MVP workflow. |
| Confirm product roles: supplier, buyer, investor, operator | P0 | Product | Role and permission matrix. |
| Define invoice state machine | P0 | Product + Engineering | State transition table. |
| Define invoice hash inputs | P0 | Engineering | Hash specification. |
| Define core data model | P0 | Engineering | Entity schema for orgs, invoices, factoring, settlement. |
| Select implementation stack | P0 | Engineering | Frontend/backend/contract/storage decision. |
| Define Circle integration approach | P0 | Engineering | Wallet and USDC settlement plan. |
| Define demo environment | P1 | Engineering | Local/testnet deployment plan. |
| Map use cases to implementation stories | P0 | Product + Engineering | Sprint backlog references UC IDs and sub-use cases. |

### Phase 0 Acceptance Criteria

- MVP workflow is frozen.
- State machine is documented.
- Required entities and fields are identified.
- Technical architecture is approved.
- Demo assumptions are explicit.

## 6. Phase 1: Invoice Truth and Buyer Acceptance

### Goal

Build the core value-resolution flow from supplier Ask Value to buyer-confirmed Due Value.

### Sprint 1.1: Supplier Invoice Creation and Registry Draft

Mapped use cases: **UC-001, UC-002, UC-003**

| Task | Priority | Deliverable |
| --- | --- | --- |
| Supplier organization profile | P0 | Supplier can exist as an entity. |
| Buyer organization profile | P0 | Buyer can exist as an entity. |
| Supplier-issued invoice form | P0 | Supplier can create structured invoice data without PDF. |
| Invoice fields: number, buyer, supplier, amount, currency, issue date, maturity date, PO reference, delivery reference | P0 | Required invoice object. |
| Invoice status: `SUPPLIER_ISSUED`, `PENDING_BUYER_REVIEW` | P0 | Initial lifecycle states. |
| Optional supporting document metadata | P1 | PDF/document can be referenced, but not required. |
| Invoice list for supplier | P1 | Supplier can see created invoice status. |
| Relationship-level invoice mode default | P0 | Supplier-issued mode is applied to new invoices. |
| Buyer import/upload placeholder | P1 | Buyer can create an intake record for imported invoice data. |

### Sprint 1.2: Buyer Review, Matching, and Acceptance

Mapped use cases: **UC-004**

| Task | Priority | Deliverable |
| --- | --- | --- |
| Buyer invoice review queue | P0 | Buyer sees pending invoices. |
| Matching panel for PO, delivery, contract, and tolerance fields | P0 | Buyer can validate invoice evidence. |
| Buyer actions: accept, dispute, reject, hold | P0 | Invoice can leave review queue with correct state. |
| Partial acceptance support | P1 | Accepted amount can differ from face value. |
| Buyer acceptance signature or confirmation record | P0 | Payable obligation is auditable. |
| Accepted value field | P0 | Financeable value is separated from face value. |
| Statuses: `MATCHED`, `DISPUTED`, `REJECTED`, `PARTIALLY_ACCEPTED`, `ACCEPTED` | P0 | State machine supports core decisions. |
| Buyer acceptance audit event | P0 | Acceptance is recorded as Due Value confirmation. |

### Phase 1 Acceptance Criteria

- Supplier can create invoice data manually.
- Buyer can review invoice and validate evidence.
- Buyer can accept or dispute invoice.
- Accepted invoice has buyer-confirmed `acceptedValue`.
- Non-accepted invoices cannot be financed.

## 7. Phase 2: Factoring Marketplace and Yield

### Goal

Allow accepted invoices to become factoring opportunities and let investors fund them.

### Sprint 2.1: Invoice Hashing and Financeability

Mapped use cases: **UC-005, UC-010**

| Task | Priority | Deliverable |
| --- | --- | --- |
| Deterministic invoice hash generation | P0 | `invoiceHash` generated from agreed inputs. |
| Duplicate invoice check | P0 | Exact duplicate registration is blocked. |
| Financeability engine | P0 | Only eligible accepted invoices are financeable. |
| Financing permission flag | P0 | Buyer/platform can enable or disable financing. |
| Risk mode assignment: representation/warranty recourse default | P0 | Default risk mode attached to invoice. |
| Accepted invoice registry | P0 | Financeable invoice record is registered. |
| Basic audit trail | P1 | Key state transitions are recorded. |
| Financeability reason codes | P1 | Platform explains why invoice is financeable or blocked. |

### Sprint 2.2: Supplier Factoring Request and Investor Marketplace

Mapped use cases: **UC-007, UC-008, UC-012**

| Task | Priority | Deliverable |
| --- | --- | --- |
| Supplier factoring request screen | P0 | Supplier can request funding on accepted invoice. |
| Marketplace list of accepted invoices | P0 | Investor can view available opportunities. |
| Investor profile and wallet placeholder | P0 | Investor can participate in funding flow. |
| Yield calculator: simple discount rate | P0 | Deal-level discount shown. |
| Yield calculator: annualized yield | P0 | Time-adjusted return shown. |
| Risk/yield panel | P1 | Buyer rating, maturity, risk mode, accepted value displayed. |
| Investor funding action | P0 | Investor can commit to fund invoice. |
| Supplier recourse disclosure | P1 | Supplier sees representation/warranty terms before request. |

### Phase 2 Acceptance Criteria

- Duplicate accepted invoice cannot be registered twice.
- Supplier can request factoring only for accepted value.
- Investor can see invoice risk/yield details.
- Investor can fund a selected accepted invoice in demo flow.

## 8. Phase 3: USDC Escrow and Settlement

### Goal

Move value programmatically through advance payment, retention, buyer repayment, and settlement distribution.

### Sprint 3.1: Escrow Funding and Supplier Advance

Mapped use cases: **UC-008, UC-009, UC-012**

| Task | Priority | Deliverable |
| --- | --- | --- |
| Escrow data model or smart contract interface | P0 | Funding terms are represented. |
| Investor funding deposit flow | P0 | Investor funds invoice in USDC/demo USDC. |
| Advance amount calculation | P0 | Supplier advance calculated from accepted value. |
| Retention/reserve calculation | P1 | Optional retained balance tracked. |
| Supplier advance transfer | P0 | Supplier receives advance in wallet/ledger. |
| Status: `FACTORED` | P0 | Invoice moves after funding. |
| Settlement ledger creation | P0 | Principal, advance, fee, reserve, yield are recorded. |
| Funding transaction reference | P1 | Ledger stores wallet/transaction or demo ledger reference. |

### Sprint 3.2: Buyer Repayment and Distribution

Mapped use cases: **UC-009, UC-014**

| Task | Priority | Deliverable |
| --- | --- | --- |
| Buyer repayment flow | P0 | Buyer can pay full accepted amount at maturity. |
| Investor principal + yield distribution | P0 | Investor receives repayment and yield. |
| Supplier residual/retention release | P0 | Supplier receives remaining amount if applicable. |
| Platform fee tracking | P1 | Fees are visible in ledger. |
| Status: `SETTLED` | P0 | Invoice closes after repayment. |
| Transaction history | P1 | Supplier, buyer, investor can see settlement events. |
| Profile settlement event | P1 | Buyer and supplier profiles update after settlement. |

### Phase 3 Acceptance Criteria

- Investor can fund invoice.
- Supplier receives advance.
- Buyer can repay at maturity.
- Escrow/ledger distributes funds correctly.
- Invoice reaches `SETTLED`.

## 9. Phase 4: Risk, Default, and Audit Hardening

### Goal

Make the MVP credible by adding basic risk controls, delinquency handling, and auditability.

### Sprint 4.1: Risk Controls and Default Workflow

Mapped use cases: **UC-010, UC-011, UC-014**

| Task | Priority | Deliverable |
| --- | --- | --- |
| Buyer credit ceiling | P0 | Buyer exposure cap blocks excess financing. |
| Basic buyer rating/profile | P1 | Buyer risk summary visible. |
| Supplier representation/warranty terms display | P0 | Supplier recourse terms shown before factoring. |
| Delinquency trigger | P0 | Late repayment marks invoice/buyer delinquent. |
| Freeze new financing for delinquent buyer | P0 | Risk control enforced. |
| Profile updates for on-time/late/default settlement | P1 | Payment history is tracked. |
| Audit event export | P1 | State and settlement events can be reviewed. |
| Risk mode gate checks | P1 | Funding checks invoice status and risk mode compatibility. |

### Phase 4 Acceptance Criteria

- Buyer exposure limits are enforced.
- Late repayment triggers `DELINQUENT`.
- Delinquent buyer cannot generate new financeable opportunities.
- Audit trail captures invoice, funding, and settlement events.

## 10. Phase 5: MVP Polish and Demo Readiness

### Goal

Prepare Verity for demo, pilot discussion, or hackathon submission.

### Sprint 5.1: Product Polish, QA, and Documentation

Mapped use cases: **UC-001 through UC-014 validation**

| Task | Priority | Deliverable |
| --- | --- | --- |
| End-to-end happy path QA | P0 | Supplier to settlement flow verified. |
| Negative path QA | P0 | Duplicate, dispute, rejected, and delinquent flows verified. |
| UI polish for three dashboards | P1 | Supplier, buyer, investor screens are coherent. |
| Demo data set | P0 | Example buyer, supplier, investor, invoices, settlements. |
| Product README | P0 | Setup, demo, architecture, and limitations. |
| Architecture diagram update | P1 | Implementation architecture reflects MVP. |
| Known limitations document | P1 | Clear post-MVP scope boundary. |
| Use-case traceability review | P0 | Each implemented sprint story maps back to UC and sub-use-case IDs. |

### Phase 5 Acceptance Criteria

- MVP demo can run from invoice creation to settlement.
- Demo includes at least one failed duplicate attempt.
- Demo includes at least one disputed invoice path.
- README explains how to run and evaluate the MVP.
- Product limitations and next phases are documented.

## 11. Cross-Phase Task Priority Backlog

### P0: Must Have for MVP

- Role model: supplier, buyer, investor.
- Supplier-issued invoice manual creation.
- Buyer review queue.
- Buyer acceptance as payable confirmation.
- Invoice state machine.
- Deterministic invoice hash.
- Duplicate prevention.
- Accepted invoice registry.
- Financeability rule: accepted value only.
- Supplier factoring request.
- Investor marketplace and funding.
- Yield display: simple discount and annualized yield.
- USDC/demo USDC escrow flow.
- Supplier advance.
- Buyer repayment.
- Investor repayment + yield.
- Invoice settlement state.
- Basic delinquency trigger.

### P1: Should Have for Strong MVP

- Partial acceptance.
- Optional PDF/supporting document metadata.
- Buyer credit ceiling.
- Basic buyer/supplier profiles.
- Risk mode display.
- Representation/warranty disclosure.
- Settlement ledger and transaction history.
- Audit trail.
- Demo data and polished dashboards.

### P2: Post-MVP Extensions

- Self-billing / buyer-created invoice flow.
- Supplier ERP/AP/AR import.
- Buyer ERP/AP import.
- PDF extraction/OCR.
- Reserve-backed non-recourse.
- Buyer collateral vault.
- Ecosystem reserve fund.
- CCTP cross-chain investor funding.
- Fiat invoice/stablecoin settlement FX workflow.

### P3: Future Scale

- Full legal receivable assignment workflow.
- Multi-jurisdiction compliance engine.
- AI anomaly and near-duplicate detection.
- Insurance underwriting.
- Secondary receivable market.
- Full fiat-only settlement integration.
- Advanced credit oracle integrations.
- Institutional reporting and portfolio analytics.

## 12. Dependency Map

| Dependency | Blocks |
| --- | --- |
| Role and organization model | All user workflows. |
| Invoice state machine | Buyer acceptance, financeability, marketplace, settlement. |
| Invoice hash inputs | Duplicate prevention and registry. |
| Accepted value model | Factoring, yield, escrow, settlement. |
| Wallet/ledger abstraction | Funding, advance, repayment, yield distribution. |
| Risk mode model | Marketplace pricing and investor disclosure. |
| Settlement ledger | Audit, profiles, yield history, default handling. |

## 13. Suggested Delivery Sequence

The highest leverage build order:

1. Data model and state machine.
2. Supplier invoice creation.
3. Buyer acceptance.
4. Hash and duplicate registry.
5. Factoring request.
6. Investor funding.
7. Escrow/ledger settlement.
8. Yield display.
9. Default trigger.
10. UI polish and demo data.

This order protects the core thesis: Verity is not just an invoice upload product; it is a Due Value financing platform.

## 14. Phase Exit Checklist

| Phase | Exit checklist |
| --- | --- |
| Phase 0 | Requirements frozen, schemas drafted, state machine approved. |
| Phase 1 | Supplier invoice and buyer acceptance work end to end. |
| Phase 2 | Accepted invoice can be funded by investor. |
| Phase 3 | USDC/demo USDC settlement ledger works end to end. |
| Phase 4 | Risk gates and delinquency behavior are enforced. |
| Phase 5 | Demo is stable, documented, and ready to present. |

## 15. Implementation Risks

| Risk | Impact | Mitigation |
| --- | --- | --- |
| Trying to support too many invoice modes in MVP | Slows delivery and weakens demo clarity. | Keep supplier-issued invoice as MVP default. |
| Treating PDFs as required source of truth | Adds OCR complexity too early. | Use structured data first; PDF optional. |
| Building non-recourse too early | Requires deeper underwriting and legal controls. | Start with representation/warranty recourse. |
| Overbuilding smart contracts before workflow is stable | Expensive rework. | Prototype state machine and ledger first, then harden contracts. |
| Under-specifying accepted value | Breaks partial acceptance and settlement. | Separate `faceValue` from `acceptedValue` from day one. |
| Weak duplicate prevention | Undermines trust. | Make deterministic hash a P0 task. |
| No default path | Investor risk feels unrealistic. | Implement minimal delinquency trigger in Phase 4. |

## 16. Recommended MVP Demo Scenario

1. Supplier creates invoice `INV-001` for `50,000 USDC`.
2. Buyer reviews invoice against PO and delivery reference.
3. Buyer accepts full invoice as Due Value.
4. Platform generates invoice hash.
5. Supplier attempts duplicate registration and system blocks it.
6. Supplier requests factoring.
7. Investor sees 2.5% simple discount and annualized yield.
8. Investor funds invoice.
9. Supplier receives `40,000 USDC` advance.
10. Buyer pays `50,000 USDC` at maturity.
11. Investor receives principal plus yield.
12. Supplier receives residual/retention after fees.
13. Buyer and supplier profiles update with successful settlement.
