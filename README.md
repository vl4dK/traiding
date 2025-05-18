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


## Автоматический поиск лучших ликвидных пар к USDC на Kraken для автоторговли
Kraken_Top_Liquid_USDC_Pairs.ipynb
Автоматический поиск лучших ликвидных пар к USDC на Kraken для автоторговли
Этот Jupyter Notebook автоматически анализирует все пары с USDC на бирже Kraken и отбирает три лучших по следующим критериям:

Минимальный спред (bid/ask) — ниже 0.2%

Высокий суточный объём — выше $100,000 за последние 24 часа

Реальная торговая активность — максимальный реальный объём сделок за последний час

Скрипт предназначен для:

Автоматического отбора только реально ликвидных и “живых” пар для торговых стратегий на Coinrule, в ботах, или для ручного трейдинга.

Снижения рисков работы на “мертвых” и манипулируемых рынках.

Регулярного мониторинга и адаптации списка торговых пар под реальную ликвидность Kraken.

Как использовать
Открой ноутбук в Jupyter Lab или Jupyter Notebook.

Запусти все ячейки.

На выходе получишь таблицу с тремя лучшими USDC-парами Kraken по итогам анализа (спред, объём, реальная активность).

Рекомендуется запускать скрипт регулярно (например, перед началом торгового дня или раз в несколько часов) для поддержания актуального списка ликвидных пар.

Пример вывода
diff
Копировать
Редактировать
=== Топ-3 пары для торговли по критериям ликвидности, спреда и реального оборота ===

    wsname      spread_%    volume_24h   trades_last_hour   real_volume_last_hour
0   XRP/USDC    0.00041     228253.21        126               56964.05
1   ADA/USDC    0.18116     3766546.00       101               45215.22
2   DOGE/USDC   0.16133     2004755.00       77                39112.67
Требования
Python 3.x

pandas

requests

Примечания
Скрипт рассчитан на реальное использование в автоторговых и аналитических стратегиях. Все параметры фильтрации (спред, объём, минимальное число сделок) можно легко изменить в коде.

Для максимальной эффективности рекомендуется использовать скрипт вместе с основным торговым алгоритмом или интегрировать его в пайплайн автоматизации.

Автоматический скрипт для отбора лучших торговых пар к USDC на Kraken по реальному объёму сделок в долларах (USDC).

Ищет и анализирует все USDC-пары на Kraken

Вычисляет реальный объём сделок за последний час в USDC

Сортирует пары по ликвидности и выдаёт топ-3

Учитывает также спред и суточный оборот для максимальной объективности

Скрипт предназначен для трейдеров и ботов, которым важна реальная ликвидность и низкие издержки на комиссию и проскальзывание.

Выбирай только действительно живые пары!