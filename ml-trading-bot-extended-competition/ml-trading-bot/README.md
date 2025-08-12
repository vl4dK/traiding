# ML Trading Bot (1H/4H, daily adaptive)

Ежедневно адаптирующийся к рынку Python-бот на 1H/4H с walk-forward переобучением, управлением риском и реалистичным исполнением (комиссии/проскальзывание). Временная зона: Europe/Amsterdam.

## Ключевые возможности
- Мульти-таймфрейм 1H/4H, синхронизация по закрытой свече
- Ежедневное переобучение моделей (LightGBM/XGBoost; опц. River для онлайн-дообучения)
- Triple-barrier/forward-return разметка и мета-лейблинг
- Волатильностное таргетирование, лимиты просадки, ATR-стопы
- Бэктест с издержками и purged walk-forward
- Коннекторы: ccxt (CEX), опц. Hyperliquid SDK / DEX-агрегаторы
- Оркестрация: Prefect/Airflow; учёт DST/праздников
- Логи и мониторинг: Prometheus/Grafana, алерты в Telegram

## Техстек
Python 3.11, Poetry/uv, Docker, pandas/Polars, numpy, ta, lightgbm/xgboost, scikit-learn, mlflow, vectorbt, Prefect, ccxt, pydantic, DVC (опц.).

## Архитектура
- **data/**: загрузка OHLCV/метаданных → parquet + каталоги по инструментам
- **features/**: генерация признаков 1H + агрегирование 4H, нормализация, хранение версий
- **labeling/**: triple-barrier или k-bar forward returns (с комиссиями)
- **models/**: обучение/калибровка, сохранение артефактов (sklearn/LightGBM), MLflow
- **policy/**: позиционирование и риск-менеджмент
- **exec/**: исполнение ордеров, менеджер заявок, ретраи, идемпотентность
- **orchestration/**: DAG ежедневного переобучения и часовых решений
- **monitoring/**: метрики PnL/рисков/дрейфа, алерты

## Дерево репозитория
```
.
├─ README.md
├─ ROADMAP.md
├─ config.yaml
├─ .env.example
├─ pyproject.toml
├─ docker-compose.yml
├─ src/
│  ├─ data/               # загрузчики, валидаторы, хранилище parquet
│  ├─ features/           # генераторы фич, мульти-TF агрегации
│  ├─ labeling/           # triple-barrier, forward returns, meta-labeling
│  ├─ models/             # train.py, predict.py, calibration, drift checks
│  ├─ policy/             # position sizing, risk, stop/ttl
│  ├─ exec/               # brokers (ccxt, hyperliquid), order router
│  ├─ backtest/           # engine, slippage/fees, walk-forward
│  ├─ orchestration/      # Prefect flows / Airflow dags
│  ├─ monitoring/         # exporters, alerting
│  └─ utils/              # общие утилиты, таймзона, pydantic-схемы
├─ notebooks/             # исследование и отчёты
└─ mlruns/                # MLflow (локально)
```

## Быстрый старт
1) Установи зависимости:
```
# через uv или Poetry
uv pip install -r requirements.txt || true
poetry install || true
```
2) Заполни `.env` на основе `.env.example`, проверь `config.yaml`.
3) Инициализируй хранилище данных:
```
python -m src.data.bootstrap --config config.yaml
```
4) Запусти ежедневное переобучение и принятие решений (Prefect):
```
python -m src.orchestration.run --flow daily_retrain
python -m src.orchestration.run --flow live_decisions
```
5) Мониторинг (опционально):
```
docker compose up -d prometheus grafana
```

## Конфигурация
- **Биржа**: через `ccxt` (по умолчанию). Опционально Hyperliquid/DEX.
- **Универсум**: 10–50 ликвидных инструментов, задаётся в `config.yaml`.
- **Издержки**: комиссии/спред/скольжение в backtest и live.
- **Режимы рынка**: опционально кластеризация (k-means/HDBSCAN) для контекстных фичей.

## Признаки (примеры)
- Базовые: лог-ренды, волатильность (Parkinson/RS), ATR, скос/эксцесс
- Тренд/моментум: EMA/SMA дифф/кроссы, RSI, Stoch, MACD, Donchian, ADX
- Объём: OBV, вола объёма; для перпетов — funding/базис (если доступно)
- Мульти-TF: 1H фичи + 4H агрегаты (mean/max/min/std/last/slope)

## Разметка таргетов
- **Forward return**: sign(return_{t→t+k}), 1H: k=4, 4H: k=2, net of fees
- **Triple-barrier**: по Lopez de Prado (верх/низ в N*ATR, TTL)
- **Meta-labeling**: базовый сигнал → метамодель «брать/не брать»

## Модели
- LightGBM/XGBoost + калибровка вероятностей (Platt/Isotonic)
- Ансамбли по режимам рынка
- Ежедневный re-fit на скользящем окне N месяцев; опц. partial_fit/River

## Валидация
- Purged walk-forward split + embargo
- Метрики: CAGR, Sharpe/Sortino, Calmar, MDD, turnover, PBO
- Робастность: стресс-тест издержек/задержек/дрейфа

## Управление риском
- Vol targeting: size ~ target_vol / realized_vol
- Лимиты: дневная просадка, макс. плечо/позиция, ATR-SL, time-stop
- Портфель: risk parity/корреляции (обновление ежедневно/нед.)

## Исполнение
- Лимитные post-only где возможно; fallback в market
- Проверки: маржа, шаги цены/лота, режимы биржи
- Rate-limits, ретраи, идемпотентность внешних order-id

## Ежедневный цикл (Europe/Amsterdam)
- 00:05–00:20 — догрузка и валидация данных
- 00:20–01:10 — фичи/режимы/обучение на скользящем окне
- 01:10–01:30 — sanity-backtest и обновление порогов
- 01:30–01:40 — деплой артефактов (модели/скалеры/пороги)
- Каждый час и каждые 4 часа — принятие решений и исполнение

## Безопасность
- Ключи только из ENV/Vault, read-only логи
- Fail-safe: закрыть/хеджировать позиции при сбое и уведомить
- Полный аудит-трейл: сигнал → решение → ордер → исполнение → PnL

## Лицензия
MIT (по умолчанию) или укажи свою.


## GUI и API

В проект добавлены:
- **API** на FastAPI (`api/main.py`) для управления ботом (start/stop, статус, позиции, сделки).
- **GUI** на Streamlit (`ui/dashboard.py`) для мониторинга PnL/сделок и управления ботом.

Запуск локально:
```bash
# 1) API (в отдельном терминале)
uvicorn api.main:app --reload --port 8000

# 2) GUI (в другом терминале)
streamlit run ui/dashboard.py
```
По умолчанию GUI обращается к `http://localhost:8000`.
## Installation

```bash
# Python 3.11
python3.11 -m venv .venv
source .venv/bin/activate
pip install --upgrade pip
pip install -r requirements.txt
