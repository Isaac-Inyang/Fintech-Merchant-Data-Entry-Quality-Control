# Data Entry Cleaning, Integrity & Reconciliation Changelog

**Candidate:** Isaac Uko

**Project:** Fintech Merchant Data Entry & Quality Control Simulation

**Build Version:** v1.0

**Status:** Clean Portfolio Build

**Date:** 29/09/2026

---

## Project Disclaimer

This project is a **fully simulated fintech data-entry and data-quality exercise** created for portfolio and skills-demonstration purposes.

The Merchant Upload Data, CRM Master Data, business rules, product records, and reconciliation workflow are simulated. No real fintech company's confidential, proprietary, or customer data was used.

The project demonstrates the use of **Microsoft Excel and Power Query** to clean incoming merchant data, compare it against a simulated CRM Master Data table, identify discrepancies, apply data-entry controls, and prepare a cleaned CSV output.

---

# 1. Data Architecture & Version Structure

The project uses two simulated source datasets.

### Source 1 — Merchant Upload Data

**File:**

[Merchant upload file](/data/raw/merchant_upload_batch.csv)


This represents an incoming merchant inventory bulk-upload file.

The dataset intentionally contains data-quality and data-entry issues that require cleaning before downstream processing.

### Source 2 — CRM Master Data

**File:**

[CRM master data file](/data/raw/crm_master_data.csv)


This represents the simulated authoritative product information used to validate the incoming merchant upload.

`Product_ID` is used as the relational key between the Merchant Upload Data and CRM Master Data.

### Working Environment

[Fintech merchant file](/workbook/fintech_merchant_data_quality.xlsx)



The Excel workbook contains the Power Query workflow, cleaned data, reconciliation results, discrepancy reporting, and data-entry validation controls.

### Processed Outputs

[Cleaned merchant data](/data/processed/merchant_upload_clean.csv)

[discrepancy audit report](/data/processed/discrepancy_report.csv)


The first file contains the cleaned Merchant Upload Data.

The second contains records identified during reconciliation that require review.

### Encoding Standard

The cleaned merchant upload is exported as:

```text
CSV UTF-8 (Comma Delimited)
```

---

# 2. Merchant Upload Data Cleaning

The simulated Merchant Upload Data was cleaned and standardized using Microsoft Power Query.

## 2.1 SKU_Code

The incoming data contained **1,000 SKU string entries shorter than the required 8-character structure**.

The SKU values were standardized using the Power Query expression:

```text
Text.PadStart([SKU_Code], 8, "0")
```

This restored structural leading zeros where required.

### Result

All cleaned SKU codes conform to the required 8-character structure.

---

## 2.2 Price & Stock_Level

The Merchant Upload Data contained corrupted character encodings in the `Price` field.

### Transformations performed

- Removed **152 instances** of the identified corrupted character encoding:

  * `Ã¢â€šÂ¦`
- Replaced the affected values with blanks where appropriate.
- Converted `Price` to a decimal number data type.
- Converted `Stock_Level` to a whole number data type.

These transformations ensure that the fields are represented using appropriate data types for further processing and validation.

---

# 3. Category Standardization

The incoming Merchant Upload Data contained inconsistent category formatting and casing.

The category field was standardized to the approved category values:

```text
Apparel
Groceries
Provisions
Electronics
Drinks
Pharmacy
```

The cleaning process updated the following records:

| Category    | Records Updated |
| ----------- | --------------: |
| Apparel     |              67 |
| Groceries   |              69 |
| Provisions  |              60 |
| Electronics |              63 |
| Drinks      |              61 |
| Pharmacy    |              41 |

The final dataset uses a consistent category naming convention.

---

# 4. Date_Added Cleaning

The `Date_Added` field contained missing-value placeholders and inconsistent date representations.

### Missing values

**41 records** contained the text placeholder:

```text
N/A
```

These values were converted to blank/null values.

### Date format correction

**4 records** containing non-standard date representations were re-parsed using an UK English regional locale before being converted to a structured date data type.

### Result

The `Date_Added` field is represented as a structured date field in the cleaned dataset, with missing values retained as blanks where the source data did not provide a valid date.

---

# 5. Merchant Upload vs CRM Master Reconciliation

After the Merchant Upload Data was cleaned, it was reconciled against the simulated CRM Master Data.

The reconciliation was performed using **Power Query Merge** with: **Product_ID** as the relational key.

The purpose of the reconciliation was to identify differences between the information submitted through the Merchant Upload Data and the corresponding reference values in the CRM Master Data.

---

## 5.1 Price Reconciliation

The merchant-uploaded `Price` value was compared against the corresponding CRM Master `Price`.

The reconciliation identified:

**67 price mismatches**

These records were flagged for review rather than silently overwriting the uploaded values.

---

## 5.2 Category Reconciliation

The merchant-uploaded `Category` value was compared against the corresponding CRM Master `Category`.

The reconciliation identified:

**44 category mismatches**

These records were flagged as discrepancies requiring review.

---

## 5.3 Requires_Review

A conditional `Requires_Review` field was created to identify records containing reconciliation discrepancies.

The resulting discrepancy audit contained:

**109 records requiring review.**

The discrepancy records were isolated into a dedicated **Discrepancy Audit Report** within the Excel workbook.

The report allows potentially incorrect or conflicting records to be reviewed separately from records that passed the reconciliation checks.

---

# 6. Data Entry & Quality Controls

The working Excel template contains validation controls designed to reduce future data-entry errors.

## 6.1 Category Validation

A Data Validation dropdown was configured for the category field.

The dropdown restricts entries to the approved category values:

```text
Apparel
Groceries
Provisions
Electronics
Drinks
Pharmacy
```

This reduces the possibility of future spelling and casing inconsistencies.

---

## 6.2 SKU_Code Validation

The `SKU_Code` field was configured with a text-length validation rule requiring exactly **8 characters**

This helps prevent truncated SKU codes from being entered into the working template.

---

## 6.3 Date_Added Validation

The `Date_Added` field was configured with Excel date validation.

The validation requires a valid date later than  1 January 2024

This helps reduce invalid date entries and text-based date errors.

---

## 6.4 Product_ID Uniqueness Validation

A custom Excel validation rule was implemented to prevent duplicate `Product_ID` values:

```excel
=COUNTIF($A:$A,A2)<=1
```

The validation was configured with a hard-stop alert so that duplicate product keys are rejected during data entry.

---

# 7. Quality Assurance Results

The final cleaned Merchant Upload Data contains:

* **1,000 inventory records**
* Unique `Product_ID` values
* 8-character `SKU_Code` values
* Standardized product categories
* Numeric `Price` values
* Whole-number `Stock_Level` values
* Structured `Date_Added` values
* Missing dates represented as blanks where applicable

The reconciliation process identified:

| QA Check                 | Result |
| ------------------------ | -----: |
| Merchant Upload Records  |  1,000 |
| Price Mismatches         |     67 |
| Category Mismatches      |     44 |
| Records Requiring Review |    109 |

The records requiring review are retained separately in the discrepancy audit rather than being silently modified.

---

# 8. End-to-End Workflow

The complete workflow is:

```text
Simulated Merchant Upload Data
              │
              ▼
       Power Query Import
              │
              ▼
      Data Cleaning & Transformation
              │
       ┌──────┼────────┐
       │      │        │
       ▼      ▼        ▼
     SKU   Category   Dates
       │      │        │
       └──────┼────────┘
              ▼
       Clean Merchant Data
              │
              │
              │       Simulated CRM Master Data
              │                  │
              │                  │
              └──────┬───────────┘
                     ▼
             Power Query Merge
                Product_ID
                     │
          ┌──────────┴──────────┐
          ▼                     ▼
   Price Comparison      Category Comparison
          │                     │
          └──────────┬──────────┘
                     ▼
             Requires_Review
                     │
                     ▼
          Discrepancy Audit Report
                     │
                     ▼
          Excel Data Validation
                     │
                     ▼
        Clean Merchant Upload CSV
```

---

# 9. Final Output

The final cleaned Merchant Upload Data was exported as:

[Clean Merchant Upload](/data/processed/merchant_upload_clean.csv)


The reconciliation exceptions were isolated as:

[Discrepancy report](/data/processed/discrepancy_report.csv)
`

The complete working environment is retained in:

[Fintech Merchant Wookbook](/workbook/fintech_merchant_data_quality.xlsx)


---

# 10. Skills Demonstrated

This project demonstrates practical experience with:

1.  Data entry quality control
2.  Data cleaning
3.  Data standardization
4.  Microsoft Excel
5.  Power Query
6.  Power Query Merge
7.  Data type transformation
8.  Text transformation
9.  Missing-value handling
10. Data reconciliation
11. Exception identification
12. Data validation
13. Duplicate prevention
14. Quality assurance
15. CSV preparation
16. Operational data workflows

---

## Project Principle

The workflow follows a simple data-quality principle:

> **Clean incoming data, validate it against a trusted reference dataset, isolate discrepancies for review, and only then prepare the cleaned data for downstream processing.**

---

## Disclaimer

This repository contains a **fully simulated fintech data-entry and data-quality project**.

All Merchant Upload Data, CRM Master Data, product records, business rules, and reconciliation scenarios were created or simulated for portfolio purposes.

The project does not represent work performed for, or access to the internal systems or proprietary data of, any real fintech company.
