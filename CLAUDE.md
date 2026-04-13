# CLAUDE.md — Adverse Event Analysis

Adverse event (AE) analysis for the HARMONY terlipressin registry. Shared context (dataset, variables, conventions) is in the parent `CLAUDE.md`.

## Repository Structure

```
├── code/
│   └── Adverse-event.Rmd          # Organized copy of main analysis
├── docs/
│   ├── _quarto.yml                # Quarto website config
│   ├── index.qmd                  # Summary: variable definitions, analysis overview
│   ├── results.qmd                # Results: all tables, regressions, competing risk models
│   ├── derived_variables.qmd      # Symlink → main project
│   ├── redcap_variables.qmd       # Symlink → main project
│   └── _site/                     # Rendered HTML output
├── data/                          # Output directory (CSV tables)
├── requests/                      # Data request documents (.docx) from PI
├── results/                       # Output CSV tables and rendered HTML
└── CLAUDE.md
```

## Project-Specific Derived Variables

- `abdominal_ae`: composite of `terli_ae_abdpain`, `terli_ae_diarrhea`, `terli_ae_nausea`
- `serious_ae`: composite of `terli_ae_respfail`, `terli_ae_brady`, `terli_7`, `terli_8`
- `meld_fda`: binary indicator for MELD > 35 (coalesced from `day0_meld`, `admit_meld`, `admit_meld_3`)
- `rrt_90days`, `lt_90days`: 90-day renal replacement therapy and liver transplant indicators
- `BMI`: derived from `aki_weight` and `aki_height`

## Analyses

1. **Descriptive tables** (stratified by `criteria_2`, `criteria_3`, `criteria_4`, `any_ae`, `abdominal_ae`, `serious_ae`, `terli_ae_respfail`) via `create_table_one()` helper
2. **Logistic regression** — univariable models for `any_ae` outcome
3. **Competing risk regression** — Fine-Gray models for 90-day mortality and transplant
4. **Multivariable models** — adjusted for age, race, hispanic, MELD scores
5. **Subgroup analyses** — ICU patients, transplant-listed patients

## Rendering

```bash
quarto render docs/
open docs/_site/index.html
```
