🏀 NBA Pace Analytics: Pace, Efficiency & Winning in the Modern NBA
A Multi-Season, Data-Driven Strategy Analysis
________________________________________
📌 Project Overview
This project investigates the relationship between NBA team pace of play and offensive efficiency across three seasons:
•	2023–24
•	2024–25
•	2025–26 (mid-season)
The core question is:
Does playing faster actually lead to better offense and more wins, or does efficiency matter more than tempo?
Using both game-level and team-level data derived from official NBA sources, this project separates:
•	Scoring Volume → Pace (possessions per game)
•	Scoring Quality → Offensive Rating (ORtg), True Shooting Percentage (TS%)
and evaluates which actually translates to:
•	Win Percentage
•	Point Differential per Game (Net Performance Proxy)
________________________________________
🎯 Key Questions
•	Does increasing pace improve win percentage?
•	Do efficiency and shot quality matter more than tempo?
•	Are these relationships consistent across seasons?
•	Are results driven by extreme pace outliers?
•	Is there meaningful statistical separation between fast and slow teams?
________________________________________
🌐 Data Sources
All data is pulled programmatically using the official NBA API (nba_api):
•	LeagueGameLog → Team game logs (game-level stats)
•	LeagueDashTeamStats → Team win %, games played, plus-minus
To control for the partial 2025–26 season, this project uses:
•	✅ Win Percentage (W_PCT) instead of total wins
•	✅ Plus-Minus per Game (PLUS_MINUS / GP) as a Net Rating proxy
________________________________________
🛠️ Tech Stack
•	Python
•	pandas, numpy
•	nba_api
•	matplotlib, seaborn
•	scipy
•	jupyter
________________________________________
⚙️ Analysis Workflow
1. Setup & Configuration
•	Import libraries
•	Define seasonal constants and thresholds
2. Data Acquisition (01_get_data.ipynb)
•	Pull raw game logs and team stats using nba_api
•	Validate statistical integrity
•	Save structured outputs to:
o	data/raw/
o	data/processed/
3. Feature Engineering
•	Derived advanced metrics:
o	Possessions
o	Pace (per 48 minutes)
o	Offensive Rating (ORtg)
o	True Shooting Percentage (TS%)
4. Team Aggregation
•	Aggregate game-level metrics to team-season averages
•	Identify fastest and slowest teams by pace
5. Year-over-Year Analysis
•	Compute YoY deltas for:
o	Pace
o	ORtg
o	TS%
o	Win %
o	Point Differential per Game
6. Statistical Analysis
•	Pearson correlations between:
o	Pace & winning
o	Efficiency & winning
•	T-tests:
o	Fast vs slow teams
7. Comparative Analysis (Validation)
•	Full analysis repeated for:
o	2024 → 2025
o	2025 → 2026
•	Tests whether relationships are stable across seasons
8. Outlier Sensitivity Testing
•	Extreme pace-change teams (PHX, DAL, IND) removed
•	Analysis repeated to test robustness
9. Visualization
•	Scatter plots
•	Bucket averages
•	Correlation comparisons
•	Outlier impact analysis
________________________________________
📊 Key Findings
1️⃣ Pace Is a Weak Predictor of Winning
Across both multi-season windows:
Relationship	Correlation Strength
Pace vs Win %	Very weak (~0.06 – 0.26)
Pace vs Point Differential	Weak to negligible
✅ Pace increases scoring opportunities
❌ Pace does not consistently improve win outcomes
________________________________________
2️⃣ Offensive Efficiency Strongly Drives Wins
Metric	Correlation With Win %
Offensive Rating (ORtg)	~0.71
True Shooting % (TS%)	~0.59 – 0.69
✅ Teams that improve shot quality and scoring efficiency are far more likely to improve:
•	Win percentage
•	Point differential per game
________________________________________
3️⃣ Slower Teams Often Improved Efficiency the Most
Bucket analysis by pace change showed:
•	Fastest 25% of teams → Minimal efficiency gains
•	Slowest 25% of teams → Largest gains in ORtg & TS%
✅ Suggests many teams are prioritizing half-court shot quality and execution over raw tempo.
________________________________________
📈 Visuals

Pace Change vs Offensive Rating Change (YoY)
![Scatter plot of pace delta vs offensive rating delta]<img width="989" height="590" alt="image" src="https://github.com/user-attachments/assets/e92a123d-ad69-4300-9906-27f4f67b4cd5" />


Efficiency Change by Pace Bucket
![Bar chart of efficiency deltas by pace bucket]<img width="889" height="590" alt="image" src="https://github.com/user-attachments/assets/84be0d0c-d441-431c-a841-66a6e92e4db0" />

Efficiency Change vs. Win % Change (YoY)
![Scatter plot of change in Offensive Rating vs Win Percentage Delta]<img width="889" height="590" alt="image" src="https://github.com/user-attachments/assets/a43d931a-29a3-4656-8f3e-06e82a1d1de2" />



________________________________________
🔍 Outlier Robustness Check
As a sensitivity test, three extreme pace-change outliers (PHX, DAL, IND) were removed.
Results:
•	Pace correlations increased slightly
•	Efficiency metrics (ORtg, TS%) remained dominant
•	The core conclusion did not change
✅ Confirms results are not driven by a small number of extreme teams.
________________________________________
🧠 Strategic Interpretation
Modern NBA offenses appear to follow two distinct strategies:
•	Volume-based scoring → Higher pace, limited impact on winning
•	Quality-based scoring → Better shot selection, strong impact on wins
📌 Scoring efficiency, not speed, is the most reliable driver of team success.
This insight directly informs:
•	Front-office roster construction
•	Offensive system design
•	Shot profile optimization
•	Player evaluation & fit
________________________________________
🗂️ Repository Structure
nba-pace-analytics/
│
├── notebooks/                  # Jupyter notebooks for analysis
│   ├── 01_get_data.ipynb       # Data acquisition & preprocessing
│   ├── 02_analysis.ipynb       # Metrics, stats & visualization
│   └── 03_extra_code.ipynb     # Scratchpad; migrate stable code to scripts/
│
├── data/                       # Project datasets
│   ├── raw/                    # Raw NBA API outputs (CSV)
│   │   ├── team_game_logs_2023-24.csv
│   │   ├── team_game_logs_2024-25.csv
│   │   └── team_game_logs_2025-26.csv
│   └── processed/              # Cleaned & structured datasets
│       ├── team_game_logs_with_metrics_2024_2026.csv
│       └── team_season_outcomes_2024_2026.csv
│
├── scripts/                    # Reusable utilities (move code here from 03_extra_code)
├── images/                     # Exported charts
├── env/                        # Python virtual environment (Windows PowerShell)
├── requirements.txt            # Python dependencies
├── STYLE_GUIDE.md              # Code style & best practices
├── NOTEBOOK_STRUCTURE.md       # Recommended notebook organization
└── README.md
________________________________________
🚀 Future Enhancements
•	Defensive efficiency integration
•	Roster continuity & coaching impact controls
•	Playoff vs regular season pace comparison
•	Multi-year panel regression modeling
•	Shot-location-based efficiency analysis
________________________________________
🤝 Contributing
Pull requests and suggestions are welcome!
Please follow the code style and organization guidelines in:
• STYLE_GUIDE.md
• NOTEBOOK_STRUCTURE.md

⚙️ Setup (Windows PowerShell)
```
# Activate the virtual environment
& env\Scripts\Activate.ps1

# Install dependencies
pip install -r requirements.txt
```

📁 Paths & Persistence
- Use `pathlib.Path` and project-relative directories (avoid absolute Windows paths).
- Read from `data/raw/` when available; write processed outputs to `data/processed/` with season-encoded filenames.
________________________________________
👤 Author
Jakob Welman
Focus: NBA Analytics, Strategy, Data Science, Sports Business
________________________________________
