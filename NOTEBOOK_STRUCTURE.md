# Notebook Structure

## Recommended Cell Organization

### Notebook Roles
- `01_get_data.ipynb`: Fetch/validate raw NBA data and persist to `data/raw` and basic processed outputs to `data/processed`.
- `02_analysis.ipynb`: Feature engineering, team-level aggregation, YoY analysis, correlations, and visualizations.
- `03_extra_code.ipynb`: Temporary experiments; migrate stable code into `scripts/` or the main notebooks before publishing.

### **SECTION 1: Setup & Configuration**
- Cell 1: Markdown header "NBA Pace Analytics: Setup"
- Cell 2: Import libraries (include `pathlib.Path`; set base paths)
- Cell 3: Configuration (constants: seasons, columns, style settings, and data directories)

### **SECTION 2: Data Acquisition**
- Cell 4: Markdown "Data Acquisition"
- Cell 5: Fetch game logs (2024, 2025, 2026) or load existing CSVs from `data/raw/` when available
- Cell 6: Fetch team stats (wins, PLUS_MINUS)
- Cell 7: Persist raw pulls to `data/raw/` (idempotent writes; skip if files exist)

### **SECTION 3: Feature Engineering**
- Cell 8: Markdown "Calculate Game-Level Metrics"
- Cell 9: Calculate POSS, PACE, ORTG, TS_PCT
- Cell 10: Persist processed game-level metrics to `data/processed/` with season-encoded filenames

### **SECTION 4: Team Aggregation**
- Cell 11: Markdown "Aggregate to Team Level"
- Cell 12: Calculate team averages (PACE, ORTG, TS%, 3PA)
- Cell 13: Create team comparison dataframe; save to `data/processed/`

### **SECTION 5: Year-over-Year Analysis**
- Cell 14: Markdown "Year-over-Year Changes"
- Cell 15: Calculate YoY deltas for all metrics
- Cell 16: Add team abbreviations for plotting

### **SECTION 6: Option B Statistical Analysis (2025→2026)**
- Cell 17: Markdown "Statistical Analysis: Pace Impact (2025-2026)"
- Cell 18: Calculate deltas, buckets (use percentiles), correlations
- Cell 19: Correlation with wins/PM (2025→2026)

### **SECTION 7: Comparative Analysis (2024→2025)**
- Cell 20: Markdown "Comparative Analysis: Pace Impact (2024-2025)"
- Cell 21: Parallel analysis for 2024→2025 period

### **SECTION 8: Visualizations**
- Cell 22: Markdown "Visualizations"
- Cell 23: Scatter: PACE_DELTA vs ORTG_DELTA
- Cell 24: Bar chart: Bucket summary (label axes; include sample size)
- Cell 25: Scatter: ORTG_DELTA vs WIN% (2025→2026); consistent `PLOT_FIGSIZE`

### **SECTION 9: Results Summary**
- Cell 26: Markdown "Key Findings"
- Cell 27: Print summary table of all correlations and write to `data/processed/summary_metrics_2024_2026.csv`

---

## **Code Quality Standards to Apply**

### **Docstrings & Comments**
```python
# Use section headers for major blocks
# ========== SECTION: Data Preparation ==========

# Explain the why, not just the what
# Filter out games with 0 minutes played to avoid division by zero
df_filtered = df[df['MIN'] > 0]
```

### **Variable Naming**
- ❌ `df` → ✅ `game_logs_2026`
- ❌ `row` → ✅ `team_row`
- ❌ `abbrev` → ✅ `team_abbrev`

### **Constants at Top**
```python
# Configuration
SEASONS = ['2023-24', '2024-25', '2025-26']
MIN_THRESHOLD = 0
BUCKET_PERCENTILES = [0.25, 0.75]
PLOT_FIGSIZE = (10, 6)
```

### **Function Extraction**
Create helper functions for repeated logic:
```python
def calculate_pace(df):
    """Calculate PACE from possessions and minutes."""
    return 48 * (df['POSS'] / (df['MIN'] / 5))

def calculate_metrics_by_team(df, team_col, metric_cols):
    """Group metrics by team and return averages."""
    return df[df['MIN'] > 0].groupby(team_col)[metric_cols].mean()
```

### **Paths & Persistence (Use `pathlib`)**
```python
from pathlib import Path

# In notebooks, assume working directory is `notebooks/`
BASE_DIR = Path('..')
DATA_RAW_DIR = BASE_DIR / 'data' / 'raw'
DATA_PROCESSED_DIR = BASE_DIR / 'data' / 'processed'

DATA_RAW_DIR.mkdir(parents=True, exist_ok=True)
DATA_PROCESSED_DIR.mkdir(parents=True, exist_ok=True)

# Read raw if present, otherwise fetch and save
logs_2025_path = DATA_RAW_DIR / 'team_game_logs_2024-25.csv'
# Example usage:
# df_2025 = pd.read_csv(logs_2025_path) if logs_2025_path.exists() else fetch_and_save_2025()

# Save processed outputs with clear, season-encoded names
output_path = DATA_PROCESSED_DIR / 'team_game_logs_with_metrics_2024_2026.csv'
# game_logs_with_metrics.to_csv(output_path, index=False)
```

### **Output Clarity**
```python
# ❌ Poor
print(wins_pace_corr, wins_ortg_corr, wins_ts_corr)

# ✅ Good
print("\n=== 2025→2026 Correlations: Wins ===")
print(f"  PACE change vs Win%: {wins_pace_corr:.3f}")
print(f"  ORTG change vs Win%: {wins_ortg_corr:.3f}")
print(f"  TS% change vs Win%:  {wins_ts_corr:.3f}")
```

---

## **Environment & Execution (Windows)**

- Activate venv in PowerShell: `& env\Scripts\Activate.ps1`
- Install dependencies: `pip install -r requirements.txt`
- Run notebook top-to-bottom: Kernel → Restart & Run All
- Avoid absolute Windows paths; use `pathlib.Path` and project-relative directories.

---

## **Files to Create**

1. **README.md** — Project overview, usage instructions
2. **requirements.txt** — Dependencies
3. **Refactored notebooks** — Ensure `01_get_data`, `02_analysis`, `03_extra_code` follow this structure and persist outputs under `data/processed/`

