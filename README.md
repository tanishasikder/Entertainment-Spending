Consumer Expenditure Survey (CE) Public-Use Microdata

Overview

This project uses the U.S. Bureau of Labor Statistics (BLS) Consumer
Expenditure Surveys (CE) Public-Use Microdata to analyze household
spending patterns.

The Consumer Expenditure Surveys collect information on expenditures,
income, demographics, and other characteristics of U.S. consumer units.
The public-use microdata are provided by BLS as downloadable files that
can be processed and analyzed at the household and expenditure-record
level.

This repository focuses on working with the raw CE microdata and the
accompanying BLS documentation/codebooks needed to interpret the
variables and coded values.

Data Source

Primary source: U.S. Bureau of Labor Statistics, Consumer
Expenditure Surveys.

Official CE website:

https://www.bls.gov/cex/

CE Public-Use Microdata:

https://www.bls.gov/cex/pumd.htm

The data are produced and maintained by the U.S. Bureau of Labor
Statistics (BLS). Please cite BLS as the data provider when using
these datasets.

Suggested dataset citation

U.S. Bureau of Labor Statistics. Consumer Expenditure Surveys,
Public-Use Microdata. U.S. Department of Labor.
https://www.bls.gov/cex/pumd.htm

When publishing results, also specify the survey year(s), survey
component (Interview or Diary), and specific public-use files used in
the analysis.

How to Download the Data

The CE Public-Use Microdata are available directly from the BLS website.

Step 1: Open the CE Public-Use Microdata page

Go to:

https://www.bls.gov/cex/pumd.htm

Step 2: Choose the survey component

The CE has two main components:

Interview Survey: designed to collect information on larger or
recurring expenditures and expenditures that respondents may have
difficulty recalling over short periods.

Diary Survey: respondents record expenditures as they occur over
a shorter diary period and is particularly useful for smaller or
frequently purchased items.

Choose the component that matches your research question.

Step 3: Choose the survey year

Select the year of the public-use microdata you want to analyze.

Keep the year consistent across the data files and documentation you
use. Variable definitions and coding can change between years.

Step 4: Download the required data files

Download the CSV/data files associated with the survey component and
year you are analyzing.

Do not assume that every file on the BLS download page is required.
CE microdata are distributed across multiple files, and the appropriate
files depend on whether your analysis uses household characteristics,
expenditures, income, assets, demographics, or other information.

Also download the corresponding documentation/data
dictionary/codebook for the same survey component and year.

A typical project directory can be organized as:

data/
├── raw/
│   ├── interview/
│   │   └── YYYY/
│   └── diary/
│       └── YYYY/
├── dictionaries/
│   └── ce-pumd-interview-diary-dictionary.xlsx
└── processed/

Replace YYYY with the relevant survey year.

Understanding the Data Dictionary

The CE documentation contains two pieces of information that are
especially important when working with the CSV files.

1. Variable definitions

The Variables section/sheet describes what each column represents.

For example:

ACQMETH → Method by which property was acquired
ACQUIRMO → Month property was acquired
ACQUIRYR → Year property was acquired

If a CSV contains a column named ACQMETH, use the variable
documentation to determine what the column measures.

2. Value/code definitions

The Codes section/sheet explains what coded values inside a variable
mean.

For example, a categorical variable might contain:

1
2
3

The codebook may define those values as:

1 → Purchased for own use
2 → Rented
3 → Purchased as gift to others

Therefore, the variable dictionary answers:

What does this column represent?

while the code dictionary answers:

What does each value inside this column represent?

Both are necessary for correctly interpreting the CE microdata.

Important: Do Not Decode Every Numeric Column

Not every numeric value in the CE data is a category code.

For example, a column containing:

125.50
87.00
42.75

may represent an expenditure amount and should remain numeric.

A column containing:

1
2
1
3

may instead be a categorical variable whose values need to be translated
using the codebook.

Before converting or decoding a column, check its variable definition
and the corresponding code information.

Handling Survey Years

Variable definitions and value codes can change across survey years.

For reproducible analysis:

Identify the year of each raw data file.

Use the documentation corresponding to that year/component.

Match the CSV variable name to the Variables documentation.

Match the variable's coded values to the Codes documentation.

Do not automatically assume that the same numeric code has the same
meaning across all years.

For longitudinal analysis, explicitly check whether variables and codes
remain comparable across the years being combined.

Referencing Data in Code

When loading the raw files, preserve the original BLS variable names
whenever possible.

Example:

import pandas as pd

df = pd.read_csv("data/raw/interview/YYYY/your_file.csv")

Keep the raw files unchanged and perform cleaning/decoding in a separate
processing step.

A useful workflow is:

BLS raw CSV
     ↓
Load data
     ↓
Match columns to Variables dictionary
     ↓
Identify coded/categorical variables
     ↓
Match values to Codes dictionary
     ↓
Clean and transform
     ↓
Analysis / visualization / modeling

Reproducibility

To make analyses reproducible, record:

BLS CE dataset name

Survey component: Interview or Diary

Survey year

Exact files used

Date the files were downloaded

Data dictionary/codebook version

Any filtering, cleaning, or recoding performed

Do not modify the original downloaded files. Store transformed datasets
separately so that the analysis can be traced back to the original BLS
data.

Data Citation in a Project

For a project README, paper, portfolio project, or report, a concise
reference can be written as:

Data: U.S. Bureau of Labor Statistics, Consumer Expenditure
Surveys, Public-Use Microdata. U.S. Department of Labor.
https://www.bls.gov/cex/pumd.htm

For a more complete reference, include the specific year and survey
component:

U.S. Bureau of Labor Statistics. Consumer Expenditure Surveys:
Public-Use Microdata, [YEAR] [Interview/Diary] Survey. U.S.
Department of Labor. https://www.bls.gov/cex/pumd.htm

Disclaimer

This project is an independent analysis of publicly available U.S.
Bureau of Labor Statistics data. The BLS is the source of the underlying
data and documentation but is not responsible for the analyses,
interpretations, or conclusions presented in this project.