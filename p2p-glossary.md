# SAP S/4HANA Procure-to-Pay (P2P) Glossary

## About This Glossary

This glossary is part of my independent SAP S/4HANA FI-MM learning project.

It explains important business and SAP concepts used in the end-to-end Procure-to-Pay (P2P) process. The goal is to understand how Materials Management (MM), Financial Accounting (FI), and Accounts Payable (AP) connect.

All business examples and data used in this project are synthetic.

---

## 1. Purchase Requisition (PR)

### Definition

A Purchase Requisition is an internal request to purchase a material or service.

It represents a business requirement before a Purchase Order is created for a supplier.

A PR can include:

- Material or service
- Quantity
- Required delivery date
- Plant
- Purchasing information
- Account assignment, where applicable

### Business Example

Nova Manufacturing requires:

```text
Material: RM-1001
Quantity: 100 Units
Plant: PL01
```

The requirement is recorded internally through a Purchase Requisition.

### Key Learning

A PR represents an internal requirement to purchase. It is not a supplier invoice and it is not a payment.

---

## 2. Purchase Order (PO)

### Definition

A Purchase Order is a purchasing document used to communicate an approved purchasing requirement to a supplier.

A PO can include:

- Supplier
- Material or service
- Quantity
- Unit price
- Delivery information
- Plant
- Purchasing organization
- Payment terms

### Business Example

| Field | Synthetic Value |
|---|---|
| Supplier | V10001 - Alpha Industrial Supplies |
| Material | RM-1001 |
| Quantity | 100 Units |
| Unit Price | ₹500 |
| Net PO Value | ₹50,000 |
| Plant | PL01 |

### Accounting Impact

In this base learning scenario, creating the PO does not itself create an FI accounting posting.

The financial impact begins when later transactions, such as Goods Receipt and Invoice Receipt, are posted.

### Key Learning

The PO connects the approved business requirement with the supplier and becomes an important reference for later P2P transactions.

---

## 3. Goods Receipt (GR)

### Definition

Goods Receipt records that material ordered from a supplier has been received.

For PO-based procurement, the GR is normally recorded with reference to the relevant PO.

### Important Checks

- Correct material
- Correct quantity
- Valid PO reference
- Over-delivery or under-delivery
- Delivery condition, where applicable

### Business Example

```text
PO Quantity: 100 Units
Actual Material Received: 100 Units
GR Quantity: 100 Units
```

### Simplified Accounting Concept

For this synthetic valuated-stock scenario:

| Debit | Credit |
|---|---|
| Inventory ₹50,000 | GR/IR ₹50,000 |

### Key Learning

GR records the physical receipt of material and creates an important connection between logistics and financial accounting.

---

## 4. Invoice Receipt (IR)

### Definition

Invoice Receipt represents the supplier invoice entering invoice verification and accounting processing.

For PO-based procurement, the supplier invoice can be checked against the PO and GR information.

### Business Example

```text
Supplier: V10001 - Alpha Industrial Supplies
Invoice Quantity: 100 Units
Unit Price: ₹500
Net Invoice Amount: ₹50,000
```

### Simplified Accounting Concept

Ignoring tax for the initial learning scenario:

| Debit | Credit |
|---|---|
| GR/IR ₹50,000 | Supplier ₹50,000 |

The supplier credit creates the liability owed to the supplier.

### Key Learning

Invoice Receipt connects supplier invoice processing with AP and FI-MM integration.

---

## 5. GR/IR Clearing Account

### Definition

GR/IR means Goods Receipt / Invoice Receipt.

The GR/IR clearing account helps manage the timing difference between receiving goods and receiving the corresponding supplier invoice.

### At Goods Receipt

```text
Dr Inventory ₹50,000
Cr GR/IR     ₹50,000
```

### At Invoice Receipt

```text
Dr GR/IR    ₹50,000
Cr Supplier ₹50,000
```

### Matching Situation

If the relevant GR and Invoice Receipt quantities and values correspond, the related GR/IR items can offset.

### Exception Situations

Open GR/IR items may require investigation when:

- Goods are received but the invoice is missing
- An invoice exists but the GR is missing
- Quantities differ
- Amounts or prices differ
- References are incorrect
- Old items remain unresolved

### Key Learning

GR/IR is a major FI-MM integration concept because it connects the physical receipt of goods with supplier invoice accounting.

---

## 6. Three-Way Match

### Definition

Three-Way Match compares information from:

1. Purchase Order
2. Goods Receipt
3. Supplier Invoice

The objective is to identify whether the purchasing, receiving, and invoicing data correspond according to applicable business rules and configured tolerances.

### Successful Example

| Document | Quantity | Unit Price | Value |
|---|---:|---:|---:|
| Purchase Order | 100 | ₹500 | ₹50,000 |
| Goods Receipt | 100 | - | ₹50,000 |
| Supplier Invoice | 100 | ₹500 | ₹50,000 |

**Result:** The example represents a successful match.

### Quantity Exception Example

```text
PO Quantity: 100
GR Quantity: 80
Invoice Quantity: 100
```

The difference requires investigation.

### Price Exception Example

```text
PO Unit Price: ₹500
Invoice Unit Price: ₹520
```

The price difference should be investigated according to applicable business rules, configuration, and tolerance settings.

### Key Learning

Three-Way Match is an important P2P control because it helps identify discrepancies before normal invoice and payment processing continues.

---

## 7. Supplier / Business Partner

### Definition

A supplier is an external party from whom an organization purchases materials or services.

SAP S/4HANA uses the Business Partner approach for relevant supplier master information.

Supplier information can include:

- Name and address
- Company-code data
- Purchasing data
- Payment information
- Bank information
- Tax information, where applicable

### Synthetic Example

| Field | Synthetic Value |
|---|---|
| Supplier ID | V10001 |
| Supplier Name | Alpha Industrial Supplies |
| Country | India |
| Currency | INR |

The supplier used in this project is fictional.

### Key Learning

Accurate supplier master data is important because errors can affect purchasing, invoice processing, and payment.

---

## 8. Payment Terms

### Definition

Payment Terms define the agreed conditions for when an invoice becomes due for payment.

They can also support cash-discount conditions, depending on the agreement and SAP configuration.

### Simple Example

```text
Invoice Date: 1 April
Payment Terms: 30 Days
```

The exact due date is determined using the configured payment-term rules and the relevant baseline date.

### Why Payment Terms Matter

Payment terms influence:

- Invoice due date
- Payment scheduling
- Cash discounts, where applicable
- Supplier payment processing
- AP cash planning

### Key Learning

Payment terms are more than comments on an invoice. They influence when a supplier liability becomes due.

---

## 9. Blocked Invoice

### Definition

An invoice may be blocked for payment or require additional review when a variance or another configured control condition is triggered.

Whether a block occurs depends on SAP configuration, tolerance settings, and business rules.

Potential causes include:

- Price variance
- Quantity variance
- Missing GR
- Other invoice exceptions
- Manual payment block

### Example

```text
Purchase Order: 100 Units × ₹500
Goods Receipt: 100 Units
Supplier Invoice: 100 Units × ₹520
```

The invoice price differs from the PO price and requires investigation.

### Possible Resolution

- Review the PO
- Check the GR
- Confirm the supplier invoice
- Contact Purchasing
- Correct the relevant business document, if required
- Obtain the required approval
- Remove the block only after the issue is resolved

### Key Learning

The objective is not simply to remove a block. First understand and resolve the root cause according to the applicable process.

---

## 10. Company Code

### Definition

A Company Code is an organizational unit used in Financial Accounting.

It is important for recording accounting transactions and producing financial statements.

### Synthetic Project Example

```text
Company: Nova Manufacturing Pvt. Ltd.
Company Code: NM01
```

NM01 is fictional and was created only for this project.

### Key Learning

Business transactions must be associated with the correct organizational units so that accounting and reporting are accurate.

---

## How These Concepts Connect

```text
Business Requirement
        ↓
Purchase Requisition
        ↓
Purchase Order
        ↓
Goods Receipt
        ↓
Supplier Invoice
        ↓
Invoice Verification
        ↓
Three-Way Match
        ↓
Supplier Liability
        ↓
Payment
        ↓
Clearing
        ↓
GR/IR Reconciliation
```

The flow demonstrates how MM purchasing activities connect with FI and AP.

---

## Synthetic Project Reference

| Field | Synthetic Value |
|---|---|
| Company | Nova Manufacturing Pvt. Ltd. |
| Company Code | NM01 |
| Plant | PL01 |
| Supplier | V10001 - Alpha Industrial Supplies |
| Material | RM-1001 |
| Quantity | 100 Units |
| Unit Price | ₹500 |
| Net Purchase Value | ₹50,000 |

---

## What I Learned

While preparing this glossary, I developed a clearer understanding of:

- How PR and PO differ
- How GR connects MM and FI
- How Invoice Receipt creates a supplier liability
- Why the GR/IR clearing account is needed
- How Three-Way Match identifies procurement discrepancies
- Why accurate supplier master data is important
- How payment terms influence AP
- Why invoice blocks require root-cause investigation
- How Company Code connects transactions with Financial Accounting

I will continue updating the glossary as my SAP S/4HANA FI-MM learning progresses.

---

## Project Disclaimer

This is an independent SAP S/4HANA learning and portfolio project.

All company names, suppliers, materials, Purchase Orders, invoices, amounts, Company Codes, and other transaction data shown in this project are fictional or synthetic.

No employer, customer, supplier, or other confidential information is used.

SAP and SAP S/4HANA are trademarks of SAP SE.

This project is not affiliated with or endorsed by SAP SE.
