# SAP S/4HANA Procure-to-Pay (P2P) Glossary

**About This Glossary**

This glossary is part of my independent SAP S/4HANA FI-MM learning project.

It documents the key business and SAP concepts I am learning while studying the end-to-end Procure-to-Pay (P2P) process.

The objective is not only to remember SAP terminology, but also to understand how purchasing activities in Materials Management (MM) connect with Financial Accounting (FI) and Accounts Payable (AP).
All **Business Example**s and data used in this project are synthetic.

**1. Purchase Requisition (PR)**

# Definition
A Purchase Requisition is an internal request to procure a material or service.

It represents a business requirement before a Purchase Order is created for a supplier.

A Purchase Requisition can contain information such as
	• Material or service required
	• Quantity required
	• Delivery requirement
	• Plant
	• Purchasing information
	• Account assignment where applicable
  
**Business Example**

In my synthetic project, Nova Manufacturing Pvt. Ltd. needs
	• Material           : RM-1001
	• Description        : Synthetic Production Material
	• Quantity           : 100 units
	• Plant              : PL01
  
The requirement is recorded internally through a Purchase Requisition.

**Key Learning**
A Purchase Requisition represents the internal requirement to purchase.
It is not the supplier invoice and it is not the payment.

**2. Purchase Order (PO)**

# Definition
A Purchase Order is a purchasing document used to communicate the procurement requirement to a supplier.

It can contain information such as:
	• Supplier
	• Material or service
	• Quantity
	• Unit price
	• Delivery information
	• Plant
	• Purchasing organization
	• Payment terms
  
**Business Example**

After the purchase requirement is approved, a Purchase Order is created with the following synthetic data:
**Field**	**Value**
Supplier	V10001 - Alpha Industrial Supplies
Material	RM-1001
Quantity	100 Units
Unit Price	₹500
Net PO **Value**	₹50,000
Plant	PL01

**Accounting Impact**

For the basic learning scenario, creating the Purchase Order itself does not create an FI accounting entry.
The financial impact begins later when relevant business transactions, such as the Goods Receipt, are posted.

**Key Learning**
The PO connects the purchasing requirement with the supplier and becomes an important reference for subsequent P2P transactions.

**3. Goods Receipt (GR)**

# Definition
Goods Receipt records that material ordered from the supplier has been received.

For a PO-based procurement process, the Goods Receipt is normally recorded with reference to the relevant Purchase Order.

**Important Checks**

At Goods Receipt, the business can check:
	• Whether the correct material was received
	• Whether the correct quantity was received
	• Whether there is an over-delivery or under-delivery
	• Whether the receipt refers to the correct Purchase Order
  
**Business Example**

Purchase Order:
100 units
Actual material received:
100 units
Goods Receipt:
100 units

**Simplified Accounting Concept**

For the synthetic valuated-stock scenario:
Dr Inventory          ₹50,000
    Cr GR/IR          ₹50,000

**Key Learning**
Goods Receipt is important because it records the receipt of material and creates a connection between logistics and financial accounting.

**4. Invoice Receipt (IR)**

# Definition
Invoice Receipt represents the supplier invoice entering the invoice-verification/accounting process.

For PO-based procurement, the supplier invoice can be checked against the purchasing and receipt information.

**Business Example**
Supplier Invoice:

**Field**	       **Value**
Supplier	         V10001
Quantity	         100 Units
Unit Price         ₹500
Net Invoice Amount	₹50,000

**Simplified Accounting Concept**

Ignoring tax for the initial learning scenario:
 
Dr GR/IR              ₹50,000
    Cr Supplier       ₹50,000
    
The credit creates the liability owed to the supplier.

**Key Learning**
Invoice Receipt connects the supplier invoice with Accounts Payable and FI-MM integration.

**5. GR/IR Clearing Account**

# Definition
GR/IR means: - Goods Receipt / Invoice Receipt

The GR/IR clearing account helps manage the timing difference between receiving goods and receiving the corresponding supplier invoice.

**Goods Receipt**
In the simplified project scenario:
 
Dr Inventory          ₹50,000
    Cr GR/IR          ₹50,000

**Invoice Receipt**

When the matching supplier invoice is recorded:
 
Dr GR/IR              ₹50,000
    Cr Supplier       ₹50,000
    
**Matching Situation**

If the Goods Receipt and Invoice Receipt **Value**s correspond, the relevant GR/IR items can be cleared.

**Exception Situation**

Open GR/IR items may require investigation when:
	• Goods were received but the invoice has not arrived
	• An invoice exists but the corresponding receipt is missing
	• Quantity differs
	• Price differs
	• Incorrect transaction references exist
  
**Key Learning**
GR/IR is one of the most important FI-MM integration concepts in my P2P learning project.

**6. Three-Way Match**

# Definition

Three-Way Match involves comparing information associated with:
	1. Purchase Order
	2. Goods Receipt
	3. Supplier Invoice
  
The purpose is to identify whether the purchasing, receiving and invoice information correspond according to applicable business rules and configured tolerances.

**Successful Example**

Document	                       Quantity	                  Unit Price
Purchase Order	                  100	                       ₹500
Goods Receipt	                  	100                       	N/A
Supplier Invoice	                100                        ₹500

Result:

The example represents a successful match.

**Quantity Exception Example**

Document                 Quantity
Purchase Order	         100
Goods Receipt            80
Supplier Invoice         100

There is a difference between the quantity received and the quantity invoiced.

The difference requires investigation.

**Price Exception Example**

PO Price:
₹500 per unit
Supplier Invoice Price:
₹520 per unit

The price difference should be investigated according to the applicable business rules, configuration and tolerance settings.

**Key Learning**
Three-Way Match is an important control in the P2P and Accounts Payable process because it helps identify discrepancies before normal invoice and payment processing continues.

**7. Supplier / Business Partner**

# Definition
A supplier is the external business party from whom an organization purchases materials or services.
SAP S/4HANA uses the Business Partner approach for relevant business-partner master information.

Supplier-related information can include:
	• Name
	• Address
	• Company-code-related information
	• Purchasing information
	• Payment information
	• Bank information
  
**Synthetic Example**

**Field**                                 **Value**
Supplier ID                                 V10001
Supplier Name	                          Alpha Industrial Supplies
Country	                                    India
Currency                                     INR

The supplier in this project is completely fictional. 

**Key Learning**
Correct supplier master information is important because errors in master data can affect purchasing, invoice processing and payments.

**8. Payment Terms**

**Definitio**
Payment Terms define agreed conditions relating to when an invoice becomes due for payment.
Payment terms can also support cash-discount conditions depending on the agreed arrangement and SAP configuration.

**Simple Example**
 
Invoice Date: 01 April
Payment Terms: 30 Days

The payment due date is determined according to the configured payment-term rules and the relevant baseline date.

**Why Payment Terms Matter**

Payment terms influence:
	• Invoice due dates
	• Payment scheduling
	• Cash discounts where applicable
	• Supplier payment processing
	• Accounts Payable planning
  
**Key Learning**
Payment terms are not simply comments on an invoice. They influence when supplier liabilities become due for payment.

**9. Blocked Invoice**

# Definition
An invoice can require additional review or be prevented from normal payment processing when a discrepancy or block exists.

Potential causes can include:
	• Price variance
	• Quantity variance
	• Missing Goods Receipt
	• Other invoice exceptions
	• Payment blocks
  
**Example**
Purchase Order:
100 Units × ₹500

Goods Receipt:
100 Units

Supplier Invoice:
100 Units × ₹520

The supplier's invoice price differs from the Purchase Order price.
The difference needs to be evaluated according to the applicable tolerances and business rules.

**Possible Resolution**

Depending on the cause, the investigation may involve:
	• Reviewing the PO
	• Checking the Goods Receipt
	• Confirming the supplier invoice
	• Contacting Purchasing
	• Correcting an incorrect business document
	• Obtaining the required approval
	• Resolving the applicable block
  
**Key Learning**
The objective is not simply to remove a block.
The root cause of the discrepancy should first be understood and resolved according to the applicable process.

**10. Company Code**

# Definition
A Company Code is an organizational unit used in Financial Accounting.
The Company Code is important for financial reporting and accounting transactions.

**Synthetic Project Example**

**Field**	                                     **Value**
Company	                               Nova Manufacturing Pvt. Ltd.
Company Code                                NM01

NM01 is a fictional Company Code created only for this learning project.

**Key Learning**
Understanding organizational structure is important because business transactions need to be associated with the correct organizational units.

**How These Concepts Connect**
The terms in this glossary are connected through the overall Procure-to-Pay process:
 
# Business Requirement
        ↓
# Purchase Requisition
        ↓
# Purchase Order
        ↓
# Goods Receipt
        ↓
# Supplier Invoice
        ↓
# Invoice Verification
        ↓
# Three-Way Match
        ↓
# Supplier Liability
        ↓
# Payment
        ↓
# Clearing
        ↓
# GR/IR Reconciliation

The process demonstrates how Materials Management and Financial Accounting work together during procurement and Accounts Payable processing.

**Synthetic Project Reference**

The same fictional scenario is used throughout my project to maintain consistency.

**Field**	                                        **Synthetic Value**
Company	                                     Nova Manufacturing Pvt. Ltd.
Company Code	                                         NM01
Plant	                                                 PL01
Supplier	                                  V10001 - Alpha Industrial Supplies
Material	                                            RM-1001
Quantity	                                           100 Units
Unit Price	                                           ₹500
Net Purchase Value                                   ₹50,000

**What I Learned**

While preparing this glossary, I developed a clearer understanding of:
	• How PR and PO differ
	• How Goods Receipt connects MM and FI
	• How Invoice Receipt creates a supplier liability
	• Why the GR/IR clearing account is needed
	• How a Three-Way Match identifies procurement discrepancies
	• Why accurate supplier master data is important
	• How payment terms influence Accounts Payable
	• Why invoice blocks require root-cause investigation
	• How the Company Code connects transactions to Financial Accounting
  
I will continue updating this glossary as my SAP S/4HANA FI-MM learning progresses.


# Project Disclaimer

This is an independent SAP S/4HANA learning and portfolio project.

All company names, suppliers, materials, Purchase Orders, invoices, amounts, Company Codes and other transaction information shown in this project are fictional or synthetic.
No employer, customer, supplier or other confidential information is used.

SAP and SAP S/4HANA are trademarks of SAP SE. This learning project is not affiliated with or endorsed by SAP SE.
