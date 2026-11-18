# SAP S/4HANA FI-MM | Three-Way Match Scenarios

> Independent learning project using fictional data. This document explains the business logic; it does not claim access to or configuration of a live SAP system.

Three-way matching compares the **Purchase Order (PO)**, **Goods Receipt (GR)**, and **Supplier Invoice** before payment. The aim is simple: confirm that the business ordered the item, received the supported quantity, and was invoiced at the expected price.

---

## 1. Base Business Scenario

| Field | Synthetic value |
|---|---|
| Company | Nova Manufacturing Pvt. Ltd. |
| Company Code | `NM01` |
| Plant / Storage Location | `PL01` / `SL01` |
| Purchasing Organization / Group | `NP01` / `PG01` |
| Supplier | `V10001` - Alpha Industrial Supplies |
| Material | `RM-1001` - Synthetic Production Material |
| PO Quantity | 100 units |
| PO Unit Price | ₹500 |
| PO Net Value | ₹50,000 |
| Payment Terms | 30 days |
| Tax | Excluded from the base example |

All names, codes, quantities, and values are fictional.

---

## 2. Three-Way Match at a Glance

```text
PURCHASE ORDER
What did we approve?
Quantity: 100 | Price: ₹500 | Value: ₹50,000
        |
        v
GOODS RECEIPT
What did we receive?
Quantity: 100 | Value: ₹50,000
        |
        v
SUPPLIER INVOICE
What is the supplier charging?
Quantity: 100 | Price: ₹500 | Value: ₹50,000
        |
        v
MATCH RESULT
Quantity matches + price matches + basic controls pass
        |
        v
Continue through the normal invoice process
subject to approvals and configured controls
```

---

## 3. What Is Compared?

| Control | PO evidence | GR evidence | Invoice evidence | Question answered |
|---|---|---|---|---|
| Supplier | Approved supplier | PO reference | Invoice supplier | Is the invoice from the expected supplier? |
| Material or service | PO item | Received item | Invoice item/reference | Is the billed item supported? |
| Quantity | Ordered quantity | Received quantity | Invoiced quantity | Are we paying only for supported quantity? |
| Unit price | PO price/conditions | Reference value | Invoice price | Is the supplier charging the expected price? |
| Unit of measure | PO unit | Receipt unit | Invoice unit | Are quantities being compared on the same basis? |
| Duplicate check | PO history | Receipt history | Supplier invoice reference | Has this invoice already been entered? |
| Tax and charges | PO conditions where applicable | Delivery evidence | Invoice tax/freight | Are additional charges supported and correctly treated? |

The exact checks, tolerances, warnings, and payment blocks depend on the SAP configuration and the organization's approved business rules.

---

## 4. Exact-Match Example

| Document | Quantity | Unit Price | Net Value | Result |
|---|---:|---:|---:|---|
| Purchase Order | 100 | ₹500 | ₹50,000 | Approved basis |
| Goods Receipt | 100 | ₹500 reference value | ₹50,000 | Full receipt |
| Supplier Invoice | 100 | ₹500 | ₹50,000 | Exact match |

### Result

- ✅ Supplier and document reference are consistent.
- ✅ Invoice quantity equals the received quantity.
- ✅ Invoice price equals the PO price.
- ✅ No duplicate is identified in the learning example.
- ✅ The invoice may continue through the normal process, subject to workflow and other configured checks.

### Simplified accounting flow

| Event | Debit | Credit | Amount |
|---|---|---|---:|
| Goods Receipt | Inventory | GR/IR | ₹50,000 |
| Invoice Receipt | GR/IR | Supplier | ₹50,000 |
| Payment | Supplier | Bank | ₹50,000 |

The matched GR and invoice amounts offset in GR/IR in this simplified base case.

---

## 5. Decision Logic Used in This Portfolio

```text
Is the supplier and PO reference valid?
        |
        +-- No --> Stop and investigate
        |
        v
Is a duplicate invoice indicated?
        |
        +-- Yes --> Stop and investigate
        |
        v
Does invoice quantity have sufficient receipt support?
        |
        +-- No --> Review quantity variance / missing GR
        |
        v
Does invoice price agree with the PO?
        |
        +-- No --> Review price variance
        |
        v
Are tax, unit of measure, and other charges supported?
        |
        +-- No --> Review supporting evidence
        |
        v
Exact match or acceptable result under approved rules
```

**Important:** A variance does not always mean the invoice is rejected. Depending on the scenario and configuration, SAP may allow posting, issue a warning, or place a payment block. The responsible team must investigate and document the resolution.

---

## 6. Scenario Summary

| ID | Scenario | PO Qty | GR Qty | Invoice Qty | PO Price | Invoice Price | Expected business result |
|---|---|---:|---:|---:|---:|---:|---|
| S01 | Exact match | 100 | 100 | 100 | ₹500 | ₹500 | Normal processing |
| S02 | Partial receipt, matching partial invoice | 100 | 80 | 80 | ₹500 | ₹500 | Invoice matches received quantity; 20 units remain open on PO |
| S03 | Invoice quantity greater than GR | 100 | 80 | 100 | ₹500 | ₹500 | Review unsupported quantity; possible payment block |
| S04 | Partial invoice after full receipt | 100 | 100 | 90 | ₹500 | ₹500 | Partial invoice; open GR/IR remains for the balance |
| S05 | Higher invoice price | 100 | 100 | 100 | ₹500 | ₹540 | Review ₹4,000 adverse price variance |
| S06 | Lower invoice price | 100 | 100 | 100 | ₹500 | ₹480 | Review ₹2,000 favorable price variance |
| S07 | Invoice entered before GR | 100 | 0 | 100 | ₹500 | ₹500 | Review missing receipt according to process rules |
| S08 | Duplicate invoice reference | 100 | 100 | 100 | ₹500 | ₹500 | Stop and investigate duplicate risk |
| S09 | Supplier mismatch | 100 | 100 | 100 | ₹500 | ₹500 | Stop and correct supplier or PO reference |
| S10 | Unit-of-measure mismatch | 100 EA | 100 EA | 10 BOX | ₹500/EA | ₹5,000/BOX | Validate conversion before approval |
| S11 | Tax or charge mismatch | 100 | 100 | 100 | ₹500 | ₹500 | Review tax/freight support separately |
| S12 | Small price difference | 100 | 100 | 100 | ₹500 | ₹502 | Evaluate against approved tolerance; workbook threshold is illustrative only |

---

## 7. How to Handle Common Exceptions

### Quantity variance

Example: 80 units received, but 100 units invoiced.

1. Confirm the PO quantity and PO history.
2. Confirm whether 20 units were delivered but the GR was not posted.
3. Check for a receipt posted to the wrong PO item, plant, storage location, or unit of measure.
4. Ask Receiving to post or correct the GR only when physical receipt evidence exists.
5. If goods were not received, contact the supplier for a corrected invoice or credit memo.
6. Keep the payment block or exception status until the issue is resolved under company policy.

### Price variance

Example: PO price ₹500, invoice price ₹540, quantity 100.

```text
Invoice value              ₹54,000
PO value                   ₹50,000
Adverse price variance      ₹4,000
```

Check PO amendments, approved price changes, freight, currency, unit-of-measure conversion, and invoice-entry accuracy. Do not change the PO merely to force a match without valid commercial approval.

### Invoice before goods receipt

Confirm whether invoice-before-GR processing is permitted. If the goods have arrived, correct the missing operational receipt using approved evidence. If they have not arrived, do not create a receipt only to release payment.

### Duplicate invoice

Compare supplier, company code, invoice reference, invoice date, gross amount, currency, and PO history. A similar invoice may be a true duplicate, a corrected invoice, or a valid recurring charge, so evidence is required before rejection or posting.

### Supplier or PO mismatch

Stop processing and validate the invoice source. Do not move an invoice to a different supplier or PO without confirming the commercial and master-data relationship.

---

## 8. GR/IR Impact

GR/IR is a temporary clearing account between goods receipt and invoice receipt.

| Situation | Simplified GR/IR position | Meaning |
|---|---|---|
| GR ₹50,000 and IR ₹50,000 | ₹0 | Matched base case |
| GR ₹50,000 and IR ₹45,000 | ₹5,000 credit remaining | More value received than cleared by invoice |
| GR ₹40,000 and IR ₹50,000 | ₹10,000 debit difference in simplified comparison | Invoice exceeds supported receipt value; investigate |
| GR posted, no invoice | Credit remains | Goods received but not invoiced |
| Invoice posted, no GR | Debit-side timing/exception may remain | Invoice exists without matching receipt |

Actual posting behavior can also depend on price control, account determination, planned delivery costs, tax, exchange rates, subsequent debits/credits, and the exact invoice-verification design.

---

## 9. Roles and Responsibilities

| Role | Main responsibility in the match process |
|---|---|
| Requester | Confirms the business need and service evidence where applicable |
| Buyer / Purchasing | Maintains the approved PO, supplier, quantity, price, and changes |
| Receiving / Stores | Records the physical receipt accurately and on time |
| Accounts Payable | Captures invoice data, performs checks, and coordinates exceptions |
| Business Approver | Reviews exceptions according to authority limits |
| Supplier | Provides a valid invoice and correction or credit memo when required |
| Finance / GR/IR Owner | Reviews unresolved GR/IR and period-end items |
| Master Data / Controls | Maintains governed supplier and material data and monitors sensitive changes |

Segregation of duties should prevent one person from controlling the complete supplier-to-payment chain.

---

## 10. Evidence Checklist

Before resolving an exception, confirm the relevant evidence:

- [ ] Valid supplier invoice and unique invoice reference
- [ ] Correct company code, supplier, currency, and PO number
- [ ] Approved PO and approved PO changes
- [ ] Goods-receipt or service-entry evidence
- [ ] Quantity and unit-of-measure agreement
- [ ] Price, freight, and tax support
- [ ] Duplicate-invoice review
- [ ] Documented reason for any block release or manual adjustment
- [ ] Correct approver based on the organization's authority matrix
- [ ] Clear audit trail in comments, attachments, or workflow

---

## 11. SAP Reference Transactions

These are **learning references**, not proof of live-system execution. Availability depends on the SAP S/4HANA edition, release, activated applications, business roles, and company design.

| Purpose | Classic SAP GUI reference | S/4HANA approach |
|---|---|---|
| Display Purchase Order | `ME23N` | Purchase-order display/management apps or SAP GUI |
| Post / review Goods Movement | `MIGO` | Goods-movement apps or SAP GUI |
| Enter PO-based Supplier Invoice | `MIRO` | Supplier-invoice / invoice-verification apps or SAP GUI |
| Display Invoice Document | `MIR4` | Invoice-display apps or SAP GUI |
| Release Blocked Invoices | `MRBR` | Blocked-invoice management apps or SAP GUI |
| Display Supplier Line Items | `FBL1N` | Supplier line-item apps or SAP GUI |
| Analyze GR/IR | `MB5S` / `MR11` as applicable | GR/IR reconciliation and clearing apps/processes |

---

## 12. Using the Excel Workbook

The companion workbook, `Three_Way_Match_Scenarios.xlsx`, contains:

1. **Start Here** - purpose, color key, and quick instructions.
2. **Match Calculator** - editable PO, GR, invoice, and control fields.
3. **Scenario Library** - formula-driven examples of common match outcomes.
4. **Exception Workflow** - simple ownership and resolution steps.
5. **GR-IR Tracker** - estimated open balances for learning scenarios.
6. **Journal Entries** - simplified formula-driven postings and balance checks.
7. **SAP Reference** - process and transaction references with cautions.
8. **Notes & Shortcuts** - Excel tips, formula guide, and GitHub location.

### Workbook color key

- **Blue cells:** user inputs that may be changed.
- **Green cells:** formula results or completed checks.
- **Amber cells:** review, pending evidence, or attention required.
- **Red cells:** stop, block, or investigate.

The tolerance fields in the workbook are demonstration inputs only. They are not recommended production settings.

---

## 13. Key Learning Points

- A PO supports commercial approval, but it does not prove that goods were received.
- A GR supports physical receipt, but it does not by itself create the supplier invoice liability.
- The invoice must be compared with both purchasing and receipt evidence.
- Partial deliveries and partial invoices can be valid; they must be understood rather than automatically treated as errors.
- Tolerance is a control design, not permission to ignore a variance.
- Posting an invoice and releasing it for payment are separate control points.
- GR/IR ageing helps identify missing invoices, missing receipts, incorrect references, and unresolved differences.
- Good exception handling depends on evidence, ownership, approval, and an audit trail.

---

## 14. Scope and Disclaimer

This portfolio uses a simplified stock-material scenario. It does not fully model services, account-assigned procurement, evaluated receipt settlement, consignment, subcontracting, imports, withholding tax, foreign currency, planned delivery costs, returns, subsequent debit/credit, or country-specific tax rules.

Actual SAP behavior depends on master data, PO design, invoice-verification settings, tolerance keys, workflow, authorizations, valuation, price control, account determination, tax, and organizational policy.

---

## 15. References

- SAP Learning - [Processing Invoices](https://learning.sap.com/courses/inventory-management-and-physical-inventory-in-sap-s-4hana/processing-invoices)
- SAP Learning - [Handling Variances Without Item Reference](https://learning.sap.com/courses/exploring-foundations-of-logistics-invoice-verification-in-sap-s-4hana-cloud-private-edition/handling-variances-without-item-reference)
- SAP Learning - [Maintaining Business Partner Master Data](https://learning.sap.com/courses/business-processes-in-sap-s-4hana-sourcing-procurement/maintaining-business-partner-master-data)
- SAP Help Portal - [WRX: GR/IR Clearing Account](https://help.sap.com/docs/s4hana-best-practices/ycoa/wrx-gr-ir-clearing-account?locale=en-US)

---

**Repository location:** `06-three-way-match/three-way-match.md`
