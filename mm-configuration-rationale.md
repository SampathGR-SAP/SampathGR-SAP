# MM Configuration Rationale

## 1. Purpose

This document explains the Materials Management (MM) configuration concepts that support the Nova Manufacturing Pvt. Ltd. Procure-to-Pay learning scenario in SAP S/4HANA.

It focuses on organizational assignments, master data, purchasing, inventory management, material valuation, logistics invoice verification, automatic account determination, and integration with FI.

> **Portfolio note:** This is a conceptual configuration rationale using synthetic data, not evidence of configuration in a live system.

---

## 2. Scenario Summary

| Element | Synthetic value |
|---|---|
| Company Code | NM01 |
| Plant | PL01 |
| Storage Location | SL01 |
| Purchasing Organization | NP01 |
| Purchasing Group | PG01 |
| Supplier | V10001 - Alpha Industrial Supplies |
| Material | RM-1001 - Synthetic Production Material |
| Procurement Type | External procurement |
| Quantity | 100 units |
| Unit Price | ₹500 |
| PO Value | ₹50,000 |
| Payment Terms | 30 days |

Process scope:

```text
Purchase Requisition
        ↓
Purchase Order
        ↓
Goods Receipt
        ↓
Logistics Invoice Verification
        ↓
Supplier Payment in FI
```

---

## 3. Organizational Structure

### Plant PL01

PL01 is the manufacturing and receiving location.

**Rationale:** Purchasing and inventory transactions must identify where the material is required and received. PL01 is assigned to NM01 so relevant material movements generate FI postings for the correct company code.

### Storage Location SL01

SL01 is the raw-material stock location within PL01.

**Rationale:** Stock quantity is managed at storage-location level, giving an explicit destination for goods receipt and a basis for inventory control.

### Purchasing Organization NP01

NP01 is responsible for procurement for NM01 and PL01.

**Rationale:** The purchasing organization supports supplier purchasing data, purchasing documents, conditions, and procurement responsibility.

### Purchasing Group PG01

PG01 identifies the buyer or buying team.

**Rationale:** It provides operational ownership for requisition and PO follow-up. It is not an enterprise-structure assignment in the same way as the purchasing organization.

---

## 4. Organizational Assignments

| Assignment | Learning design | Business purpose |
|---|---|---|
| Plant to Company Code | PL01 → NM01 | Connects inventory movements to FI entity |
| Storage Location to Plant | SL01 → PL01 | Defines the stock-holding location |
| Purchasing Organization to Company Code | NP01 → NM01 | Establishes procurement responsibility for company code |
| Purchasing Organization to Plant | NP01 → PL01 | Permits procurement for the receiving plant |
| Standard Purchasing Organization | NP01 for PL01 | Provides a clear standard relationship |

The productive design could use centralized, cross-company, or plant-specific purchasing organizations. This simple model is chosen for traceability.

---

## 5. Supplier Business Partner

`V10001 - Alpha Industrial Supplies` represents the external supplier.

Required conceptual data includes:

- General Business Partner data
- Supplier role for Financial Accounting
- Supplier role for purchasing
- Purchasing-organization data for NP01
- Company-code data for NM01
- Order currency and purchasing terms
- Payment terms and reconciliation account
- Tax and bank data where legally and operationally required

**Rationale:** Purchasing requires supplier procurement data, while Accounts Payable requires company-code accounting data. S/4HANA Business Partner integration supports both views.

**Controls:** Sensitive supplier changes, duplicates, blocks, and bank details should be governed by workflow and segregation of duties.

---

## 6. Material Master

`RM-1001` is treated as a valuated stock material for the base case.

Relevant views may include:

- Basic Data
- Purchasing
- MRP
- Plant/Storage
- Accounting
- Costing

Key design attributes include:

- Material type
- Base unit of measure
- Purchasing group
- Valuation class
- Price control
- Standard or moving price as appropriate
- Goods-receipt processing time
- Batch management if required
- Shelf-life or quality-management data if required

**Rationale:** The material master drives purchasing, planning, inventory, valuation, and automatic postings. Incorrect master data can affect quantities, prices, accounts, and planning results.

---

## 7. Material Type and Number Range

The material type controls important behavior such as:

- Permitted number range
- Field selection
- Quantity and value updating
- Allowed views
- Account-category reference and valuation-class options

**Learning design:** RM-1001 is a raw-material-style stock item.

**Rationale:** A stock material must support both quantity and value management. Productive material type and number-range design must follow master-data governance and business requirements.

---

## 8. Units of Measure

**Base case:** 100 units at ₹500 per unit.

A real design must define:

- Base unit of measure
- Purchase-order unit
- Conversion factors
- Rounding rules
- Order-price unit where used

**Rationale:** Incorrect conversions can create quantity variances, inventory errors, and invoice blocks.

---

## 9. Material Valuation

The base scenario assumes the material is valuated at plant level for learning purposes.

At goods receipt:

```text
100 × ₹500 = ₹50,000

Dr Inventory      ₹50,000
    Cr GR/IR                  ₹50,000
```

Key valuation decisions include:

- Valuation level
- Valuation area
- Valuation class
- Price control
- Standard price or moving average price
- Split valuation, when required
- Material Ledger and currency/valuation settings, when required

**Rationale:** Valuation converts a physical goods movement into a financial amount and helps determine the G/L accounts.

---

## 10. Valuation Class and Account Category Reference

The valuation class groups materials for automatic account determination.

Conceptually:

```text
Material Type
    ↓
Account Category Reference
    ↓
Allowed Valuation Classes
    ↓
Material Valuation Class
    ↓
Automatic Account Determination
```

**Rationale:** Materials with similar financial treatment can post to appropriate inventory and consumption accounts without manual G/L selection.

No productive valuation class or G/L account is invented in this portfolio.

---

## 11. Automatic Account Determination

The MM-to-FI posting logic considers the transaction event and valuation information.

| Transaction key | Conceptual purpose | Base-scenario use |
|---|---|---|
| BSX | Inventory posting | Debit inventory for stock goods receipt |
| WRX | GR/IR clearing | Credit at GR and debit at invoice receipt |
| PRD | Price differences | Records relevant price differences when conditions require it |

Other keys may be relevant for consumption, freight, offsets, or tax depending on the process.

**Rationale:** Automatic determination produces consistent FI postings from MM transactions and reduces manual account-selection risk.

---

## 12. Purchase Requisition Design

The purchase requisition represents an internal requirement.

Base fields include:

- Material RM-1001
- Quantity 100
- Delivery date
- Plant PL01
- Storage location SL01 where applicable
- Purchasing group PG01

**Accounting impact:** No FI posting in the base case.

**Control rationale:** Requisition approval or flexible workflow can confirm business need, budget, and responsibility before supplier commitment.

---

## 13. Purchase Order Design

The purchase order represents the external purchasing commitment.

Base fields include:

- Supplier V10001
- Purchasing organization NP01
- Purchasing group PG01
- Company code NM01
- Material RM-1001
- Plant PL01
- Storage location SL01
- Quantity 100
- Price ₹500 per unit
- Payment terms 30 days

**Accounting impact:** No FI posting in the base case.

**Control rationale:** The PO provides the approved commercial reference for goods receipt and invoice verification.

### Document type and number range

A real design should determine:

- Standard and special PO document types
- Number ranges
- Field selection
- Allowed item categories
- Release or flexible-workflow requirements

---

## 14. Purchasing Info Records and Source Determination

Where used, a purchasing info record can store supplier-material purchasing data such as prices, planned delivery time, and order-unit information for the purchasing organization and plant context.

Other source-determination objects can include source lists, quota arrangements, and contracts.

**Rationale:** Controlled source data improves consistency and reduces manual price or supplier selection.

This base scenario assumes a known supplier and does not claim that all source-determination functions are configured.

---

## 15. Pricing Conditions

PO pricing can include:

- Gross price
- Discounts or surcharges
- Freight and planned delivery costs
- Taxes where applicable
- Effective price

**Rationale:** Condition technique determines the commercial value used by purchasing and invoice verification. Productive condition types, access sequences, and pricing procedures are organization-specific.

---

## 16. Goods Receipt

The base case uses a standard goods receipt against the PO.

Operational result:

- Stock quantity increases by 100 units in PL01/SL01.
- A material document is created.
- An accounting document is created for the valuated movement.
- PO history is updated.

Financial result:

```text
Dr Inventory      ₹50,000
    Cr GR/IR                  ₹50,000
```

### Movement type concept

The movement type controls quantity update, stock type, field selection, account grouping, and reversal behavior.

**Rationale:** Movement types should be governed carefully because they influence both inventory and financial postings. The portfolio refers conceptually to a standard GR and does not redesign SAP-delivered movement logic.

---

## 17. Stock Types and Quality Considerations

Goods may be received into unrestricted-use, quality-inspection, or blocked stock depending on process design.

**Rationale:** The stock type should reflect whether material can be consumed immediately. Quality-management integration, batch management, shelf life, and inspection requirements are outside the base case but should be assessed for manufacturing materials.

---

## 18. Logistics Invoice Verification

Invoice verification compares the supplier invoice with purchasing and receipt data.

Base relationship:

```text
Purchase Order:   100 units × ₹500 = ₹50,000
Goods Receipt:    100 units           ₹50,000
Supplier Invoice: 100 units × ₹500 = ₹50,000
```

Financial result:

```text
Dr GR/IR          ₹50,000
    Cr Supplier               ₹50,000
```

**Rationale:** The process creates the supplier liability only after validating the invoice against the procurement history and configured rules.

---

## 19. Three-Way Match

The three-way match compares:

1. Purchase Order
2. Goods Receipt
3. Supplier Invoice

Key comparison dimensions include:

- Supplier
- Material or service
- Quantity
- Price
- Unit of measure
- Tax and delivery costs where applicable

**Rationale:** The match helps prevent payment for unsupported quantities or prices.

---

## 20. GR-Based Invoice Verification

When GR-based invoice verification is used, invoice matching is tied more directly to individual goods receipts.

**Rationale:** This can improve control where multiple or partial deliveries occur, but the business process and supplier data must support it. The indicator should be designed deliberately rather than assumed for every supplier.

---

## 21. Tolerance Design

Invoice verification can evaluate quantity, price, date, and other differences using configured tolerance logic.

Example exception:

- PO price: ₹500 per unit
- Invoice price: ₹540 per unit
- Quantity: 100
- Total variance: ₹4,000

Potential result:

```text
Invoice difference
        ↓
Tolerance evaluation
        ↓
Post, warn, block for payment, or investigate
```

**Rationale:** Tolerances should be approved, risk-based, and tested. This portfolio intentionally does not invent thresholds.

---

## 22. Invoice Blocks and Release

An invoice can be posted but blocked for payment depending on the variance and configuration.

Recommended controls:

- Clear block reason
- Assigned resolution owner
- Evidence for price or quantity corrections
- Controlled release authorization
- Ageing review of blocked invoices

**Rationale:** Posting and payment release are separate control points.

---

## 23. Returns, Reversals, and Credit Memos

Testing should include:

- Goods-receipt reversal
- Return to supplier
- Subsequent debit or credit
- Supplier credit memo
- PO quantity or price amendment
- Invoice cancellation

**Rationale:** Exception design is as important as the happy path because reversals affect inventory, GR/IR, supplier balances, and audit trails.

---

## 24. FI-MM Integration Summary

| Stage | MM result | FI result |
|---|---|---|
| Purchase Requisition | Requirement recorded | No base-case posting |
| Purchase Order | Commitment/purchasing document | No base-case posting |
| Goods Receipt | Stock and PO history updated | Dr Inventory / Cr GR/IR |
| Invoice Receipt | PO history and invoice document updated | Dr GR/IR / Cr Supplier |
| Payment | Outside core MM; supplier item settled in FI | Dr Supplier / Cr Bank |

---

## 25. Key Controls

### Purchasing

- Approved requisition and PO workflow
- Controlled supplier source
- Price and quantity approval
- PO-change monitoring

### Receiving

- Goods receipt against valid PO
- Quantity and stock-type checks
- Timely reversal of errors
- Separation between ordering and receiving where practical

### Invoice Verification

- Duplicate-invoice checks
- PO/GR/invoice comparison
- Tolerance-based payment blocks
- Controlled block release

### Master Data and Configuration

- Supplier and material change governance
- Restricted movement types and sensitive transactions
- Controlled valuation-class changes
- Governed automatic account determination

---

## 26. Test Matrix

| Test | Expected result |
|---|---|
| Exact PO-GR-invoice match | Invoice posts without variance block |
| Partial goods receipt | Stock and GR/IR reflect received quantity |
| Partial invoice | Supplier liability reflects invoiced quantity/value |
| Price above tolerance | Invoice is handled according to block rules |
| Duplicate invoice reference | Duplicate control identifies or warns according to configuration |
| Goods-receipt reversal | Stock and accounting impact reverse correctly |
| Return to supplier | Quantity, value, and PO history update correctly |
| Closed MM/FI period | Posting is prevented or redirected according to authorized period controls |
| Wrong valuation class | Test detects incorrect account determination before production use |

---

## 27. Scope and Disclaimer

All names, codes, quantities, values, and transactions are fictional. Productive purchasing, valuation, movement-type, tax, tolerance, workflow, and account-determination settings depend on detailed business and legal requirements.

---

## 28. References

- SAP Learning — [Identifying the Organizational Units in Sourcing and Procurement](https://learning.sap.com/courses/introducing-sourcing-and-procurement-and-organizational-structure/identifying-the-organizational-units-in-procurement_ab34d826-b383-4344-83b1-ac810a8dc3a3)
- SAP Learning — [Defining and Assigning Plants and Storage Locations](https://learning.sap.com/courses/exploring-cross-functional-customizing-in-sap-s-4hana-cloud-private-edition/defining-and-assigning-plants-and-storage-locations_d132dd73-f77d-4a67-8fa4-d6dbde3f3b66)
- SAP Learning — [Maintaining Business Partner Master Data](https://learning.sap.com/courses/business-processes-in-sap-s-4hana-sourcing-procurement/maintaining-business-partner-master-data)
- SAP Learning — [Handling Variances Without Item Reference](https://learning.sap.com/courses/exploring-foundations-of-logistics-invoice-verification-in-sap-s-4hana-cloud-private-edition/handling-variances-without-item-reference)
- SAP Help Portal — [WRX: GR/IR Clearing Account](https://help.sap.com/docs/s4hana-best-practices/ycoa/wrx-gr-ir-clearing-account?locale=en-US)
- SAP Help Portal — [BSX: Inventory Posting](https://help.sap.com/docs/SAP_S4HANA_BEST_PRACTICES/1f2ae10b96f740759d66e695f953aa8f/a1dbf3f2ed0145ca85705cd43f548da3.html?locale=en-US)
