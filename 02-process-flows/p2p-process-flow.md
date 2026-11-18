# SAP S/4HANA FI-MM Procure-to-Pay (P2P) Process Flow

## Project Overview

This document explains the end-to-end Procure-to-Pay (P2P) process used in my independent SAP S/4HANA FI-MM learning project.

The project demonstrates how a business requirement progresses through purchasing, goods receipt, supplier invoice verification, Accounts Payable, payment, clearing, and GR/IR reconciliation.

The main learning objective is to understand the integration between:

- Materials Management (MM)
- Financial Accounting (FI)
- Accounts Payable (AP)

All company, supplier, material, organizational, and transaction data used in this project is fictional or synthetic.

---

## 1. End-to-End P2P Flow

```text
Business Requirement
        ↓
Purchase Requisition (PR)
        ↓
PR Approval
        ↓
Purchase Order (PO)
        ↓
PO Approval / Release
        ↓
Goods Receipt (GR)
        ↓
Supplier Invoice Receipt
        ↓
Invoice Verification
        ↓
Three-Way Match
        ↓
Invoice Posting
        ↓
Supplier Open Item
        ↓
Payment
        ↓
Clearing
        ↓
GR/IR Reconciliation
```

Purchasing activities primarily involve MM. Invoice accounting, supplier liability, payment, and clearing connect the process with FI and AP.

---

## 2. Synthetic Business Scenario

Nova Manufacturing Pvt. Ltd. is a fictional manufacturing company. The production department requires 100 units of raw material for production.

### Project Data

| Field | Synthetic Value |
|---|---|
| Company | Nova Manufacturing Pvt. Ltd. |
| Company Code | NM01 |
| Plant | PL01 |
| Purchasing Organization | NP01 |
| Purchasing Group | PG01 |
| Supplier | V10001 - Alpha Industrial Supplies |
| Material | RM-1001 |
| Description | Synthetic Production Material |
| Quantity | 100 Units |
| Unit Price | ₹500 |
| Net Purchase Value | ₹50,000 |

---

## 3. Business Requirement

The production department identifies the need for 100 units of RM-1001.

```text
Required Quantity: 100 Units
Unit Price:        ₹500
Expected Value:    ₹50,000
```

At this stage, the requirement has been identified, but the material has not yet been ordered or received.

**Output:** The requirement can proceed to the Purchase Requisition process.

---

## 4. Purchase Requisition (PR)

A Purchase Requisition records the internal requirement for the material.

### Example

| Field | Value |
|---|---|
| Material | RM-1001 |
| Quantity | 100 Units |
| Plant | PL01 |
| Expected Value | ₹50,000 |

### Accounting Impact

No FI accounting posting is generated when the PR is created in this base learning scenario.

### Next Step

The PR proceeds through the applicable approval process.

---

## 5. Purchase Requisition Approval

The PR is reviewed according to the organization's approval process.

```text
PR Review
   ├── Approved → Continue to Purchase Order
   └── Rejected → Review or correct the PR
```

---

## 6. Purchase Order (PO)

Purchasing creates a Purchase Order for the selected supplier.

### Synthetic Purchase Order

| Field | Value |
|---|---|
| Supplier | V10001 - Alpha Industrial Supplies |
| Material | RM-1001 |
| Quantity | 100 Units |
| Unit Price | ₹500 |
| Net PO Value | ₹50,000 |
| Plant | PL01 |

The PO communicates the agreed purchasing requirement to the supplier.

### Accounting Impact

In this standard base scenario, PO creation does not itself generate an FI accounting posting. However, it establishes important information such as supplier, quantity, price, account assignment, and purchasing conditions for later processing.

---

## 7. PO Approval / Release

Depending on the configured business process, the PO may require approval before it is sent to the supplier.

```text
PO Review
   ├── Approved → Send PO to supplier
   └── Rejected → Review or correct the PO
```

Once approved, the supplier can fulfil the order.

---

## 8. Goods Receipt (GR)

The supplier delivers the material. The receiving team checks:

- Correct material
- Correct quantity
- Valid PO reference
- Delivery information
- Over-delivery or under-delivery

### Synthetic Example

```text
PO Quantity: 100 Units
GR Quantity: 100 Units
Result: Full quantity received
```

The GR is recorded with reference to the PO.

### Simplified Accounting Entry

| Debit | Credit |
|---|---|
| Inventory ₹50,000 | GR/IR ₹50,000 |

### Result

- Material receipt is recorded.
- Inventory is updated.
- A financial document is generated.
- GR/IR is credited for the receipt value.

The exact accounting treatment depends on the business scenario and SAP configuration.

---

## 9. Supplier Invoice Receipt

After delivering the material, the supplier submits an invoice.

### Synthetic Invoice

| Field | Value |
|---|---|
| Supplier | V10001 - Alpha Industrial Supplies |
| Material | RM-1001 |
| Invoice Quantity | 100 Units |
| Unit Price | ₹500 |
| Net Invoice Amount | ₹50,000 |
| PO Reference | Synthetic PO Reference |

Tax is intentionally excluded from this first scenario. The invoice proceeds to invoice verification.

---

## 10. Invoice Verification

The supplier invoice is checked against the available purchasing and receipt information.

Important checks can include:

- Supplier
- PO reference
- Material
- Quantity
- Price
- Goods Receipt
- Invoice reference
- Potential duplicate invoice
- Applicable tolerances

The objective is to determine whether the invoice can continue through normal processing or requires investigation.

---

## 11. Three-Way Match

Three-Way Match compares:

```text
Purchase Order + Goods Receipt + Supplier Invoice
```

### Successful Scenario

| Check | PO | GR | Invoice | Result |
|---|---:|---:|---:|---|
| Quantity | 100 | 100 | 100 | Match |
| Unit Price | ₹500 | - | ₹500 | Match |
| Value | ₹50,000 | ₹50,000 | ₹50,000 | Match |

### Decision Flow

```text
Three-Way Match
   ├── Match → Continue with processing
   └── Variance → Investigate the exception
```

Whether a difference creates a block depends on the applicable SAP configuration, tolerances, and business rules.

---

## 12. Invoice Posting

Once the invoice satisfies the applicable validation requirements, it can proceed to posting.

### Simplified Accounting Entry

| Debit | Credit |
|---|---|
| GR/IR ₹50,000 | Supplier ₹50,000 |

### Result

- GR/IR is debited.
- Supplier liability is created.
- An FI accounting document is generated.

If the GR and Invoice Receipt values correspond, the relevant GR/IR items can clear.

---

## 13. Supplier Open Item

After invoice posting, the supplier liability remains open until it is cleared through payment.

```text
Supplier:       V10001 - Alpha Industrial Supplies
Invoice Amount: ₹50,000
Open Liability: ₹50,000
Status:         Awaiting Payment
```

Payment eligibility can depend on:

- Payment terms
- Due date
- Payment block
- Required approvals
- Valid supplier payment information

---

## 14. Payment

When the invoice becomes due and is approved, the supplier can be paid.

### Simplified Accounting Entry

| Debit | Credit |
|---|---|
| Supplier ₹50,000 | Bank ₹50,000 |

This is a simplified example. Actual payment accounting can involve bank-clearing accounts, payment media, approvals, and other configuration-dependent steps.

---

## 15. Clearing

After payment processing, the supplier invoice and corresponding payment can be cleared.

```text
Before Clearing: Supplier Invoice Open
After Clearing:  Supplier Invoice Cleared
Remaining Open Liability: ₹0
```

The purpose is to ensure that the supplier invoice and payment are correctly reflected in AP.

---

## 16. GR/IR Reconciliation

GR/IR reconciliation reviews differences between Goods Receipts and Invoice Receipts.

Examples requiring investigation include:

- GR exists but the invoice is missing
- Invoice exists but the corresponding GR is missing
- Quantity difference
- Amount or price difference
- Incorrect reference
- Old unresolved GR/IR item

### Simplified Relationship

```text
Goods Receipt
Dr Inventory
Cr GR/IR
      ↓
Invoice Receipt
Dr GR/IR
Cr Supplier
      ↓
Matching Values
      ↓
GR/IR Items Clear
```

Open differences require investigation and appropriate resolution.

---

## 17. Exception Flow

Not every invoice follows the successful path.

```text
Invoice Verification
        ↓
Three-Way Match
        ↓
Variance?
   ├── No  → Continue to posting
   └── Yes → Identify cause
                  ↓
             Investigate
                  ↓
             Resolve issue
                  ↓
              Revalidate
```

### Potential Exceptions

- Missing PO
- Missing GR
- Quantity variance
- Price variance
- Potential duplicate invoice
- Incorrect purchasing information
- Supplier or master-data issue
- Payment block

These exceptions are demonstrated separately in the portfolio using synthetic data.

---

## 18. FI-MM Integration Points

| Process | Main Business Area | Simplified FI Impact |
|---|---|---|
| Purchase Requisition | MM / Purchasing | No FI posting in this scenario |
| Purchase Order | MM / Purchasing | No FI posting in this scenario |
| Goods Receipt | MM / Inventory | Dr Inventory / Cr GR/IR |
| Invoice Receipt | MM Invoice Verification + FI-AP | Dr GR/IR / Cr Supplier |
| Supplier Open Item | FI-AP | Liability remains outstanding |
| Payment | FI-AP | Dr Supplier / Cr Bank |
| Clearing | FI-AP | Invoice and payment cleared |
| GR/IR Reconciliation | MM + FI | Review unresolved differences |

---

## 19. Key Control Points

### Purchase Controls

- Approved Purchase Requisition
- Appropriate supplier selection
- Correct PO quantity and price
- Required PO approval

### Receiving Controls

- Correct material
- Correct quantity
- Valid PO reference
- Accurate Goods Receipt

### Invoice Controls

- Valid supplier
- PO verification
- GR verification
- Quantity comparison
- Price comparison
- Duplicate invoice check
- Applicable tolerance checks

### Payment Controls

- Correct payment terms
- Due-date determination
- Payment-block review
- Required approvals
- Valid supplier payment information

---

## 20. What I Learned

Through this process-flow exercise, I developed a clearer understanding of:

1. How a business requirement becomes a PR.
2. How an approved requirement progresses to a PO.
3. Why PO creation does not itself create an FI posting in this base scenario.
4. How GR connects MM with FI.
5. Why GR/IR is required.
6. How Invoice Receipt creates a supplier liability.
7. How Three-Way Match identifies procurement discrepancies.
8. Why exceptions should be investigated before normal processing continues.
9. How supplier open items relate to payment.
10. How payment and clearing complete the AP lifecycle.
11. Why GR/IR reconciliation is important for FI-MM integration.

---

## 21. Next Project Steps

The next sections of this portfolio cover:

- Detailed FI-MM accounting entries
- Three-Way Match scenarios
- Synthetic transaction data
- Functional test cases
- Invoice exception analysis
- GR/IR reconciliation examples
- Lessons learned

---

## Project Disclaimer

This is an independent SAP S/4HANA FI-MM learning and portfolio project.

All company names, suppliers, materials, organizational structures, Purchase Orders, invoices, amounts, and transaction information shown in this project are fictional or synthetic.

No employer, customer, supplier, or other confidential business information is used.

SAP and SAP S/4HANA are trademarks of SAP SE.

This project is not affiliated with or endorsed by SAP SE.
