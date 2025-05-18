# tradring
разработка трейдинг стратегии с ChatGPT

# Kraken USDC Top 10 Dynamic Trend Entry

Jupyter Notebook for scanning all tradable USDC pairs on Kraken and finding coins that match a dynamic day trading strategy using EMA, RSI, Supertrend, and 15m volume spike. 

- Works out of the box in Jupyter Lab.
- No API keys required (public data only).
- All logic and usage documented in `scripts/USDC Top 10 Dynamic Trend Entry.ipynb`.

## Liquidity & Spread Scanner

See [`scripts/Kraken_Liquid_USDC_Pairs.ipynb`](scripts/Kraken_Liquid_USDC_Pairs.ipynb) — this notebook helps you to automatically scan all USDC trading pairs on Kraken and select only those with high liquidity and low spread (bid/ask difference).  
Ideal for updating your Coinrule basket and avoiding pairs with low activity or high slippage.

Just open in Jupyter, run all cells, and copy the resulting pairs!

Requirements: `pandas`, `requests`
