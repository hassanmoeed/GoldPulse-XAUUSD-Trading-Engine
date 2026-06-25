# ⚡ GoldPulse — Autonomous XAUUSD Algorithmic Trading Engine

> Self-executing MetaTrader 5 Expert Advisor engineered for live gold trading. Ingests real-time XAUUSD price data, validates multi-timeframe structural bias, anchors execution to confirmed SR zones, and autonomously manages the full trade lifecycle from entry through exit.

[![MQL5](https://img.shields.io/badge/MQL5-MetaTrader%205-1a1a2e?style=flat&logoColor=white)](https://www.mql5.com)
[![Asset](https://img.shields.io/badge/Asset-XAUUSD%20Gold-FFD700?style=flat)](https://www.gold.org)
[![Exhibition](https://img.shields.io/badge/CIPE%20Spring%202026-AI%20Domain-brightgreen?style=flat)](https://github.com/hassanmoeed)

---

## What It Executes

Most bots fire on a single indicator. GoldPulse cross-validates three timeframes simultaneously, confirms structural alignment before committing capital, and enforces risk normalized position sizing on every trade. Once live, it monitors, adjusts, hedges, and exits without human input.

---

## System Flow

```
OnTick() fires on every price update
    ↓
ManageOpenTrades()        →  partial close  |  trail SL  |  news hedge
    ↓
CheckNewsBlockAndTrade()  →  blocks entry during news windows
    ↓
DetectAndDrawActiveZones() →  swing scan  |  zone validation  |  SR rendering
    ↓
UpdateDashboard()         →  refreshes live metrics and trade matrix
```

---

## Core Modules

### Trend Engine
Computes dual EMA crossover (20/50) independently across Daily, H4 and H1 timeframes. Confirms directional bias only when Daily and H4 align. H1 serves as entry timing confirmation. Reports strength as Strong, Moderate or Weak based on percentage separation between EMAs.

### SR Zone Detector
Scans 150 candles for swing highs and lows using a configurable wick lookback window. Validates each level by touch frequency  a zone must be tested a minimum number of times to qualify as an active execution level. Renders only the single closest support and resistance to eliminate chart noise.

### Signal Generator
Evaluates zone proximity against confirmed trend bias. Triggers a BUY only when price approaches a validated support zone with bullish Daily and H4 alignment. Triggers SELL under the inverse condition. Returns Waiting when structural confluence is absent.

### ATR Risk Engine
Classifies market volatility regime  Low, Normal, High or Extreme  based on ATR pip value. Computes position size dynamically from account balance, configurable risk percentage, SL distance and tick value. Eliminates fixed lot guessing entirely.

### Trade Lifecycle Manager

| Phase | Trigger | Execution |
|---|---|---|
| Entry | Zone proximity confirmed, bias aligned | Market order dispatched with IOC then FOK then RETURN fallback |
| Partial Close | Unrealized move reaches 1:1 RR | Closes 50% of position to lock profits |
| Trailing SL | Move exceeds SL x RR target | Relocates SL to current price minus ATR x 0.4 |
| News Hedge | High impact event imminent, position in drawdown | Deploys counter position of equal size |
| Exit | TP or SL triggered | Closes position, resets all state variables |

### News Block System
Monitors scheduled economic events and suppresses new entries within a configurable window before high or medium impact releases. Activates the hedge module automatically if an open position is exposed to an approaching event.

### Live Dashboard
Renders a two panel overlay directly on the MT5 chart. Left panel broadcasts market structure, AI bias, volatility regime, active session with PKT end time, and next scheduled news event. Right panel streams the active trade matrix  entry, SL, TP, RR, expected pips, projected USD profit, live PnL, trail status and hedge status.

---

## Configuration

```mql5
input int    Fast_MA_Period    = 20;     // Fast EMA period
input int    Slow_MA_Period    = 50;     // Slow EMA period
input int    ATR_Period        = 14;     // ATR lookback
input int    SR_Bars           = 150;    // Candles scanned for SR detection
input int    SR_Min_Touches    = 2;      // Minimum touches to validate a zone
input double RR_Target         = 1.5;    // Risk to Reward ratio
input double Risk_Pct          = 1.0;    // Capital risked per trade (%)
input int    News_Block_Mins   = 10;     // Pre-news blackout window (minutes)
input double Partial_Close_Pct = 50.0;  // Partial close percentage at 1:1
input bool   Enable_Hedge      = true;   // Activate news hedge module
input bool   Enable_Trail      = true;   // Activate trailing SL
input bool   Enable_Partial    = true;   // Activate partial close
input bool   Enable_AutoLot    = true;   // Activate dynamic lot sizing
```

---

## Deployment

1. Drop `Custom_AI_Dashboard_v6.mq5` into your MT5 Experts folder
2. Compile via MetaEditor (F7)
3. Attach to a XAUUSD chart and enable Auto Trading
4. Set inputs in the EA properties panel and confirm

Recommended timeframe: H1 for intraday, H4 for swing sessions.

---

## Signal Logic

```
Price enters validated SR zone?
    ├── YES  +  Daily and H4 both Bullish  →  BUY   →  Entry at ASK
    ├── YES  +  Daily and H4 both Bearish  →  SELL  →  Entry at BID
    └── NO   /  Mixed bias                 →  Waiting  →  No position opened
```

---

## Recognition

Presented at the **Computer Engineering In-House Project Exhibition (CIPE) Spring 2026**
Sir Syed University of Engineering and Technology AI Domain
Official Participation Certificate awarded

---

## Disclaimer

Engineered for educational and research purposes. Forex and gold trading carries substantial financial risk. Validate thoroughly on a demo account before deploying real capital.
![Bot Dashboard](Bot%20Dashboard.jpeg)

---

## Author

Hassan Bin Moeed
Computer Engineering Student Sir Syed University of Engineering and Technology
