# PolymarketFarm Bot

> **EN** | [RU](#ru)

Automated liquidity farming bot for [Polymarket](https://polymarket.com) prediction markets.  
Places limit orders inside reward zones, earns LP rewards passively, and auto-sells after fills.

---

## ⬇️ Download

**[→ Download Latest Release (Windows EXE)](../../releases/latest)**

No Python installation required.

---

## How it works

- Scans all markets with active LP rewards on Polymarket
- Scores each market: `score = (daily_reward / zone_liquidity) × activity_factor`
- Places BUY limit orders inside the reward zone — rewards accumulate while the order sits there
- Monitors positions every 5 seconds — repositions on price drift, cancels on market degradation
- Automatically places a SELL order after a BUY is filled (configurable strategy)

---

## Quick Start

### 1. Download and extract

Download `PolymarketFarm-win64.zip` from the [Releases](../../releases/latest) page and extract anywhere.

### 2. Get your Polymarket API keys

1. Go to [polymarket.com](https://polymarket.com) and sign in
2. Navigate to **Profile → Settings → API Keys**
3. Create a new key — copy your `API Key`, `Private Key`, and wallet address

### 3. Create `.env` file

Create a file named `.env` in the same folder as `PolymarketFarm.exe`:

```env
WALLET_ADDRESS=0xYourWalletAddress
PRIVATE_KEY=0xYourPrivateKey
API_KEY=your-api-key
```

> ⚠️ **Never share your `.env` file** — it contains your wallet private key.

### 4. Run

Double-click `PolymarketFarm.exe` — the browser opens automatically at **http://127.0.0.1:8000**

---

## Settings

All parameters are changed live through the web UI — no restart needed.

| Setting | Description | Default |
|---------|-------------|---------|
| **Order depth** | Where to place inside reward zone: edge / mid / first | Mid |
| **% per slot** | Capital % per position | 25% |
| **Max slots per market** | Max positions per single market | 2 |
| **Min daily reward, $** | Minimum market daily reward to enter | 7.0 |
| **Scan interval, sec** | How often to search for new markets | 60 |
| **Volatility threshold** | Max price std over 24h (0.05 = 5%) | 0.05 |
| **Min spread, ¢** | Min reward zone width in cents | 2.0 |
| **Max OB spread, ¢** | Max bid-ask spread — liquidity filter | 2.0 |
| **Max daily trades** | Max price changes per day | 3 |
| **Max $ per order** | Fixed order size in USDC (0 = auto) | 0 |
| **Max positions** | Open position cap (0 = auto) | 0 |
| **Word blacklist** | Skip markets containing these words | — |
| **Token side** | Which token to buy: cheap (more shares → more rewards) or expensive | Expensive |
| **Min days to expiry** | Ignore markets closing sooner | 30 |
| **Max book depth, ¢** | Max spread between bid levels 1 and 4 | 4.0 |
| **Sell strategy** | After BUY fills: **at_cost** (post-only at buy price), **market** (immediate sell at best bid), **none** (manual) | at_cost |

---

## Strategy

The bot targets **"dead" markets** — where price barely moves. In such markets:
- Orders sit in the reward zone for a long time, accumulating rewards
- Low competition → larger share of rewards
- Minimal execution risk

Polymarket rewards are calculated by **shares**, not dollars:
```
Score = ((max_spread - order_spread) / max_spread)² × order_size_in_shares
```
Buying the **cheaper token** gives more shares per dollar → more rewards.

---

## Important

- The bot trades with **real money** — start with a small balance
- Set **Max $ per order** to control risk per position
- Use the **word blacklist** to exclude unwanted categories (e.g. `politics`, `russia`)
- When the bot stops, open orders remain on Polymarket — cancel them via the UI or manually

---

## Disclaimer

Use at your own risk. The bot does not guarantee profit. Prediction markets carry financial risk.

---

---

# RU

# PolymarketFarm Bot

Автоматический бот для фарминга LP-наград на [Polymarket](https://polymarket.com).  
Размещает лимитные ордера в reward zone, пассивно зарабатывает награды и автоматически продаёт после исполнения.

---

## ⬇️ Скачать

**[→ Скачать последний релиз (Windows EXE)](../../releases/latest)**

Python устанавливать не нужно.

---

## Как это работает

- Сканирует все рынки с активными LP-наградами на Polymarket
- Оценивает каждый рынок: `score = (daily_reward / zone_liquidity) × activity_factor`
- Размещает BUY лимитные ордера внутри reward zone — награды капают пока ордер там висит
- Каждые 5 секунд проверяет позиции — репозиционирует при дрейфе, отменяет при ухудшении рынка
- После исполнения BUY автоматически выставляет SELL

---

## Быстрый старт

### 1. Скачай и распакуй

Скачай `PolymarketFarm-win64.zip` со страницы [Releases](../../releases/latest) и распакуй в любую папку.

### 2. Получи API ключи Polymarket

1. Зайди на [polymarket.com](https://polymarket.com) и войди в аккаунт
2. Перейди в **Profile → Settings → API Keys**
3. Создай новый ключ — скопируй `API Key`, `Private Key` и адрес кошелька

### 3. Создай файл `.env`

Создай файл `.env` в той же папке, где лежит `PolymarketFarm.exe`:

```env
WALLET_ADDRESS=0xТвойАдресКошелька
PRIVATE_KEY=0xТвойПриватныйКлюч
API_KEY=твой-api-ключ
```

> ⚠️ **Никогда не публикуй `.env` файл** — в нём приватный ключ от кошелька.

### 4. Запуск

Дважды кликни `PolymarketFarm.exe` — браузер откроется автоматически на **http://127.0.0.1:8000**

---

## Настройки

Все параметры меняются через веб-интерфейс без перезапуска бота.

| Параметр | Описание | По умолчанию |
|----------|----------|-------------|
| **Глубина ордера** | Куда ставить внутри reward zone: край / середина / первая позиция | Середина |
| **% на позицию** | Процент от капитала на одну позицию | 25% |
| **Макс. слотов на рынок** | Максимум позиций на один рынок | 2 |
| **Мин. дневная награда, $** | Минимальная дневная награда рынка для входа | 7.0 |
| **Интервал сканирования, сек** | Как часто искать новые рынки | 60 |
| **Порог волатильности** | Макс. std цены за 24ч (0.05 = 5%) | 0.05 |
| **Мин. спред, ¢** | Мин. ширина reward zone в центах | 2.0 |
| **Макс. спред стакана, ¢** | Макс. bid-ask спред — фильтр ликвидности | 2.0 |
| **Макс. сделок в день** | Макс. изменений цены за день | 3 |
| **Макс. $ на ордер** | Фиксированный размер ордера в USDC (0 = авто) | 0 |
| **Макс. позиций** | Лимит открытых позиций (0 = авто) | 0 |
| **Фильтр слов** | Пропускать рынки с этими словами в названии | — |
| **Сторона покупки** | Дешёвый токен (больше шейрсов → больше наград) или дорогой | Дорогой |
| **Мин. дней до закрытия** | Игнорировать рынки с меньшим сроком | 30 |
| **Макс. глубина стакана, ¢** | Макс. разница между 1-м и 4-м уровнем bid | 4.0 |
| **Стратегия продажи** | После исполнения BUY: **по цене покупки** (безубыток), **по рынку** (немедленно), **не продавать** (вручную) | По цене покупки |

---

## Стратегия

Бот ищет **"мёртвые" рынки** — где цена почти не двигается:
- Ордер долго висит в reward zone и накапливает награды
- Низкая конкуренция → большая доля наград достаётся нам
- Минимальный риск исполнения ордера

Награды считаются по **шейрсам**, а не по долларам:
```
Score = ((max_spread - order_spread) / max_spread)² × order_size_in_shares
```
Покупка **дешёвого токена** даёт больше шейрсов за доллар → больше наград.

---

## Важно

- Бот работает с **реальными деньгами** — начни с небольшой суммы
- Установи **Макс. $ на ордер** чтобы контролировать риск на позицию
- Используй **Фильтр слов** чтобы исключить нежелательные категории (например `politics`, `russia`)
- При остановке бота открытые ордера остаются на Polymarket — закрой их через UI или вручную

---

## Дисклеймер

Используй на свой страх и риск. Бот не гарантирует прибыль.
