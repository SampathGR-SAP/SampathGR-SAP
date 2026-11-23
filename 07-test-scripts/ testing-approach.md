# P2P Functional Testing Approach

## 1. Purpose

This document describes the functional testing approach for the synthetic SAP S/4HANA Procure-to-Pay (P2P) learning project. It explains how the configured procurement process will be checked from purchase requisition through supplier invoice posting.

> **Portfolio note:** This is a learning project created with synthetic business data. It does not represent a live customer implementation.

## 2. Testing Objectives

The testing activities are designed to confirm that:

- Purchase requisitions and purchase orders follow the intended business process.
- Approval rules work for the defined purchase-order value limits.
- Goods receipts update quantities, document flow, inventory, and FI postings correctly.
- Supplier invoices are checked against the purchase order and goods receipt.
- Price and quantity variances trigger the expected tolerance or blocking controls.
- MM transactions create the expected FI accounting impact.
- Master data, source determination, pricing, and account assignment support the P2P process.

## 3. Scope

### In scope

- Purchase requisition creation
- Purchase order creation with PR reference
- Purchase order approval workflow
- Source determination
- Purchase-order pricing conditions
- Full and partial goods receipt
- Goods-receipt reversal
- Three-way match
- Quantity and price variance handling
- Duplicate invoice control
- Credit memo processing
- Stock and consumable procurement
- Automatic account determination checks
- Supplier payment terms
- End-to-end P2P document flow

### Out of scope

- Production system performance testing
- Interfaces with external supplier portals
- Bank payment file generation
- Country-specific tax configuration
- Security penetration testing
- Real customer master data or business transactions

## 4. Test Levels

### 4.1 Unit / Configuration Testing

Each configuration area is checked separately. Examples:

- Confirm a source list proposes the fixed supplier.
- Confirm the PO approval threshold is recognized.
- Confirm movement type 101 creates the expected material and accounting documents.
- Confirm automatic account determination selects the required inventory and GR/IR accounts.

### 4.2 Integration Testing

Connected process steps are tested together:

1. Create purchase requisition.
2. Convert the requisition into a purchase order.
3. Complete the required approval.
4. Post the goods receipt.
5. Post the supplier invoice.
6. Review PO history and accounting documents.

The main focus is the integration between Materials Management and Financial Accounting.

### 4.3 Negative Testing

Negative tests confirm that controls respond correctly when data does not match. Examples:

- Invoice quantity is higher than the received quantity.
- Invoice price is higher than the PO price.
- The same supplier invoice reference is entered twice.
- Required account-assignment data is missing.
- A PO exceeds the approval threshold.

### 4.4 Regression Testing

Important end-to-end tests should be repeated after a configuration correction. At minimum, repeat:

- Standard PR-to-PO process
- Full goods receipt
- Exact-match invoice
- Price-variance invoice
- Quantity-variance invoice
- FI-MM accounting-document review

## 5. Synthetic Business Scenario

The project uses the following sample design:

| Object | Example |
|---|---|
| Company Code | SM01 |
| Plant | BLR1 |
| Purchasing Organization | IN01 |
| Purchasing Group | P01 |
| Storage Location | RM01 |
| Material | LAPTOP-001 |
| Supplier | SUP1001 - ABC Technologies |
| Cost Center | IT100 |
| Standard Quantity | 10 EA |
| Unit Price | INR 50,000 |
| Approval Threshold | Additional approval above INR 500,000 |

## 6. Entry and Exit Criteria

### Entry criteria

Testing can begin when:

- The relevant configuration is available.
- Required synthetic master data is prepared.
- Test users have the necessary access.
- Expected results and accounting impacts are documented.
- The test environment is available.

### Exit criteria

Testing is considered complete when:

- All Critical and High-priority test cases have been executed.
- Failed cases have a recorded defect or clear explanation.
- Critical defects are resolved and retested.
- Actual results and evidence are recorded.
- The end-to-end P2P scenario is completed successfully.

## 7. Test Execution Method

For each test case in `P2P_Functional_Test_Cases.xlsx`:

1. Review the objective, preconditions, and test data.
2. Perform the documented steps in sequence.
3. Compare the system result with the expected result.
4. Check the material document, PO history, invoice status, and accounting document where applicable.
5. Record the actual result.
6. Set the status to `Pass`, `Fail`, `Blocked`, or `Not Applicable`.
7. Record a defect ID when the result does not match the expectation.
8. Add evidence such as a screenshot name or document number.

## 8. Status Definitions

| Status | Meaning |
|---|---|
| Not Executed | Testing has not started. |
| Pass | Actual result matches the expected result. |
| Fail | Actual result does not match the expected result. |
| Blocked | Testing cannot continue because of an access, data, configuration, or environment issue. |
| Not Applicable | The test does not apply to the final project scope. |

## 9. Priority Definitions

| Priority | Meaning |
|---|---|
| Critical | Essential end-to-end business process or major financial posting. |
| High | Important control or core procurement transaction. |
| Medium | Supporting process or master-data behavior. |
| Low | Minor or optional scenario. |

## 10. Expected FI-MM Checks

### Goods Receipt

For a valuated stock-material receipt:

```text
Inventory Account     Dr
    GR/IR Clearing        Cr
```

### Supplier Invoice

For a matching PO-based invoice, excluding tax for simplicity:

```text
GR/IR Clearing        Dr
    Supplier/Vendor       Cr
```

### Goods Receipt Reversal

```text
GR/IR Clearing        Dr
    Inventory Account     Cr
```

The exact G/L accounts depend on the configured valuation class, transaction/event key, valuation grouping, and chart of accounts. Therefore, test execution should verify the actual accounting document instead of relying only on the expected description.

## 11. Defect Handling

When a test fails, record:

- Defect ID
- Related test-case ID
- Short description
- Expected result
- Actual result
- Severity or priority
- Screenshot/document number
- Current status
- Retest result

A defect should be closed only after the correction is moved to the test environment and the failed test case is executed again successfully.

## 12. Evidence Guidelines

Use synthetic or masked evidence only. Suitable evidence includes:

- PR or PO document number
- Material document number
- Supplier invoice document number
- Accounting document number
- Workflow log screenshot
- Invoice block reason
- PO history screenshot

Do not upload confidential company, supplier, employee, bank, invoice, or production-system data to a public GitHub repository.

## 13. Repository Files

```text
07-test-scripts/
├── P2P_Functional_Test_Cases.xlsx
└── testing-approach.md
```

The Excel workbook contains:

- `Test Cases`: 18 functional scenarios with expected results and accounting impact
- `Summary`: execution-status and priority dashboard
- `Sample Test Data`: synthetic data used across the scenarios

## 14. Learning Outcome

This testing package demonstrates an understanding of how functional consultants translate business requirements and configuration decisions into structured test cases. It supports the larger SAP S/4HANA P2P learning portfolio without claiming real implementation experience.
