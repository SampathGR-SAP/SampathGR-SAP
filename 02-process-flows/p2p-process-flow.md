# SAP S/4HANA FI-MM Procure-to-Pay (P2P) Process Flow

**Project Overview**

This document explains the end-to-end Procure-to-Pay (P2P) process used in my independent SAP S/4HANA FI-MM learning project.

The project demonstrates how a business requirement progresses through purchasing, goods receipt, supplier invoice verification, Accounts Payable, payment, and GR/IR reconciliation.

The main learning objective is to understand the integration between:
	• Materials Management (MM)
	• Financial Accounting (FI)
	• Accounts Payable (AP)

All company, supplier, material, organizational, and transaction data used in this project is fictional or synthetic.


***1. End-to-End P2P Flow***
The complete process used in this learning scenario is:

Business Requirement
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
Invoice Verification
        ↓
Three-Way Match
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

The purchasing activities primarily involve Materials Management, while invoice accounting, supplier liability, payment, and clearing connect the process with Financial Accounting and Accounts Payable.


***2. Synthetic Business Scenario***

Nova Manufacturing Pvt. Ltd. is a fictional manufacturing company.

The production department requires 100 units of raw material for production.

# Project Data
# Field	                              # Synthetic Value
Company	                          Nova Manufacturing Pvt. Ltd.
Company Code	                            NM01
Plant	                                    PL01
Purchasing Organization	                  NP01
Purchasing Group	                        PG01
Supplier	                      V10001 - Alpha Industrial Supplies
Material	                               RM-1001
Description	                    Synthetic Production Material
Quantity	                              100 Units
Unit Price	                             ₹500
Net Purchase Value	                   ₹50,000


***3. Business Requirement***

The production department identifies the need for 100 units of RM-1001.

Required Quantity = 100 Units
Unit Price        = ₹500
Expected Value    = ₹50,000

At this stage, the business has identified the requirement but has not yet purchased or received the material.

# Output

A purchasing requirement exists and can proceed to the Purchase Requisition process.


***4. Purchase Requisition (PR)***

A Purchase Requisition is created to document the internal requirement for the material.

# Example
# Field	                                           Value
Material	                                        RM-1001
Quantity	                                        100 Units
Plant                                             PL01
Expected Value	                                  50,000

**Accounting Impact**

For this standard learning scenario, creation of the Purchase Requisition does not generate an FI accounting posting.

**Next Step**

The Purchase Requisition proceeds through the applicable approval process.


***5. Purchase Requisition Approval***

The Purchase Requisition is reviewed according to the organization's applicable approval process.

             PR Review
                 |
          +------+------+
          |             |
       Approved       Rejected
          |             |
          ↓             ↓
     Continue      Review / Correct
          |
          ↓
      Create PO

If approved, Purchasing can continue with procurement.

If rejected, the requirement needs to be reviewed or corrected.


***6. Purchase Order (PO)***

Purchasing creates a Purchase Order for the selected supplier.

# Synthetic                           Purchase Order
Field	Value
Supplier	                          V10001 - Alpha Industrial Supplies
Material	                            RM-1001
Quantity	                           100 Units
Unit Price	                         ₹500
Net PO Value	                       ₹50,000
Plant	                               PL01

The Purchase Order communicates the purchasing requirement to the supplier.

**Accounting Impact**

In this standard procurement scenario, creation of the Purchase Order does not itself generate an FI accounting posting.

However, important information such as account assignments, prices, and purchasing conditions is established for subsequent processing.


***7. PO Approval / Release***

Depending on the configured business process, the Purchase Order may require approval before being sent to the supplier.
             PO Review
                 |
          +------+------+
          |             |
       Approved       Rejected
          |             |
          ↓             ↓
    Send to Supplier  Review PO

Once approved, the supplier can fulfil the order.


***8. Goods Receipt (GR)***

The supplier delivers the material.

The receiving team verifies information such as:
		○ Correct material
		○ Correct quantity
		○ Purchase Order reference
		○ Delivery information
    ○ Over-delivery or under-delivery
    
**Synthetic Example**
 
PO Quantity = 100 Units
GR Quantity = 100 Units

The Goods Receipt is recorded with reference to the Purchase Order.

**Simplified Accounting Entry**

For the valuated stock-material scenario used in this project:

 
Dr Inventory          ₹50,000
    Cr GR/IR          ₹50,000
    
# Result

  • Material receipt is recorded.
	• Inventory is updated.
	• A financial document is generated.
	• GR/IR is credited for the value of the receipt.
	 

The actual accounting treatment depends on the business scenario and SAP configuration.


***9. Supplier Invoice Receipt***

After supplying the material, the supplier submits the invoice.

# Synthetic Invoice
Field	                      Value
Supplier	                 V10001
Material	                 RM-1001
Quantity	                 100 Units
Unit Price	               ₹500
Net Invoice Amount         ₹50,000
PO Reference	         Synthetic PO Reference

Tax is intentionally excluded from this first learning scenario.

The invoice now proceeds to invoice verification.


***10. Invoice Verification***

The supplier invoice is checked against the available purchasing and receipt information.

Important checks can include:

  ► Supplier
	► Purchase Order
	► Material
	► Quantity
	► Price
	► Goods Receipt
	► Invoice reference
	► Potential duplicate
	► Applicable tolerances


The objective is to determine whether the invoice can continue through normal processing or requires investigation.


***11. Three-Way Match***

Three-Way Match compares information associated with:

 
Purchase Order
      +
Goods Receipt
      +
Supplier Invoice
      ↓
Three-Way Match

**Successful Scenario**

Check         PO	     GR	     Invoice	     Result
Quantity  	 100	     100	     100	       Match
Unit Price 	 ₹500	     N/A	     ₹500	       Match

For this synthetic example, the quantities and relevant prices correspond.

**Decision Flow**
 
          Three-Way Match
                 |
          +------+------+
          |             |
       Match          Variance
          |             |
          ↓             ↓
   Continue with     Investigate
   Processing        Exception

Whether a difference causes an invoice block depends on the applicable SAP configuration, tolerances, and business rules.


***12. Invoice Posting***

Once the invoice satisfies the applicable validation requirements, it can proceed to posting.

# Simplified Accounting Entry

For this initial no-tax example:

 
Dr GR/IR              ₹50,000
    Cr Supplier       ₹50,000
    
**Result**
	Ø GR/IR is debited.
	Ø Supplier liability is created.
	Ø An FI accounting document is generated.


If the Goods Receipt and Invoice Receipt values correspond, the relevant GR/IR items can clear.


***13. Supplier Open Item***

After invoice posting, the supplier liability remains open until it is cleared through the applicable payment process.

 Supplier Liability = ₹50,000

Payment eligibility can depend on:
    • Payment terms
	• Due date
	• Payment block
	• Required approvals
    • Valid supplier payment information


***14. Payment***

On becoming eligible for payment, the supplier liability can be paid according to the applicable payment process.

# Simplified Accounting Entry
 
Dr Supplier           ₹50,000
    Cr Bank           ₹50,000

This example is intentionally simplified. Actual bank and payment accounting can involve additional clearing steps depending on system configuration and the organization's payment process.


***15. Clearing***

After payment processing, the corresponding supplier open item can be cleared.

The objective is to ensure that the supplier invoice and corresponding payment are correctly reflected in Accounts Payable.


***16. GR/IR Reconciliation***

GR/IR reconciliation is used to review open items and investigate differences between Goods Receipts and Invoice Receipts.

Examples requiring investigation include:
     Goods Receipt exists but invoice is missing
	 Invoice exists but corresponding Goods Receipt is missing
	 Quantity difference
	 Amount or price difference
	 Incorrect reference
	 Old unresolved GR/IR item

 **Simplified GR/IR Relationship**
Goods Receipt
Dr Inventory
    Cr GR/IR

        ↓

Invoice Receipt
Dr GR/IR
    Cr Supplier

        ↓

Matching values
        ↓
GR/IR items clear

Open differences require investigation and appropriate resolution.


***17. Exception Flow***

Not every invoice will follow the successful path. 
             Invoice Verification
                     |
                     ↓
              Three-Way Match
                     |
               Variance?
                /     \
              No       Yes
              |         |
              ↓         ↓
          Continue   Identify Cause
              |         |
              ↓         ↓
           Posting   Investigate
                        |
                        ↓
                  Resolve Issue
                        |
                        ↓
                    Revalidate

# Potential exceptions include:	
  • Missing Goods Receipt
  • Quantity variance
	• Price variance
	• Potential duplicate invoice
	• Incorrect purchasing information
	• Supplier/master-data issue
	• Payment block

These exceptions will be demonstrated separately later in the portfolio using synthetic scenarios.


***18. FI-MM Integration Points***


**Purchase Requisition**
## Main Business Area: MM / Purchasing
FI Impact: No FI posting in this learning scenario.

Purchase Order
Main Business Area: MM / Purchasing
FI Impact: No FI posting in this learning scenario.

Goods Receipt
Main Business Area: MM / Inventory

Simplified FI Impact:

Dr Inventory
Cr GR/IR

Invoice Receipt
Main Business Area: MM Invoice Verification + FI/AP

Simplified FI Impact:

Dr GR/IR
Cr Supplier

Supplier Open Item
Main Business Area: FI/AP
FI Impact: Supplier liability remains outstanding until payment/clearing.

Payment
Main Business Area: FI/AP

Simplified FI Impact:

Dr Supplier
Cr Bank

Clearing
Main Business Area: FI/AP
FI Impact: The relevant supplier open item is cleared.

GR/IR Reconciliation
Main Business Area: MM + FI
Purpose: Review and resolve differences between Goods Receipt and Invoice Receipt.


***19. Key Control Points***

Important controls in this process include:
 
# Purchase Controls
	• Appropriate Purchase Requisition
	• Supplier selection
	• Purchase Order approval
	
# Receiving Controls
	• Correct material
    • Correct quantity
	• Valid Purchase Order reference
	 
# Invoice Controls
	• Valid supplier
	• Purchase Order verification
	• Goods Receipt verification
	• Quantity comparison
	• Price comparison
	• Duplicate check
	• Applicable tolerance checks
	 

# Payment Controls
	• Valid payment terms
	• Due-date determination
	• Payment blocks
	• Required approvals
	• Supplier payment information
	 


***20. What I Learned***

Through this process-flow exercise, I developed a clearer understanding of:
	1. How a business requirement becomes a Purchase Requisition
	2. How an approved requirement progresses to a Purchase Order 
	3. Why PO creation does not itself create an FI posting in this scenario
	4. How Goods Receipt connects Materials Management with Financial Accounting
	5. Why GR/IR is required
	6. How Invoice Receipt creates a supplier liability
	7. How Three-Way Match helps identify procurement discrepancies
    8. Why invoice exceptions should be investigated before normal processing continues
	9. How supplier open items relate to payment
	10. How payment and clearing complete the Accounts Payable lifecycle
	11. Why GR/IR reconciliation is important for FI-MM integration


***21. Next Project Steps***

The next sections of this portfolio will cover:

	Ø Detailed FI-MM accounting entries
	Ø Three-Way Match scenarios
	Ø Synthetic transaction data
	Ø Functional test cases
	Ø Invoice exception analysis
	Ø GR/IR reconciliation examples
	Ø Lessons learned


## Project Disclaimer

This is an independent SAP S/4HANA FI-MM learning and portfolio project.

All company names, suppliers, materials, organizational structures, Purchase Orders, invoices, amounts, and transaction information shown in this project are fictional or synthetic.

No employer, customer, supplier, or other confidential business information is used.

SAP and SAP S/4HANA are trademarks of SAP SE.

This project is not affiliated with or endorsed by SAP SE.
