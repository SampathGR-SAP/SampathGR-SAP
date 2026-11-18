# SAP S/4HANA FI-MM Procure-to-Pay (P2P) Business Scenario

## 1. Project Overview

This is an independent SAP S/4HANA FI-MM learning project designed to demonstrate my understanding of the end-to-end Procure-to-Pay (P2P) business process.

The project focuses on the integration between:

- Materials Management (MM)
- Financial Accounting (FI)
- Accounts Payable (AP)

The complete project uses synthetic business data created specifically for learning and portfolio purposes.

---

## 2. Business Scenario

Nova Manufacturing Pvt. Ltd. is a fictional manufacturing company that purchases raw materials from external suppliers for production.

The production department requires 100 units of RM-1001, a fictional raw material.

The business needs to complete the purchasing process from identifying the requirement through supplier payment while ensuring that:

- The requirement is properly requested and approved.
- A PO is issued to the supplier.
- The correct material and quantity are received.
- The supplier invoice is checked against the PO and GR.
- Accounting entries are generated correctly.
- Invoice discrepancies are identified and resolved.
- The supplier is paid according to agreed payment terms.
- Supplier and GR/IR open items are reviewed and reconciled.

---

## 3. Fictional Organization Structure

| Element | Synthetic Value |
|---|---|
| Company | Nova Manufacturing Pvt. Ltd. |
| Company Code | NM01 |
| Plant | PL01 |
| Purchasing Organization | NP01 |
| Purchasing Group | PG01 |
| Storage Location | SL01 |

These values are fictional and were created only for this learning project.

---

## 4. Synthetic Master Data

### Supplier

| Field | Synthetic Value |
|---|---|
| Supplier ID | V10001 |
| Supplier Name | Alpha Industrial Supplies |
| Country | India |
| Currency | INR |
| Payment Terms | 30 Days |

### Material

| Field | Synthetic Value |
|---|---|
| Material ID | RM-1001 |
| Description | Synthetic Production Material |
| Material Type | Raw Material |
| Quantity Required | 100 Units |
| Unit Price | ₹500 |
| Net Purchase Value | ₹50,000 |

All supplier, material, company, and transaction data used in this repository is invented for learning purposes.

---

## 5. Business Requirement

The production department identifies a requirement for 100 units of RM-1001.

The purchasing process must ensure that the material is procured from an approved supplier and that the supplier invoice is processed only after the relevant purchasing and receipt information has been validated.

The process should provide visibility into:

- Material requirement
- Purchase Requisition
- Purchase Order
- Goods Receipt
- Supplier invoice
- Invoice verification
- Supplier liability
- Payment
- Clearing
- GR/IR reconciliation

---

## 6. End-to-End Process

```text
Business Requirement
        ↓
Purchase Requisition
        ↓
PR Approval
        ↓
Purchase Order
        ↓
PO Approval / Release
        ↓
Goods Receipt
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

---

## 7. Process Steps

### Step 1: Identify Purchase Requirement

The production department determines that 100 units of RM-1001 are required.

```text
Required Quantity: 100 Units
Expected Unit Price: ₹500
Expected Purchase Value: ₹50,000
```

### Step 2: Create Purchase Requisition

A PR is created to record the internal requirement.

The PR includes:

- Material
- Quantity
- Required delivery date
- Plant
- Purchasing information

At this stage, the requirement is internal and does not create an FI accounting posting in this base scenario.

### Step 3: Approve Purchase Requisition

The PR goes through the applicable approval process.

- If approved, Purchasing can continue.
- If rejected, the requirement must be reviewed or corrected.

### Step 4: Create Purchase Order

Purchasing creates a PO for the selected supplier.

| Field | Synthetic Value |
|---|---|
| Supplier | V10001 - Alpha Industrial Supplies |
| Material | RM-1001 |
| Quantity | 100 Units |
| Unit Price | ₹500 |
| Net PO Value | ₹50,000 |
| Plant | PL01 |

The PO communicates the purchasing requirement to the supplier. PO creation does not itself create an FI posting in this base scenario.

### Step 5: Receive Material

The supplier delivers the ordered material.

The receiving team verifies:

- Correct material
- Correct quantity
- Valid PO reference
- Delivery information
- Over-delivery or under-delivery

If the receipt is acceptable, the GR is recorded.

### Step 6: Receive and Verify Supplier Invoice

The supplier submits an invoice. AP verifies the supplier, PO, GR, quantity, price, invoice reference, and possible duplicate conditions.

### Step 7: Post Invoice

If the invoice satisfies the applicable controls, it is posted and creates a supplier liability.

### Step 8: Pay and Clear Supplier

When the invoice becomes due and is approved, the supplier is paid and the related open item is cleared.

### Step 9: Reconcile GR/IR

Open GR/IR differences are reviewed, investigated, and resolved according to the applicable process.

---

## 8. Goods Receipt Accounting Concept

For this simplified valuated stock-material scenario:

| Account | Debit | Credit |
|---|---:|---:|
| Inventory | ₹50,000 | - |
| GR/IR Clearing | - | ₹50,000 |

```text
Dr Inventory ₹50,000
Cr GR/IR     ₹50,000
```

The exact accounting treatment depends on the configured business scenario and account determination.

---

## 9. Supplier Invoice

After supplying the material, the supplier submits an invoice.

| Field | Synthetic Value |
|---|---|
| Supplier | V10001 - Alpha Industrial Supplies |
| Material | RM-1001 |
| Invoice Quantity | 100 Units |
| Unit Price | ₹500 |
| Net Invoice Amount | ₹50,000 |
| PO Reference | Synthetic PO Reference |

The supplier invoice must be checked before normal processing continues.

---

## 10. Three-Way Match

The project compares:

1. Purchase Order
2. Goods Receipt
3. Supplier Invoice

### Quantity Check

```text
PO Quantity: 100
GR Quantity: 100
Invoice Quantity: 100
Result: Match
```

### Price Check

```text
PO Unit Price: ₹500
Invoice Unit Price: ₹500
Result: Match
```

Based on the synthetic data, the quantity, price, and value are consistent. This represents a successful Three-Way Match, subject to applicable SAP tolerances and invoice-verification settings.

---

## 11. Invoice Posting Accounting Concept

Using the simplified example before tax:

| Account | Debit | Credit |
|---|---:|---:|
| GR/IR Clearing | ₹50,000 | - |
| Supplier Payable | - | ₹50,000 |

```text
Dr GR/IR             ₹50,000
Cr Supplier Payable  ₹50,000
```

This creates the supplier liability.

Tax is handled separately later in the project so this base scenario remains focused on core FI-MM integration.

---

## 12. Supplier Payment

The invoice becomes eligible for payment according to the agreed payment terms, provided no unresolved payment blocks or exceptions remain.

```text
Dr Supplier Payable ₹50,000
Cr Bank             ₹50,000
```

This is a simplified payment entry. Actual payment processing can include bank-clearing accounts and additional configuration-dependent steps.

---

## 13. GR/IR Reconciliation

GR/IR open items may require investigation when:

- Goods are received but the invoice is missing
- An invoice exists but the GR is missing
- Quantities differ
- Amounts or prices differ
- References are incorrect
- Old items remain unresolved

The reconciliation section of this portfolio uses synthetic examples to demonstrate these situations.

---

## 14. Exception Scenarios

### Scenario A: Successful Three-Way Match

```text
PO Quantity: 100
GR Quantity: 100
Invoice Quantity: 100
Price: Match
```

Expected result: Normal invoice processing can continue, subject to applicable controls.

### Scenario B: Quantity Mismatch

```text
PO Quantity: 100
GR Quantity: 80
Invoice Quantity: 100
```

Expected investigation: Check whether the remaining goods are pending, the GR is missing, or the supplier invoiced an incorrect quantity.

### Scenario C: Price Mismatch

```text
PO Unit Price: ₹500
Invoice Unit Price: ₹520
Difference: ₹20 per unit
```

Expected investigation: Review the PO price, supplier invoice, approvals, and applicable tolerance settings.

### Scenario D: Invoice Received Before Goods Receipt

A PO exists and the invoice has been received, but the expected GR has not yet been recorded.

Expected investigation: Confirm whether the material was received and determine whether the missing GR is caused by timing, process delay, or a receiving issue.

### Scenario E: Potential Duplicate Invoice

Two invoices contain the same supplier, invoice reference, and amount.

Expected investigation: Review the documents before proceeding to prevent duplicate processing or payment.

---

## 15. Project Objectives

By completing this project, I aim to demonstrate an understanding of:

- SAP S/4HANA P2P
- FI-MM integration
- Accounts Payable
- Purchase Requisitions
- Purchase Orders
- Goods Receipts
- Invoice verification
- Three-Way Match
- GR/IR
- Supplier accounting
- Payments and clearing
- Reconciliation
- Business-process exceptions
- Functional testing

---

## 16. Project Deliverables

The repository will gradually include:

- Business scenario
- P2P glossary
- Process-flow documentation
- Accounting-entry examples
- Configuration rationale
- Synthetic master-data templates
- Three-Way Match scenarios
- Test scripts
- Exception analysis
- Defect log
- User guide
- Project presentation
- Interview questions
- Lessons learned

---

## 17. Repository Structure

```text
sap-p2p-process-design/
├── README.md
├── 01-business-requirements/
├── 02-process-flows/
├── 03-accounting-entries/
├── 04-configuration-rationale/
├── 05-master-data-templates/
├── 06-three-way-match/
├── 07-test-scripts/
├── 08-exception-analysis/
├── 09-defect-log/
├── 10-user-manual/
├── 11-project-presentation/
├── 12-interview-questions/
└── 13-lessons-learned/
```

---

## 18. Learning Disclaimer

This repository is an independent SAP S/4HANA learning portfolio.

All companies, suppliers, materials, POs, invoices, amounts, organizational structures, and transaction data used in the project are fictional or synthetic.

The repository does not contain employer, customer, supplier, or other confidential business information.

SAP and SAP S/4HANA are trademarks of SAP SE.

This project is not affiliated with or endorsed by SAP SE.
