# SAP S/4HANA FI-MM | P2P Accounting Entries

	Independent SAP S/4HANA FI-MM learning project using synthetic data.
	
This section focuses on the accounting logic behind the Procure-to-Pay (P2P) process and shows how Materials Management (MM) transactions integrate with Financial Accounting (FI) and Accounts Payable (AP).

# 1. Business Scenario
   
           Company:               Nova Manufacturing Pvt. Ltd.
           Company Code:          NM01
           Plant:                 PL01
           Supplier:              V10001 - Alpha Industrial Supplies
           Material:              RM-1001
           Material Description:  Synthetic Production Material
           Purchase Quantity:     100 Units
           Unit Price:            ₹500
           Net Purchase Value:    ₹50,000
   
Tax is excluded from the base scenario so that the core FI-MM integration can be understood first.


# 2. Accounting Flow at a Glance

           PURCHASE REQUISITION
                   |
                   |  No FI Posting
                   v
           PURCHASE ORDER
                   |
                   |  No FI Posting
                   v
           GOODS RECEIPT
                   |  Dr Inventory     ₹50,000
                   |  Cr GR/IR         ₹50,000
                   v
           INVOICE RECEIPT
                   |
                   |  Dr GR/IR         ₹50,000
                   |  Cr Supplier      ₹50,000
                   v
           SUPPLIER OPEN ITEM
                   |
                   |  Liability = ₹50,000
                   v
           PAYMENT
                   |
                   |  Dr Supplier      ₹50,000
                   |  Cr Bank          ₹50,000
                   v
           CLEARING
                   |
                   |  Supplier Open Item Cleared
                   v
           GR/IR RECONCILIATION
                   |
                   v
           Review unresolved differences


# 3. Core P2P Accounting Entries
 
    Process Stage                   Debit	             Credit	             Amount	            Business Impact
    Purchase Requisition           No FI Posting         No FI Posting         ---	            Internal purchasing requirement
    Purchase Order                 No FI Posting         No FI Posting	       ---	           	Purchasing commitment
    Goods Receipt	               Inventory	         GR/IR	             ₹50,000	        Material received into stock
    Invoice Receipt	               GR/IR	             Supplier	         ₹50,000	        Supplier liability created
    Payment	                       Supplier	             Bank	             ₹50,000	        Supplier liability settled
    Clearing	                    ---  	             ---        	      ---       	    Invoice and payment cleared
 

# 4. Goods Receipt Accounting
 
The supplier delivers all 100 units.
 
PO Quantity: 100 Units
 GR Quantity: 100 Units
 Value Received: ₹50,000
 
**Accounting Entry**
 
       Debit	               Credit
     Inventory ₹50,000   	GR/IR ₹50,000
 
## Why?
 
Inventory is debited because the company has received an asset.
 
**GR/IR is credited** because the goods have been received but the corresponding supplier invoice has not yet completed the accounting flow.
 
## Position After GR
 
    Inventory     + ₹50,000
    GR/IR Credit    ₹50,000
 

# 5. Invoice Receipt Accounting
 
The supplier submits an invoice for the material received.
 
Invoice Quantity: 100 Units
 Unit Price: ₹500
 Net Invoice: ₹50,000
 
##vAccounting Entry

        Debit	           Credit
    GR/IR ₹50,000	    Supplier ₹50,000
 
## Why?
 
GR/IR is debited to offset the credit created at Goods Receipt.
 
Supplier is credited because the business now owes ₹50,000 to the supplier.
 
    GR Posting      Cr ₹50,000
    Invoice Posting Dr ₹50,000
                   -----------
    Balance             ₹0
 
✅ In the matching base scenario, the GR/IR values offset.
 

# 6. Supplier Open Item
 
After invoice posting:

    Supplier:        V10001 - Alpha Industrial Supplies
    Invoice Amount:  ₹50,000
    Open Liability:  ₹50,000
    Status:          Awaiting Payment
 
The liability remains open until payment and clearing.
 
Payment can depend on:
	• Payment terms
	• Due date
	• Payment blocks
	• Required approvals
	• Valid supplier payment information
	 


# 7. Payment and Clearing
 
Assume the invoice is due and approved for payment.
 
## Payment Entry
 
         Debit	          Credit
    Supplier ₹50,000	Bank ₹50,000
 
## Position
 
    Supplier Liability Before Payment    ₹50,000
    Payment                              ₹50,000
                                         -------
    Remaining Liability                       ₹0
 
✅ The supplier liability is settled.
 
	
	This is a simplified learning entry. Real payment processing can involve bank-clearing accounts, payment media and additional configuration-dependent steps.
	 


# 8. Why GR/IR Matters
 
GR/IR acts as a temporary bridge between the physical receipt of goods and the supplier invoice.
 
## Example

Goods received: 25 March
Supplier invoice received: 3 April
 
## March - Goods Receipt

    Dr Inventory       ₹50,000
      Cr GR/IR                  ₹50,000
 
## April - Invoice Receipt
 
    Dr GR/IR          ₹50,000
       Cr Supplier            ₹50,000
 
This allows the Goods Receipt and Invoice Receipt to be recognized in the periods in which they occur.
 
Open differences are then reviewed through GR/IR reconciliation.
 

# 9. Three-Way Match
 
Three-way matching compares:
       
    Purchase Order
          +
    Goods Receipt
          +
    Supplier Invoice

## Successful Example

    Document	       Quantity	         Unit Price	     Value
    Purchase Order	   100	             ₹500	         ₹50,000
    Goods Receipt	   100	             ---	         ₹50,000
    Supplier Invoice   100	             ₹500	         ₹50,000
 
## Result
 
 ✅ Quantity matches
 ✅ Price matches
 ✅ Value matches
 
Outcome: Normal invoice processing can continue, subject to the applicable SAP configuration and organizational controls.
 

# 10. Partial Goods Receipt
 
A Purchase Order does not always result in a single delivery.
 
## Purchase Order
 
    100 Units × ₹500 = ₹50,000
 
## First Goods Receipt

    60 Units × ₹500 = ₹30,000
 

         Debit	           Credit
    Inventory ₹30,000	GR/IR ₹30,000
 
## Remaining:
 
    100 - 60 = 40 Units
 
## Second Goods Receipt
 
    40 Units × ₹500 = ₹20,000

           Debit	      Credit
    Inventory ₹20,000	GR/IR ₹20,000
 
## Final Position
 
    First GR       ₹30,000
    Second GR      ₹20,000
                   -------
    Total GR       ₹50,000
 
✅ Full PO quantity has now been received.
 

# 11. Quantity Variance
 
Scenario
 
    Document	         Quantity	    Unit Price	        Value
    Purchase Order	     100	        ₹500	            ₹50,000
    Goods Receipt	     80	        	₹500	            ₹40,000
    Supplier Invoice	 100	        ₹500	            ₹50,000
 
## Difference
 
 Unreceived Quantity = 20 Units
 20 × ₹500 = ₹10,000
 
#### Investigation
 
• Are the remaining 20 units still in transit?
• Is a Goods Receipt missing?
• Did the supplier invoice for material not yet received?
• Is the invoice quantity incorrect?
• Does the variance exceed configured tolerances?
	 

**⚠️ Result: Investigation required**.
 
A variance does not automatically lead to the same posting or block in every SAP environment. The outcome depends on configuration and tolerance settings.
 

# 12. Price Variance
 
Scenario

    PO:
    100 Units × ₹500 = ₹50,000

    Invoice:
    100 Units × ₹520 = ₹52,000
 
## Difference

    ₹52,000 - ₹50,000 = ₹2,000
 
### Investigation
 
• Verify the PO price.
• Verify the invoice price.
• Confirm whether a price change was approved.
• Review the applicable tolerance settings.
• Determine whether the PO or invoice requires correction.
	
⚠️ The exact accounting treatment depends on material valuation and SAP configuration.
 

# 13. PO-Based vs Non-PO Invoice
 
    Area	                    PO-Based Invoice	              Non-PO Invoice
    Purchase Order	            Yes	            	              No
    Purchasing Reference        Available	                      Not available
    Goods Receipt	            Can be relevant	                  Not part of the same PO/GR flow
    Three-Way Match	            Can apply	            	      Does not use PO-GR-Invoice matching
    Typical Debit	            GR/IR in base case	              Expense / Asset / Other account
    Credit	                    Supplier	            	      Supplier
 
### PO-Based Example
 
     Dr GR/IR         ₹50,000
        Cr Supplier           ₹50,000
 
### Non-PO Example
 
Professional Services Invoice:

₹15,000
 
## Simplified entry:
 
    Dr Professional Services Expense     ₹15,000
       Cr Supplier                               ₹15,000
 
	
	Actual non-PO account assignment and approval requirements depend on the business process and SAP configuration.
	 

# 14. Tax Learning Example
 
Tax is separated from the base scenario intentionally.
 
## Synthetic Example
 
    Net Invoice           ₹50,000
    Illustrative Tax 18%   ₹9,000
                          -------
    Gross Liability       ₹59,000
 
## Simplified Entry
 
           Debit	             Credit
           GR/IR ₹50,000	
           Input Tax ₹9,000	
	                             Supplier ₹59,000
 
## Balance Check
 
Total Debit     ₹59,000
Total Credit    ₹59,000
 
### ✅ Balanced
 
	
	The 18% rate is used only as a synthetic calculation example. Actual tax treatment depends on jurisdiction, tax code, master data, transaction type and SAP configuration.
	 

# 15. Credit Memo Concept
 
Assume the supplier issues a ₹5,000 credit memo relating to a previous invoice.
 
Business Impact
 
Original Supplier Liability:
₹50,000
 
Supplier Credit Memo:
₹5,000
 
Adjusted Liability:
₹45,000
 
A supplier credit memo reduces the amount owed to the supplier.
 
The exact offsetting account depends on the original transaction and the reason for the credit.
 
For that reason, this portfolio does not assume one universal credit-memo journal entry.
 

# 16. GR/IR Reconciliation
 
GR/IR reconciliation helps identify procurement transactions where Goods Receipt and Invoice Receipt do not align.
 
## Typical Exceptions
 

    Exception                            What It May Indicate
    GR exists, invoice missing           Goods received but invoice not processed
    Invoice exists, GR missing           Invoice arrived before receipt or GR missing
    Quantity mismatch                    Receipt and invoice quantities differ
    Amount mismatch                      PO/GR/invoice values differ
    Old open item                        Transaction requires investigation
    Incorrect reference                  Documents may not be matched correctly
 
## Reconciliation Questions
 
    What was ordered?
            ↓
    What was received?
            ↓
    What was invoiced?
            ↓
    Do quantities match?
            ↓
    Do values match?
            ↓
    If not, why?
           ↓
    Investigate and resolve

 
The objective is to understand the reason for the difference, not simply force the GR/IR balance to zero.
 

# 17. FI-MM Integration Summary
 

    P2P Stage                   Primary Area               Simplified FI Impact
    Purchase Requisition      	MM                         No FI posting in this scenario
    Purchase Order      	    MM                         No FI posting in this scenario
    Goods Receipt      	      	MM + FI                    Dr Inventory / Cr GR/IR
    Invoice Receipt      	    MM-IV + FI-AP              Dr GR/IR / Cr Supplier
    Supplier Open Item       	FI-AP                      Liability remains outstanding
    Payment      	      	    FI-AP                      Dr Supplier / Cr Bank
    Clearing      	      	    FI-AP                      Invoice/payment open items cleared
	

# 18. SAP Reference Transactions
 
    Business Activity                      Classic SAP GUI Reference
    Create Purchase Requisition	           ME51N
    Create Purchase Order                  ME21N
    Display Purchase Order                 ME23N
    Goods Movement / Goods Receipt         MIGO
    Logistics Invoice Verification         MIRO
    Display Invoice Document               MIR4
    Release Blocked Invoices               MRBR
    Automatic Payment Program              F110
    Manual Outgoing Payment                F-53
    Supplier Line Items                    FBL1N
    Supplier Clearing                      F-44
    GR/IR Analysis / Maintenance           MB5S / MR11
 
	
	Transaction codes are included for learning and reference purposes. Availability depends on the SAP S/4HANA edition, release, configuration, activated applications and assigned business roles. This project does not claim that these transactions were executed in a live SAP system unless specifically stated.
	 


# 19. What I Learned
 
Building this accounting section helped me understand:
	• Why PR and PO normally do not create FI postings in this base scenario.
	• How Goods Receipt connects MM with FI.
	• Why Inventory is debited and GR/IR credited at Goods Receipt.
	• How Invoice Receipt converts the temporary GR/IR position into a supplier liability.
	• Why the supplier account is credited at invoice posting.
	• Why the supplier account is debited during payment.
	• How partial Goods Receipts affect accounting.
	• Why quantity and price variances require investigation.
	• The conceptual difference between PO and non-PO invoices.
	• Why tax should be separated from the basic FI-MM learning scenario.
	• Why GR/IR reconciliation is important during period-end activities.


# Disclaimer
 
This is an independent SAP S/4HANA FI-MM learning and portfolio project.
 
All company names, suppliers, materials, Purchase Orders, invoices, amounts and organizational values used in this project are fictional or synthetic.
 
The accounting entries are simplified learning examples. Actual postings depend on business scenario, material valuation, account assignment, tax treatment and SAP configuration.
 
No employer, customer, supplier or other confidential information is used.
 
SAP and SAP S/4HANA are trademarks of SAP SE.
 
This project is not affiliated with or endorsed by SAP SE.
 

