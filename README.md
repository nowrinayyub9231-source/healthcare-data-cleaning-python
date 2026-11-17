# Healthcare Data Cleaning Capstone Project

Cleaning and preparing hospital admission records for analysis using Python, pandas and Jupyter Notebook.

## Overview

Healthcare analytics depends on reliable data. Duplicate patient records, inconsistent text, mixed date formats and incorrect billing amounts can all lead to misleading analysis, billing disputes and poor resource planning.

This project takes a raw hospital admissions dataset and turns it into a clean, consistent dataset that can reliably support later analysis. Every cleaning step is documented in the notebook with the problem found, the action taken and the reason for it.

## Objective

Clean and prepare the dataset so that it can support hospital administrators in:

- maintaining accurate patient records
- reducing billing conflicts
- improving patient satisfaction, which contributes to better patient outcomes

## Dataset

| Item | Details |
|---|---|
| Source | [https://github.com/HarshChoudhary2003/Hospital-Performance-Analytics/tree/main/Data] |
| Size (raw) | 55,500 rows × 15 columns |
| Granularity | One row = one hospital admission |
| Period covered | 8 May 2019 – 7 May 2024 |

**Columns (grouped by theme)**

- **Patient:** Name, Age, Gender, Blood Type
- **Admission:** Date of Admission, Discharge Date, Admission Type, Room Number
- **Clinical:** Medical Condition, Medication, Test Results
- **Provider:** Doctor, Hospital
- **Billing:** Insurance Provider, Billing Amount

## Data Quality Issues Found

| Area | Issue |
|---|---|
| Duplicates | 534 fully duplicated rows |
| Name | Random capitalisation (e.g. "bObBy jAcKsOn") |
| Doctor | 1,112 names began with a title (Dr., Mr., Mrs., Ms., Miss) and 1,372 ended with a suffix (MD, DDS, DVM, PhD, Jr., II, III, IV) |
| Hospital | 4,736 names ended with a stray comma; 5,698 began with "and" and 5,547 ended with "and" |
| Dates | Date of Admission and Discharge Date stored as text |
| Billing Amount | 106 negative values and long decimals |
| Missing values | None found |
| Other categorical columns | Gender, Blood Type, Medical Condition, Insurance Provider, Admission Type, Medication and Test Results were checked with `value_counts()` |

## Cleaning Steps

1. **Loading and viewing the data**: shape, data types, unique counts.
2. **Duplicates**: removed 534 duplicate rows with `drop_duplicates()`, keeping the first copy.
3. **Null values**: checked every column; none were missing.
4. **Inconsistencies**
   - *Name*: standardised with `.str.strip().str.title()`; re-checked for hidden duplicates (none).
   - *Hospital*: removed stray trailing commas and the stray word "and" at the start and end of names.
   - *Doctor*: removed titles and suffixes using lists and a loop (`removeprefix` / `removesuffix`).
5. **Date columns**: converted Date of Admission and Discharge Date to datetime and confirmed that no discharge date is earlier than its admission date.
6. **Billing Amount**: converted 106 negative values to positive with `.abs()` and rounded to 2 decimal places.
7. **New columns**: created `Month of Admission` and `Year of Admission` from Date of Admission.

## Result

| | Before | After |
|---|---|---|
| Rows | 55,500 | 54,966 |
| Columns | 15 | 17 |

The cleaned dataset has no duplicates, no missing values, consistent text formatting, proper datetime columns and valid billing amounts.

## Assumptions and Limitations

- **Negative billing amounts** were treated as data-entry sign errors because they are few and small compared with typical bills (about 25,000). If the data source confirms they are refunds or adjustments, they should be handled separately.
- **Commas inside hospital names** (e.g. "Smith, Jones and Brown") were kept, as they appear to be part of the naming format.
- Hospital and doctor names have thousands of distinct values, so they were cleaned with pattern checks and not category counts.

## Tools

- Python
- pandas
- NumPy
- Jupyter Notebook

## How to Run

1. Clone the repository.
2. Install the requirements: `pip install pandas numpy jupyter`
3. Place the raw dataset in the project folder.
4. Open `Cleaned_Data.ipynb` and update the file path in the data-loading cell (for example `pd.read_csv("healthcare_dataset.csv")`).
5. Run all cells from top to bottom (Kernel → Restart & Run All Cells).

## Repository Structure

```
├── Cleaned_Data.ipynb               # Full cleaning workflow with notes
├── healthcare_dataset.csv           # Raw dataset (if licence allows)
├── healthcare_dataset_cleaned.csv   # Cleaned output
└── README.md
```

## Author

**Nowrin Ayyub**
