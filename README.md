# football-metric-stability

Year-to-year stability of performance metrics across positions in the Big Five European football leagues (2017/18–2024/25).

---

## Overview

This repository contains the data preparation and analysis code for a study examining the year-to-year stability of player performance metrics across the Big Five European football leagues (Premier League, La Liga, Bundesliga, Serie A, Ligue 1) over seven consecutive season pairs (2017/18–2024/25).

Stability is quantified using Pearson r and ICC on per-90 rates across 24 position × metric combinations (FWD, MID, DEF), with extensions covering age group differences, league-level variation, two-year decay, and robustness checks.

The central question is: *which performance metrics remain consistent enough year-to-year to be worth using in player evaluation and scouting models?*

---

## Repository Structure

```
football-metric-stability/
├── code/
│   ├── MetricStabilityDataPreparation.ipynb   # Data loading, cleaning, panel construction
│   └── MetricStabilityAnalysis.ipynb          # Stability computation, extensions, figures
├── outputs/
│   ├── outputmsdp.pdf                         # Full output of data preparation notebook
│   └── outputmsa.pdf                          # Full output of analysis notebook
├── requirements.txt
└── README.md
```

---

## Data

Data sourced from **FBref** via Kaggle, covering the Big Five European leagues across 8 seasons (2017/18–2024/25). Two CSV formats are used:

- Historical format: 2017/18–2023/24
- Modern format: 2024/25 (different column names, standardised in the data preparation notebook)

The raw data files are **not included** in this repository. Place the season CSVs (`players_data-YYYY_YYYY.csv`) in the working directory before running the notebooks.

---

## Notebooks

### 1. `MetricStabilityDataPreparation.ipynb`
Loads all 8 seasons, standardises column names across formats, computes per-90 rates, applies league and position filters, and constructs the consecutive-season panel saved as `player_panel.csv`.

**Key outputs:** `player_panel.csv`

### 2. `MetricStabilityAnalysis.ipynb`
Loads the panel, applies the MIN_90S = 5.0 filter (8,207 player-season pairs), and computes:

- Main stability table (Pearson r + 95% CI + ICC) for 24 position × metric combinations
- Extension A: Stability by age group (Young / Prime / Veteran) with Fisher z-tests
- Extension B: Stability by league
- Extension C: Two-year decay (t → t+2)
- Extension D: MIN_90S sensitivity sweep (3.0–7.0)
- Extension E: Role-change robustness check

**Key outputs:** `stability_main.csv`, `stability_by_age.csv`, `stability_by_league.csv`, `stability_fisher_tests.csv`, `stability_t2.csv`, `stability_sensitivity.csv`, `sensitivity_min90s.csv`, and figures `fig1`–`fig7`.

---

## Key Results

| Position | Most Stable | Least Stable |
|----------|-------------|--------------|
| FWD | PrgC (r = 0.733), PrgP (r = 0.709) | Assists (r = 0.428) |
| MID | PassPct (r = 0.824), PrgDist (r = 0.676) | GCA (r = 0.524) |
| DEF | PassPct (r = 0.824), Clr (r = 0.588) | Blocks (r = 0.399) |

Veterans are systematically more stable than young players (18/24 combinations significant via Fisher z-test). Stability decays universally at the two-year horizon. Results are robust to MIN_90S threshold variation (Δr = 0.006) and role changes (23/24 combinations).

---

## Requirements

Python 3.12.13

```
pandas==2.2.2
numpy==2.0.2
matplotlib==3.10.0
seaborn==0.13.2
scipy==1.16.3
```

Install with:

```bash
pip install -r requirements.txt
```

---

## License

MIT License
