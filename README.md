# Fintech Merchant Data Entry & Quality Control

A simulated fintech merchant inventory data-entry and quality-control project demonstrating how Excel and Power Query can be used to clean, standardize, validate, reconcile, and prepare merchant inventory data for bulk upload.

> **Project type:** Simulated portfolio project
> **Tools:** Microsoft Excel, Power Query
> **Output:** Clean UTF-8 CSV suitable for downstream processing

---

## Project Overview

This project simulates a fintech data-entry workflow where merchant inventory records need to be prepared for a bulk-upload process.

The source dataset intentionally contains common data-entry and data-quality problems, including:

* Truncated SKU codes
* Inconsistent category values
* Corrupted character encodings
* Text values in numeric fields
* Missing/invalid date values
* Inconsistent date formats
* Price differences between uploaded records and an internal reference dataset
* Category differences between uploaded records and an internal reference dataset

I used **Microsoft Excel and Power Query** to clean and standardize the data, perform a reconciliation against a simulated internal master dataset, identify records requiring review, and prepare a final production CSV.

This is a **simulated project created for portfolio and skills-demonstration purposes**. It is not based on confidential fintech company data or an actual company's internal workflow.

---

## Objectives

The project was designed to demonstrate the ability to:

1. Clean messy data using Power Query.
2. Standardize fields before bulk upload.
3. Correct structural issues in inventory identifiers.
4. Handle missing and malformed values.
5. Reconcile incoming records against a reference dataset.
6. Identify discrepancies requiring manual review.
7. Add Excel validation controls to reduce future data-entry errors.
8. Produce a clean CSV export for downstream use.

---

## Tools & Technologies

* **Microsoft Excel**
* **Power Query**
* **CSV**
* Excel Data Validation
* Power Query Merge
* Conditional logic
* Data type transformation
* Text transformation

---

## Dataset

The project uses simulated fintech merchant inventory data.

The main fields include:

| Field          | Description                                 |
| -------------- | ------------------------------------------- |
| `Product_ID`   | Unique identifier for the inventory product |
| `SKU_Code`     | Inventory tracking code                     |
| `Product_Name` | Product description/name                    |
| `Category`     | Product category                            |
| `Price`        | Merchant/product price                      |
| `Stock_Level`  | Available stock quantity                    |
| `Date_Added`   | Date the product was added                  |

A separate simulated **CRM/System Master** dataset was used as the reference point for reconciliation.

---

# 1. Data Cleaning

The initial dataset contained several data-quality problems.

### SKU Code Standardization

Some SKU codes were shorter than the expected 8-character structure.

I used Power Query to restore the missing leading zeros using:

```text
Text.PadStart([SKU_Code], 8, "0")
```

After cleaning, the SKU codes were standardized to 8 characters.

### Price and Stock Fields

The `Price` and `Stock_Level` fields were converted to appropriate numeric data types for consistent downstream processing.

Corrupted character values found in these fields were removed and replaced with blank values where appropriate.

### Category Standardization

Category values contained inconsistent casing and other string inconsistencies.

The categories were standardized to the approved category values:

* Apparel
* Groceries
* Provisions
* Electronics
* Drinks
* Pharmacy

### Date Cleaning

`Date_Added` contained missing values represented by `N/A`, as well as several inconsistent date representations.

The `N/A` placeholders were converted to blank/null values and inconsistent date values were parsed using the appropriate regional locale before being converted to a structured date type.

---

# 2. Data Reconciliation

After the initial cleaning process, the incoming merchant data was compared against a simulated internal CRM/System Master dataset.

`Product_ID` was used as the relational key between the two datasets.

The reconciliation checked for differences in:

* Product price
* Product category

### Reconciliation Results

The audit identified:

| Check                    | Records |
| ------------------------ | ------: |
| Price mismatches         |      67 |
| Category mismatches      |      44 |
| Records requiring review |     109 |

A `Requires_Review` field was used to isolate records containing reconciliation discrepancies.

These records were also surfaced in a separate **Discrepancy Audit Report** within the Excel workbook.

The purpose of the report was to make exceptions visible for manual review rather than silently overwriting differences.

---

# 3. Data Entry Controls

The working Excel template includes validation controls intended to reduce future data-entry errors.

### Category Validation

A dropdown list restricts category entries to the approved category values.

### SKU Validation

The `SKU_Code` field uses a text-length validation rule requiring exactly 8 characters.

### Date Validation

The `Date_Added` field uses Excel date validation to reduce invalid date entries.

### Product ID Uniqueness

A custom validation rule was implemented to prevent duplicate `Product_ID` values:

```excel
=COUNTIF($A:$A,A2)<=1
```

A hard-stop validation alert was configured to reject duplicate entries.

---

# 4. Output

The cleaned dataset was exported as a UTF-8 comma-delimited CSV.

The production output contains:

* 1,000 inventory records
* Unique `Product_ID` values
* Standardized 8-character SKU codes
* Standardized product categories
* Numeric price and stock fields
* Structured date values
* Records requiring reconciliation review identified separately

The cleaned CSV represents the final output of the simulated data-entry preparation workflow.

---

# 5. Workflow

The overall workflow was:

```text
Raw Merchant Data
        │
        ▼
Power Query Import
        │
        ▼
Data Cleaning & Standardization
        │
        ├── SKU correction
        ├── Category standardization
        ├── Numeric type conversion
        ├── Corrupted-value handling
        └── Date cleaning
        │
        ▼
Clean Merchant Dataset
        │
        ▼
Power Query Merge
        │
        ▼
CRM/System Master Reconciliation
        │
        ├── Price mismatch check
        └── Category mismatch check
        │
        ▼
Discrepancy Audit Report
        │
        ▼
Excel Validation Controls
        │
        ▼
Production CSV Export
```

---

# 6. What This Project Demonstrates

This project demonstrates practical experience with:

* Data entry quality control
* Data cleaning
* Data standardization
* Power Query transformations
* Data validation
* Data reconciliation
* Exception identification
* Spreadsheet quality controls
* Preparing structured data for bulk upload
* Working with messy operational data

The project also demonstrates an important data-operations principle:

> **Clean and validate incoming data before it reaches downstream systems, and isolate discrepancies rather than silently changing potentially important business values.**

---

# 7. Project Files

```text
data/
├── raw/
│   └── fintech_data_entry_test.csv
│
└── processed/
    └── fintech_data_entry_test_v1_clean.csv

workbook/
└── fintech_data_entry_test_working.xlsx

documentation/
└── data_cleaning_changelog.md
```

### Raw Data

The raw dataset is retained separately from the processed output to preserve the original input and make the transformation process reproducible.

### Working Workbook

The Excel workbook contains the Power Query workflow, cleaned data, reconciliation results, discrepancy report, and data-entry validation controls.

### Production Export

The processed CSV represents the cleaned output prepared for downstream bulk-upload processing.

### Changelog

The changelog documents the cleaning, reconciliation, and quality-control steps performed during the project.

---

# Disclaimer

This is a **simulated fintech data-entry and data-quality project created for portfolio purposes**.

The dataset, CRM/System Master data, business rules, and workflow are simulated. The project does not represent confidential data, proprietary processes, or an actual internal system belonging to a fintech company.

The purpose of the project is to demonstrate practical data-entry, data-cleaning, reconciliation, validation, and quality-control skills using Microsoft Excel and Power Query.
