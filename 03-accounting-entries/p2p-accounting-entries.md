SAP S/4HANA FI-MM | P2P Accounting Entries

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
 

    Process Stage                   	Debit	               Credit	         Amount	          Business Impact
    Purchase Requisition           No FI Posting         No FI Posting        -	            Internal purchasing requirement
    Purchase Order                 No FI Posting         No FI Posting	      -	           	Purchasing commitment
    Goods Receipt	                 Inventory	           GR/IR	           ₹50,000	        Material received into stock
    Invoice Receipt	               GR/IR	               Supplier	         ₹50,000	        Supplier liability created
    Payment	                       Supplier	             Bank	             ₹50,000	        Supplier liability settled
    Clearing	                        -	                   -	                -	            Invoice and payment cleared
 

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
 

5. Invoice Receipt Accounting
 
The supplier submits an invoice for the material received.
 
Invoice Quantity: 100 Units
 Unit Price: ₹500
 Net Invoice: ₹50,000
 
Accounting Entry
 

Debit	Credit
GR/IR ₹50,000	Supplier ₹50,000
 
Why?
 
GR/IR is debited to offset the credit created at Goods Receipt.
 
Supplier is credited because the business now owes ₹50,000 to the supplier.
 
GR/IR Position
 
 

GR Posting Cr ₹50,000
Invoice Posting Dr ₹50,000
-----------
Balance ₹0
 
✅ In the matching base scenario, the GR/IR values offset.
 

6. Supplier Open Item
 
After invoice posting:
 
 

Supplier: V10001 - Alpha Industrial Supplies
Invoice Amount: ₹50,000
Open Liability: ₹50,000
Status: Awaiting Payment
 
The liability remains open until payment and clearing.
 
Payment can depend on:
 
	
	• Payment terms
	 
	• Due date
	 
	• Payment blocks
	 
	• Required approvals
	 
	• Valid supplier payment information
	 


7. Payment and Clearing
 
Assume the invoice is due and approved for payment.
 
Payment Entry
 

Debit	Credit
Supplier ₹50,000	Bank ₹50,000
 
Position
 
 

Supplier Liability Before Payment ₹50,000
Payment ₹50,000
-------
Remaining Liability ₹0
 
✅ The supplier liability is settled.
 
	
	This is a simplified learning entry. Real payment processing can involve bank-clearing accounts, payment media and additional configuration-dependent steps.
	 


8. Why GR/IR Matters
 
GR/IR acts as a temporary bridge between the physical receipt of goods and the supplier invoice.
 
Example
 
Goods received:
 
25 March
 
Supplier invoice received:
 
3 April
 
March - Goods Receipt
 
 

Dr Inventory ₹50,000
Cr GR/IR ₹50,000
 
April - Invoice Receipt
 
 

Dr GR/IR ₹50,000
Cr Supplier ₹50,000
 
This allows the Goods Receipt and Invoice Receipt to be recognized in the periods in which they occur.
 
Open differences are then reviewed through GR/IR reconciliation.
 

9. Three-Way Match
 
Three-way matching compares:
 
 

Purchase Order
+
Goods Receipt
+
Supplier Invoice
 
Successful Example
 

Document	Quantity	Unit Price	Value
Purchase Order	100	₹500	₹50,000
Goods Receipt	100	-	₹50,000
Supplier Invoice	100	₹500	₹50,000
 
Result
 
✅ Quantity matches
 ✅ Price matches
 ✅ Value matches
 
Outcome: Normal invoice processing can continue, subject to the applicable SAP configuration and organizational controls.
 

10. Partial Goods Receipt
 
A Purchase Order does not always result in a single delivery.
 
Purchase Order
 
 

100 Units × ₹500 = ₹50,000
 
First Goods Receipt
 
 

60 Units × ₹500 = ₹30,000
 

Debit	Credit
Inventory ₹30,000	GR/IR ₹30,000
 
Remaining:
 
 

100 - 60 = 40 Units
 
Second Goods Receipt
 
 

40 Units × ₹500 = ₹20,000
 

Debit	Credit
Inventory ₹20,000	GR/IR ₹20,000
 
Final Position
 
 

First GR ₹30,000
Second GR ₹20,000
-------
Total GR ₹50,000
 
✅ Full PO quantity has now been received.
 

11. Quantity Variance
 
Scenario
 

Document	Quantity	Unit Price	Value
Purchase Order	100	₹500	₹50,000
Goods Receipt	80	₹500	₹40,000
Supplier Invoice	100	₹500	₹50,000
 
Difference
 
 

Unreceived Quantity = 20 Units
 
20 × ₹500 = ₹10,000
 
Investigation
 
	
	• Are the remaining 20 units still in transit?
	 
	• Is a Goods Receipt missing?
	 
	• Did the supplier invoice for material not yet received?
	 
	• Is the invoice quantity incorrect?
	 
	• Does the variance exceed configured tolerances?
	 

⚠️ Result: Investigation required.
 
A variance does not automatically lead to the same posting or block in every SAP environment. The outcome depends on configuration and tolerance settings.
 

12. Price Variance
 
Scenario
 
 

PO:
100 Units × ₹500 = ₹50,000
 
Invoice:
100 Units × ₹520 = ₹52,000
 
Difference
 
 

₹52,000 - ₹50,000 = ₹2,000
 
Investigation
 
	
	• Verify the PO price.
	 
	• Verify the invoice price.
	 
	• Confirm whether a price change was approved.
	 
	• Review the applicable tolerance settings.
	 
	• Determine whether the PO or invoice requires correction.
	 

⚠️ The exact accounting treatment depends on material valuation and SAP configuration.
 

13. PO-Based vs Non-PO Invoice
 

Area	PO-Based Invoice	Non-PO Invoice
Purchase Order	Yes	No
Purchasing Reference	Available	Not available
Goods Receipt	Can be relevant	Not part of the same PO/GR flow
Three-Way Match	Can apply	Does not use PO-GR-Invoice matching
Typical Debit	GR/IR in base case	Expense / Asset / Other account
Credit	Supplier	Supplier
 
PO-Based Example
 
 

Dr GR/IR ₹50,000
Cr Supplier ₹50,000
 
Non-PO Example
 
Professional Services Invoice:
 
 

₹15,000
 
Simplified entry:
 
 

Dr Professional Services Expense ₹15,000
Cr Supplier ₹15,000
 
	
	Actual non-PO account assignment and approval requirements depend on the business process and SAP configuration.
	 


14. Tax Learning Example
 
Tax is separated from the base scenario intentionally.
 
Synthetic Example
 
 

Net Invoice ₹50,000
Illustrative Tax 18% ₹9,000
-------
Gross Liability ₹59,000
 
Simplified Entry
 

Debit	Credit
GR/IR ₹50,000	
Input Tax ₹9,000	
	Supplier ₹59,000
 
Balance Check
 
 

Total Debit ₹59,000
Total Credit ₹59,000
 
✅ Balanced
 
	
	The 18% rate is used only as a synthetic calculation example. Actual tax treatment depends on jurisdiction, tax code, master data, transaction type and SAP configuration.
	 


15. Credit Memo Concept
 
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
 

16. GR/IR Reconciliation
 
GR/IR reconciliation helps identify procurement transactions where Goods Receipt and Invoice Receipt do not align.
 
Typical Exceptions
 

Exception	What It May Indicate
GR exists, invoice missing	Goods received but invoice not processed
Invoice exists, GR missing	Invoice arrived before receipt or GR missing
Quantity mismatch	Receipt and invoice quantities differ
Amount mismatch	PO/GR/invoice values differ
Old open item	Transaction requires investigation
Incorrect reference	Documents may not be matched correctly
 
Reconciliation Questions
 
 

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
 

17. FI-MM Integration Summary
 

P2P Stage	Primary Area	Simplified FI Impact
Purchase Requisition	MM	No FI posting in this scenario
Purchase Order	MM	No FI posting in this scenario
Goods Receipt	MM + FI	Dr Inventory / Cr GR/IR
Invoice Receipt	MM-IV + FI-AP	Dr GR/IR / Cr Supplier
Supplier Open Item	FI-AP	Liability remains outstanding
Payment	FI-AP	Dr Supplier / Cr Bank
Clearing	FI-AP	Invoice/payment open items cleared
GR/IR Reconciliation	MM + FI	Review unresolved differences
 

18. SAP Reference Transactions
 

Business Activity	Classic SAP GUI Reference
Create Purchase Requisition	ME51N
Create Purchase Order	ME21N
Display Purchase Order	ME23N
Goods Movement / Goods Receipt	MIGO
Logistics Invoice Verification	MIRO
Display Invoice Document	MIR4
Release Blocked Invoices	MRBR
Automatic Payment Program	F110
Manual Outgoing Payment	F-53
Supplier Line Items	FBL1N
Supplier Clearing	F-44
GR/IR Analysis / Maintenance	MB5S / MR11
 
	
	Transaction codes are included for learning and reference purposes. Availability depends on the SAP S/4HANA edition, release, configuration, activated applications and assigned business roles. This project does not claim that these transactions were executed in a live SAP system unless specifically stated.
	 


19. What I Learned
 
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
	 


20. Portfolio Status
 
Business Scenario: Complete ✅
 P2P Glossary: Complete ✅
 Process Flow: Complete ✅
 Accounting Entries: Complete ✅
 Three-Way Match Scenarios: Next 🔄
 Functional Test Cases: Planned
 Exception Analysis: Planned
 SAP Practice: Planned
 Lessons Learned: Planned
 

Disclaimer
 
This is an independent SAP S/4HANA FI-MM learning and portfolio project.
 
All company names, suppliers, materials, Purchase Orders, invoices, amounts and organizational values used in this project are fictional or synthetic.
 
The accounting entries are simplified learning examples. Actual postings depend on business scenario, material valuation, account assignment, tax treatment and SAP configuration.
 
No employer, customer, supplier or other confidential information is used.
 
SAP and SAP S/4HANA are trademarks of SAP SE.
 
This project is not affiliated with or endorsed by SAP SE.
 





# SAP S/4HANA FI-MM Procure-to-Pay Accounting Entries

### Project Overview

This document is part of my independent SAP S/4HANA FI-MM Procure-to-Pay learning portfolio.

The purpose of this section is to understand the accounting logic behind the P2P process and how Materials Management (MM) transactions integrate with Financial Accounting (FI) and Accounts Payable (AP).

The examples use the same fictional business scenario throughout the project so that the purchasing documents, accounting entries, and exception scenarios remain connected.

All values and business data in this project are synthetic.

# 1. Synthetic Business Scenario
 
    Company:               Nova Manufacturing Pvt. Ltd.
    Company Code:          NM01
    Plant:                 PL01
    Supplier:              V10001 - Alpha Industrial Supplies
    Material:              RM-1001
    Material Description:  Synthetic Production Material
    Purchase Quantity:     100 Units
    Unit Price:            ₹500
    Net Purchase Value:    ₹50,000
    
Tax: Excluded from the initial scenario for simplicity

This base case assumes a standard purchase of valuated stock material.
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

# 2. Accounting Flow Overview

The simplified accounting flow used in the base scenario is:
Purchase Requisition ↓ No FI Posting
Purchase Order ↓ No FI Posting
Goods Receipt ↓ Debit Inventory ₹50,000 Credit GR/IR ₹50,000
Supplier Invoice ↓ Debit GR/IR ₹50,000 Credit Supplier ₹50,000
Payment ↓ Debit Supplier ₹50,000 Credit Bank ₹50,000
Clearing ↓ Supplier Open Item Cleared
GR/IR Reconciliation ↓ Review Remaining GR/IR Differences
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

# 3. Purchase Requisition

A Purchase Requisition represents an internal requirement to purchase a material or service.
Synthetic Example:
Material: RM-1001
Quantity: 100 Units
Expected Unit Price: ₹500
Expected Value: ₹50,000
Accounting Impact:
No FI accounting entry is created in this standard learning scenario.
Reason:
The Purchase Requisition represents an internal procurement requirement. No goods have been received and no supplier liability has been created yet.
Key Learning:
A Purchase Requisition initiates the purchasing process but does not itself create a financial posting in this scenario.

4. Purchase Order
After the requirement is approved, Purchasing creates a Purchase Order for the selected supplier.
Synthetic Example:
Supplier: V10001 - Alpha Industrial Supplies
Material: RM-1001
Quantity: 100 Units
Unit Price: ₹500
Net PO Value: ₹50,000
Accounting Impact:
No FI accounting entry is created when the Purchase Order is created in this standard stock-procurement learning scenario.
Reason:
The Purchase Order represents a commitment to purchase, but the material has not yet been received and the supplier invoice has not yet been posted.
Key Learning:
The Purchase Order establishes important procurement information such as the supplier, quantity, price, and account-assignment information used later in the process.

5. Goods Receipt - Stock Material
The supplier delivers all 100 units of RM-1001.
Purchase Order Quantity: 100 Units
Goods Receipt Quantity: 100 Units
Value Received: ₹50,000
Simplified Accounting Entry:
Debit: Inventory ₹50,000
Credit: GR/IR ₹50,000
Explanation:
Inventory is debited because the company has received material with economic value.
GR/IR is credited because the goods have been received but the corresponding supplier invoice has not yet been fully accounted for in the process.
Accounting Equation:
Inventory increases by ₹50,000.
GR/IR carries the corresponding credit of ₹50,000.
Key Learning:
Goods Receipt is one of the key points where the MM procurement process creates an FI accounting impact.

6. GR/IR Clearing Account
GR/IR stands for:
Goods Receipt / Invoice Receipt
The GR/IR clearing account helps bridge the timing difference between receiving goods and receiving the corresponding supplier invoice.
After Goods Receipt:
Debit: Inventory ₹50,000
Credit: GR/IR ₹50,000
GR/IR Balance After GR:
Credit ₹50,000
The balance represents goods that have been received but whose corresponding invoice has not yet offset the GR/IR posting.

7. Supplier Invoice Receipt
The supplier submits an invoice relating to the 100 units received.
Supplier: V10001 - Alpha Industrial Supplies
Invoice Quantity: 100 Units
Invoice Unit Price: ₹500
Net Invoice Value: ₹50,000
Simplified Accounting Entry:
Debit: GR/IR ₹50,000
Credit: Supplier ₹50,000
Explanation:
GR/IR is debited because the supplier invoice is being matched against the value previously credited to GR/IR during Goods Receipt.
The supplier account is credited because the company now owes the supplier ₹50,000.
After Invoice Receipt:
GR/IR Debit: ₹50,000
Earlier GR/IR Credit: ₹50,000
Net GR/IR Balance for the matching example: ₹0
Supplier Liability: ₹50,000
Key Learning:
Invoice Receipt transfers the liability from the temporary GR/IR clearing relationship to the supplier account.

8. Supplier Open Item
After the invoice is posted, the supplier account contains an open liability.
Supplier: V10001 - Alpha Industrial Supplies
Invoice Amount: ₹50,000
Supplier Open Item: ₹50,000
Status: Open - Awaiting Payment
The open item remains outstanding until the invoice is paid and cleared according to the applicable Accounts Payable process.
Factors affecting payment can include:
Payment terms
Due date
Payment blocks
Required approvals
Supplier payment information
Cash-discount conditions where applicable

9. Supplier Payment
Assume the invoice becomes due and is approved for payment.
Payment Amount: ₹50,000
Simplified Accounting Entry:
Debit: Supplier ₹50,000
Credit: Bank ₹50,000
Explanation:
The supplier account is debited because the liability is being reduced.
The bank is credited in this simplified example because cash is being paid to the supplier.
Supplier Liability Before Payment: ₹50,000
Payment: ₹50,000
Remaining Supplier Liability: ₹0
Important Note:
Real SAP payment accounting can involve bank-clearing accounts, payment media, approval workflows, and additional clearing steps depending on system and organizational configuration.

10. Supplier Clearing
After payment, the supplier invoice and the corresponding payment can be cleared.
The purpose of clearing is to link the supplier invoice with the payment so that the invoice no longer remains as an outstanding open item.
Before Clearing:
Supplier Invoice: Open
After Payment and Clearing:
Supplier Invoice: Cleared
Remaining Open Liability: ₹0
Key Learning:
Posting a payment and clearing an open item are related activities, but the exact process depends on the organization's SAP configuration.

11. Complete Base-Case Accounting Flow
Step 1 - Purchase Requisition
Accounting Entry:
None
Reason:
Internal purchasing requirement.

Step 2 - Purchase Order
Accounting Entry:
None
Reason:
Purchasing commitment in this standard learning scenario.

Step 3 - Goods Receipt
Debit: Inventory ₹50,000
Credit: GR/IR ₹50,000
Result:
Inventory increases and GR/IR receives the temporary credit.

Step 4 - Invoice Receipt
Debit: GR/IR ₹50,000
Credit: Supplier ₹50,000
Result:
GR/IR is offset and supplier liability is created.

Step 5 - Payment
Debit: Supplier ₹50,000
Credit: Bank ₹50,000
Result:
Supplier liability is reduced and cash is paid.

Step 6 - Clearing
Result:
Supplier invoice is cleared against the corresponding payment.

12. Why GR/IR Is Important
Suppose the goods arrive on 25 March but the supplier invoice arrives on 3 April.
The company needs to recognize the Goods Receipt in March even though the invoice arrives in April.
At Goods Receipt:
Debit: Inventory ₹50,000
Credit: GR/IR ₹50,000
The GR/IR account therefore provides the temporary accounting bridge between the logistics event and the supplier invoice.
When the invoice arrives:
Debit: GR/IR ₹50,000
Credit: Supplier ₹50,000
If the relevant values match, the GR/IR postings for that transaction can offset each other.
This demonstrates why GR/IR is important for FI-MM integration and period-end analysis.

13. Partial Goods Receipt Scenario
Purchase Order:
100 Units × ₹500 = ₹50,000
First Goods Receipt:
60 Units
Value:
60 × ₹500 = ₹30,000
Accounting Entry:
Debit: Inventory ₹30,000
Credit: GR/IR ₹30,000
Remaining PO Quantity:
40 Units
If the remaining 40 units are received later:
Value:
40 × ₹500 = ₹20,000
Accounting Entry:
Debit: Inventory ₹20,000
Credit: GR/IR ₹20,000
Total Goods Receipt Value:
₹30,000 + ₹20,000 = ₹50,000
Key Learning:
A Purchase Order does not necessarily result in one single Goods Receipt. Partial receipts can occur depending on the business scenario.

14. Missing Invoice Scenario
Assume all goods have been received.
Goods Receipt Value: ₹50,000
Supplier Invoice: Not yet received
Accounting Position:
Debit: Inventory ₹50,000
Credit: GR/IR ₹50,000
GR/IR therefore contains an open credit relating to the Goods Receipt.
Possible reason:
Goods have physically arrived but the supplier invoice has not yet been received or processed.
Required Action:
Investigate the open GR/IR item according to the applicable reconciliation process.

15. Missing Goods Receipt Scenario
Assume the Purchase Order exists and the supplier invoice is received, but the Goods Receipt has not yet been recorded.
This creates an exception that requires investigation.
Questions to investigate:
Was the material physically received?
Was the Goods Receipt omitted?
Was the invoice received before the goods?
Is the invoice related to the correct Purchase Order?
Should the invoice continue through normal processing?
Important Learning:
Goods Receipt and Invoice Receipt can occur at different times. The exact posting and blocking behaviour depends on the business scenario and configured controls.

16. Quantity Variance Scenario
Purchase Order Quantity: 100 Units
Goods Receipt Quantity: 80 Units
Supplier Invoice Quantity: 100 Units
Difference:
20 Units
At ₹500 per unit:
Potential Quantity Difference:
20 × ₹500 = ₹10,000
Required Investigation:
Check whether the remaining 20 units are still expected.
Check whether the supplier invoiced for material that has not yet been received.
Check whether a Goods Receipt is missing.
Check the applicable tolerance and blocking configuration.
Important:
A variance does not automatically mean the same accounting treatment in every SAP system.
The result depends on configuration, tolerances, business rules, and the actual cause of the variance.

17. Price Variance Scenario
Purchase Order:
100 Units × ₹500 = ₹50,000
Supplier Invoice:
100 Units × ₹520 = ₹52,000
Price Difference:
₹52,000 - ₹50,000 = ₹2,000
Required Investigation:
Check the Purchase Order price.
Check the supplier invoice.
Confirm whether the price change is valid.
Determine whether the Purchase Order requires correction.
Review configured invoice tolerances.
Determine whether the invoice should be blocked or otherwise handled according to business rules.
Important Learning:
The exact accounting treatment for price differences depends on factors such as material valuation and SAP configuration.
Therefore, this learning project does not assume a universal posting for the ₹2,000 difference.

18. Expense Purchase Scenario
Not every purchase is posted to Inventory.
A company may purchase consumable items or services that are charged to an expense or cost object.
Example:
Office Supplies Purchase: ₹10,000
For a simplified expense-procurement scenario, the accounting may involve an expense or consumption account instead of Inventory.
Example Concept at Goods Receipt:
Debit: Expense / Consumption ₹10,000
Credit: GR/IR ₹10,000
At Invoice Receipt:
Debit: GR/IR ₹10,000
Credit: Supplier ₹10,000
Important:
The exact account depends on account assignment, material type, valuation, and configuration.

19. PO-Based vs Non-PO Invoice
PO-Based Invoice
A PO-based invoice has a purchasing reference.
The process can use information from:
Purchase Order
Goods Receipt
Supplier Invoice
This supports invoice verification and three-way matching where applicable.
Typical Accounting Concept for the Base Scenario:
Debit: GR/IR
Credit: Supplier

Non-PO Invoice
A non-PO invoice does not use the same Purchase Order-based logistics flow.
Depending on the business scenario, a non-PO invoice may post directly to an expense, asset, or another applicable account.
Simplified Example:
Supplier invoice for professional services: ₹15,000
Debit: Professional Services Expense ₹15,000
Credit: Supplier ₹15,000
Important:
The actual account assignment, approval process, tax treatment, and posting logic depend on organizational policy and SAP configuration.

20. Credit Memo Scenario
Assume the supplier previously invoiced ₹50,000 but later issues a ₹5,000 credit memo.
A supplier credit memo reduces the amount owed to the supplier.
Simplified Concept:
Debit: Supplier ₹5,000
Credit: Relevant Expense / Inventory / Adjustment Account ₹5,000
Important:
The exact offsetting account depends on the original transaction and the business reason for the credit memo.
This project therefore treats this entry as a simplified accounting concept rather than a universal SAP posting rule.

21. Tax Example - Separate Learning Scenario
Tax was excluded from the base ₹50,000 scenario so that the FI-MM integration can be understood first.
For a separate simplified learning example, assume:
Net Invoice Amount: ₹50,000
Illustrative Input Tax Rate: 18%
Illustrative Input Tax:
₹50,000 × 18% = ₹9,000
Gross Supplier Liability:
₹50,000 + ₹9,000 = ₹59,000
Simplified Invoice Entry:
Debit: GR/IR ₹50,000
Debit: Input Tax ₹9,000
Credit: Supplier ₹59,000
Important:
The 18% rate is used only as a synthetic numerical example.
Actual tax determination depends on the jurisdiction, tax code, transaction type, master data, and SAP configuration.
This project does not treat 18% as a universal tax rate.

22. GR/IR Reconciliation
GR/IR reconciliation helps identify Goods Receipt and Invoice Receipt items that have not offset as expected.
Examples include:
Goods Receipt exists but invoice is missing.
Invoice exists but Goods Receipt is missing.
Goods Receipt and Invoice Receipt quantities differ.
Amounts differ.
Incorrect references exist.
Old GR/IR items remain unresolved.
A simple reconciliation review can ask:
What was ordered?
What was received?
What was invoiced?
Do the quantities match?
Do the values match?
Is an item still expected?
Does a business document need correction?
Does the difference require further investigation?
The objective is to understand and resolve the reason for the open item rather than simply force a balance to zero.

23. Debit and Credit Logic
Inventory
Normal increase:
Debit
Reason:
Inventory is an asset.

Expense
Normal increase:
Debit
Reason:
Expenses normally increase on the debit side.

GR/IR
At Goods Receipt:
Credit
At matching Invoice Receipt:
Debit
Purpose:
Temporary clearing relationship between receipt and invoice.

Supplier
At Invoice Posting:
Credit
Reason:
The company owes money to the supplier.
At Payment:
Debit
Reason:
The supplier liability is reduced.

Bank
In the simplified payment entry:
Credit
Reason:
Cash decreases when the supplier is paid.

24. Simple T-Account View
Inventory
Debit:
Goods Receipt ₹50,000
Balance Increase:
₹50,000

GR/IR
Credit:
Goods Receipt ₹50,000
Debit:
Invoice Receipt ₹50,000
Matching Example Balance:
₹0

Supplier
Credit:
Invoice Receipt ₹50,000
Debit:
Payment ₹50,000
Matching Example Balance:
₹0

Bank
Credit:
Payment ₹50,000
Cash decreases by:
₹50,000

25. FI-MM Integration Summary
Purchase Requisition
Main Area:
MM / Purchasing
FI Impact:
No FI posting in this learning scenario.

Purchase Order
Main Area:
MM / Purchasing
FI Impact:
No FI posting in this learning scenario.

Goods Receipt
Main Area:
MM / Inventory Management
FI Impact:
Debit Inventory or applicable consumption/expense account
Credit GR/IR

Invoice Receipt
Main Area:
MM Invoice Verification + FI/AP
FI Impact in the base case:
Debit GR/IR
Credit Supplier

Supplier Payment
Main Area:
FI/AP
Simplified FI Impact:
Debit Supplier
Credit Bank

Clearing
Main Area:
FI/AP
Purpose:
Clear the supplier invoice against the corresponding payment.

GR/IR Reconciliation
Main Areas:
MM + FI
Purpose:
Identify and resolve differences between Goods Receipt and Invoice Receipt.

26. SAP Transaction References
The following classic SAP GUI transaction codes are included for learning and reference purposes.
Purchase Requisition:
ME51N - Create Purchase Requisition
Purchase Order:
ME21N - Create Purchase Order
ME23N - Display Purchase Order
Goods Receipt:
MIGO - Goods Movement / Goods Receipt Processing
Supplier Invoice:
MIRO - Logistics Invoice Verification
MIR4 - Display Invoice Document
Blocked Invoice:
MRBR - Release Blocked Invoices
Automatic Payment:
F110 - Automatic Payment Program
Manual Outgoing Payment:
F-53 - Manual Outgoing Payment
Supplier Open Items:
FBL1N - Supplier/Vendor Line Item Display reference
Supplier Clearing:
F-44 - Clear Supplier
GR/IR:
MB5S / MR11 - Classic GR/IR analysis or maintenance references, subject to system availability.
Important Note:
SAP S/4HANA supports SAP Fiori applications as well as classic SAP GUI transactions depending on the edition, release, activated applications, configuration, and assigned business roles.
The transaction codes above are included as learning references.
This portfolio does not claim that these transactions were executed in a live SAP system unless specifically stated.

27. Accounting Controls
Key controls I would expect around this process include:
Purchase Controls:
Approved purchasing requirement
Appropriate supplier
Correct Purchase Order
Agreed quantity and price
Required approval
Receiving Controls:
Correct material
Correct quantity
Valid PO reference
Accurate Goods Receipt
Invoice Controls:
Valid supplier
Valid invoice reference
PO verification
GR verification where applicable
Quantity check
Price check
Duplicate invoice check
Applicable tolerance checks
Payment Controls:
Correct supplier
Correct due date
Valid payment terms
Applicable payment block review
Approved bank information
Required payment authorization
Reconciliation Controls:
Supplier open-item review
GR/IR review
Old-item investigation
Correction of identified errors
Period-end reconciliation

28. What I Learned
Through this accounting-entry section, I developed a stronger understanding of:
Why Purchase Requisition and Purchase Order normally do not create FI postings in this base scenario.
How Goods Receipt creates the first FI impact in stock procurement.
Why Inventory is debited at Goods Receipt.
Why GR/IR is credited at Goods Receipt.
Why GR/IR is debited during a matching Invoice Receipt.
How the supplier liability is created.
Why the supplier account is debited during payment.
How Goods Receipt and Invoice Receipt can occur in different periods.
Why GR/IR is important for period-end reconciliation.
How partial receipts affect the accounting flow.
Why quantity and price variances require investigation.
How PO-based and non-PO invoices differ conceptually.
How supplier credit memos reduce the amount owed.
Why tax should be treated separately from the basic FI-MM integration example.
How MM purchasing transactions ultimately connect with FI and Accounts Payable.

29. Interview Practice Questions
	1. What accounting entry is generated at Goods Receipt for a standard stock-material procurement scenario?
	2. Why is the GR/IR account required?
	3. What is the accounting impact of Purchase Order creation?
	4. What happens to GR/IR when a matching supplier invoice is posted?
	5. Why is the supplier account credited when an invoice is posted?
	6. Why is the supplier account debited at payment?
	7. What happens when goods are received but the supplier invoice has not arrived by month-end?
	8. What could cause an open GR/IR balance?
	9. What is the difference between a PO-based invoice and a non-PO invoice?
	10. What would you investigate if the PO quantity is 100, GR quantity is 80, and invoice quantity is 100?
	11. What would you investigate if the PO price is ₹500 but the supplier invoices ₹520?
	12. How does a supplier credit memo affect the supplier liability?
	13. Why can an expense account be used instead of Inventory?
	14. What is the relationship between MM Invoice Verification and FI/AP?
	15. What is the purpose of supplier open-item clearing?

30. Next Steps in This Portfolio
The next deliverables for this accounting section will include:
P2P Accounting Entries Excel workbook
Formula-driven accounting examples
Synthetic debit and credit scenarios
GR/IR reconciliation examples
PO-based and non-PO invoice comparison
Variance scenarios
Functional test cases
The project will be expanded only as each topic is studied and understood.

Project Disclaimer
This is an independent SAP S/4HANA FI-MM learning and portfolio project.
All company names, suppliers, materials, Purchase Orders, invoices, amounts, organizational values, accounting examples, and transaction information used in the project are fictional or synthetic.
No employer, customer, supplier, or other confidential business information is used.
The accounting entries are simplified learning examples. Actual postings depend on the business scenario, material valuation, account assignment, tax treatment, SAP S/4HANA edition, system configuration, and organizational policies.
The transaction codes included in this document are for learning and reference purposes and do not represent a claim of hands-on execution in a live SAP environment.
SAP and SAP S/4HANA are trademarks of SAP SE.
This project is not affiliated with or endorsed by SAP SE.
