Digest: Value Resolution Layer Between Buyer and Supplier

Your diagram is strong because it separates accounting views from the commercial value negotiation layer. The most important insight:

The buyer and supplier do not exchange “cash” first. They exchange claims of value that later become accounting entries.

## This middle layer can be modeled as a value-state machine.

1. Core Value States in Your Diagram

```
| Value State             | Buyer Meaning                                 | Supplier Meaning                     | Accounting Result       |
| ----------------------- | --------------------------------------------- | ------------------------------------ | ----------------------- |
| **Request Value / PO**  | Buyer asks for goods/services at agreed price | Supplier receives commercial demand  | Usually no GL entry     |
| **Reserved Value**      | Buyer expects fulfillment                     | Supplier reserves capacity/inventory | Operational commitment  |
| **Accepted Raw Value**  | Buyer accepts order terms                     | Supplier confirms order              | Still mostly off-ledger |
| **Accepted True Value** | Buyer validates delivery/terms                | Supplier has fulfilled obligation    | Recognition begins      |
| **Ask Value / Invoice** | Supplier requests payment                     | Supplier formalizes AR               | Buyer AP / Supplier AR  |
| **Due Value**           | Buyer now owes confirmed amount               | Supplier has collectible claim       | AP / AR maturity        |
| **Received Value**      | Buyer pays                                    | Supplier receives cash               | Cash settlement         |

```
2. The Critical Middle Layer: Exchange of Value

This layer is not simply “invoice processing.” It is where the transaction evolves from:
```
Intent → Commitment → Fulfillment → Claim → Obligation → Settlement
```
That is the real architecture.

Traditional ERP hides this across PO, GR, IR, AP, AR, billing, logistics, and bank modules. Your diagram pulls it into one conceptual layer.

# Chapter 1 — PO as “Request Value”

A Purchase Order is the buyer’s declared demand:

```
Request Value = quantity × price × terms × delivery expectation
```
Accounting-wise, the PO usually does not create a GL entry because no asset has been received and no liability has been incurred. It is a commercial commitment, not yet an accounting event.

In AP control, the PO becomes one side of the later 3-way match: PO, receipt, and invoice. Three-way matching is used to verify that what was ordered, received, and invoiced agrees before payment is approved.

# Chapter 2 — Delivery as “Accepted True Value”

Delivery is where the commercial promise becomes economic substance.

For the buyer:
```
Dr Inventory / Expense
Cr GR/IR
```
For the supplier:

```
Dr Accounts Receivable
Cr Revenue

Dr Cost of Goods Sold
Cr Inventory
```

Under IFRS 15, revenue is recognized when the seller satisfies a performance obligation by transferring control of goods or services to the customer. That means invoice timing alone is not the deepest rule; the deeper rule is control transfer / performance satisfaction.

This maps neatly to your Accepted True Value concept.

# Chapter 3 — Invoicing as “Ask Value”

An invoice is the supplier’s formal claim:
```
Ask Value = Supplier’s declared collectible amount
```
But an invoice is not automatically “truth.” It must be tested against:
```
PO value
Receipt value
Contract terms
Tax
Freight
Discounts
Tolerance rules
```
This is why invoice matching exists. AP systems compare the supplier invoice with supporting documents to detect price errors, quantity mismatches, duplicate billing, or fraud.

So your diagram is accurate: Ask Value must pass through Match before becoming Due Value.

# Chapter 4 — Reverse Invoicing / Self-Billing

Reverse invoicing means the buyer, not the supplier, generates the invoice-like payable document.

Instead of:
```
Supplier ships → Supplier invoices → Buyer validates
```
Reverse invoicing becomes:
```
Supplier ships → Buyer verifies receipt/contract → Buyer creates invoice on supplier’s behalf
```
This is also called self-billing. It is commonly used where the buyer has stronger control over measured quantities, rates, delivery data, or settlement terms. Sources describe it as a process where the customer calculates the amount owed and generates the invoice based on agreed terms, rather than waiting for the supplier invoice.

In your diagram, reverse invoicing is powerful because it shifts “truth generation” from supplier-side Ask Value to buyer-side verified value.

Strategic interpretation

Reverse invoicing converts:

Supplier-declared value

into:

Buyer-verified value

That reduces dispute risk, but increases responsibility on the buyer’s system.

# Chapter 5 — Re-Invoicing as “Convergent Value Adjustment”

Your diagram’s Convergent Value (Adjusted) is an excellent concept.

This is where the commercial parties reconcile differences:

```
Original invoice amount
± quantity correction
± price variance
± tax correction
± freight adjustment
± discount
± credit memo
± debit memo
= adjusted due value
```

Re-invoicing happens when the original Ask Value is not accepted as final. In ERP terms, this can appear as:

```
Invoice reversal
Credit memo
Debit memo
Corrected invoice
Adjustment invoice
```

Invoice reversal systems often create negative invoices or reversing entries to cancel or correct the original invoice amount.

Your “Convergent Value” label is stronger than normal ERP language because it captures the business reality:

The final payable is not always the first invoice. It is the mutually converged value after validation.

# Chapter 6 — Due Value as Confirmed Obligation

Once matching succeeds, the transaction becomes Due Value.

For buyer:
```
Dr GR/IR
Cr Accounts Payable
```
For supplier:
```
Dr Accounts Receivable
Cr Revenue
```
Due Value is stronger than invoice value. It means the claim has passed controls and is now payable under terms.

This is the state where financing becomes possible:
```
Approved invoice
Confirmed payable
Receivable financing
Supply chain finance
Dynamic discounting
```

In supply chain finance, the buyer’s approval of the supplier invoice/payable can make the receivable more financeable because the risk shifts toward the buyer’s confirmed obligation.

# Chapter 7 — Payment as “Received Value”

Payment converts Due Value into Received Value.

Buyer:
```
Dr Accounts Payable
Cr Cash
```
Supplier:
```
Dr Cash
Cr Accounts Receivable
```
At this point, the value exchange is financially settled. But operationally, the system still needs reconciliation:
```
Payment instruction
Bank confirmation
Remittance advice
Cash application
AR clearing
AP clearing
Bank reconciliation
```
This is where many systems still fail: they treat payment as the end, but accounting requires payment to be matched back to the original obligation.

Stronger Framing for Your Diagram

I would name the middle layer:

Value Resolution Layer

Because it resolves differences between:
```
Requested value
Delivered value
Invoiced value
Accepted value
Due value
Paid value
Received value
```
This is stronger than only “Exchange of Value.”

Proposed State Machine

```
REQUESTED
   ↓
ACCEPTED
   ↓
RESERVED
   ↓
DELIVERED
   ↓
VERIFIED
   ↓
INVOICED / SELF-BILLED
   ↓
MATCHED
   ↓
ADJUSTED
   ↓
DUE
   ↓
PAID
   ↓
RECEIVED
   ↓
RECONCILED
```
The key architecture insight:

Payment should not be triggered by an invoice alone. Payment should be triggered by a resolved value state.

Connextium / CLOB Demo Interpretation

For your CLOB Demo — Account Receivable, the “order book” is not only matching bid/ask price. It can match accounting states:
```
Buyer Request Value
↔ Supplier Ask Value
↔ Delivery Proof
↔ Invoice Claim
↔ Payment Terms
↔ Settlement Event
```
That makes the demo much deeper than normal AR automation.

The product thesis becomes:

Connextium models commerce as a sequence of accounting-valid value states, not as isolated invoices and payments.