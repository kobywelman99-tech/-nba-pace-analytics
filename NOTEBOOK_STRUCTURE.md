# Proposed Notebook Structure for GitHub

## Recommended Cell Organization

### **SECTION 1: Setup & Configuration**
- Cell 1: Markdown header "NBA Pace Analytics: Setup"
- Cell 2: Import libraries
- Cell 3: Configuration (constants: seasons, columns, style settings)

### **SECTION 2: Data Acquisition**
- Cell 4: Markdown "Data Acquisition"
- Cell 5: Fetch game logs (2024, 2025, 2026)
- Cell 6: Fetch team stats (wins, PLUS_MINUS)

### **SECTION 3: Feature Engineering**
- Cell 7: Markdown "Calculate Game-Level Metrics"
- Cell 8: Calculate POSS, PACE, ORTG, TS_PCT
- Cell 9: Save raw logs (optional, commented)

### **SECTION 4: Team Aggregation**
- Cell 10: Markdown "Aggregate to Team Level"
- Cell 11: Calculate team averages (PACE, ORTG, TS%, 3PA)
- Cell 12: Create team comparison dataframe

### **SECTION 5: Year-over-Year Analysis**
- Cell 13: Markdown "Year-over-Year Changes"
- Cell 14: Calculate YoY deltas for all metrics
- Cell 15: Add team abbreviations for plotting

### **SECTION 6: Option B Statistical Analysis (2025→2026)**
- Cell 16: Markdown "Statistical Analysis: Pace Impact (2025-2026)"
- Cell 17: Calculate deltas, buckets, correlations
- Cell 18: Correlation with wins/PM (2025→2026)

### **SECTION 7: Comparative Analysis (2024→2025)**
- Cell 19: Markdown "Comparative Analysis: Pace Impact (2024-2025)"
- Cell 20: Parallel analysis for 2024→2025 period

### **SECTION 8: Visualizations**
- Cell 21: Markdown "Visualizations"
- Cell 22: Scatter: PACE_DELTA vs ORTG_DELTA
- Cell 23: Bar chart: Bucket summary
- Cell 24: Scatter: ORTG_DELTA vs WIN% (2025→2026)

### **SECTION 9: Results Summary**
- Cell 25: Markdown "Key Findings"
- Cell 26: Print summary table of all correlations

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

## **Files to Create**

1. **README.md** — Project overview, usage instructions
2. **requirements.txt** — Dependencies
3. **Refactored notebook** — With clear sections and structure

