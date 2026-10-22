**SAP S/4HANA Procure-to-Pay (P2P) and AP Process Design**

**Learning project**: end-to-end design of the Procure-to-Pay cycle in SAP S/4HANA, covering Accounts Payable, Materials Management and FI-MM integration.

**Disclaimer**: All data is synthetic. This project is not affiliated with SAP or any employer.

**Business scenario**
A manufacturing company buys raw materials from domestic vendors. This project documents how a purchase moves from requisition to payment, what accounting entries are created, and how mismatches are handled.

**Process flow**
PR → PO → Goods Receipt → Invoice Receipt → Payment → Clearing

Key accounting entries
Step	Debit	Credit
Goods receipt	Inventory / Expense	GR/IR
Invoice receipt	GR/IR, Input tax	Vendor
Payment	Vendor	Bank
Contents

Folder	Description	Status
01-business-requirements	           Scenario and requirements	         🔄 In progress
02-process-flows	                   P2P flow diagrams	                 ⏳ Planned
03-config-rationale                  What would be configured, and why   ⏳ Planned
04-test-cases	                       20 test cases	                     ⏳ Planned
05-synthetic-data                    Invented vendors, POs and invoices  ⏳ Planned
07-defect-log	                       Issues found and fixed	             ⏳ Planned
08-lessons-learned	                 Reflections and improvements	       ⏳ Planned

├── 01-business-requirements/
│   ├── business-scenario.md
│   └── p2p-glossary.md
│
├── 02-process-flows/
│   └── P2P_Process_Flow_Steps.xlsx
│
├── 03-config-rationale/
│   └── configuration-notes.md
│
├── 04-test-cases/
│   └── p2p-test-cases.xlsx
│
├── 05-synthetic-data/
│   └── sample-transactions.xlsx
│
├── 06-scripts/
│
├── 07-defect-log/
│   └── defect-log.xlsx
│
└── 08-lessons-learned/
    └── lessons-learned.md

What I learned
**To be updated as the project progresses.**
