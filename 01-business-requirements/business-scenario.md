**SAP S/4HANA FI-MM Procure-to-Pay (P2P) Business Scenario**


**1. Project Overview**
This is an independent SAP S/4HANA FI-MM learning project designed to demonstrate my understanding of the end-to-end Procure-to-Pay (P2P) business process.

**The project focuses on the integration between:**

Materials Management (MM)
Financial Accounting (FI)
Accounts Payable (AP)
The complete project uses synthetic business data created specifically for learning and portfolio purposes.


**2. Business Scenario**
Nova Manufacturing Pvt. Ltd. is a fictional manufacturing company that purchases raw materials from external suppliers for its production activities.

The production department requires 100 units of a raw material.

The business needs to complete the purchasing process from identifying the requirement through supplier payment while ensuring that:

• The purchase is properly requested and approved.
• A Purchase Order is issued to the supplier.
• The correct quantity of material is received.
• The supplier invoice corresponds with the Purchase Order and Goods Receipt.
• Accounting entries are generated correctly.
• Invoice discrepancies are identified and resolved.
• The supplier is paid according to the agreed payment terms.
• Open supplier and GR/IR items are reviewed and reconciled

**3. Fictional Organization Structure**
Element	                           Synthetic Value
Company                          	Nova Manufacturing Pvt. Ltd.
Company Code	                    NM01
Plant	                            PL01
Purchasing Organization	          NP01
Purchasing Group	                PG01
Storage Location	                SL01

These values are fictional and were created only for this learning project.


**4. Synthetic Master Data**
Supplier
Field                           	Value
Supplier ID	                      V10001
Supplier Name	                  Alpha Industrial Supplies
Country	                          India
Currency	                      INR
Payment Terms	                  30 Days

Material
Field	                            Value
Material ID	                      RM-1001
Description	                      Synthetic Production Material
Material Type	                  Raw Material
Quantity Required	              100 Units
Unit Price	                      ₹500
Net Purchase Value	              ₹50,000

All supplier, material, company and transaction data used in this repository are invented for learning purposes.


**5. Business Requirement**
The production department identifies a requirement for 100 units of RM-1001.

The purchasing process must ensure that the material is procured from an approved supplier and that the supplier invoice is processed only after the relevant purchasing and receipt information has been validated.

The process should provide visibility into:

• Material requirement
• Purchase Requisition
• Purchase Order
• Goods Receipt
• Supplier Invoice
• Invoice verification
• Supplier liability
• Payment
• Clearing
• GR/IR reconciliation


**6. End-to-End Process**
The project follows this business flow:

Purchase Requirement
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
Invoice Verification / Three-Way Match
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


**7. Process Steps**
_Step 1: Identify Purchase Requirement_
The production department determines that 100 units of RM-1001 are required for production.

Required quantity:

100 Units

Expected unit price:

₹500

Expected purchase value:

₹50,000

_Step 2: Create Purchase Requisition_
A Purchase Requisition is created to record the internal requirement for the material.

The requisition includes:

• Material
• Quantity
• Required delivery date
• Plant
• Purchasing information

At this stage, the requirement is internal.

_Step 3: Approve Purchase Requisition_
The Purchase Requisition goes through the applicable approval process.

If approved, purchasing can continue with procurement.

If rejected, the requirement must be reviewed or corrected before continuing.

_Step 4: Create Purchase Order_
Purchasing creates a Purchase Order for the selected supplier.

Example PO
Field                        	Value
Supplier                    V10001 – Alpha Industrial Supplies
Material	                RM-1001
Quantity                    100 Units
Unit Price	                ₹500
Net Value	                ₹50,000
Plant	                    PL01
The Purchase Order communicates the purchasing requirement to the supplier.

_Step 5: Receive Material_
The supplier delivers the ordered material.

The warehouse checks the delivery against the Purchase Order.

The receiving team verifies:

• Correct material
• Correct quantity
• Delivery condition
• Purchase Order reference

If everything is acceptable, the Goods Receipt is recorded.


**8. Goods Receipt Accounting Concept**
For this learning scenario, the simplified accounting concept for a valuated stock-material receipt is:

Account	               Debit	   Credit
Inventory           	₹50,000	
GR/IR Clearing		               ₹50,000
Simplified entry:

Dr Inventory ₹50,000
Cr GR/IR ₹50,000

The exact accounting treatment in a real implementation depends on the configured business scenario and account determination.


**9. Supplier Invoice**
After supplying the material, the supplier submits an invoice.

Example invoice:

Field	                    Value
Supplier                	V10001
Material                	RM-1001
Quantity                	100 Units
Net Amount                  ₹50,000
PO Reference Synthetic PO reference

The supplier invoice must be checked before normal processing continues.


**10. Three-Way Match**
The project compares three business documents:

1. Purchase Order
2. Goods Receipt
3. Supplier Invoice

The main checks include:

Quantity
PO Quantity = 100
GR Quantity = 100
Invoice Quantity = 100

Result: Match

Price
PO Unit Price = ₹500
Invoice Unit Price = ₹500

Result: Match

Based on the synthetic data used in this scenario, the Purchase Order quantity and price, Goods Receipt quantity, and supplier invoice quantity and price are consistent. The invoice therefore represents a successful three-way-match example, subject to the applicable SAP tolerance and invoice-verification settings.

In an actual SAP implementation, applicable tolerances and controls depend on system configuration.

**11. Invoice Posting Accounting Concept**
Using the simplified example before tax:

Account	                Debit	Credit
GR/IR Clearing	      ₹50,000	
Supplier Payable		           ₹50,000

Simplified entry:

Dr GR/IR ₹50,000
Cr Supplier Payable ₹50,000

This creates the supplier liability.

Tax will be handled separately in later versions of this project so that this initial scenario remains focused on the core FI-MM process.


**12. Supplier Payment**
The supplier invoice becomes eligible for payment according to the agreed payment terms, provided there are no unresolved payment blocks or exceptions.

For the simplified learning example:

Dr Supplier Payable ₹50,000
Cr Bank ₹50,000

The supplier liability is cleared through the payment process.


**13. GR/IR Reconciliation**
The project also includes review of the GR/IR clearing account.

Examples of situations that may require investigation include:

• Goods received but invoice not yet received
• Invoice received but corresponding receipt is incomplete or missing
• Quantity differences
• Price differences
• Incorrect references
• Unresolved old items

The reconciliation section of this portfolio will document synthetic examples of these situations.


**14. Exception Scenarios**
The project will test both successful and unsuccessful transactions.

**Scenario A** - Successful Three-Way Match
PO = 100 Units
GR = 100 Units
Invoice = 100 Units

Price matches.

Expected result:

Invoice proceeds through the normal process.

**Scenario B** - Quantity Mismatch
PO = 100 Units
GR = 80 Units
Invoice = 100 Units

Expected investigation:

Check whether the remaining goods are pending receipt or whether the supplier invoiced an incorrect quantity.

**Scenario C** - Price Mismatch
PO Price = ₹500 per unit
Invoice Price = ₹520 per unit

Expected investigation:

Review the Purchase Order price and supplier invoice and resolve the difference according to the applicable business rules and configured tolerances.

**Scenario D** - Invoice Received Before Goods Receipt	.

A Purchase Order exists and the supplier invoice has been received, but the expected Goods Receipt has not yet been recorded.

Expected investigation:

Verify whether the material has actually been received, review the PO and invoice-verification controls, and determine whether the missing Goods Receipt is caused by a timing difference, process delay, or receiving issue.

**Scenario E**- Potential Duplicate Invoice
Two invoices contain the same supplier, invoice reference and amount.

Expected investigation:

Investigate the documents before proceeding to prevent duplicate processing or payment.


**15. Project Objectives**
By completing this project, I aim to demonstrate an understanding of:

• SAP S/4HANA Procure-to-Pay
• SAP FI-MM integration
• Accounts Payable
• Purchase Requisitions
• Purchase Orders
• Goods Receipts
• Invoice processing
• Three-Way Matching
• GR/IR
• Supplier accounting
• Payments
• Clearing
• Reconciliation
• Business-process exceptions
• Functional testing


**16. Project Deliverables**
The repository will gradually include:

• Business scenario
• P2P glossary
• Process-flow documentation
• Accounting-entry examples
• Synthetic master data
• Synthetic transaction data
• Three-way-match scenarios
• Test cases
• Exception scenarios
• Defect log
• Configuration rationale
• Lessons learned


**17. Repository Structure**
sap-p2p-process-design/
│
├── README.md
│
├── 01-business-requirements/
│   ├── business-scenario.md
│   └── p2p-glossary.md
│
├── 02-process-flows/
│
├── 03-config-rationale/
│
├── 04-test-cases/
│
├── 05-synthetic-data/
│
├── 06-scripts/
│
├── 07-defect-log/
│
└── 08-lessons-learned/


**18. Current Project Status**
**Deliverable	                           Status **
	1. Business Scenario	              Complete
	2. P2P Glossary	                    In Progress
	3. Process Flow	                    In Progress
	4. Accounting Entries               In Progress
	5. Synthetic Dataset                Planned
	6. Test Cases	                      Planned
	7. Exception Analysis               Planned
	8. Defect Log	                      Planned
	9. Lessons Learned                  Planned


The status will be updated as I complete each part of the project.


**19. Learning Disclaimer**
This repository is an independent SAP S/4HANA learning portfolio.

All companies, suppliers, materials, Purchase Orders, invoices, amounts, organizational structures and transaction data used in the project are fictional or synthetic.

The repository does not contain employer, customer, supplier or other confidential business information.
