# Solving Perfect Multicollinearity with Nested Geographic Variables in R

![R](https://img.shields.io/badge/R-4.x-276DC3?style=flat&logo=r&logoColor=white)
![RMarkdown](https://img.shields.io/badge/RMarkdown-document-blue?style=flat&logo=markdown)

## The Problem

When building regression models with hierarchical geographic variables such as
provinces nested within regions including both levels simultaneously causes
perfect multicollinearity. This makes the model fail entirely: R drops
coefficients silently, VIF values explode, and the X'X matrix becomes singular.

This repository documents the exact methodological solution I developed to fix
this problem, using Belgian housing price data as a case study.

## When to Use This Solution

You will face this exact problem whenever your dataset contains:

- Provinces nested within regions (e.g., Ecuador: Costa, Sierra, Amazonia)
- Municipalities nested within departments (Colombia, Peru)
- Districts nested within cities
- Any categorical variable with a strict hierarchical structure

## The Solution (in one paragraph)

Create a `BASE` reference category by merging one representative province from
each region, then apply **sum-to-zero contrasts** (`contr.sum`) to both the
region and province variables. This breaks the perfect linear dependence between
the two levels while keeping both in the model and allowing meaningful
interpretation of coefficients.

## Repository Structure

```
nested-geographic-variables-multicollinearity/
├── README.md
├── data/
│   └── HousePrices.txt
├── analysis/
│   └── solution.Rmd
├── output/
│   └── solution.html
├── images/
│   └── vif_comparison.png
```


## How to Reproduce

1. Clone this repository
2. Open `analysis/solution.Rmd` in RStudio
3. Place `HousePrices.txt` in the `data/` folder
4. Click **Knit** — the output will render to `output/solution.html`

Required R packages:
```r
install.packages(c("tidyverse", "car", "here"))
```

## Academic Context

This solution was developed during the course **Applied Statistical Models I**
at **ESPOL (Escuela Superior Politécnica del Litoral)**, Ecuador, as part of a
group project modeling municipal housing prices in Belgium.

The full analysis was a collaborative effort. This repository isolates and
documents the specific methodological contribution I made: resolving perfect
multicollinearity caused by nested geographic variables.

## Author

**Paul Mosquera**

