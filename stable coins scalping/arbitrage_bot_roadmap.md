**Project: Autonomous DEX Arbitrage Bot (Dry-Run Phase)**

**Objective:**
Develop an autonomous Python-based arbitrage bot for decentralized exchanges (DEX) operating in dry-run mode (no real transactions). Target L2 networks for low fees, focusing on profitability, latency, and execution logic prior to deployment.

**Key Design Principles:**
- No fiat interaction. All trades are crypto-to-crypto.
- L2-native deployment (Arbitrum, Optimism, Base, zkSync).
- Real-time price collection from multiple DEX aggregators (1inch, OpenOcean, CowSwap).
- Dry-run only: simulate arbitrage routes, calculate potential profit, and log decisions without executing trades.
- Operation restricted to working hours (e.g., 08:00–18:00 UTC).

**System Components:**
- `core.py`: main loop with data fetching, route detection, and dry-run execution.
- `dex_clients/`: wrappers for aggregator APIs and on-chain DEXs.
- `profit_calculator.py`: considers gas, slippage, and token prices.
- `tx_simulator.py`: mimics trade execution without signing.
- `logger.py`: structured logging to file/CSV/SQLite.
- `config.json`: routing rules, token pairs, minimum profit thresholds.

**Execution Environment:**
- Ubuntu x86_64 with Python 3.10+
- Compatible with GNOME Terminal (Textual interface)
- Uses `web3.py`, `requests`, `pandas`, `apscheduler`, `textual`

**User Interface (Control Panel):**
- Implemented via Textual (TUI)
- Panels:
  - Active Arbitrage Opportunities
  - Latest Logs and Errors
  - Gas/Slippage Summary
  - Time Restriction Status

**Future Phases:**
- Add real trade execution module (with Flashbots for frontrun protection)
- Integrate gas estimation
- Transition to continuous 24/7 operation
- Optional web interface via Streamlit for analytics

**Deployment Notes:**
- Local testing via Jupyter Notebook for data pipeline validation
- Systemd or nohup for daemonization
- Containerization via Docker (optional)

**Status:**
Dry-run phase to begin next week. Core architecture outlined, initial logic to be implemented in `core.py` with dummy aggregator inputs and basic opportunity detection.

**License:**
To be defined at release stage.

