# Solving Perfect Multicollinearity with Nested Geographic Variables in R

![R](https://img.shields.io/badge/R-4.x-276DC3?style=flat&logo=r&logoColor=white)
![RMarkdown](https://img.shields.io/badge/RMarkdown-document-blue?style=flat&logo=markdown)

## The Problem

When modeling data with hierarchical geographic variables, including both levels simultaneously in a regression model causes perfect multicollinearity. The design matrix becomes singular, R silently drops coefficients, and the model cannot be estimated.

The standard response to this problem is to drop one of the two levels, sacrificing interpretability, or to switch to a multilevel model, which adds complexity. This repository documents a third path: a structural fix that keeps both geographic levels in a classical linear regression model with directly interpretable coefficients.

## What Makes This Solution Different

Most resources on multicollinearity suggest either removing one of the correlated variables or applying dimensionality reduction. Neither option is satisfying when both geographic levels carry meaningful information and the goal is to interpret regional and provincial effects simultaneously.

This solution combines two techniques that are individually well known but whose joint application to this specific problem is rarely documented: creating a principled BASE reference category that breaks the perfect linear dependence, and applying sum-to-zero contrasts to both variables so that every coefficient represents a deviation from the global mean.

## When to Use This Solution

You will face this exact problem whenever your dataset contains strictly nested categorical variables such as provinces nested within regions in Ecuador (Costa, Sierra, Amazonia), municipalities nested within departments in Colombia or Peru, districts nested within cities in any urban study, schools nested within districts in education research, or products nested within categories in business analytics.

## The Solution

Create a `BASE` reference category by merging the most representative group at the lower level within each upper level group. Then apply `contr.sum` to both variables. This breaks the perfect linear dependence while keeping both levels in the model.

## Statistical Validity and Assumptions

This solution operates within the classical linear regression framework. It is statistically valid when the following conditions hold.

The solution guarantees a full rank design matrix with identifiable coefficients, acceptable VIF values for all predictors, and coefficients that are directly interpretable as deviations from the global mean.

The key assumption to verify before applying this solution is homoscedasticity across geographic groups. If the variance of the response differs substantially between regions or provinces, the coefficient estimates remain unbiased but standard errors may be underestimated. In that case, heteroscedasticity-consistent standard errors via the `sandwich` package or a multilevel model with `lme4` would be more appropriate.

Use this solution when interpretability of fixed coefficients at both geographic levels is the primary goal and the sample is large enough for the Central Limit Theorem to protect inference. Consider multilevel models when the focus is on modeling within-group correlation or when group-level variance components are of direct interest.

The analysis document includes a full assumptions check with Levene's test across regions and provinces, normality checks for the response variable, and model diagnostics so you can evaluate applicability in your own context.

## Repository Structure

```
nested-geographic-variables-multicollinearity/
├── README.md
├── data/
│   └── HousePrices.txt
├── analysis/
│   └── solution.Rmd
└── output/
    ├── solution.html
    └── solution.pdf
```

## How to Reproduce

1. Clone this repository
2. Open `analysis/solution.Rmd` in RStudio
3. Place `HousePrices.txt` in the `data/` folder
4. Run the following in the RStudio console:

```r
rmarkdown::render(
  here::here("analysis", "solution.Rmd"),
  output_dir = here::here("output")
)
```

Required R packages: `tidyverse`, `car`, `here`, `kableExtra`, `plotly`

## View the Analysis

- Interactive HTML version (with interactive charts): [View Full Analysis (HTML)](https://paulmosq.github.io/nested-geographic-variables-multicollinearity/)
- PDF version (static, includes assumption checks and model diagnostics): available in `output/solution.pdf`

## Academic Context

This solution was developed during the course Applied Statistical Models I at ESPOL (Escuela Superior Politécnica del Litoral), Ecuador, as part of a group project modeling municipal housing prices in Belgium.

The full analysis was a collaborative effort. This repository isolates and documents the specific methodological contribution I made: resolving perfect multicollinearity caused by nested geographic variables, while preserving both levels in the model for interpretability.

## Author

**Paul Mosquera**
