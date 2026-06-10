# ₿ Bitcoin Deep Analysis Dashboard

> **A comprehensive Jupyter notebook for fetching, engineering, and visualizing Bitcoin market data using interactive Plotly charts.**

---

## 🚀 Quick Start

```bash
# Install Jupyter if you haven't already
pip install notebook

# Launch the notebook
jupyter notebook bitcoin_analysis.ipynb
```

The first cell automatically installs all required Python packages (`yfinance`, `plotly`, `pandas`, `numpy`, `scipy`, `requests`, `kaleido`). Just run all cells top to bottom.

---

## 📦 Data Sources

| Source | What it provides |
|---|---|
| **Yahoo Finance** (`yfinance`) | OHLCV price data for BTC-USD and comparison assets (ETH, Gold, SPY, etc.) |
| **Alternative.me API** | Fear & Greed Index — last 365 days, updated daily |

By default, price history is fetched from **2018-01-01** to today. Change `START` and `END` in Cell 4 to adjust the range.

---

## 📊 Plot Reference

---

### 1 · OHLCV Candlestick Chart + Volume + Moving Averages

**What it is:**  
A classic Japanese candlestick chart showing the Open, High, Low, and Close price for each day, combined with a volume bar chart below and three moving average overlays.

**How to read it:**
- 🟢 **Green candles** — close was higher than open (bullish day)
- 🔴 **Red candles** — close was lower than open (bearish day)
- The **wick** (thin line) represents the full High-Low range for that day
- **Volume bars** are colored to match the day's direction

**Moving Averages overlaid:**

| Line | Color | What it means |
|---|---|---|
| SMA20 | 🟠 Orange | Short-term trend (1 month) |
| SMA50 | 🟡 Gold | Medium-term trend (2 months) |
| SMA200 | 🟣 Purple | Long-term trend (almost 1 year) |

**What to look for:**
- **Golden Cross**: SMA50 crosses above SMA200 → historically a strong bullish signal
- **Death Cross**: SMA50 crosses below SMA200 → historically a bearish signal
- **High volume on up days** confirms buying pressure; high volume on down days signals panic selling
- Price consistently above SMA200 = long-term bull market; below = bear market

---

### 2 · Bollinger Bands

*Displayed in the top panel of the Technical Indicators Dashboard*

**What it is:**  
A 20-day simple moving average (the midline) surrounded by upper and lower bands set at ±2 standard deviations of price.

**How to read it:**
- The **band width** expands during high volatility and contracts during low volatility ("the squeeze")
- Price touching or breaching the **upper band** signals the asset may be overbought
- Price touching or breaching the **lower band** signals it may be oversold

**What to look for:**
- **Bollinger Squeeze**: when the bands narrow sharply, a big breakout (in either direction) is often imminent
- **Walking the bands**: during strong trends, price can "ride" the upper or lower band for extended periods — don't automatically fade it
- **W-bottom / M-top** patterns at the bands are classic reversal setups

---

### 3 · RSI (Relative Strength Index)

*Middle panel of the Technical Indicators Dashboard*

**What it is:**  
A momentum oscillator (0–100) that measures the speed and magnitude of recent price changes. Calculated over a 14-day window.

**How to read it:**
- **Above 70** → overbought zone (🔴 red dashed line) — price may be due for a pullback
- **Below 30** → oversold zone (🟢 teal dashed line) — price may be due for a bounce
- **50 = neutral** — crossing above 50 is often seen as confirming a bullish trend, below 50 as bearish

**What to look for:**
- **Divergence**: RSI makes a lower high while price makes a higher high → hidden weakness, potential reversal
- In strong bull markets, RSI can stay overbought (>70) for weeks — useful for timing entries but not exits alone
- RSI recovering from below 30 back above 30 is often a buy signal

---

### 4 · MACD (Moving Average Convergence/Divergence)

*Bottom panel of the Technical Indicators Dashboard*

**What it is:**  
The MACD is the difference between a 12-period EMA and 26-period EMA. The "Signal" line is a 9-period EMA of the MACD. The histogram shows the gap between the two lines.

**How to read it:**
- **MACD crosses above Signal** → bullish momentum building
- **MACD crosses below Signal** → bearish momentum building
- **Histogram above zero and growing** → accelerating uptrend
- **Histogram shrinking** (even if still positive) → trend losing steam

**What to look for:**
- **Zero-line crossovers**: MACD crossing from negative to positive territory is a medium-strength bullish signal
- **Divergence**: price hits new highs but MACD doesn't → trend exhaustion
- The histogram turning from red to green before price moves is a leading signal

---

### 5 · Returns Distribution + Q-Q Plot

**What it is:**  
Two side-by-side charts. Left: a histogram of daily percentage returns overlaid with a theoretical Normal distribution curve. Right: a Quantile-Quantile (Q-Q) plot comparing Bitcoin returns against a perfect normal distribution.

**How to read it:**
- **Histogram**: The taller and narrower the peak around 0%, the more "stable" the asset. Bitcoin's peak is sharper than the normal curve.
- **Q-Q Plot**: If points fall perfectly on the diagonal line, returns are normally distributed. Deviations at the extremes = fat tails.

**Key stats printed below the chart:**

| Stat | What it means |
|---|---|
| **Mean** | Average daily return |
| **Std Dev** | Typical day-to-day fluctuation |
| **Skewness** | Asymmetry of returns (positive = more upside outliers) |
| **Excess Kurtosis** | Fat-tailedness — Bitcoin consistently shows very high kurtosis (>3), meaning extreme days happen far more often than a normal distribution would predict |

**What to look for:**
- Fat tails in the Q-Q plot (points curving away from the line at the extremes) confirm that **black swan days** — both to the upside and downside — happen regularly
- High kurtosis means standard risk models (like Value-at-Risk using normal distributions) underestimate true risk

---

### 6 · Cumulative Returns: Buy & Hold vs SMA200 Trend Strategy

**What it is:**  
A comparison of two strategies over the full history:
1. **Buy & Hold**: Simply buy BTC on Day 1 and hold
2. **SMA200 Trend**: Be long only when price > SMA200, otherwise sit in cash

**How to read it:**
- The Y-axis shows a multiplier (e.g., `10×` means you turned $1 into $10)
- When the orange line (B&H) is above the teal line (trend strategy), staying invested outperformed

**What to look for:**
- The SMA200 strategy typically underperforms in strong bull runs (you miss early-trend gains) but significantly reduces drawdown in bear markets
- This chart is a useful reminder that **risk-adjusted returns** often matter more than raw returns

---

### 7 · Price vs Realized Volatility

**What it is:**  
Two-panel chart: BTC price on top, annualized rolling realized volatility (%) below, shown for both 20-day and 60-day windows.

**How to read it:**
- **High volatility** periods correspond to major crashes and euphoric rallies
- **Realized volatility** = standard deviation of log-returns × √252 (annualized). A value of 80% means Bitcoin moves ±80% per year on average in that window.

**What to look for:**
- **Volatility clustering**: calm periods tend to stay calm, volatile periods stay volatile (useful for options pricing)
- Spikes in Vol20 (red) before Vol60 (gold) catches up signal a sudden market shock
- Low volatility environments (e.g., below 40%) often precede a large move — the "calm before the storm"

---

### 8 · Monthly Realized Volatility Heatmap

**What it is:**  
A grid where rows = years, columns = months, and each cell shows the average annualized realized volatility for that month. Color goes from blue (low vol) to yellow/red (high vol).

**How to read it:**
- Darker red/yellow cells = historically turbulent months
- Blue cells = historically calm periods

**What to look for:**
- Certain months (e.g., November–December historically) tend to show elevated activity
- Compare current year to past years to gauge whether this year is unusually calm or stormy

---

### 9 · Monthly Returns Heatmap

**What it is:**  
A calendar grid showing the total percentage return for each calendar month across all years. Green = positive month, red = negative month.

**How to read it:**
- Each cell value is the **compounded return** for that specific month/year
- Brighter green = stronger bull month; deeper red = bigger crash

**What to look for:**
- **January Effect**: Some years show strong January rallies
- **"Sell in May"**: Check whether the May-September stretch is consistently weak
- Look for recurring patterns — but remember small sample sizes (only ~7 years of data) so treat with caution

---

### 10 · Day-of-Week & Month-of-Year Seasonality Bars

**What it is:**  
Two bar charts showing the **average daily return** grouped by day of the week and by month of the year.

**How to read it:**
- Green bars = historically positive on average; red bars = historically negative
- Values are small (fractions of a percent) because they're averages across hundreds of data points

**What to look for:**
- **Weekend effects**: Crypto markets run 24/7, so weekend patterns differ from equities
- Month-level bars reveal the strongest and weakest calendar months historically
- These are statistical tendencies, not guarantees — but useful when combined with other signals

---

### 11 · Drawdown Chart

**What it is:**  
Two panels: BTC price on top, and the percentage decline from the all-time high (at each point in time) on the bottom.

**How to read it:**
- A drawdown of **-80%** means price is 80% below its previous all-time high at that moment
- The red shaded area fills in whenever you're "underwater" relative to the ATH
- The annotated point marks the **maximum drawdown** in the dataset

**What to look for:**
- **Recovery time**: How many months/years does it take for Bitcoin to recover from major drawdowns?
- **Current drawdown**: If the bottom panel is deep red right now, it indicates you're in a bear market
- Bitcoin has historically seen 80%+ drawdowns in every major cycle — useful context for risk management

---

### 12 · Performance Summary Table

**What it is:**  
A formatted table of key risk/return statistics computed over the full data range.

**Metrics explained:**

| Metric | What it means | Good value for BTC |
|---|---|---|
| **Total Return** | Raw cumulative gain | — (historical fact) |
| **CAGR** | Compound Annual Growth Rate — the "true" annualized return | Higher is better |
| **Ann. Volatility** | Standard deviation of returns × √252 | Context-dependent |
| **Sharpe Ratio** | CAGR ÷ Volatility (risk-adjusted return vs cash) | >1.0 is good |
| **Sortino Ratio** | Like Sharpe but only penalizes downside volatility | >1.5 is good |
| **Calmar Ratio** | CAGR ÷ Max Drawdown — rewards consistency | >0.5 is reasonable |
| **Max Drawdown** | Worst peak-to-trough decline ever | Smaller magnitude is better |
| **Win Rate** | % of days with a positive return | ~50–55% is typical |
| **Best / Worst Day** | Extreme single-day moves | Shows tail risk |
| **Avg Up/Down Day** | Typical gain on a green/red day | Asymmetry matters |

---

### 13 · Cross-Asset Correlation Heatmap

**What it is:**  
A symmetric matrix showing the Pearson correlation coefficient between daily returns of BTC, ETH, Gold, S&P 500 (SPY), Nasdaq (QQQ), the US Dollar Index (DXY), and Oil.

**How to read it:**
- **+1.0** (bright green) = moves in perfect lockstep
- **0.0** (dark) = no relationship
- **-1.0** (bright red) = moves in perfect opposition

**What to look for:**
- **BTC ↔ ETH**: typically high positive correlation (both are crypto risk assets)
- **BTC ↔ DXY**: historically negative — when the dollar strengthens, crypto often falls
- **BTC ↔ Gold**: low correlation historically, but this has been shifting — BTC is increasingly treated as "digital gold"
- **BTC ↔ SPY/QQQ**: correlation spikes during macro risk-off events (e.g., 2022) showing BTC is not always a safe haven

---

### 14 · Rolling 90-Day Correlation

**What it is:**  
Line chart tracking how Bitcoin's correlation with ETH, Gold, S&P 500, and Nasdaq **changes over time** using a 90-day rolling window.

**How to read it:**
- When a line moves **up toward +1**, BTC is moving in tandem with that asset
- When a line moves **down toward -1**, BTC is moving in the opposite direction
- Correlations are not stable — they shift dramatically with macro regimes

**What to look for:**
- During **market crises**, correlations with equities spike (everything sells off together)
- During **crypto bull markets**, the BTC-ETH correlation may drop as different coins rotate
- A rising BTC-Gold correlation suggests the market is pricing BTC as a store-of-value asset

---

### 15 · Annual Returns Bar Chart

**What it is:**  
A simple bar chart showing Bitcoin's total return for each calendar year, colored green for positive and red for negative.

**How to read it:**
- Each bar represents the full-year compounded return
- Values outside each bar show the exact percentage

**What to look for:**
- The **4-year halving cycle**: historically years following a halving (e.g., 2017, 2021, 2025) tend to be strong
- Only 2018 and 2022 have been significantly negative years in the dataset — the rest were positive or extremely positive
- This chart is one of the most powerful arguments for long-term Bitcoin holding

---

### 16 · Annual High-Low Range Chart (Log Scale)

**What it is:**  
A chart where each year is represented as a vertical bar spanning the annual Low to the annual High, with a dot marking the year-end closing price. The Y-axis is **logarithmic**.

**How to read it:**
- A **tall bar** = high intra-year volatility (e.g., 2017 or 2021)
- A **short bar** = more subdued year
- The log scale compresses large prices so earlier years are still visible

**What to look for:**
- The **range as a percentage** of the low is the key insight — e.g., a move from $3,000 to $20,000 is a 567% range
- The year-end dot position tells you whether the year closed near its highs (bullish) or near its lows (bearish)

---

### 17 · Fear & Greed Gauge

**What it is:**  
A real-time gauge indicator (pulled from Alternative.me) showing the current market sentiment on a scale of 0–100.

| Range | Label | Meaning |
|---|---|---|
| 0–24 | 🔴 Extreme Fear | Panic selling; historically a buy signal |
| 25–44 | 🟠 Fear | Bearish sentiment; cautious market |
| 45–55 | ⚪ Neutral | Balanced market |
| 55–74 | 🟢 Greed | Optimism; watch for overheating |
| 75–100 | 🟡 Extreme Greed | Euphoria; historically a caution/sell signal |

**What to look for:**
- **Contrarian signal**: Warren Buffett's "be fearful when others are greedy, be greedy when others are fearful" applies strongly to crypto
- Extended periods of Extreme Fear (0–20) near major cycle bottoms
- Extreme Greed near all-time highs often precedes corrections

---

### 18 · BTC Price vs Fear & Greed (Time Series)

**What it is:**  
A 1-year time series showing BTC price on top and the daily Fear & Greed Index on the bottom, with dashed lines at 25 (fear) and 75 (greed).

**What to look for:**
- Large divergences: does price rally while sentiment stays fearful? That can signal smart money accumulation
- Sentiment leading or lagging price — sometimes Fear & Greed turns before price does
- Tracking whether the current reading matches the price action (confirmation vs divergence)

---

### 19 · On-Balance Volume (OBV)

**What it is:**  
OBV is a cumulative volume indicator. Each day, if price closes up, that day's volume is added; if price closes down, volume is subtracted. It tracks whether volume is flowing into or out of Bitcoin.

**How to read it:**
- A **rising OBV** = more volume on up-days → accumulation (bullish)
- A **falling OBV** = more volume on down-days → distribution (bearish)
- OBV EMA20 (gold dashed) smooths the signal

**What to look for:**
- **OBV divergence**: OBV makes a new high while price doesn't → price is likely to follow OBV up
- OBV breaking below its EMA during a price rally is a red flag — the rally may not be supported by real buying
- Best used to confirm breakouts: a price breakout with surging OBV is far more reliable than one with flat OBV

---

### 20 · Volume Profile

**What it is:**  
A horizontal histogram showing how much total trading volume occurred at each price level over the last 12 months. Displayed alongside the price chart.

**How to read it:**
- **Wide bars** = high-volume price zones — this is where most trading has happened, creating strong support/resistance
- **Narrow bars** = low-volume price zones — price tends to move quickly through these areas
- The **Point of Control (POC)** is the price level with the largest bar (the widest bar)

**What to look for:**
- Price returning to a **high-volume node** after being away often finds support or resistance there
- A **low-volume gap** between two clusters means price could travel quickly if it enters that zone
- Breakouts above a high-volume node on strong volume confirm the move

---

### 21 · Log Price Chart with Halving Events

**What it is:**  
Bitcoin's full price history on a **logarithmic scale**, with vertical dashed lines marking each of the four halving events.

**How to read it:**
- On a log scale, equal vertical distances represent equal **percentage** moves (not dollar moves)
- This is the most honest way to view Bitcoin's long-term price trajectory
- Each halving reduces the block reward (newly minted BTC) by 50%

**Halvings marked:**

| Event | Date | Block Reward |
|---|---|---|
| 1st Halving | Nov 28, 2012 | 25 BTC → 12.5 BTC |
| 2nd Halving | Jul 9, 2016 | 12.5 BTC → 6.25 BTC |
| 3rd Halving | May 11, 2020 | 6.25 BTC → 3.125 BTC |
| 4th Halving | Apr 19, 2024 | 3.125 BTC → 1.5625 BTC |

**What to look for:**
- Each halving has historically been followed by a major bull cycle within 12–18 months (supply shock thesis)
- On a log scale, Bitcoin's long-term uptrend appears as a consistent slope — zoom-out perspective is key
- The magnitude of each cycle's gain has diminished over time as market cap grows

---

### 22 · 4-Panel Technical Summary Dashboard

**What it is:**  
A compact 2×2 grid combining four of the most important charts into a single view over the last 6 months:
1. **Price + SMA20/50/200** — trend context
2. **RSI (14)** — momentum
3. **MACD (12, 26, 9)** — trend strength & crossovers
4. **Rolling Volatility (20D & 60D)** — risk environment

**What to look for:**
- Use all four panels together for a **holistic view** before making a decision
- Bullish setup: Price > all MAs, RSI 50–65, MACD above signal, volatility moderate or contracting
- Bearish setup: Price < SMA200, RSI < 45, MACD negative, volatility elevated and rising

---

## 🎨 Color Legend

| Color | Meaning |
|---|---|
| 🟠 Orange `#F7931A` | Bitcoin's brand color — price/close lines |
| 🟢 Teal `#00D4AA` | Bullish / positive / buy signals |
| 🔴 Red `#FF4560` | Bearish / negative / sell signals |
| 🟡 Gold `#FFB347` | Secondary MAs, longer-term trends |
| 🔵 Blue `#008FFB` | Bollinger Bands |
| 🟣 Purple `#775DD0` | RSI, SMA200, Fear & Greed |

---

## 📁 Repository Structure

```
bcoin/
├── bitcoin_analysis.ipynb   # Main analysis notebook
└── README.md                # This file
```

---

## ⚠️ Disclaimer

This notebook is for **educational and research purposes only**. Nothing here constitutes financial advice. Cryptocurrency markets are highly volatile and past performance is not indicative of future results. Always do your own research.
