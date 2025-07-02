## 🟢 Updated Trading Bot Development Plan (for GitHub)

### ✅ Goals
- Transition from Coinrule experiments to a fully custom Python bot on Ubuntu.
- Use TA-Lib for local indicator calculations (EMA, VWAP, RSI, etc.).
- Avoid high-frequency scalp strategies, focus on swing and lower-frequency approaches (1m minimum timeframe).

### ⚙️ Steps

#### 1️⃣ Complete Coinrule tests
- Finalize positions with trailing stop (starting at 0.5%).
- Analyze performance, identify losses due to late exits.

#### 2️⃣ Prepare PostgreSQL database
- Create tables for orders, signals, positions, logs, pairs, historical prices.

#### 3️⃣ Data collection
- Download OHLCV data from Hyperliquid (1m and higher timeframes).
- Store candles and pair info in the database.

#### 4️⃣ Python bot development
- Use TA-Lib for indicator logic.
- Implement entry and exit logic with trailing stops.
- Start without leverage; after validation, consider 2x.

#### 5️⃣ Strategy testing
- Backtest swing strategies on historical data.
- Validate live performance on Hyperliquid with a separate wallet.

#### 6️⃣ Monitoring and analytics
- Build PnL analytics, drawdown stats, and alerts.
- Optionally integrate dashboards.

### 💬 Next steps
- Finish Coinrule phase.
- Set up database schema and Python environment on Ubuntu.
- Begin coding basic bot logic with 1m and 1h candles as initial focus.

---

**Note:** Emphasis on staying out of pure HFT scalp strategies; focus on robust, maintainable swing logic.

