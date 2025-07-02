## 🟢 Trading Bot Development Plan (for GitHub)

### ✅ Goals
- Analyze real trading results on Hyperliquid and Kraken to understand true net profitability (after fees, spread, funding).
- Compare spot vs futures trading profitability and risk profile.
- Move from rule-based external bots (e.g., Coinrule) to a custom Python bot with full control.

### ⚙️ Steps

#### 1️⃣ Complete current Coinrule tests
- Close 3 current positions.
- Collect final PnL and compare theoretical vs actual returns.
- Identify how much potential profit is lost to fees, funding, or late exit signals.

#### 2️⃣ Analyze spot vs futures
- Download historical OHLCV data for trading pairs.
- Calculate volatility, liquidity, and funding cost for futures.
- Evaluate same strategies on spot without leverage.
- Decide preferred market (spot or futures) for each strategy (swing, scalp).

#### 3️⃣ Database design (PostgreSQL)
- Orders table: store executed trades and performance.
- Signals table: store strategy signals and parameters.
- Positions table: track open positions.
- Logs table: log bot events and errors.
- Pairs table: info on each pair (spot/futures, fees, min size).
- Historical_prices table: store candles and volume data.

#### 4️⃣ Python bot development
- Fetch market data (candles, orderbook if needed).
- Compute indicators (EMA, VWAP, RSI, etc.).
- Generate entry/exit signals.
- Send orders to exchange via API (Hyperliquid, Kraken spot).
- Record all actions in the database.

#### 5️⃣ Strategy refinement
- Test trailing stop logic (e.g., activate after +1%, trail at -0.5%).
- Optimize conditions for swing or scalp.
- Reduce slippage and unnecessary fees.

#### 6️⃣ Monitoring & visualization
- Build summary analytics (PnL charts, win rate, average % per trade).
- Optionally integrate with Telegram or email for alerts.
- Consider dashboard (e.g., using Metabase or Grafana).

### 💬 Next steps
- Finalize current positions.
- Prepare PostgreSQL schema and initial scripts.
- Define and backtest first basic strategy on historical data.

---

**Note:** After successful tests, full transition to custom bot with autonomous live trading on selected market(s).

