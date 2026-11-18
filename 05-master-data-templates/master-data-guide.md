# SAP S/4HANA FI-MM | Master Data Templates

## 1. Purpose

This folder is Step 05 of the `sap-p2p-process-design` learning portfolio.

It contains simple, reusable templates for two important master-data objects used in the synthetic Nova Manufacturing Procure-to-Pay scenario:

- Supplier / Business Partner
- Material master

The templates show what information should be collected, who normally owns it, and which checks should be completed before data is reviewed or created.

> Portfolio note: This is a conceptual learning design using fictional data. It does not show master data created in a live SAP system.

## 2. Folder structure

```text
05-master-data-templates/
├── Supplier_Master_Template.xlsx
├── Material_Master_Template.xlsx
└── master-data-guide.md
```

Use these exact file names. This is the fifth folder in the existing project, not a separate GitHub repository.

## 3. Synthetic project data

| Element | Fictional value |
|---|---|
| Company | Nova Manufacturing Pvt. Ltd. |
| Company Code | NM01 |
| Plant | PL01 |
| Storage Location | SL01 |
| Purchasing Organization | NP01 |
| Purchasing Group | PG01 |
| Supplier | V10001 - Alpha Industrial Supplies |
| Material | RM-1001 - Synthetic Production Material |
| Currency | INR |
| Base quantity | 100 units |
| Illustrative unit price | ₹500 |
| Illustrative purchase value | ₹50,000 |
| Payment terms | 30 days |

All names, codes, addresses, contact details, quantities, and prices in this folder are fictional.

## 4. Important data-safety rule

Do not add any real employer, supplier, customer, employee, bank, tax, phone, email, pricing, contract, or production-system information to these files.

For sensitive fields, the supplier workbook records only a status such as:

- Not maintained
- Pending validation
- Validated synthetic

It does not store bank-account numbers or real tax IDs.

## 5. How the workbooks are designed

Both workbooks use the same simple visual method:

| Color | Meaning |
|---|---|
| Blue | User input or dropdown selection |
| Green | Formula or automatic check; do not overwrite |
| Amber | Confirmation or caution is needed |

Each workbook includes:

- A `Start Here` sheet
- One fictional sample record in row 5
- Blank template rows for additional fictional records
- Dropdown lists for common choices
- Formula-driven completion percentages
- Duplicate-ID checks
- Cross-sheet ID checks
- A simple field guide
- An automatic and manual quality checklist
- Official learning references
- Excel shortcuts
- A change log

## 6. Supplier workbook

File: `Supplier_Master_Template.xlsx`

### 6.1 Start Here

Explains the workbook purpose, color key, synthetic scenario, and safe-use rules.

### 6.2 Supplier General

Contains the supplier's general Business Partner information, including:

- Supplier ID
- Business Partner category
- Supplier name and search term
- Address and country
- Language
- Fictional contact details
- Tax-ID validation status
- Bank-data validation status
- General block and deletion flag
- Completion percentage
- Review status

The sample supplier is:

```text
V10001 - Alpha Industrial Supplies
```

### 6.3 Company Code Data

Contains Financial Accounting data for company code `NM01`, including:

- Supplier ID
- Company Code
- Reconciliation account description
- Payment terms
- Payment method
- Payment block
- Dunning procedure
- Withholding-tax indicator
- Duplicate-invoice check
- AP owner

The reconciliation account remains conceptual because a productive G/L account was not designed or configured in this portfolio.

### 6.4 Purchasing Data

Contains purchasing-organization data for `NP01`, including:

- Order currency
- Incoterms
- Minimum order value
- GR-based invoice verification
- Evaluated receipt settlement
- Automatic PO indicator
- Purchasing block
- Partner functions
- Purchasing Group `PG01`

### 6.5 Field Guide

Explains each important field in simple English and identifies a typical business owner.

### 6.6 Quality Checklist

Provides automatic counts and manual review checks for:

- Record completeness
- Duplicate supplier IDs
- Consistent IDs across sheets
- Payment controls
- Reconciliation-account wording
- Purchasing settings
- Data privacy
- Version control

### 6.7 References, Notes & Shortcuts, and Change Log

These sheets provide learning sources, keyboard shortcuts, safe-working notes, and version history.

## 7. Material workbook

File: `Material_Master_Template.xlsx`

### 7.1 Start Here

Explains the workbook purpose, color key, synthetic scenario, and safe-use rules.

### 7.2 Material General

Contains cross-plant material information, including:

- Material ID
- Description
- Material type
- Industry sector
- Base unit of measure
- Material group
- Weight information
- Batch-management indicator
- Shelf-life data
- Cross-plant status
- Completion percentage
- Review status

The sample material is:

```text
RM-1001 - Synthetic Production Material
```

### 7.3 Plant & Purchasing

Contains plant and procurement information for `PL01`, including:

- Purchasing Group `PG01`
- Order unit and conversion
- Planned delivery days
- Goods-receipt processing days
- Automatic PO indicator
- Source-list requirement
- Storage Location `SL01`

### 7.4 MRP Data

Contains illustrative planning data, including:

- MRP type
- MRP controller
- Lot size
- Procurement type
- Safety stock
- Reorder point
- Minimum and maximum lot size
- Availability check

The values are learning examples. A real planning policy must be agreed with business and planning teams.

### 7.5 Valuation Data

Contains accounting and valuation fields, including:

- Valuation area
- Valuation class
- Price control
- Standard price
- Moving price
- Currency
- Price unit
- Automatic price check

The sample uses price control `V` and an illustrative moving price of ₹500. The valuation class is intentionally shown as `To be confirmed` because no productive configuration is claimed.

### 7.6 Field Guide

Explains important material fields, their purpose, an example value, and a typical owner.

### 7.7 Quality Checklist

Provides automatic counts and manual review checks for:

- Duplicate material IDs
- Cross-sheet ID consistency
- Units of measure
- Plant and storage-location assignments
- MRP data
- Valuation class wording
- Price control and price maintenance
- Data privacy
- Version control

### 7.8 References, Notes & Shortcuts, and Change Log

These sheets provide learning sources, keyboard shortcuts, safe-working notes, and version history.

## 8. How to enter a new supplier

1. Open `Supplier_Master_Template.xlsx`.
2. Read the `Start Here` sheet.
3. Go to `Supplier General`.
4. Copy a blank row or use the next blank row.
5. Enter a unique fictional Supplier ID.
6. Complete the blue general-data cells.
7. Check the green completion and status cells.
8. Use the same Supplier ID in `Company Code Data`.
9. Complete the blue company-code cells.
10. Use the same Supplier ID in `Purchasing Data`.
11. Complete the blue purchasing cells.
12. Open `Quality Checklist` and review the automatic counts.
13. Complete each manual review item.
14. Update the `Change Log`.
15. Save the workbook without changing its file name.

## 9. How to enter a new material

1. Open `Material_Master_Template.xlsx`.
2. Read the `Start Here` sheet.
3. Go to `Material General`.
4. Copy a blank row or use the next blank row.
5. Enter a unique fictional Material ID.
6. Complete the blue general-data cells.
7. Use the same Material ID in `Plant & Purchasing`.
8. Use the same Material ID in `MRP Data` when planning is in scope.
9. Use the same Material ID in `Valuation Data` when the material is valuated.
10. Confirm that the selected price control has the related illustrative price.
11. Review the green completion and status cells.
12. Open `Quality Checklist` and review the automatic counts.
13. Complete each manual review item.
14. Update the `Change Log`.
15. Save the workbook without changing its file name.

## 10. Suggested ownership

| Data area | Typical owner or reviewer |
|---|---|
| Supplier general data | Supplier Master Data team |
| Supplier company-code data | AP / FI |
| Supplier purchasing data | Procurement |
| Supplier tax data | Tax / AP |
| Supplier bank validation | AP Master Data / Treasury control |
| Material general data | Material Master Data team |
| Material purchasing data | Procurement / MM |
| Plant and storage data | MM / Warehouse |
| MRP data | Planning |
| Material valuation data | FI / MM |

Actual ownership depends on the organization's governance and segregation-of-duties design.

## 11. Pre-upload checklist

Before uploading, confirm all of the following:

- [ ] The folder name is exactly `05-master-data-templates`.
- [ ] The two Excel file names match the required names.
- [ ] The guide file is named `master-data-guide.md`.
- [ ] All three files open correctly.
- [ ] Only fictional data is included.
- [ ] No real bank account, tax ID, supplier, employer, or personal data is present.
- [ ] Formula cells were not overwritten.
- [ ] The quality-check sheets were reviewed.
- [ ] The change logs were updated if edits were made.
- [ ] No duplicate files such as `(1)`, `final`, `corrected`, or `copy` are being uploaded.

## 12. Exact GitHub upload steps

### Option A: Upload with the GitHub website

1. Download and extract the provided `05-master-data-templates.zip` file.
2. Open your existing `sap-p2p-process-design` repository on GitHub.
3. Go to the repository root. Do not open `04-configuration-rationale`.
4. Select `Add file`.
5. Select `Create new file`.
6. In the file-name box, type:

   ```text
   05-master-data-templates/README.md
   ```

7. Add temporary text such as `Folder created`.
8. Select `Commit changes`.
9. Use this commit message:

   ```text
   Create master data templates folder
   ```

10. Open the new `05-master-data-templates` folder.
11. Select `Add file` and then `Upload files`.
12. Drag and drop these three files together:

    ```text
    Supplier_Master_Template.xlsx
    Material_Master_Template.xlsx
    master-data-guide.md
    ```

13. Make sure the upload path shows:

    ```text
    05-master-data-templates/
    ```

14. Use this commit message:

    ```text
    Add supplier and material master data templates
    ```

15. Optional commit description:

    ```text
    Add GitHub-ready supplier and material master templates with synthetic Nova Manufacturing data, field guidance, validation checks, quality reviews, references, shortcuts and change logs.
    ```

16. Select `Commit directly to the main branch`.
17. Select `Commit changes`.
18. Delete the temporary `README.md` if it is still present and not required.

> GitHub does not store empty folders. Creating a temporary file is only needed when the folder does not already exist. You can also upload the three files by dragging the whole extracted folder into the repository root if your browser supports folder upload.

### Option B: Upload with Git commands

From a local copy of the repository:

```bash
mkdir -p 05-master-data-templates
cp Supplier_Master_Template.xlsx 05-master-data-templates/
cp Material_Master_Template.xlsx 05-master-data-templates/
cp master-data-guide.md 05-master-data-templates/
git add 05-master-data-templates
git commit -m "Add supplier and material master data templates"
git push origin main
```

Run the commands only from the local root of the correct repository.

## 13. Verify the GitHub upload

After committing, open:

```text
sap-p2p-process-design/05-master-data-templates/
```

Confirm that the folder shows exactly:

```text
05-master-data-templates/
├── Supplier_Master_Template.xlsx
├── Material_Master_Template.xlsx
└── master-data-guide.md
```

Then verify:

1. The Markdown guide opens and headings render correctly.
2. Both Excel files can be downloaded from GitHub.
3. File names do not contain `(1)`, `copy`, `final`, `corrected`, or `humanized`.
4. There is no temporary `README.md` unless you intentionally keep one.
5. The commit appears in repository history.

## 14. Suggested root README entry

Add this row to the project-status table in the root `README.md`:

```markdown
| 05 | Master Data Templates | Supplier and material master templates with validation and quality checks | Complete |
```

Suggested link section:

```markdown
### 05 - Master Data Templates

- [Master Data Guide](05-master-data-templates/master-data-guide.md)
- [Supplier Master Template](05-master-data-templates/Supplier_Master_Template.xlsx)
- [Material Master Template](05-master-data-templates/Material_Master_Template.xlsx)
```

## 15. How to explain this folder in an interview

Use honest wording such as:

> I designed supplier and material master-data templates for a fictional SAP S/4HANA P2P scenario. I separated general, company-code, purchasing, plant, MRP, and valuation data; added validation choices, completeness checks, cross-sheet ID checks, data-quality controls, and field guidance. The templates demonstrate my functional understanding and do not claim live-system master-data creation.

## 16. Scope and limitations

These templates are not SAP upload programs, migration files, or production data models.

They do not replace:

- Approved business requirements
- SAP configuration
- Data-governance standards
- Legal or tax review
- Bank validation
- Workflow approval
- Role and authorization design
- Migration mapping and load validation
- SAP Fiori or SAP GUI execution

## 17. References

- SAP Learning - [Maintaining Business Partner Master Data](https://learning.sap.com/courses/business-processes-in-sap-s-4hana-sourcing-procurement/maintaining-business-partner-master-data)
- SAP Learning - [Supplier Master Data Fields Relevant for Payables Management](https://learning.sap.com/courses/explaining-payables-management-master-data/explaining-supplier-master-data-fields-relevant-for-payables-management_c9fa0ac7-354f-45f5-a3f1-3e2964393059)
- SAP Learning - [Maintaining Material Master Data](https://learning.sap.com/courses/business-processes-in-sap-s-4hana-sourcing-procurement/maintaining-material-master-data)
- SAP Learning - [Identifying Organizational Units in Procurement](https://learning.sap.com/courses/introducing-sourcing-and-procurement-and-organizational-structure/identifying-the-organizational-units-in-procurement_ab34d826-b383-4344-83b1-ac810a8dc3a3)

---

All project data in this folder is fictional and created for learning purposes.
