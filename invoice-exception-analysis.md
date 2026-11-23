# Invoice Exception Analysis

## 1. Purpose

This document explains how invoice exceptions are identified, classified, analyzed, resolved, and prevented in the synthetic SAP S/4HANA Procure-to-Pay learning project.

> **Portfolio note:** All company names, suppliers, invoices, purchase orders, amounts, dates, and findings in this repository are synthetic. They do not represent an actual employer, supplier, customer, or SAP production system.

## 2. Business Context

A supplier invoice should normally match the purchasing and receiving information available in SAP:

```text
Purchase Order
      ↓
Goods Receipt
      ↓
Supplier Invoice
      ↓
Three-Way Match
      ↓
Post or Block Invoice
```

An exception occurs when the invoice cannot be processed normally because required information is missing, inconsistent, invalid, or outside an approved tolerance.

## 3. Analysis Objectives

The exception-analysis process should answer five questions:

1. **What failed?** Identify the exception category and affected document.
2. **Why did it fail?** Determine the underlying root cause, not only the error message.
3. **Who owns the correction?** Assign the issue to AP, Procurement, Receiving, Master Data, Tax, the requestor, or the supplier.
4. **How will it be resolved?** Record the immediate corrective action.
5. **How can recurrence be reduced?** Define a preventive control or process improvement.

## 4. Key Exception Categories

| Exception | Typical condition | Likely owner | Main risk |
|---|---|---|---|
| Price Variance | Invoice price differs from PO price | Buyer / AP | Overpayment or invoice block |
| Quantity Variance | Invoice quantity exceeds GR or PO quantity | Receiving / AP | Payment delay or overbilling |
| Missing Goods Receipt | PO invoice exists without an available GR | Receiving / Requestor | Invoice remains blocked |
| Duplicate Invoice | Same supplier reference or invoice details already exist | AP | Duplicate payment |
| Missing or Invalid PO | Invoice has no valid PO reference | Requestor / Procurement | Manual effort and uncontrolled buying |
| Tax Mismatch | Tax value or treatment differs from expectation | Tax / AP | Incorrect tax posting or compliance exposure |
| Payment Terms Mismatch | Invoice, PO, and supplier master contain different terms | Procurement / Master Data | Wrong due date or lost discount |
| Account Assignment Error | Cost center, G/L, or project assignment is invalid | Requestor / Controlling | Posting failure or wrong expense allocation |
| Credit Memo Reference Issue | Original invoice reference is missing or unclear | Supplier / AP | Credit cannot be matched or cleared |
| Blocked or Unapproved PO | Required PO approval is incomplete | Approver / Procurement | Invoice cannot proceed |
| Freight Variance | Invoice freight is absent from PO pricing conditions | Buyer | Price variance and manual investigation |
| Missing Invoice Number | Supplier document has no reliable invoice reference | Supplier / AP | Duplicate-control and posting risk |

## 5. Root Cause Classification

The register separates the visible exception from its underlying cause.

### Supplier error

Examples:

- Supplier billed a price different from the PO.
- Supplier entered the wrong PO number.
- Supplier resubmitted the same invoice.
- Credit memo does not include the original invoice reference.

### Process timing

Examples:

- Invoice arrived before goods receipt posting.
- Remaining delivery is still in transit.
- Approval is not completed before invoice receipt.

### Master-data issue

Examples:

- Payment terms are outdated.
- Cost center is invalid or expired.
- Supplier purchasing or company-code data is incomplete.

### PO configuration or maintenance issue

Examples:

- Freight or surcharge was not included in PO conditions.
- Account assignment was entered incorrectly.
- PO price was not updated after an approved commercial change.

### User or process gap

Examples:

- Receipt was not posted after physical delivery.
- Requestor did not provide adequate purchasing information.
- Buyer selected an incorrect document or item setup.

### System-configuration issue

Examples:

- Tolerance limits do not reflect approved business policy.
- Duplicate checking is not configured as expected.
- Automatic account determination selects an incorrect G/L account.

A system configuration cause should only be selected after verifying the transaction data, master data, and process steps.

## 6. Practical Analysis Method

For each exception, follow this sequence:

### Step 1: Confirm the invoice data

Check the supplier, invoice reference, invoice date, PO number, currency, quantity, amount, tax, and payment terms.

### Step 2: Review the purchase order

Confirm:

- PO supplier
- Ordered quantity
- PO price and currency
- Delivery status
- Pricing conditions such as freight or discounts
- Account assignment
- Payment terms
- Approval status

### Step 3: Review goods receipt and PO history

Check whether goods or services were received, whether the quantity is complete or partial, and whether any receipt was reversed.

### Step 4: Compare invoice, PO, and GR

Use a three-way comparison:

| Check | Example |
|---|---|
| PO | 10 EA × INR 50,000 = INR 500,000 |
| GR | 8 EA received |
| Invoice | 10 EA × INR 50,000 = INR 500,000 |
| Finding | Quantity variance because invoice exceeds received quantity |

### Step 5: Identify configuration influence

Determine whether the outcome is controlled by:

- Invoice tolerance configuration
- GR-based invoice verification
- Duplicate invoice controls
- Payment terms
- Tax code
- PO pricing conditions
- Workflow status
- Account assignment
- Automatic account determination

### Step 6: Assign ownership and target date

Assign the exception to the function capable of correcting the root cause. AP should not be assigned every issue merely because AP received the invoice.

### Step 7: Record correction and prevention

The resolution action fixes the current invoice. The preventive action reduces recurrence.

Example:

```text
Resolution:
Obtain a corrected invoice that matches the approved PO price.

Prevention:
Require supplier PO confirmation before shipment and invoicing.
```

## 7. Worked Scenarios

### Scenario A: Price variance

```text
PO Value       INR 500,000
Invoice Value  INR 525,000
Variance       INR 25,000
```

**Analysis:** The supplier billed above the PO price. Check whether the commercial change was approved but not updated in the PO, or whether the supplier invoice is incorrect.

**Possible result:** The invoice is blocked when the configured price tolerance is exceeded.

**Resolution:** Correct the PO only with proper approval, or obtain a corrected invoice.

**Preventive control:** Buyer and supplier should confirm the final PO price before invoicing.

### Scenario B: Quantity variance

```text
PO Quantity       10 EA
Received Quantity  8 EA
Invoice Quantity  10 EA
```

**Analysis:** Determine whether two units are still in transit, physically received but not posted, or incorrectly invoiced.

**Resolution:** Post a valid missing GR after confirming receipt, wait for delivery, or request a corrected invoice.

**Preventive control:** Review open receipts and remind receiving users to post GR promptly.

### Scenario C: Duplicate invoice

```text
Supplier       SUP1003
Invoice Ref.   INV-10004
Amount         INR 120,000
Existing Item  Same reference and amount already posted
```

**Analysis:** Confirm whether this is a resubmission, a legitimate corrected invoice, or a different document using the same reference.

**Resolution:** Stop posting and reject the duplicate when the original invoice is valid.

**Preventive control:** Use duplicate-invoice checks based on supplier, reference, date, and amount.

### Scenario D: Missing goods receipt

**Analysis:** Confirm delivery with the requestor or receiving team. Do not create a receipt only to release payment if goods or services were not actually accepted.

**Resolution:** Post the GR only after valid confirmation or return/dispute the invoice.

**Preventive control:** Monitor PO items with overdue receipts and outstanding supplier invoices.

## 8. Priority Guidance

| Priority | Use when |
|---|---|
| Critical | Duplicate-payment exposure, major financial misstatement, or an exception stopping an essential end-to-end process |
| High | Core invoice cannot be posted, material variance exists, or financial/tax impact requires urgent correction |
| Medium | Process delay or data issue exists but immediate financial exposure is limited |
| Low | Minor administrative issue with no material posting or payment impact |

## 9. Status Guidance

| Status | Meaning |
|---|---|
| Open | Exception is identified but work has not started |
| In Progress | Owner is actively investigating or correcting it |
| Awaiting Supplier | Corrected document or clarification is required from supplier |
| Pending Business Input | Requestor, receiver, buyer, or another function must respond |
| Pending Approval | Approval is required before correction or processing |
| Resolved | Corrective action is completed and the invoice outcome is confirmed |
| Closed | Resolution is verified and no further action is required |

## 10. Metrics to Review

The `Invoice_Exception_Register.xlsx` workbook can support these measures:

- Total exception count
- Open exception count
- High and Critical exception count
- Exception value
- Exception count by category
- Exception count by root cause
- Exception count by responsible function
- Aging in days
- Resolved versus unresolved exceptions
- Repeat issues that require preventive action

A high exception count does not automatically mean AP performance is poor. The analysis should show whether causes originate in supplier behavior, purchasing, receiving, master data, approval timing, configuration, or invoice processing.

## 11. Workbook Structure

The Excel workbook contains:

### `Exception Register`

A detailed log with synthetic examples, ownership, status, aging, resolution, preventive action, impact, and test-case linkage.

### `Summary`

A formula-based dashboard showing exception volume, status, priority, invoice value, and category counts.

### `Reference Guide`

A quick guide to common exception categories, owners, actions, controls, and risks.

## 12. Relationship to the P2P Portfolio

This section extends the earlier project content:

```text
Business Requirement
        ↓
Process Flow
        ↓
Accounting Entries
        ↓
Configuration Rationale
        ↓
Master Data
        ↓
Three-Way Match
        ↓
Functional Test Cases
        ↓
Invoice Exception Analysis
```

It demonstrates how a functional learner moves from identifying an invoice block to analyzing its process, master-data, supplier, and configuration causes.

## 13. Confidentiality Rule

Use only synthetic or fully anonymized data in this public portfolio. Do not upload:

- Employer invoices or screenshots
- Real supplier names or IDs
- Real purchase orders or invoice numbers
- Bank information
- Employee names or email addresses
- SAP production-system screenshots
- Internal procedures, reports, or tolerance values

The objective is to demonstrate P2P analysis skills without revealing confidential information or overstating implementation experience.
