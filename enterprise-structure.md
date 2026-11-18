# Enterprise Structure Rationale

## 1. Purpose

This document explains the proposed enterprise structure for the **Nova Manufacturing Pvt. Ltd.** SAP S/4HANA FI-MM Procure-to-Pay (P2P) learning scenario.

The purpose is to demonstrate why each organizational unit is required, how the units are assigned, and how they support purchasing, inventory management, Financial Accounting, and Accounts Payable.

> **Portfolio note:** This is a conceptual design using synthetic data. It does not claim that a live SAP system was configured.

---

## 2. Synthetic Business Scenario

Nova Manufacturing purchases raw material for a single manufacturing location in India.

| Element | Synthetic value | Purpose |
|---|---|---|
| Company | Nova Manufacturing Pvt. Ltd. | Legal enterprise used in the scenario |
| Company Code | NM01 | Financial accounting and statutory reporting unit |
| Plant | PL01 | Manufacturing and goods-receipt location |
| Storage Location | SL01 | Raw-material stock location within PL01 |
| Purchasing Organization | NP01 | Procurement responsibility |
| Purchasing Group | PG01 | Buyer or buyer-team responsibility |
| Supplier | V10001 - Alpha Industrial Supplies | External source of raw material |
| Material | RM-1001 - Synthetic Production Material | Stock material purchased for production |
| Currency | INR | Company-code and transaction currency for the base case |

The base purchase is **100 units × ₹500 = ₹50,000**, with payment due in 30 days.

---

## 3. Proposed Structure

```text
Nova Manufacturing Pvt. Ltd.
│
├── Company Code NM01
│   └── Plant PL01
│       └── Storage Location SL01
│
└── Purchasing Organization NP01
    ├── Assigned to Company Code NM01
    ├── Assigned to Plant PL01
    └── Purchasing Group PG01
```

The design uses one organizational unit of each major type so the portfolio remains easy to follow while still showing the core FI-MM relationships.

---

## 4. Organizational Units and Rationale

### 4.1 Company

**Synthetic design:** Nova Manufacturing Pvt. Ltd.

The company represents the broader enterprise or consolidation context. The learning scenario uses one company and one company code, so no cross-company-code process is included.

**Why it matters:**

- Establishes the business context for the solution.
- Provides a logical parent for the accounting unit.
- Keeps the scenario simple enough to trace from procurement to payment.

### 4.2 Company Code

**Synthetic design:** `NM01`

The company code is the primary Financial Accounting unit in this scenario. Complete financial statements can be considered at this level.

**P2P relevance:**

- Supplier liabilities are posted in NM01.
- Inventory, GR/IR, expense, tax, and bank postings are recorded in NM01.
- Payment terms, reconciliation-account data, and payment methods are maintained for the supplier's company-code view.
- Posting periods and document controls are applied to FI documents in NM01.

**Design rationale:** One company code supports a clear one-to-one relationship between the fictional legal entity and its accounting records.

### 4.3 Plant

**Synthetic design:** `PL01`

PL01 represents the manufacturing and receiving location that requires RM-1001.

**P2P relevance:**

- The purchase requisition and purchase order identify where material is required.
- Goods receipt is posted for PL01.
- Material stock and valuation are connected to the plant or its assigned valuation area, depending on the configured valuation level.
- The plant is assigned to company code NM01 so inventory movements can generate the correct FI impact.

**Design rationale:** One plant is sufficient for the base case and avoids introducing inter-plant or cross-company procurement before the core process is understood.

### 4.4 Storage Location

**Synthetic design:** `SL01`

SL01 represents the raw-material inventory location within PL01.

**P2P relevance:**

- Identifies where the received stock is held.
- Enables stock quantities to be managed below plant level.
- Supports inventory visibility and physical-inventory control.

**Design rationale:** A dedicated raw-material storage location makes the goods-receipt destination explicit without adding unnecessary warehouse complexity.

### 4.5 Purchasing Organization

**Synthetic design:** `NP01`

NP01 represents the procurement organization responsible for purchasing materials and maintaining supplier purchasing relationships.

**P2P relevance:**

- Creates and manages purchasing documents.
- Supports supplier purchasing data and negotiated conditions.
- Is assigned to NM01 and PL01 for the base scenario.

**Design rationale:** A company-code-specific purchasing organization is easy to explain and clearly connects procurement responsibility to the accounting entity. Other organizational models are possible in real implementations.

### 4.6 Purchasing Group

**Synthetic design:** `PG01`

PG01 represents the buyer or buyer team responsible for the transaction.

**P2P relevance:**

- Identifies operational purchasing responsibility.
- Supports follow-up and reporting.
- Can be used on purchase requisitions and purchase orders.

**Design rationale:** Purchasing group is an operational responsibility key rather than a formal enterprise-structure assignment like company code, plant, or purchasing organization.

---

## 5. Required Assignments

| Assignment | Proposed relationship | Reason |
|---|---|---|
| Company Code to Company | NM01 → Nova Manufacturing | Connects accounting unit to enterprise context |
| Plant to Company Code | PL01 → NM01 | Ensures plant transactions post to the correct accounting unit |
| Storage Location to Plant | SL01 → PL01 | Places raw-material stock inside the receiving plant |
| Purchasing Organization to Company Code | NP01 → NM01 | Defines company-code-specific procurement responsibility |
| Purchasing Organization to Plant | NP01 → PL01 | Allows NP01 to procure for the plant |
| Standard Purchasing Organization for Plant | NP01 for PL01 | Provides a clear default procurement relationship for the scenario |

The exact assignment model must follow the implementation's operating model. A centralized or cross-company purchasing organization would require a different design and additional controls.

---

## 6. Master-Data Views Supported by the Structure

### Supplier / Business Partner

The fictional supplier `V10001` requires data that supports both sides of the process:

- **General data:** name, address, communication details, and bank-related controls where applicable.
- **Company-code data:** reconciliation account, payment terms, payment methods, dunning or withholding-tax data where relevant.
- **Purchasing-organization data:** order currency, Incoterms where applicable, partner functions, purchasing controls, and supplier evaluation data.

### Material Master

The fictional material `RM-1001` requires organizational views appropriate to the process:

- Basic data
- Purchasing data for PL01/NP01 as required by the design
- MRP data when planning is in scope
- Storage-location data for SL01 when created or extended
- Accounting and costing data for the applicable valuation area

---

## 7. Transaction Ownership Across the Structure

| P2P stage | Main organizational reference | Expected accounting impact in the base case |
|---|---|---|
| Purchase Requisition | Plant PL01, Purchasing Group PG01 | No FI posting |
| Purchase Order | Purchasing Organization NP01, Plant PL01, Company Code NM01 | No FI posting |
| Goods Receipt | Plant PL01, Storage Location SL01 | Dr Inventory / Cr GR/IR |
| Invoice Verification | Company Code NM01, Purchasing Organization NP01 | Dr GR/IR / Cr Supplier |
| Payment | Company Code NM01 | Dr Supplier / Cr Bank |

---

## 8. Key Design Decisions

1. **Single company code:** keeps statutory accounting and supplier liability in one unit.
2. **Single plant:** keeps the learning scenario focused on standard external procurement.
3. **Plant-level valuation assumption:** used as a common learning design; the productive valuation level must be confirmed during implementation.
4. **Company-code-specific purchasing organization:** gives a direct relationship between procurement and FI.
5. **One raw-material storage location:** provides clear inventory ownership without warehouse-management complexity.
6. **No cross-company or stock-transfer scenarios:** intentionally excluded from this phase.

---

## 9. Validation Checklist

Before a real implementation, confirm:

- [ ] Company code organizational data, local requirements, and currency settings
- [ ] Plant assignment to the correct company code
- [ ] Storage-location design and physical inventory ownership
- [ ] Purchasing-organization operating model and assignments
- [ ] Purchasing-group responsibilities
- [ ] Valuation level and material-ledger requirements
- [ ] Supplier Business Partner roles and organizational extensions
- [ ] Material master views for plant and valuation area
- [ ] Tax, withholding-tax, bank, payment, and local compliance requirements
- [ ] Authorization and segregation-of-duties requirements

---

## 10. Scope and Disclaimer

This enterprise structure is a learning design, not a production blueprint. Actual configuration depends on legal entities, operating model, tax requirements, ledgers, currencies, plants, warehouses, procurement responsibilities, integrations, workflow, and governance.

All names, codes, materials, suppliers, quantities, and values are fictional.

---

## 11. References

- SAP Learning — [Identifying the Organizational Units in Sourcing and Procurement](https://learning.sap.com/courses/introducing-sourcing-and-procurement-and-organizational-structure/identifying-the-organizational-units-in-procurement_ab34d826-b383-4344-83b1-ac810a8dc3a3)
- SAP Learning — [Defining Organizational Units in Accounting and Purchasing](https://learning.sap.com/courses/exploring-cross-functional-customizing-in-sap-s-4hana-cloud-private-edition/defining-organizational-units-in-accounting-and-purchasing_b89b98bc-d18b-49de-acc6-be16b9573ced)
- SAP Learning — [Defining and Assigning Plants and Storage Locations](https://learning.sap.com/courses/exploring-cross-functional-customizing-in-sap-s-4hana-cloud-private-edition/defining-and-assigning-plants-and-storage-locations_d132dd73-f77d-4a67-8fa4-d6dbde3f3b66)
