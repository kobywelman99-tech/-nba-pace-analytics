# 🏀 NBA Pace Analytics: Pace, Efficiency & Winning in the Modern NBA

A multi-season, data-driven strategy analysis that explores whether teams win more by playing faster (pace) or by being more efficient (offense/shot quality).

---

## Project overview

This project investigates the relationship between NBA team pace of play and offensive efficiency across three seasons:

- 2023–24  
- 2024–25  
- 2025–26 (mid-season)

Core question: Does playing faster actually lead to better offense and more wins, or does efficiency matter more than tempo?

We separate:
- Scoring volume → Pace (possessions per game)  
- Scoring quality → Offensive Rating (ORtg), True Shooting Percentage (TS%)

Primary outcomes:
- Win percentage (W_PCT)  
- Point differential per game (PLUS_MINUS / GP) as a net-performance proxy

---

## Key questions

- Does increasing pace improve win percentage?  
- Do efficiency and shot quality matter more than tempo?  
- Are these relationships consistent across seasons?  
- Are results driven by extreme pace outliers?  
- Is there meaningful statistical separation between fast and slow teams?

---

## Data sources

All data is pulled programmatically using the official NBA API (`nba_api`):

- `LeagueGameLog` → team game logs (game-level stats)  
- `LeagueDashTeamStats` → team-level season stats (win %, plus-minus, etc.)

Note: For the partial 2025–26 season we use win percentage (W_PCT) and plus-minus per game (PLUS_MINUS / GP) instead of raw totals.

---

## Tech stack

- Python  
- pandas, numpy  
- nba_api  
- matplotlib, seaborn  
- scipy  
- Jupyter notebooks

---

## Analysis workflow

1. Setup & configuration — imports, constants, thresholds  
2. Data acquisition (`notebooks/01_get_data.ipynb`) — pull & validate raw game logs, save to `data/raw/` and `data/processed/`  
3. Feature engineering — possessions, pace (per 48), ORtg, TS%  
4. Team aggregation — team-season averages, identify fastest/slowest teams  
5. Year-over-year (YoY) analysis — compute deltas for pace, ORtg, TS%, win %, point differential  
6. Statistical analysis — Pearson correlations, t-tests (fast vs slow teams)  
7. Comparative analysis & validation — repeat for season pairs (2024→2025, 2025→2026)  
8. Outlier sensitivity — remove extreme pace-change teams (PHX, DAL, IND) and retest  
9. Visualization — scatter plots, bucket averages, correlation comparisons, outlier impact

---

## Key findings (summary)

1. Pace is a weak predictor of winning  
   - Pace vs Win %: very weak correlation (~0.06–0.26)  
   - Pace vs Point Differential: weak to negligible  
   Conclusion: Pace increases scoring opportunities but does not reliably convert to better win outcomes.

2. Offensive efficiency strongly drives wins  
   - ORtg correlation with Win %: ~0.71  
   - TS% correlation with Win %: ~0.59–0.69  
   Conclusion: Improvements in shot quality and scoring efficiency are far more predictive of wins.

3. Slower teams often improve efficiency the most  
   - Slowest 25% of teams showed the largest gains in ORtg & TS%  
   - Fastest 25% showed minimal efficiency gains  
   Interpretation: Many teams prioritize half-court execution over raw tempo.

---

## Visuals

- Pace change vs Offensive Rating change (YoY)  
  ![Pace vs ORtg delta](https://github.com/user-attachments/assets/e92a123d-ad69-4300-9906-27f4f67b4cd5)

- Efficiency change by pace bucket  
  ![Efficiency by bucket](https://github.com/user-attachments/assets/84be0d0c-d441-431c-a841-66a6e92e4db0)

- Efficiency change vs Win % change (YoY)  
  ![Efficiency vs Win delta](https://github.com/user-attachments/assets/a43d931a-29a3-4656-8f3e-06e82a1d1de2)

---

## Outlier robustness check

- Removed three extreme pace-change outliers (PHX, DAL, IND).  
- Result: pace correlations increase slightly, but efficiency metrics remain dominant.  
- Conclusion: Core findings are not driven by a small set of extreme teams.

---

## Strategic interpretation

Two distinct offensive strategies emerge:

- Volume-based scoring — higher pace, limited impact on winning  
- Quality-based scoring — better shot selection and efficiency, strong impact on wins

Takeaway: Scoring efficiency, not speed, is the most reliable driver of team success. Useful for roster construction, system design, and player evaluation.

---

## 🗂️ Repository structure

```
nba-pace-analytics/
├── notebooks/                  # Jupyter notebooks for analysis
│   ├── 01_get_data.ipynb       # Data acquisition & preprocessing
│   ├── 02_analysis.ipynb       # Metrics, stats & visualization
│   └── 03_extra_code.ipynb     # Scratchpad; migrate stable code to scripts/
├── data/                       # Project datasets
│   ├── raw/                    # Raw NBA API outputs (CSV)
│   │   ├── team_game_logs_2023-24.csv
│   │   ├── team_game_logs_2024-25.csv
│   │   └── team_game_logs_2025-26.csv
│   └── processed/              # Cleaned & structured datasets
│       ├── team_game_logs_with_metrics_2024_2026.csv
│       └── team_season_outcomes_2024_2026.csv
├── scripts/                    # Reusable utilities (move stable code here)
├── images/                     # Exported charts
├── env/                        # Python virtual environment (Windows PowerShell)
├── requirements.txt            # Python dependencies
├── STYLE_GUIDE.md              # Code style & best practices
├── NOTEBOOK_STRUCTURE.md       # Recommended notebook organization
└── README.md
```

---

## Future enhancements

- Add defensive efficiency and opponent-adjusted metrics  
- Model roster continuity and coaching impacts  
- Compare playoff vs regular-season pace  
- Build multi-year panel regressions and shot-location analyses

---

## Contributing

Pull requests and suggestions are welcome. Please follow `STYLE_GUIDE.md` and `NOTEBOOK_STRUCTURE.md` for code and notebook guidelines.

---

## Setup (Windows PowerShell)

```powershell
# Activate the virtual environment
& env\Scripts\Activate.ps1

# Install dependencies
pip install -r requirements.txt
```

---

## Paths & persistence

- Use `pathlib.Path` and project-relative directories (avoid absolute Windows paths).  
- Read from `data/raw/` and write processed outputs to `data/processed/` with season-encoded filenames.

---

## Author

Jakob Welman — Focus: NBA analytics, strategy, data science, sports business
