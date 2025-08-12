# ML Trading Bot (1H/4H, Daily Adaptive)

**Ежедневно адаптирующийся к рынку Python-бот** с walk-forward переобучением, управлением риском и реалистичным исполнением (учёт комиссий и проскальзывания).  
Временная зона: **Europe/Amsterdam**.

---

## ✨ Основные возможности
- **Мульти-таймфрейм:** 1H и 4H, синхронизация по закрытой свече.
- **Ежедневное переобучение:** LightGBM/XGBoost (опция — River для онлайн-дообучения).
- **Triple-barrier / forward-return разметка** и meta-labeling.
- **Риск-менеджмент:** волатильностное таргетирование, лимиты просадки, ATR-стопы.
- **Реалистичный бэктест:** комиссии, проскальзывание, purged walk-forward.
- **Поддержка бирж:** ccxt (CEX), Hyperliquid SDK, DEX-агрегаторы.
- **Оркестрация:** Prefect/Airflow, учёт DST и праздников.
- **Мониторинг:** Prometheus/Grafana, алерты в Telegram.
- **GUI + API** для управления и мониторинга.

---

## 🛠 Техстек
Python 3.11 • Poetry/uv • Docker  
pandas / Polars • numpy • ta • lightgbm / xgboost • scikit-learn  
mlflow • vectorbt • Prefect • ccxt • pydantic • DVC (опц.)

---

## 📂 Архитектура
```
.
├─ README.md
├─ ROADMAP.md
├─ config.yaml
├─ .env.example
├─ pyproject.toml
├─ docker-compose.yml
├─ src/
│  ├─ data/            # загрузка, валидация, parquet-хранилище
│  ├─ features/        # генерация признаков, агрегация TF
│  ├─ labeling/        # triple-barrier, forward returns, meta-labeling
│  ├─ models/          # обучение, прогноз, калибровка
│  ├─ policy/          # риск-менеджмент, размер позиции
│  ├─ exec/            # исполнение ордеров, брокеры
│  ├─ backtest/        # движок бэктеста
│  ├─ orchestration/   # DAG'и, сценарии
│  ├─ monitoring/      # метрики, алерты
│  └─ utils/           # утилиты, схемы
├─ notebooks/          # исследование и отчёты
└─ mlruns/             # MLflow (локально)
```

---

## 🚀 Установка
```bash
# Python 3.11
python3.11 -m venv .venv
source .venv/bin/activate
pip install --upgrade pip
pip install -r requirements.txt
```

---

## ⚙️ Конфигурация окружения
Скопируйте `.env.example` → `.env` и заполните:
```
EXCHANGE_ID=binance
API_KEY=your_api_key
API_SECRET=your_api_secret
MLFLOW_TRACKING_URI=./mlruns
TELEGRAM_BOT_TOKEN=xxx
TELEGRAM_CHAT_ID=xxx
TZ=Europe/Amsterdam
```

---

## 🖥 API
**Запуск:**
```bash
uvicorn api.main:app --reload --port 8000
```

**Эндпоинты:**
- `GET /status` — состояние бота
- `POST /start {"paper": true|false}` — запуск
- `POST /stop` — остановка
- `GET /positions` — позиции
- `GET /orders` — ордера
- `GET /trades` — сделки
- `GET /metrics` — метрики (Sharpe, ASR и др.)

---

## 📊 GUI
**Запуск:**
```bash
streamlit run ui/dashboard.py
```
По умолчанию GUI обращается к `http://localhost:8000`.  
Если API на другом порту:
```bash
BOT_API_URL=http://localhost:8001 streamlit run ui/dashboard.py
```

---

## 🔄 Ежедневный цикл (Europe/Amsterdam)
- 00:05–00:20 — загрузка и валидация данных  
- 00:20–01:10 — генерация фич, обучение на скользящем окне  
- 01:10–01:30 — sanity backtest, обновление порогов  
- 01:30–01:40 — деплой артефактов  
- Каждый час / каждые 4 часа — принятие решений и исполнение

---

## 🛡 Безопасность
- Ключи — только в `.env` или Vault. Не коммитьте секреты в Git.
- Fail-safe: закрыть или хеджировать позиции при сбое (планируется в M4).
- Полный аудит-трейл: сигнал → решение → ордер → исполнение → PnL.

---

## 📌 Статус проекта
- ✅ Работает: GUI (Streamlit), API (FastAPI), демо-движок решений, модули impact/toxicity.
- 🟨 В разработке: Prefect/Airflow DAG’и, подключение Hyperliquid SDK, прод-бэктест.
- 📅 Roadmap: см. `ROADMAP.md`.

---

## 📝 Лицензия
MIT (по умолчанию) или своя.
