
# Adaptive Crypto Market States and Strategy Design

This document provides a practical framework for understanding different crypto market states from a trader’s perspective, and outlines suitable trading strategies—manual or machine learning-based—for each condition.

## Market States and Their Characteristics

### 1. Mirror Market
- **Behavior**: Frequent false breakouts, V-shaped reversals.
- **Symptoms**: Long wicks on both sides, frequent stop hunts.
- **Strategy**: Reversal scalping, tight SL/TP, low exposure.

### 2. Dust Market
- **Behavior**: Flat movement, very low volume.
- **Symptoms**: Tight range, stagnant VWAP.
- **Strategy**: Mean reversion scalping, or no trading at all.

### 3. Impatient Market
- **Behavior**: Fast, one-directional move (usually on news).
- **Symptoms**: Momentum candles, little to no pullbacks.
- **Strategy**: Breakout strategies with trailing stops and fast execution.

### 4. Anchor Market
- **Behavior**: Price keeps reverting to a central level (e.g., VWAP).
- **Symptoms**: Ping-pong movement around a mean.
- **Strategy**: VWAP-based reversion bots or EMA bounce setups.

### 5. Overheat Market
- **Behavior**: Explosive rise or fall in price with large volume.
- **Symptoms**: Wide candles, large volume spikes, high ATR.
- **Strategy**: Probabilistic breakout + strict trailing SL logic.

### 6. Snake Market
- **Behavior**: Constant direction changes within a tight band.
- **Symptoms**: Low ATR, no trend.
- **Strategy**: Avoid trend-following, deploy scalping models or abstain.

### 7. Bubble-Foam Market
- **Behavior**: Price overextends away from fundamentals or mean.
- **Symptoms**: Parabolic moves, weak volume support.
- **Strategy**: Combine with social sentiment, limit exposure, fade climax.

### 8. Void Market
- **Behavior**: Liquidity vanishes, spreads widen, price jumps erratically.
- **Symptoms**: Thin order book, sudden gaps.
- **Strategy**: Minimize position size, defensive execution, raise timeframes.

---

## Strategy Routing Framework (ML-Aware)

1. **State Detection**
   - Tools: XGBoost, LSTM, HMMs
   - Features: VWAP/EMA distance, volatility, volume spikes, candle shape, OB imbalance

2. **Model Routing**
   - Each market state has a dedicated model or rule-based strategy.
   - Examples:
     - "Overheat": Momentum breakout (GBM + delta volume)
     - "Anchor": VWAP regression or Bayesian reversion
     - "Snake": Unsupervised clustering + band scalping

3. **Adaptive Execution**
   - Live performance metrics determine future strategy weighting.
   - Self-tuning system (meta-layer) monitors switching logic.

---

Мониторинг и визуализация
- Подключить web-дэшборд (Flask, Streamlit) для мониторинга состояния, логов, отчётов.
- Реализовать Telegram/Email-уведомления для контроля сигналов и ошибок.

Рекомендуемый стек для реализации:
- Язык: Python 
- Инфраструктура: Jupyter/Colab для ресерча, SQLite/PostgreSQL для хранения данных
- Источники данных: Kraken API (Ticker, OHLC, Trades, Depth)
- Логика: модульность, возможность быстрой смены и тестирования гипотез

## Author
Developed as part of a broader initiative to build regime-aware crypto trading bots using probabilistic logic and ML routing on real-time Kraken data.
