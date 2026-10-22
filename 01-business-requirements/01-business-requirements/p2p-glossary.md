Business Scenario – Procure-to-Pay (P2P)
Project Type
Independent SAP S/4HANA FI/MM learning project using synthetic data.

Business Background
Nova Manufacturing Pvt. Ltd. is a fictional manufacturing company that purchases raw materials from suppliers for production.

For this learning scenario, the company requires 100 units of raw material.

Purchase Requirement
Material: RM-1001
Description: Synthetic Production Material
Quantity: 100 units
Unit Price: ₹500
Net PO Value: ₹50,000
Vendor: V10001 – Alpha Industrial Supplies
Company Code: NM01
Plant: PL01
All company, vendor, material and transaction details are fictitious.

Business Process
A department identifies the need for 100 units of raw material.
A Purchase Requisition (PR) is created.
After approval, Purchasing creates a Purchase Order (PO).
The vendor supplies the material.
The warehouse records the Goods Receipt (GR).
The supplier invoice is received.
The PO, GR and invoice are compared as part of the three-way match.
If the invoice satisfies the applicable checks, it is posted.
The vendor liability becomes available for the payment process.
After payment, the vendor open item is cleared.
GR/IR open items are reviewed as part of reconciliation.
Project Objectives
This project is designed to demonstrate my understanding of:

Procure-to-Pay
Accounts Payable
SAP FI-MM integration
Purchase Orders
Goods Receipt
Invoice Receipt
Three-Way Matching
GR/IR
Vendor accounting
Payment and clearing
Invoice exception handling
Planned Deliverables
Business requirements
P2P glossary
Process-flow diagram
Accounting-entry examples
Synthetic transaction dataset
Test cases
Exception scenarios
Defect log
Lessons learned
Disclaimer
This is an independent learning project created with synthetic data. It does not contain information, documents, screenshots, configurations, processes, or data from any employer, customer, or vendor.# Procure-to-Pay (P2P) Glossary

This glossary explains common Procure-to-Pay (P2P) and SAP S/4HANA FI/MM terms in simple business language.

## 1. Purchase Requisition (PR)
A Purchase Requisition is an internal request to purchase a material or service. It identifies what is required, the required quantity, and when it is needed. A PR is normally part of the internal purchasing process and is not the supplier invoice.

## 2. Purchase Order (PO)
A Purchase Order is an approved purchasing document issued to a supplier. It contains information such as the material or service, quantity, price, delivery details, and agreed purchasing conditions.

## 3. Goods Receipt (GR)
A Goods Receipt records that goods ordered from a supplier have been received. It confirms the received quantity and provides an important reference for matching the supplier invoice.

## 4. GR/IR (Goods Receipt / Invoice Receipt)
GR/IR is a clearing account used to manage timing differences between receiving goods and receiving the related supplier invoice. The corresponding GR and invoice receipt entries should ultimately be cleared when they match.

## 5. Three-Way Match
Three-Way Match compares the Purchase Order (PO), Goods Receipt (GR), and Invoice Receipt (IR). Key details such as quantity and price are checked within configured tolerance limits before an invoice can proceed normally through the payment process.

## 6. Vendor Master
The Vendor Master contains important supplier information used in purchasing and accounting processes, such as supplier details, payment information, bank information, and accounting-related data.

## 7. Payment Terms
Payment Terms define when an invoice becomes due for payment and may also specify conditions
