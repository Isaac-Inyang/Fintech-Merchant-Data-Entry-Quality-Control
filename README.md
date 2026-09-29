# Fintech Merchant Data Entry & Quality Control

A simulated fintech operations project demonstrating merchant inventory data cleaning, validation, reconciliation, and bulk-upload preparation using Microsoft Excel and Power Query.

## Project Overview

Merchant bulk uploads can contain inconsistent product identifiers, incorrect data types, missing values, and discrepancies against existing records. These issues can affect data accuracy and downstream operations if they are not identified before processing.

This project simulates that workflow using two datasets: incoming Merchant Upload Data and a reference CRM Master Data table. I used Power Query to clean and standardize the merchant data, reconcile it against the reference dataset, and identify records requiring further review.

The project also includes Excel data-validation controls designed to reduce common data-entry errors.

> **Disclaimer:** This is a fully simulated portfolio project. All datasets, product records, CRM reference data, and business rules were created or simulated for demonstration purposes. No real fintech company data or proprietary systems were used.

## Objectives

- Clean and standardize incoming merchant inventory data.
- Correct inconsistent SKU formats and category values.
- Handle corrupted values, missing dates, and data-type inconsistencies.
- Compare merchant-uploaded values against reference records.
- Flag price and category discrepancies for review.
- Implement spreadsheet validation controls to reduce future input errors.
- Prepare a cleaned CSV for downstream processing.

## Tools & Technologies

1. **Microsoft Excel** — working environment, data validation, and discrepancy reporting.
2. **Power Query** — data cleaning, transformation, merging, and reconciliation.
3. **CSV** — source datasets and processed data exports.

## Data Sources

The project uses two simulated datasets:

| Dataset              | Purpose                                                                          |
| -------------------- | -------------------------------------------------------------------------------- |
| Merchant Upload Data | Incoming inventory records requiring cleaning and validation.                    |
| CRM Master Data      | Reference product information used to check the accuracy of the merchant upload. |

`Product_ID` serves as the matching key between the two datasets.

## Workflow

1. **Import:** Load the Merchant Upload Data and CRM Master Data into the Excel/Power Query environment.
2. **Clean:** Standardize SKU codes and categories, convert numeric fields to appropriate data types, and handle corrupted values and missing dates.
3. **Reconcile:** Merge the cleaned merchant data with the CRM reference data using `Product_ID`.
4. **Identify discrepancies:** Compare price and category values and flag mismatches.
5. **Review exceptions:** Isolate records requiring review in a discrepancy audit report.
6. **Validate inputs:** Configure Excel data-validation rules for product identifiers, SKU codes, categories, and dates.
7. **Export:** Prepare the cleaned merchant dataset as a UTF-8 CSV.

## Key Results

The reconciliation process produced the following results:

| Quality Check                  | Result |
| ------------------------------ | -----: |
| Merchant inventory records     |  1,000 |
| Price mismatches identified    |     67 |
| Category mismatches identified |     44 |
| Records requiring review       |    109 |

The discrepancy report consolidates records requiring review based on the configured reconciliation checks. These figures describe identified exceptions, not necessarily records that were subsequently corrected against the reference data.

The cleaning process also standardized SKU codes to eight characters, aligned category values with the approved categories, converted price and stock fields to appropriate numeric types, and handled missing date values.

## Data Quality Controls

The working Excel template includes:

1. **Category dropdown:** Restricts entries to the approved category list.
2. **SKU length validation:** Requires SKU codes to contain exactly eight characters.
3. **Date validation:** Requires valid dates later than 1 January 2024.
4. **Product ID uniqueness validation:** Uses a custom Excel rule to reject duplicate product identifiers.

These controls are intended to reduce common data-entry errors during subsequent use of the template.

## Repository Structure

```text
fintech-merchant-data-quality/
├── README.md
├── .gitignore
├── data/
│   ├── raw/
│   │   ├── merchant_upload_data.csv
│   │   └── crm_master_data.csv
│   └── processed/
│       ├── merchant_upload_clean.csv
│       └── discrepancy_audit_report.csv
├── workbook/
│   └── fintech_merchant_data_quality.xlsx
└── documentation/
    └── data_cleaning_changelog.md
```

**Repository contents:**

- [Raw data](/data/raw) — simulated input datasets.
- [Processed](/data/processed) — cleaned merchant data and discrepancy report.
- [Workbook](/workbook) — Excel working file containing the cleaning, reconciliation, and validation workflow.
- [Changelog](/documentation/data_cleaning_changelog.md) — detailed record of transformations, reconciliation checks, validation rules, and quality-assurance results.

## Skills Demonstrated

1. Data entry and quality control
2. Data cleaning and standardization
3. Microsoft Excel
4. Power Query transformations and merging
5. Data-type conversion and missing-value handling
6. Reference-data reconciliation
7. Discrepancy identification and exception reporting
8. Excel data validation
9. CSV export and data preparation

## Project Outcome

This project demonstrates how a structured spreadsheet workflow can help improve the consistency of incoming inventory data, identify discrepancies against a reference dataset, and prepare records for further processing.

The central principle is to **clean incoming data, validate it against a reference source, and flag discrepancies for review rather than silently overwrite conflicting values**.
