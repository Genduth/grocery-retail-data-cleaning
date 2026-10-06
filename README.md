# Grocery Retail Data Cleaning

A practical data-quality project that turns a deliberately imperfect grocery retail dataset into a reliable, analysis-ready sales table using Python and pandas.

The notebook follows a transparent audit, repair, and validation workflow. Repairs are made only when the available data provides enough evidence; ambiguous records are retained and flagged rather than guessed or silently removed.

## Project objectives

- Profile the source tables and identify missing, duplicated, inconsistent, or invalid values.
- Validate relationships between sales, stores, products, vendors, and categories.
- Reconstruct missing values using defensible business rules.
- Correct systematic calculation errors while preserving an audit trail.
- Document unresolved issues that require information from the source system.
- Produce a reproducible notebook that runs from top to bottom.

## Dataset

The project uses five related CSV files:

| File | Purpose |
| --- | --- |
| `sales.csv` | Transaction-level sales records and the main focus of the cleaning workflow |
| `products.csv` | Product master data used to validate `product_id` values |
| `stores.csv` | Store reference data used to validate `store_id` values |
| `vendors.csv` | Vendor reference data |
| `categories.csv` | Product category reference data |

The dataset accompanies **Challenge 02 – Errors → Integrity** from the *Grocery Retail – Driven by Data* series. Its included documentation identifies the dataset licence as **CC BY-NC-SA 4.0**.

## Cleaning approach

The notebook works through the data in a controlled order:

1. Load and inspect all reference and transaction tables.
2. Convert transaction dates to a proper datetime type.
3. Audit missing values and check whether related fields provide enough information for reconstruction.
4. Distinguish exact duplicate records from repeated transaction IDs containing conflicting values.
5. Reconstruct missing quantities using `line_amount / unit_price`.
6. Reconstruct missing unit prices using `line_amount / qty`.
7. Validate the rule `line_amount = qty × unit_price` across every sales row.
8. Correct two confirmed scaling-error patterns and record audit flags.
9. Validate store and product IDs against the reference tables.
10. Recover IDs only when a matching duplicate transaction provides direct evidence.
11. Reconstruct missing dates only when the neighbouring transaction dates agree.
12. Run final validation checks and summarise unresolved issues.

## Results

The completed workflow produced the following results:

| Check or repair | Result |
| --- | ---: |
| Starting sales rows | 2,859 |
| Missing quantity values reconstructed | 57 |
| Missing unit prices reconstructed | 56 |
| Missing transaction dates reconstructed | 29 |
| Initial exact duplicate rows removed | 47 |
| Additional duplicates removed after evidence-based repairs | 3 |
| Quantity scaling errors corrected | 29 |
| Line amount scaling errors corrected | 14 |
| Store IDs recovered from matching transactions | 1 |
| Product IDs recovered from matching transactions | 1 |
| Final sales rows | 2,809 |

Final validation confirmed:

- No missing values remain in the core sales columns.
- Every `txn_id` is unique.
- No exact duplicate sales rows remain.
- Every quantity is a positive whole number between 1 and 4.
- Every unit price is positive.
- Every row satisfies `line_amount = qty × unit_price` within one penny.

## Unresolved data-quality issues

Some invalid IDs cannot be safely reconstructed from the available fields:

- 13 sales rows still contain an unknown `store_id` of `9999`.
- 28 sales rows still contain an unknown `product_id` of `9999`.

These records remain in the cleaned dataframe and are explicitly flagged. Replacing them would require authoritative information from the source system.

## Repository structure

```text
grocery-retail-data-cleaning/
├── Grocery Cleaning Notebook.ipynb
├── README.md
├── requirements.txt
├── .gitignore
└── challenge_02_data/
    ├── categories.csv
    ├── products.csv
    ├── sales.csv
    ├── stores.csv
    ├── vendors.csv
    └── readme.txt
```

## Tools and skills demonstrated

- Python
- pandas and NumPy
- Jupyter Notebook
- Missing-data investigation and reconstruction
- Duplicate and primary-key validation
- Referential-integrity checks
- Business-rule validation
- Conservative data repair
- Audit flags and documented data-quality decisions

## Running the project

Clone the repository and move into the project directory:

```bash
git clone https://github.com/Genduth/grocery-retail-data-cleaning.git
cd grocery-retail-data-cleaning
```

Create a virtual environment and install the dependencies:

```bash
python -m venv .venv
```

On Windows:

```powershell
.venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

On macOS or Linux:

```bash
source .venv/bin/activate
pip install -r requirements.txt
```

Start Jupyter and open the notebook:

```bash
jupyter notebook "Grocery Cleaning Notebook.ipynb"
```

The notebook uses relative paths, so it can be run directly from the repository root.

## Author

George Warlow
