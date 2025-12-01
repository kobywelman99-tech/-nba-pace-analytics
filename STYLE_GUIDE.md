# NBA Pace Analytics — Code Style Guide

## Quick Reference

### File Organization
```
nba-pace-analytics/
├── .gitignore
├── README.md                      # Project overview
├── requirements.txt               # Dependencies
├── LICENSE                        # MIT or Apache 2.0
├── PUBLICATION_GUIDE.md          # This guide
├── NOTEBOOK_STRUCTURE.md         # Detailed organization
├── notebooks/
│   └── 01_get_data.ipynb        # Main analysis
├── data/                         # Raw/processed data
└── scripts/                      # Optional utility scripts
```

---

## Notebook Cell Organization

### Header Cell
```markdown
# NBA Pace Analytics: Game-Level & Team-Level Analysis

## Overview
Brief description of what this notebook does.

## Table of Contents
1. Setup & Configuration
2. Data Acquisition
...
```

### Section Header Cell (Markdown)
```markdown
## 1. Setup & Configuration

Import libraries and define constants for the entire analysis.
```

### Code Cell (Example)
```python
# ========== SECTION: Data Acquisition ==========
# Fetch game logs for all three seasons from NBA API

from nba_api.stats.endpoints import leaguegamelog

# Configuration
SEASON_2026 = '2025-26'
SEASON_2025 = '2024-25'
SEASON_2024 = '2023-24'

# Fetch data
game_logs_2026 = leaguegamelog.LeagueGameLog(
    season=SEASON_2026,
    season_type_all_star='Regular Season'
).get_data_frames()[0]

print(f"Fetched {len(game_logs_2026)} games for {SEASON_2026}")
```

---

## Variable Naming Conventions

| Type | Example | Reason |
|------|---------|--------|
| DataFrame | `game_logs_2026` | Descriptive, includes season |
| Series | `team_pace_2026` | Clear what data it holds |
| Numeric | `pace_delta`, `q75` | `snake_case` |
| Constant | `MIN_GAMES_THRESHOLD` | `UPPER_CASE` |
| Boolean | `is_valid`, `has_error` | `is_/has_` prefix |
| List | `seasons`, `team_names` | Plural form |
| Dict | `team_name_to_abbrev` | Descriptive key→value mapping |

---

## Comment Style

### Section Break
```python
# ========== SECTION: Feature Engineering ==========
```

### Explanation (Multi-line)
```python
# Calculate PACE (Possessions per 48 minutes)
# Formula: 48 × (POSS / (MIN/5))
# This normalizes possessions to a 48-minute game
```

### Inline Comment (Brief)
```python
df = df[df['MIN'] > 0]  # Exclude games with 0 minutes
```

### Code Documentation
```python
def calculate_pace(df):
    """
    Calculate PACE from possessions and minutes.
    
    PACE = 48 × (POSS / (MIN / 5))
    
    Args:
        df (pd.DataFrame): Game logs with POSS and MIN columns
        
    Returns:
        pd.Series: PACE values
    """
    return 48 * (df['POSS'] / (df['MIN'] / 5))
```

---

## Output Formatting

### Print Headers
```python
print("\n" + "="*70)
print("PACE vs EFFICIENCY CORRELATIONS (2025→2026)")
print("="*70)
print(f"  PACE vs ORTG: {pace_ortg_corr:7.3f}")
print(f"  PACE vs TS%:  {pace_ts_corr:7.3f}")
print("="*70 + "\n")
```

### DataFrames
```python
print("\nTop 5 Fastest Teams (2026):")
print(top_5_pace.to_string())

print("\nBucket Summary Statistics:")
print(bucket_summary.round(3))
```

---

## Import Organization

```python
# Standard library
import os
import sys
from datetime import datetime

# Third-party
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from scipy.stats import ttest_ind

# NBA API
from nba_api.stats.endpoints import leaguegamelog, LeagueDashTeamStats
```

---

## Function Examples

### Metric Calculation
```python
def calculate_team_metric(game_logs_df, metric_col, min_threshold=0):
    """
    Calculate season-average team metric.
    
    Args:
        game_logs_df (pd.DataFrame): Game logs
        metric_col (str): Column name to average
        min_threshold (int): Minimum minutes threshold
        
    Returns:
        pd.Series: Team averages indexed by TEAM_NAME
    """
    return (
        game_logs_df[game_logs_df['MIN'] > min_threshold]
        .groupby('TEAM_NAME')[metric_col]
        .mean()
    )
```

### Data Validation
```python
def validate_game_logs(df):
    """
    Validate game logs DataFrame structure.
    
    Raises ValueError if required columns missing.
    """
    required_cols = ['MIN', 'FGA', 'FTA', 'OREB', 'TOV', 'PTS']
    missing = [col for col in required_cols if col not in df.columns]
    if missing:
        raise ValueError(f"Missing columns: {missing}")
    print(f"✓ Validated {len(df)} game logs")
```

---

## Best Practices Checklist

- [ ] Variables use `snake_case`
- [ ] Constants use `UPPER_CASE`
- [ ] Functions have docstrings
- [ ] Comments explain *why*, not *what*
- [ ] No magic numbers (define as constants)
- [ ] Lines ≤ 100 characters (data science standard)
- [ ] 2 blank lines between functions
- [ ] 1 blank line between logical sections
- [ ] Remove commented-out code before publishing
- [ ] All cells run top-to-bottom without errors

---

## Examples to Remove Before Publishing

❌ **Commented Code:**
```python
#print(team_pace_comparison)
#log_2026.to_csv(r"C:\path\to\file.csv")
```

❌ **Overly Verbose Comments:**
```python
# Set x equal to 5
x = 5
```

❌ **Magic Numbers:**
```python
df = df[df['MIN'] > 0]  # What is this threshold?
```

✅ **Fixed:**
```python
MIN_GAMES_THRESHOLD = 0  # Exclude forfeited/incomplete games
df = df[df['MIN'] > MIN_GAMES_THRESHOLD]
```

---

## Testing Your Code

Before publishing, run your notebook top-to-bottom:
1. Click **Kernel → Restart & Run All**
2. Fix any errors
3. Verify all outputs are correct
4. Remove any temporary debug cells

---

## Final Review

Before pushing to GitHub:

```bash
# Check for .ipynb_checkpoints
find . -name ".ipynb_checkpoints" -type d

# Verify all files are tracked
git status

# Review changes
git diff --cached

# Commit with clear message
git commit -m "Add pace analytics notebook and documentation"
```

---

## References

- **PEP 8 Style Guide:** https://pep8.org/
- **Google Python Style Guide:** https://google.github.io/styleguide/pyguide.html
- **Jupyter Best Practices:** https://jupyter.readthedocs.io/en/latest/
- **pandas Documentation:** https://pandas.pydata.org/docs/

