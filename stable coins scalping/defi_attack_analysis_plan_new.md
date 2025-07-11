# План проекта: Анализ атак (фронтран и сандвич) в DeFi-пулах

## Цель
Собрать статистику атак на DEX (front-run, sandwich), оценить риски и частоту вмешательств перед запуском собственной торговой стратегии.

## Этапы

- Выбор сетей и пулов для мониторинга (например, Arbitrum, Ethereum, определённые пулы USDC/ETH).
- Сбор on-chain истории транзакций:
  - Цены до и после сделок.
  - Slippage.
  - Газ (gas price, priority fee).
- Идентификация признаков атак:
  - Аномальные скачки газа.
  - Вставка транзакций между жертвой и исполнением (sandwich).
  - Необычные ценовые отклонения.
- Построение базы данных:
  - Время, пул, цена входа/выхода, отклонения.
  - Метки атак.
- Визуализация и статистика:
  - Частота атак по дням/часам.
  - Глубина влияния на цену.
  - Доля атакованных транзакций к общему числу.
- Модель вероятности атаки для дальнейших dryrun-тестов.
- Вывод о целесообразности запуска реальной стратегии.

## Принципы

- Нет реальных сделок на этом этапе (только сбор и анализ).
- Максимальная детализация данных.
- Приоритизация пулов с высокой ликвидностью и высокой активностью.

## Технологии

- Python, web3.py.
- База: SQLite или PostgreSQL.
- Визуализация: matplotlib или plotly.

## Оборудование

- Ubuntu-лептоп с 16 ГБ RAM, SSD.
- Собственный RPC (по возможности) или публичные RPC для анализа.

## Перспективы

- Использование модели вероятности атаки в стратегии dryrun.
- Последующая разработка боевой торговой стратегии только после понимания рисков.

# Конец плана