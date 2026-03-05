# MTF Buy Confluence Dashboard (MTF-BCD)

A multi-timeframe buying checklist indicator for TradingView, built in Pine Script v5. It consolidates weekly and daily trend signals, momentum readings, Elder Impulse states, and broad market context into a single on-chart dashboard with an overall confidence score.

Created by [@techfocused](https://x.com/techfocused)

---

## What It Does

Instead of flipping between timeframes and indicators, MTF-BCD resolves everything on one chart. It runs 11 asset-specific checks across weekly and daily timeframes, displays pass/fail for each, and rolls them into a single confidence percentage. Two additional market-wide checks (VIX and NYSE New Lows) are shown separately so you can see both the stock's setup and the environment it's trading in.

---

## Dashboard Sections

### Buying Checklist (Rows 1-11)

These are the 11 asset-specific checks that make up the confidence score:

| # | Check | What It Measures |
|---|-------|-----------------|
| 1 | **13W MA Rising** | Weekly 13 EMA is trending up (current > previous bar) |
| 2 | **34W MA Rising** | Weekly 34 EMA is trending up |
| 3 | **13W EMA > 34W EMA** | Fast weekly EMA is above slow — bullish structure |
| 4 | **W MACD Hist Rising** | Weekly MACD histogram is increasing — momentum building |
| 5 | **D MACD Hist Rising** | Daily MACD histogram is increasing — near-term momentum |
| 6 | **8D EMA > 20D EMA** | Daily short-term trend favors bulls |
| 7 | **Price > 13W EMA** | Price is above the weekly trend anchor |
| 8 | **Price > 50D SMA** | Price is above the institutional moving average |
| 9 | **MACD(12,25,5) Pos** | Daily MACD line is above zero — in bullish territory |
| 10 | **Elder W Not Red** | Weekly Elder Impulse is not red (selling pressure absent) |
| 11 | **Elder D Not Red** | Daily Elder Impulse is not red |

Each check shows a green checkmark or red X. All 11 are included in the confidence score by default, but each can be toggled off individually in settings.

### Market Context (Rows 13-14)

These two checks monitor the broader market environment. They are displayed for awareness but are **not** included in the confidence percentage:

| Check | What It Measures |
|-------|-----------------|
| **VIX in Safe Zone** | VIX is below the threshold (default: 20). High VIX = elevated fear. |
| **NYSE New Lows < 500** | Fewer than 500 new lows on the NYSE. High new lows = broad weakness. |

### Overall Confidence (Row 15)

The confidence score is the percentage of enabled checks that are passing:

- **Green (80-100%)** — Strong confluence. Most or all conditions are aligned.
- **Orange (50-79%)** — Mixed signals. Some conditions are met, but the setup is incomplete.
- **Red (0-49%)** — Weak or no confluence. Most conditions are failing.

---

## Chart Overlays

### Moving Averages

Three key moving averages are plotted directly on the chart:

- **Purple line (width 2)** — Weekly 13 EMA. The primary weekly trend reference.
- **Blue line (width 2)** — Weekly 34 EMA. The slower weekly trend reference.
- **Orange line (width 1)** — Daily 50 SMA. Institutional support/resistance level.

### Manual Trendlines (Wedge)

Two anchor-based trendlines for marking wedge or channel patterns. Enter the bar index and price for two points on each line in the settings. Both lines extend to the right automatically.

- **Upper trendline** — Red. Mark descending resistance or an upper channel boundary.
- **Lower trendline** — Green. Mark ascending support or a lower channel boundary.

### Key Horizontal Level

A single horizontal line drawn across the entire chart at a price you specify. Useful for marking a breakout level, support/resistance, or a stop reference. Set to 0 to hide.

### Measurement Label

Enter two anchor points (bar index + price) and the indicator calculates the point change and percentage change between them, displayed as a label on the chart. The label is green for positive moves and red for negative.

### Volume-by-Price (VPVR Approximation)

Since TradingView's built-in VPVR is not accessible from Pine Script, this overlay approximates it:

- Divides the price range over the lookback period (default: 100 bars) into 20 buckets
- Accumulates volume into each bucket
- Displays the top 5 highest-volume buckets as horizontal boxes on the left side of the chart
- **Yellow boxes** — High-volume nodes (above median). These are likely support/resistance zones.
- **Blue boxes** — Lower-volume nodes. Price may move through these areas more easily.

---

## Alerts

Five alert conditions are available for TradingView's alert system:

| Alert | Triggers When |
|-------|--------------|
| **All Asset Checks Passed** | Confidence hits 100% — every enabled check is green |
| **Confidence Above 80%** | Confidence is 80% or higher |
| **VIX Danger Zone** | VIX crosses above the safe zone threshold |
| **Elder Impulse Red (W)** | Weekly Elder Impulse turns red — selling pressure detected |
| **Elder Impulse Red (D)** | Daily Elder Impulse turns red |

To set up an alert: click the alert icon on TradingView, select "MTF Buy Confluence Dashboard" as the condition, and choose the desired alert from the dropdown.

---

## Settings Reference

### MACD Settings

| Input | Default | Purpose |
|-------|---------|---------|
| Fast Length | 12 | MACD fast EMA period |
| Slow Length | 25 | MACD slow EMA period |
| Signal Length | 5 | MACD signal line smoothing |

### EMA Settings

| Input | Default | Purpose |
|-------|---------|---------|
| Fast EMA Period | 13 | Weekly fast EMA (13W) |
| Slow EMA Period | 34 | Weekly slow EMA (34W) |
| SMA Period (Daily) | 50 | Daily SMA for price comparison |
| Short EMA (Daily) | 8 | Daily short-term EMA |
| Mid EMA (Daily) | 20 | Daily mid-term EMA |

### Market Context

| Input | Default | Purpose |
|-------|---------|---------|
| VIX Symbol | CBOE:VIX | Fear gauge symbol |
| VIX Safe Zone Threshold | 20.0 | VIX below this = safe |
| NYSE New Lows Symbol | NYSE:LOWN | Breadth indicator. If unavailable, use INDEX:NYLOW |
| NYSE New Lows Threshold | 500 | New lows below this = healthy market |

### Confidence Score

Eleven individual toggles, one per check. Disable any check to exclude it from the confidence percentage without removing it from the dashboard display.

---

## How to Use It as a Trader

### The Core Workflow

1. **Start with the confidence score.** If it's below 50%, the setup isn't there — move on to another ticker or wait. If it's above 80%, you have strong multi-timeframe alignment and can start planning an entry.

2. **Read the checklist top to bottom.** The weekly checks (rows 1-4) tell you whether the higher timeframe supports a long position. The daily checks (rows 5-9) tell you whether the shorter timeframe agrees. The Elder checks (rows 10-11) act as a final filter — red on either timeframe is a warning.

3. **Check market context separately.** Even if the stock scores 100%, a VIX spike or elevated new lows means the environment is hostile. Consider reducing position size or waiting for the market to stabilize.

### Entry Criteria

The strongest buy setups occur when:

- Confidence is 80% or higher
- Both Elder Impulse checks are green or blue (not red)
- VIX is in the safe zone
- NYSE New Lows are under the threshold
- Price is pulling back toward a visible support (weekly 13 EMA, daily 50 SMA, or a volume node)

### Position Sizing and Risk

Use the confidence score as a position-sizing guide:

- **100%** — Full conviction. Maximum position size within your risk rules.
- **80-99%** — High confidence. Standard position size.
- **50-79%** — Partial setup. Consider a half position or wait for more checks to align.
- **Below 50%** — No trade. The trend structure does not support a long entry.

### What to Watch For

- **Elder Red on Weekly** — This is the strongest sell signal in the system. If the weekly Elder turns red, it means the weekly EMA is falling and weekly MACD histogram is declining simultaneously. Respect this.
- **Confidence dropping from 80%+ to below 50%** — The trend is deteriorating. Tighten stops or exit.
- **VIX spiking above threshold while stock confidence is high** — The stock may look good in isolation, but systemic risk is elevated. Reduce exposure.

### Customization Tips

- **Swing traders**: Keep all 11 checks enabled. The default settings are tuned for multi-week holds.
- **Position traders**: Consider disabling the daily MACD checks (5 and 9) to reduce noise and focus on weekly structure.
- **Aggressive entries**: Disable "34W MA Rising" and "Price > 50D SMA" to catch earlier-stage breakouts before the slower averages confirm.
- **Conservative entries**: Leave everything on and only buy at 100% confidence with clean market context.

---

## Installation

1. Open TradingView and go to the Pine Script Editor (bottom panel).
2. Click "Open" > "New indicator".
3. Delete the default code and paste the entire contents of `MTF_Buy_Confluence_Dashboard.pine`.
4. Click "Add to chart".
5. The dashboard will appear at the top-right corner of your chart.

---

## Notes

- The indicator works on any ticker and any chart timeframe. Weekly and daily data are always resolved via `request.security()` regardless of the chart you're viewing.
- No lookahead is used in any `request.security()` call. All data is confirmed (closed bar) data only.
- If `NYSE:LOWN` is unavailable on your data feed, change the NYSE New Lows Symbol input to `INDEX:NYLOW`.
- The VPVR approximation uses a maximum of 5 boxes (TradingView's `max_boxes_count` limit). It displays the top 5 volume nodes rather than the full profile.

---

## Credits

Indicator concept and specifications by [@techfocused](https://x.com/techfocused).

Built with Pine Script v5 for TradingView.
