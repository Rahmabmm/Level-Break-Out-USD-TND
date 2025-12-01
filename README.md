# USD/TND Head and Shoulders Pattern Detection

> **⚠️ Educational Project**: This is a learning and exploration project created to understand technical analysis and pattern detection algorithms. **The patterns detected here are NOT profitable trading signals** and should not be used for actual trading decisions. This project is for educational purposes only.

A Python-based technical analysis tool for detecting Head and Shoulders (HS) and Inverse Head and Shoulders (IHS) patterns in USD/TND (US Dollar to Tunisian Dinar) exchange rate data with automated pattern validation and performance analysis.

## Overview

This project was created as a learning exercise to explore pattern recognition algorithms and understand how classic technical analysis patterns work in practice. It implements algorithms to identify reversal patterns in forex data, validate them using statistical measures (R² symmetry checks, neckline slope analysis), and analyze their historical performance.

**Purpose**: Learning and skill development in technical analysis and algorithmic pattern detection  
**Developed**: 2024  
**Last Updated**: December 2025

## Key Features

- **Automated Pattern Detection**: Identifies both bullish (IHS) and bearish (HS) reversal patterns
- **Statistical Validation**: Uses R² correlation and symmetry checks to filter false positives
- **Rolling Window Extremes**: Custom algorithm to detect local tops and bottoms
- **Performance Analysis**: Calculates hold returns and stop-loss based returns for each pattern
- **Early Detection Mode**: Option to identify patterns before full completion
- **Interactive Visualizations**: Candlestick charts with pattern overlays using mplfinance and Plotly
- **Neckline Analysis**: Validates patterns based on neckline slope characteristics

## What are Head and Shoulders Patterns?

**Head and Shoulders (HS)**: A bearish reversal pattern with three peaks - left shoulder, head (highest), and right shoulder. Signals potential downtrend when neckline breaks.

**Inverse Head and Shoulders (IHS)**: A bullish reversal pattern with three troughs - left shoulder, head (lowest), and right shoulder. Signals potential uptrend when neckline breaks.

## Installation

### Requirements

```bash
pip install pandas pandas-ta numpy scipy matplotlib plotly 
```

### Dependencies

- `pandas` - Data manipulation and analysis
- `pandas-ta` - Technical analysis indicators (EMA)
- `numpy` - Numerical computing
- `scipy` - Statistical analysis and pattern validation
- `matplotlib` - Base plotting functionality
- `plotly` - Interactive visualizations

## Data Format

The project expects a CSV file named `USDTND.csv` with the following structure:

```csv
date,open,high,low,close,change%
2010-01-05,1.3450,1.3500,1.3400,1.3480,0.22
2010-01-06,1.3480,1.3520,1.3460,1.3500,0.15
...
```

**Required columns:**
- `date`: Date in YYYY-MM-DD format
- `open`: Opening price
- `high`: Highest price of the day
- `low`: Lowest price of the day
- `close`: Closing price
- `change%`: Daily percentage change (optional)

**Data Range**: 2010-01-05 to 2024-08-01 (or your custom range)

## Usage

### Running the Analysis

```python
# The notebook automatically:
# 1. Loads and preprocesses data
# 2. Calculates EMA (150-period)
# 3. Detects rolling window extremes
# 4. Identifies HS and IHS patterns
# 5. Validates patterns with statistical checks
# 7. Visualizes results

```

### Key Functions

```python
# Detect local extremes
tops, bottoms = rw_extremes(close_prices, order=6)

# Check for Head and Shoulders pattern
is_hs = check_hs_pattern(extremes, r2_threshold=0.90)

# Check for Inverse Head and Shoulders pattern
is_ihs = check_ihs_pattern(extremes, r2_threshold=0.90)

# Calculate pattern performance
returns = calculate_pattern_returns(pattern_data, hold_period=20)
```

## Pattern Detection Algorithm

### Step 1: Rolling Window Extremes Detection
- Uses configurable order (default: 6) to identify local peaks and troughs
- Validates that a point is higher/lower than all surrounding points within the window

### Step 2: Pattern Recognition
For **Head and Shoulders (HS)**:
1. Identifies sequence: Top → Bottom → Higher Top (Head) → Bottom → Top
2. Validates left and right shoulders are roughly symmetrical
3. Checks that head is significantly higher than shoulders
4. Validates neckline (line connecting the two bottoms)

For **Inverse Head and Shoulders (IHS)**:
1. Identifies sequence: Bottom → Top → Lower Bottom (Head) → Top → Bottom
2. Validates shoulder symmetry
3. Checks that head is significantly lower than shoulders
4. Validates neckline formation

### Step 3: Statistical Validation
- **R² Symmetry Check**: Ensures left and right sides of pattern are symmetrical (default threshold: 0.90)
- **Neckline Slope Analysis**: Validates neckline characteristics
- **Height/Width Ratios**: Checks pattern proportions are realistic
- **Early Detection Option**: Can identify patterns before full completion


## Output

The analysis produces:

1. **Pattern DataFrames**: `hs_df` and `ihs_df` containing:
   - Pattern location (indices)
   - Head width and height
   - R² symmetry score
   - Neckline slope
   - Hold returns
   - Stop-loss adjusted returns

2. **Visualizations**:
   - Pattern overlays showing shoulders, head, and neckline


## Configuration

Key parameters to adjust:

```python
# Rolling window sensitivity
tops, bottoms = rw_extremes(close_prices, order=6)  # Lower = more sensitive

# Pattern validation strictness
r2_threshold = 0.90  # Higher = stricter validation (0.0 to 1.0)

# EMA period for trend context
data['EMA'] = ta.ema(data.close, length=150)

# Return calculation period
hold_period = 20  # Days to hold after pattern completion

# Early detection
early_detection = True  # Detect patterns before completion
```

## Learning Objectives & Use Cases

This project was created to learn:
- **Pattern Recognition Algorithms**: How to programmatically detect chart patterns
- **Statistical Validation**: Using R² and other metrics to validate patterns
- **Technical Analysis**: Understanding classic HS/IHS pattern theory
- **Data Analysis**: Processing and analyzing financial time series
- **Python Libraries**: Working with pandas, numpy, scipy, and visualization tools
- **Algorithm Design**: Implementing rolling window detection methods

**Educational Use Cases:**
- Study how pattern detection algorithms work
- Learn technical analysis implementation
- Practice financial data analysis
- Understand pattern validation techniques
- Explore statistical measures in trading


## Performance Metrics

The notebook calculates:
- **Win Rate**: Percentage of profitable patterns
- **Average Return**: Mean return per pattern type
- **Risk/Reward Ratio**: Based on neckline stop-loss
- **Pattern Frequency**: How often patterns occur
- **Time to Target**: Average duration of pattern completion

## Example Results

```
Head and Shoulders Patterns Detected: 15
Inverse Head and Shoulders Patterns Detected: 12


Pattern Success Rate (HS): 73%
Pattern Success Rate (IHS): 67%
```

## Data Sources

Historical USD/TND data can be obtained from:
- [Investing.com](https://www.investing.com/currencies/usd-tnd-historical-data)

## Future Enhancements

- [ ] Real-time pattern detection with live data feeds
- [ ] Additional patterns (Double Top/Bottom, Triangles, Wedges)
- [ ] Machine learning validation of patterns
- [ ] Backtesting framework with transaction costs
- [ ] Multi-timeframe analysis
- [ ] Pattern scanner for multiple currency pairs
- [ ] Automated alert system for new patterns
- [ ] API integration for live trading signals
- [ ] Risk management calculator
- [ ] Export trading signals to CSV/JSON

## Disclaimer

**⚠️ IMPORTANT: READ BEFORE USING THIS CODE**

This is an **educational project** created for learning purposes:

- ❌ **NOT for actual trading** - Patterns detected are not profitable
- ❌ **NOT financial advice** - Do not use for making trading decisions
- ❌ **NOT a trading system** - This is a learning exercise, not a viable strategy
- ✅ **FOR learning only** - To understand pattern detection and technical analysis
- ✅ **FOR portfolio/resume** - To demonstrate coding and analytical skills
- ✅ **FOR exploration** - To experiment with algorithmic pattern recognition


**Key Takeaway**: This project demonstrates that simply detecting classical chart patterns is not sufficient for profitable trading. It's a valuable learning experience about the challenges of algorithmic trading and the importance of rigorous testing.

** Explicitly states:**
- These patterns did NOT prove profitable in testing
- This project is shared to demonstrate technical skills, not trading profitability
- No trading decisions should be based on this code
- Past performance (especially unprofitable performance) does not indicate future results

**If you're interested in algorithmic trading:**
- This project shows what DOESN'T work (which is valuable knowledge!)
- Real profitable trading requires much more than pattern detection
- Consider professional education, risk management, and extensive backtesting
- Most retail traders lose money - never risk money you can't afford to lose

## Author

Rahma Ben Mbarek 
**Project Date**: 2024  
**Documentation**: December 2025

## Acknowledgments

- Head and Shoulders pattern theory based on classical technical analysis
- Rolling window algorithm inspired by peak detection literature
- Statistical validation methods from quantitative finance research
- USD/TND data covers 2010-2024 period

