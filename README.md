# KRYPTX · Quantum ML Terminal

A professional crypto analysis terminal powered by a Random Forest ML model, K-Means clustering, and real-time Binance data.

---

## Features

- **Random Forest Signal** — 22-tree ensemble trained on live OHLCV data, outputs BUY / SELL / NEUTRAL with confidence score
- **Liquidity Zone Detection** — K-Means clustering + volume profile analysis to find high-volume price nodes
- **Support & Resistance** — Pivot-point detection with cluster deduplication
- **Bollinger Bands** — Dynamic upper/mid/lower zones overlaid on chart
- **Live Price Chart** — Candlestick SVG chart with S/R overlays and live price line
- **ML Analytics Dashboard** — Model accuracy, training time, feature importance bars
- **Activity Log** — Full pipeline event log with timestamps

---

## Project Structure

```
kryptx-redesign/
├── index.html          # App entry point
├── css/
│   └── styles.css      # Full design system (dark theme, CSS variables)
└── js/
    ├── app.js          # Main render loop + event binding
    ├── components.js   # UI component functions (SectionHeader, Tag, MiniMeter, etc.)
    ├── constants.js    # Color palette (C) and tab definitions (TABS)
    ├── state.js        # Global state + setState/addListener/addLog
    ├── pipeline.js     # ML pipeline orchestrator
    ├── ml-models.js    # Random Forest + feature engineering
    ├── indicators.js   # RSI, ATR, Bollinger Bands, K-Means, Volume Profile
    ├── api.js          # Binance REST API + mock data generator
    └── utils.js        # fmt, fmtPct, rollingMean, rollingStd
```

---

## Getting Started

No build step required. This is a pure vanilla JS (ES Modules) app.

### Run locally

```bash
# Using Python
python3 -m http.server 8080

# Using Node.js
npx serve .

# Using VS Code
# Install "Live Server" extension, right-click index.html → Open with Live Server
```

Then open `http://localhost:8080` in your browser.

> **Note:** The app requires a local server (not `file://`) because it uses ES Modules (`type="module"`).

---

## How It Works

### Data Flow

```
Binance API → OHLCV Candles → ML Pipeline → UI Render
                    ↓
           [Fallback: Mock Data]
```

### ML Pipeline (`pipeline.js`)

1. **Volume Profile** — bins price into 35 levels, flags high-volume nodes as liquidity zones
2. **Pivot Detection** — finds swing highs/lows with a 5-bar lookback, clusters nearby levels
3. **K-Means Clustering** — groups typical prices into 8 clusters (40 iterations) for ML zones
4. **Bollinger Bands** — 20-period SMA ± 2σ, generates upper/mid/lower zone levels
5. **Feature Engineering** — builds 9 features per candle: RSI, volume ratio, BB position, MA deviation (20/50), candle body ratio, 5-bar momentum, ATR ratio, wick ratio
6. **Random Forest** — fits 22 decision trees (max depth 5, 80% bootstrap) on labeled features, predicts BUY/SELL/NEUTRAL with confidence

### Tabs

| Tab | Contents |
|-----|----------|
| ML Zones | Price hero, Predict button, Signal banner, Liquidity zones |
| Analytics | Model accuracy, train time, feature importance |
| Chart | Candlestick chart with S/R overlays |
| S/R | Support & resistance level list |
| Log | ML pipeline activity log |

---

## Design System

Built with CSS custom properties. Key variables in `styles.css`:

| Variable | Value | Use |
|----------|-------|-----|
| `--bg` | `#080c12` | Page background |
| `--panel` | `#111827` | Cards & nav |
| `--accent` | `#00d4ff` | Primary accent (cyan) |
| `--green` | `#00e5a0` | BUY / positive |
| `--red` | `#ff3d6b` | SELL / negative |
| `--amber` | `#ffaa00` | NEUTRAL / warning |
| `--font-mono` | Space Mono | All numeric data |
| `--font-ui` | Syne | UI labels & headings |

---

## Browser Support

Works in all modern browsers that support ES Modules and CSS custom properties:
- Chrome 80+
- Firefox 80+
- Safari 14+
- Edge 80+

---

## License

MIT — free to use, modify, and distribute.
