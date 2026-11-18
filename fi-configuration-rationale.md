# FI Configuration Rationale

## 1. Purpose

This document explains the Financial Accounting (FI) design concepts required to support the synthetic Nova Manufacturing Pvt. Ltd. Procure-to-Pay scenario in SAP S/4HANA.

It focuses on configuration rationale, control points, and FI-MM integration. It does not claim that configuration was performed in a live SAP system.

---

## 2. Scenario Summary

| Element | Synthetic value |
|---|---|
| Company Code | NM01 |
| Company Code Currency | INR |
| Supplier | V10001 - Alpha Industrial Supplies |
| Material | RM-1001 |
| Quantity | 100 units |
| Unit Price | ₹500 |
| Purchase Value | ₹50,000 |
| Payment Terms | 30 days |
| Tax | Excluded from the base example; to be designed separately |

Base accounting flow:

```text
Goods Receipt
Dr Inventory                       ₹50,000
    Cr GR/IR Clearing                         ₹50,000

Invoice Receipt
Dr GR/IR Clearing                  ₹50,000
    Cr Supplier                               ₹50,000

Payment
Dr Supplier                        ₹50,000
    Cr Bank                                   ₹50,000
```

---

## 3. Company Code and Global Parameters

**Design:** Company code `NM01` represents the accounting entity for the scenario.

Key decisions to confirm in a real project include:

- Company-code name and address
- Country/region and local statutory settings
- Local currency (`INR` in this example)
- Chart of accounts
- Fiscal year variant
- Posting period variant
- Field status variant
- Tax procedure and tax jurisdiction requirements
- Ledgers and accounting principles

**Rationale:** Procurement transactions become legal and financial obligations at company-code level. The organizational assignment from plant PL01 to company code NM01 ensures that material movements create FI documents for the correct accounting entity.

---

## 4. Chart of Accounts and G/L Design

The scenario requires G/L accounts for at least:

- Raw-material inventory
- GR/IR clearing
- Price differences, when applicable
- Consumption or expense accounts for non-stock procurement
- Input tax, when applicable
- Supplier reconciliation account
- Bank clearing and bank accounts
- Freight or delivery costs, when applicable

No productive G/L account numbers are invented in this portfolio.

**Rationale:** A controlled chart-of-accounts design supports consistent classification, financial reporting, account determination, and reconciliation.

### Recommended design principles

- Use clear account descriptions and governance.
- Separate balance-sheet and profit-and-loss accounts correctly.
- Configure open-item management only where appropriate.
- Use line-item display where operational reconciliation requires it.
- Restrict direct postings to reconciliation and automatic-posting accounts as appropriate.

---

## 5. Fiscal Year and Posting Periods

### Fiscal Year Variant

The fiscal year variant defines the financial periods used by NM01.

**Rationale:** Every FI document requires a posting period. The selected fiscal calendar must match statutory reporting and management requirements.

### Posting Period Variant

The posting period variant controls which account types and periods are open.

**P2P control value:**

- Prevents AP and MM postings into closed periods.
- Supports controlled month-end close.
- Allows selected adjustment periods or account-type controls when required.

**Design note:** MM and FI period controls must be coordinated. A transaction may be operationally valid but fail if the relevant posting period is closed.

---

## 6. Document Types and Number Ranges

FI postings created by goods receipt, invoice receipt, and payment require appropriate document classification and numbering.

**Rationale:** Document types and number ranges provide:

- Traceability
- Controlled document numbering
- Distinction between operational sources
- Audit support
- Reporting filters

This portfolio does not prescribe productive document-type codes or number ranges because these are organization-specific.

---

## 7. Posting Keys and Field Status

Posting logic determines debit/credit behavior and account type, while field status controls which information is required, optional, suppressed, or display-only.

**Rationale:** Consistent field control reduces incomplete postings and supports reporting. Conflicts between posting-key field status and G/L field status should be resolved during design and testing.

Important P2P fields may include:

- Assignment
- Reference
- Text
- Cost center or internal order for expense procurement
- Business area, profit center, segment, or functional area where relevant
- Payment data on supplier line items

---

## 8. Supplier as Business Partner

In SAP S/4HANA, Business Partner is the leading object for supplier master data, with supplier roles supporting Financial Accounting and purchasing.

### Company-code data for NM01

- Reconciliation account
- Payment terms
- Payment methods
- Payment block defaults where applicable
- Dunning data where applicable
- Withholding-tax data where applicable
- Correspondence and tolerance-related data where applicable

### Purchasing-organization data for NP01

- Order currency
- Purchasing terms
- Partner functions
- GR-based invoice verification indicator where required
- Purchasing blocks and controls

**Rationale:** The same supplier relationship must support both procurement documents and FI subledger accounting.

### Master-data controls

- Separate request, approval, and maintenance where possible.
- Validate tax and bank information using approved procedures.
- Monitor duplicate suppliers and sensitive changes.
- Restrict changes to reconciliation account and payment data.

---

## 9. Supplier Reconciliation Account

Supplier invoices post to the supplier subledger and update the assigned reconciliation account in the General Ledger.

```text
Supplier subledger
        ↓
Reconciliation account
        ↓
General Ledger and financial statements
```

**Rationale:** The reconciliation account connects detailed supplier balances to the G/L. Direct manual postings should normally be prevented so that the G/L remains aligned with the supplier subledger.

---

## 10. Payment Terms and Baseline Date

**Synthetic design:** 30-day payment term.

Payment terms influence due-date calculation and, where configured, cash discounts.

Key design questions:

- What determines the baseline date?
- Are discount periods required?
- May users change payment terms at document level?
- Which terms are allowed for each supplier category?
- How are blocked or disputed invoices handled?

**Rationale:** Accurate terms support working-capital management, on-time payment, discount capture, and supplier relationships.

---

## 11. Payment Methods and Automatic Payment Processing

A real design would define allowed payment methods at country, company-code, supplier, and document level as applicable.

Key considerations:

- Eligible payment methods
- Minimum and maximum amounts
- Bank determination and ranking order
- House banks and account IDs
- Payment media and file controls
- Payment proposal review
- Dual approval and release controls
- Exception handling and payment blocks

**Rationale:** Invoice posting creates the liability; controlled payment processing settles it. Master data, document data, due date, payment method, bank selection, and payment blocks must work together.

---

## 12. Tax Configuration Considerations

Tax is excluded from the ₹50,000 base entry so the core FI-MM flow remains clear.

A production design must confirm:

- Applicable tax procedure
- Input tax codes and rates
- Recoverable and non-recoverable tax treatment
- Tax account determination
- Withholding tax where applicable
- Place-of-supply, registration, and reporting requirements
- Treatment of freight, planned delivery costs, and credit memos

**Rationale:** Tax cannot be safely represented by a generic percentage without legal and transactional context.

---

## 13. GR/IR Clearing

GR/IR bridges the timing difference between goods receipt and invoice receipt.

### Matched case

At goods receipt:

```text
Dr Inventory      ₹50,000
    Cr GR/IR                  ₹50,000
```

At invoice receipt:

```text
Dr GR/IR          ₹50,000
    Cr Supplier               ₹50,000
```

The matched amounts offset in GR/IR.

### Why balances may remain

- Goods received but not invoiced
- Invoice received for a different quantity
- Price variance
- Partial delivery or partial invoice
- Reversal or return
- Incorrect PO or invoice reference

**Rationale:** GR/IR should be monitored and reconciled. Old or unusual balances may indicate timing differences, data issues, or process exceptions.

---

## 14. Automatic Account Determination

MM transactions should determine the correct G/L accounts using the configured valuation and transaction logic.

Conceptual transaction keys relevant to the base case include:

| Transaction key | Conceptual use | Base-case relationship |
|---|---|---|
| BSX | Inventory posting | Debit inventory at stock goods receipt |
| WRX | GR/IR clearing | Credit at goods receipt; debit at invoice receipt |
| PRD | Price differences | Used when configured conditions produce a price difference |

Actual determination depends on the chart of accounts, valuation grouping, valuation class, material settings, movement type, and transaction event.

**Rationale:** Automatic account determination standardizes postings and removes the need for end users to choose G/L accounts for routine material movements.

---

## 15. Invoice Posting and Duplicate Controls

Invoice verification should support:

- Purchase-order reference
- Goods-receipt reference where applicable
- Quantity and price comparison
- Tax validation
- Duplicate-invoice checks
- Payment blocks for relevant variances
- Clear reference and invoice-number capture

**Rationale:** AP accuracy depends on both system controls and disciplined data entry. Duplicate checks are stronger when supplier, invoice reference, company code, date, and amount are captured consistently.

---

## 16. Tolerances and Payment Blocks

Tolerance design determines whether differences are accepted, warned, or blocked for payment.

Example:

- PO: 100 × ₹500 = ₹50,000
- Invoice: 100 × ₹540 = ₹54,000
- Difference: ₹4,000

The response depends on configured tolerance keys, limits, user tolerances, and business rules.

**Rationale:** Tolerances must balance control and efficiency. Limits should be risk-based, approved, documented, and tested. This portfolio does not invent tolerance amounts.

---

## 17. FI Document Flow

| Event | Debit | Credit | Operational result |
|---|---|---|---|
| Goods Receipt | Inventory ₹50,000 | GR/IR ₹50,000 | Stock increases and receipt liability is accrued |
| Invoice Receipt | GR/IR ₹50,000 | Supplier ₹50,000 | Supplier open item is created |
| Payment | Supplier ₹50,000 | Bank ₹50,000 | Supplier open item is cleared |

For non-stock or account-assigned procurement, the debit may be an expense, asset, cost center, internal order, WBS element, or other object rather than inventory.

---

## 18. Period-End and Reconciliation Controls

Recommended controls include:

- Reconcile supplier subledger to the G/L.
- Review GR/IR ageing and unmatched items.
- Review blocked invoices.
- Review duplicate and parked invoices.
- Confirm open and closed periods.
- Reconcile bank clearing accounts.
- Monitor manual journals to P2P-related accounts.
- Review sensitive supplier-master changes.
- Document and approve write-offs or GR/IR adjustments.

---

## 19. Segregation of Duties

Potentially incompatible responsibilities include:

- Supplier creation and payment execution
- Supplier bank change and payment approval
- Purchase-order creation and goods receipt
- Invoice posting and payment release
- Configuration maintenance and production transaction processing

**Rationale:** Role design should prevent or detect one person controlling an entire high-risk transaction chain.

---

## 20. Configuration Decision Matrix

| Area | Learning design | Why | Confirm in a real implementation |
|---|---|---|---|
| Company Code | NM01 | Single accounting entity | Legal and statutory requirements |
| Currency | INR | Matches synthetic India scenario | Parallel currencies and ledger design |
| Supplier | BP with FI and purchasing roles | Supports FI-MM integration | Roles, number ranges, governance |
| Reconciliation Account | Conceptual AP account | Connects supplier subledger to G/L | Account and posting controls |
| Payment Terms | 30 days | Simple due-date example | Commercial terms and discounts |
| GR/IR | Automatic clearing concept | Bridges receipt and invoice timing | Account assignment and reconciliation |
| Tax | Excluded from base entry | Avoids unsupported assumptions | Full local tax design |
| Payment | Conceptual automatic-payment flow | Demonstrates settlement | Banks, methods, approvals, payment media |

---

## 21. Testing Scenarios

1. Full goods receipt and exact invoice match.
2. Partial goods receipt followed by partial invoice.
3. Price variance inside and outside tolerance.
4. Quantity variance inside and outside tolerance.
5. Invoice before goods receipt where business rules allow it.
6. Reversal of goods receipt.
7. Supplier credit memo.
8. Duplicate supplier invoice reference.
9. Payment block and approved release.
10. Automatic payment, clearing, and bank posting.
11. Closed FI or MM period.
12. Supplier-master payment-term or bank change under workflow.

---

## 22. Learning Outcome

This design shows that AP is not isolated from procurement. Correct supplier liabilities depend on organizational assignments, Business Partner data, material valuation, automatic account determination, goods movements, invoice verification, tolerances, tax, and payment configuration.

---

## 23. Scope and Disclaimer

All codes, names, amounts, and transactions are synthetic. Actual ledger, tax, account, document, workflow, payment, and control designs must be confirmed against business, statutory, security, and integration requirements.

---

## 24. References

- SAP Learning — [Maintaining Business Partner Master Data](https://learning.sap.com/courses/business-processes-in-sap-s-4hana-sourcing-procurement/maintaining-business-partner-master-data)
- SAP Learning — [Supplier Master Data Fields Relevant for Payables Management](https://learning.sap.com/courses/explaining-payables-management-master-data/explaining-supplier-master-data-fields-relevant-for-payables-management_c9fa0ac7-354f-45f5-a3f1-3e2964393059)
- SAP Help Portal — [WRX: GR/IR Clearing Account](https://help.sap.com/docs/s4hana-best-practices/ycoa/wrx-gr-ir-clearing-account?locale=en-US)
- SAP Help Portal — [BSX: Inventory Posting](https://help.sap.com/docs/SAP_S4HANA_BEST_PRACTICES/1f2ae10b96f740759d66e695f953aa8f/a1dbf3f2ed0145ca85705cd43f548da3.html?locale=en-US)
