# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Adverse event (AE) analysis for the HARMONY terlipressin registry. This is a sub-project of the main [Terlipressin project](/Users/to909/Desktop/Terlipressin projects/Terlipressin), analyzing the incidence, predictors, and prognostic impact of terlipressin-related adverse events in patients with hepatorenal syndrome-acute kidney injury (HRS-AKI).

**Parent project:** `/Users/to909/Desktop/Terlipressin projects/Terlipressin`

## Repository Structure

```
├── code/
│   └── Adverse-event.Rmd          # Organized copy of main analysis
├── docs/
│   ├── _quarto.yml                # Quarto website config (cosmo theme, code-fold)
│   ├── index.qmd                  # Summary: variable definitions, analysis overview, request history
│   ├── results.qmd                # Results: all tables, regressions, competing risk models
│   └── _site/                     # Rendered HTML output
├── data/                          # Output directory (CSV tables)
├── requests/                      # Data request documents (.docx) from PI
│   ├── ...11262025.docx           # Initial request
│   ├── ...01122025.docx           # Added multivariable models
│   ├── ...01182026.docx           # Reformatted specifications
│   ├── ...01292026.docx           # Added serious_ae, Fine-Gray spec
│   └── ...04072026.docx           # Latest: new variables, updated models (NOT YET STARTED)
├── results/                       # Output CSV tables and rendered HTML
├── Adverse-event.Rmd              # Original analysis (01/2025)
├── Adverse event 2026.Rmd         # Updated analysis (01/2026, adds aclf_grade recoding)
└── CLAUDE.md
```

## Data Source

- **Dataset:** `final_master_01282026.xlsx` from parent project at `/Users/to909/Desktop/Terlipressin projects/Terlipressin/data/`
- **Same dataset** used across all Terlipressin sub-projects

## Key Derived Variables

- `abdominal_ae`: composite of `terli_ae_abdpain`, `terli_ae_diarrhea`, `terli_ae_nausea`
- `serious_ae`: composite of `terli_ae_respfail`, `terli_ae_brady`, `terli_7`, `terli_8`
- `meld_fda`: binary indicator for MELD > 35 (coalesced from `day0_meld`, `admit_meld`, `admit_meld_3`)
- `rrt_90days`, `lt_90days`: 90-day renal replacement therapy and liver transplant indicators
- `BMI`: derived from `aki_weight` and `aki_height`

## Analyses

1. **Descriptive tables** (stratified by `criteria_2`, `criteria_3`, `criteria_4`, `any_ae`, `abdominal_ae`, `serious_ae`, `terli_ae_respfail`) via `create_table_one()` helper
2. **Logistic regression** — univariable models for `any_ae` outcome
3. **Competing risk regression** — Fine-Gray models for 90-day mortality (failcode=1) and transplant (failcode=2)
4. **Multivariable models** — adjusted for age, race, hispanic, MELD scores
5. **Subgroup analyses** — ICU patients (`location_terli_day0 == 1|2`), transplant-listed patients

## Rendering

```bash
# Render Quarto website
quarto render docs/

# Open rendered site
open docs/_site/index.html
```

## R Dependencies

tidyverse, tableone, gtsummary, survival, survminer, readxl, writexl, xlsx, ggplot2, ggpubr, splines, lattice, mice, DescTools, finalfit, tidycmprsk, ggsurvfit, car, cobalt, pammtools, adjustedCurves, rms, riskRegression, prodlim, dplyr, bodycompref, musclerefdata, transplantr, sqldf, table1

## Coding Conventions

- Follows parent project conventions (see parent CLAUDE.md)
- Variable names are lowercase with underscores
- Binary outcomes: 1 = event, 0 = no event
- REDCap field patterns: `{lab}_terli_day{N}`, `terli_ae_{event}`
- `create_table_one()` wraps `tableone::CreateTableOne` with CSV output

## Worktrees

- `summary` (branch: `claude/relaxed-satoshi`) — Summary and overview tasks
