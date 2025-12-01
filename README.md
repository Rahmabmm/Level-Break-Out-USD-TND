# USD/TND Price Analysis & Trend Detection

A Python-based technical analysis tool for identifying important price levels, local extremes, and trend directions in USD/TND (US Dollar to Tunisian Dinar) exchange rate data.

## Overview

This project implements two complementary algorithms for analyzing financial time series data:

1. **Rolling Window Extremes Detection** - Identifies local tops and bottoms using a rolling window approach
2. **Perceptually Important Points (PIPs) Algorithm** - Finds the most significant price points that define the overall shape of the price movement

The analysis generates directional coding (uptrend/downtrend) to help understand market direction between important price levels.

## Features

- **Local Extremes Detection**: Identifies peaks and troughs using configurable rolling window
- **Important Points Extraction**: Uses multiple distance measures (Euclidean, Perpendicular, Vertical) to find key price levels
- **Trend Direction Coding**: Automatically assigns trend direction (+1 for uptrend, -1 for downtrend, 0 for neutral) between important points
- **Interactive Visualizations**: Creates both static (Matplotlib) and interactive (Plotly) visualizations
- **EMA Integration**: Includes Exponential Moving Average calculation for additional technical context

## Installation

### Requirements

```bash
pip install pandas numpy scipy matplotlib plotly pandas-ta
```

### Dependencies

- `pandas` - Data manipulation and analysis
- `numpy` - Numerical computing
- `scipy` - Scientific computing utilities
- `matplotlib` - Static plotting
- `plotly` - Interactive visualizations
- `pandas-ta` - Technical analysis indicators

## Data Format

The project expects a CSV file named `USDTND.csv` with the following structure:

```
date,close
2024-01-01,3.1234
2024-01-02,3.1256
...
```

Required columns:
- `date`: Date in a format parseable by pandas (e.g., YYYY-MM-DD)
- `close`: Closing price of USD/TND exchange rate

## Usage

### Basic Analysis

```python
import pandas as pd
import numpy as np
from your_module import rw_extremes, find_pips, generate_coding

# Load data
data = pd.read_csv("USDTND.csv")
data['date'] = pd.to_datetime(data['date'])
data = data.sort_values(by='date')

# Method 1: Rolling Window Extremes
close_prices = data['close'].to_numpy()
tops, bottoms = rw_extremes(close_prices, order=10)

# Method 2: Important Points (PIPs)
pips_x, pips_y = find_pips(close_prices, n_pips=50, dist_measure=3)

# Generate trend direction coding
data['direction'] = generate_coding(data, 'close', n_pips=60, search_range=1000)
```

### Visualization

```python
from your_module import plot_pipsss

# Interactive plot with important points
plot_pipsss(data, 'close', n_pips=20, search_range=300)
```

## Algorithm Details

### Rolling Window Extremes

Detects local maxima (tops) and minima (bottoms) by examining a window of data points around each candidate point. A point is considered a top if it's higher than all points within the specified order range.

**Parameters:**
- `order`: Number of points on each side to compare (default: 10)

### Perceptually Important Points (PIPs)

Iteratively finds the most significant points by measuring the distance of intermediate points from the line connecting adjacent important points.

**Parameters:**
- `n_pips`: Number of important points to identify
- `dist_measure`: Distance calculation method
  - `1`: Euclidean Distance
  - `2`: Perpendicular Distance
  - `3`: Vertical Distance (recommended)

### Direction Coding

Assigns trend direction between consecutive important points:
- `+1`: Uptrend (next point is higher)
- `-1`: Downtrend (next point is lower)
- `0`: Neutral (points are equal)

## Output

The analysis produces:

1. **DataFrame with direction coding**: Original data augmented with trend direction
2. **Interactive charts**: Plotly visualizations showing price action with marked important points
3. **Static plots**: Matplotlib charts for report generation
4. **Important points summary**: Table of key price levels and their directions

## Use Cases

- **Swing Trading**: Identify major support/resistance levels
- **Trend Analysis**: Understand long-term directional bias
- **Pattern Recognition**: Simplify price action for pattern identification
- **Risk Management**: Set stop-loss levels based on important price points
- **Market Structure Analysis**: Understand higher highs, lower lows, etc.

## Configuration

Key parameters to adjust:

```python
# Rolling window order - smaller values = more sensitive
tops, bottoms = rw_extremes(close_prices, order=5)

# Number of important points - more points = more detail
pips_x, pips_y = find_pips(close_prices, n_pips=30, dist_measure=3)

# EMA period for trend context
data['EMA'] = ta.ema(data.close, length=150)
```

## Example Results

The analysis identifies key turning points in the USD/TND exchange rate and simplifies the price action into a series of connected important points, making it easier to:
- Spot trend reversals
- Identify support/resistance zones
- Analyze market structure
- Make informed trading decisions

## Limitations

- Performance depends on parameter selection (order, n_pips, search_range)
- Historical analysis - not predictive
- Best suited for swing trading timeframes
- Requires sufficient data history for reliable results

## Future Enhancements

- [ ] Automated parameter optimization
- [ ] Pattern recognition on simplified price structure
- [ ] Backtesting framework integration
- [ ] Real-time data feed integration
- [ ] Multi-timeframe analysis
- [ ] Export analysis results to CSV/JSON

## Author

Rahma Ben Mbarek
