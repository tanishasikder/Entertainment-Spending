# CE Entertainment Spending: Segment Sizing for Marketing

Which kinds of US households spend on entertainment, how much, and how confident can we be? Built on the BLS Consumer Expenditure Survey (CE) public-use microdata.

## Decision this project supports

A company with an entertainment product or service (tickets, streaming, pets, hobbies, etc.) needs to decide **which household segments to market to and how much budget each deserves**. This project ranks segments by estimated market size and spending behavior, with uncertainty attached.

## What the project answers

For each entertainment category and each household segment (income band, age, region, household size, homeownership, and other targetable traits):

1. **Participation rate:** what share of households spent anything in the category?
2. **Conditional spend:** among households that spent, how much?
3. **Expected spend per household:** participation x conditional spend.
4. **Share of wallet:** entertainment as a fraction of total household spending.
5. **Index vs. average household:** 100 = the average US household. A segment at 140 over-indexes.
6. **Market size:** segment households x expected spend, in estimated dollars.
7. **Income sensitivity:** does the category behave like a necessity or a luxury as income rises?
8. **Stability:** does the pattern hold across years, or was it a one-year artifact?

Every estimate ships with a confidence interval, computed with the survey's replicate weights.

## What the project predicts

A two-part (hurdle) model per category:

| Part | Question | Output |
|------|----------|--------|
| 1. Participation | Will a household with these traits spend anything on this category? | Calibrated probability |
| 2. Amount | If it spends, how much? | Expected dollars (trained on spenders only) |

Combined: `expected spend = P(spend) x E[amount | spend]`

Models are compared on weighted metrics, calibration, and segment-level error. Validation is **out-of-time** (train on earlier years, test on the latest), not a random split.

Predictors are limited to traits an ad platform or marketing team can actually target.

## What the project does NOT answer

Be clear about this when presenting results:

- **No product or brand data.** CE records spending categories, not products. It cannot say whether a household would buy a specific item.
- **No marketing response.** There are no ad exposures, conversions, or purchase intent. "Likely to buy product X" is out of scope.
- **No "what they keep" or retention.** The survey doesn't measure it.
- **Small segments are noisy.** Public files hold a few thousand households per year. Narrow segments get wide intervals and should not drive budget decisions.
- **Top incomes are top-coded.** Conclusions about the highest-income households are weak.
- **Pandemic years (2020-21) are distorted** and are treated deliberately, not ignored.
- **Survey recall and category coverage differ** between the Interview and Diary surveys.

## Data

- Source: BLS Consumer Expenditure Surveys, Public-Use Microdata (PUMD): https://www.bls.gov/cex/pumd_data.htm
- Files used: household/consumer-unit file (income, demographics, weights) joined to expenditure files via the household ID. Variable names are to be confirmed against the data dictionary.
- Note: since 2020, Interview packages contain Q2-Q4 of the year plus Q1 of the next year. Year definitions must account for this.
- "Entertainment" is defined by a documented mapping from CE item codes (UCCs) to categories. See `docs/category_mapping.md` (to be written).

## Methodology notes

- **Survey weights:** all estimates are weighted. Standard errors use the replicate weights (BRR), not textbook formulas.
- **Income:** multiply-imputed, so estimates are combined across imputations. Income is used as brackets or quantiles because of top-coding.
- **Zero-inflated spending:** handled by the hurdle model above.
- **Inflation:** multi-year comparisons are CPI-adjusted.
- **Validation:** results are checked against BLS's published tables for at least one year before modeling.

## Planned pipeline

1. Download and load each year's files (parameterized by year, idempotent, backfillable).
2. Transform and test (category mapping, weights, data checks).
3. Compute segment tables and fit models.
4. Serve precomputed segment estimates through an API.
5. Refresh annually when BLS releases new data, with drift checks on the new year.

## Status

Exploration phase: reviewing one year of data and the data dictionary before building the pipeline.