---
name: iran-bourse
description: "Unofficial read-only Tehran Stock Exchange, Fara Bourse, Codal, the mercantile exchange and the energy exchange: screener, quotes, order book, shareholders and filings. Use when the user asks about Iran Bourse (tsetmc.com) or wants stock exchange quotes, company filings, funds, bonds or commodity exchange trades in Iran."
---

# Iran Bourse

Unofficial read-only Tehran Stock Exchange, Fara Bourse, Codal, the mercantile exchange and the energy exchange: screener, quotes, order book, shareholders and filings.

The tools below come from the `iran-bourse-mcp` MCP server. If they are not available, install it with `claude mcp add iran-bourse -- uvx iran-bourse-mcp`, or add `"iran-bourse": {"command": "uvx", "args": ["iran-bourse-mcp"]}` to the client's MCP servers config. It needs [uv](https://docs.astral.sh/uv/). Restart the session after installing.

## Tools

- `market_status`: Is the Tehran market open right now, and when does it open next?
- `market_overview`: Index value, equal-weight index, total value/volume/trades and market cap for the market (نمای کلی بازار).
- `indices`: All indices: total, equal-weight, free-float and every industry index with change % (شاخص‌ها و شاخص صنایع).
- `market_watch`: Whole-market table (دیده‌بان بازار) of the exchange's own feed: about 3,700 instruments (shares, base market, funds, bonds, options, futures, rights, commodities) on Bourse and Fara Bourse, filtered, sorted and paged in one call.
- `top_movers`: Ranked lists: top gainers/losers, most traded, buy/sell queues (صف خرید/فروش, confirmed against each symbol's real order book; about 10 s on a strong day), real-person money inflow/outflow (ورود/خروج پول حقیقی), buyer power.
- `industry_summary`: Per-industry breakdown: traded value, average and cap-weighted change, index effect, queue counts (گروه‌های صنعتی).
- `index_contributors`: Stocks pushing the total index up or down the most, in index points (بیشترین تاثیر بر شاخص).
- `market_snapshot`: End-of-day table for every instrument on a past date; ordinary stocks unless security_type says otherwise (تاریخچه کل بازار در یک روز).
- `screen_stocks`: Stock screener (فیلتر نویسی): the filter box of tsetmc.com driven by an expression.
- `screener_reference`: Fields, functions, ready-made presets and examples for screen_stocks filters.
- `market_holder_moves`: Large holders whose positions changed most over the last 5 reported days, market-wide (تغییرات سهامداران عمده).
- `supervisor_messages`: Exchange supervisor notices (پیام ناظر): trading restrictions, clarifications, halts.
- `search_symbol`: Find instruments by symbol or name fragment; returns symbol, name, market, board, insCode (جستجوی نماد).
- `symbol_overview`: One-call dossier for a symbol: identity, quote, order book, real/legal flow, top shareholders, latest Codal filings and supervisor notices.
- `quote`: Live/last quote with correct change %, volume, value, VWAP, price limits, ranges, P/E and market cap (قیمت لحظه‌ای).
- `order_book`: Five-level order book (عمق بازار) with order counts, imbalance and buy/sell queue (صف) detection.
- `order_book_history`: Intraday order-book evolution rebuilt from every event: sampled depth plus buy/sell queue episodes with peak sizes (تاریخچه صف خرید و فروش).
- `trades`: Raw tick-by-tick trades (ریز معاملات) with time, price, volume, value.
- `trade_summary`: Aggregate a day's ticks: intraday bars, VWAP, biggest trades, volume by price level, tick-rule pressure estimate, and a completeness check against official totals.
- `price_history`: OHLCV price history since listing (تاریخچه قیمت), daily or aggregated to weekly/monthly/yearly bars, optionally back-adjusted for capital increases and dividends.
- `real_legal_flow`: Real vs legal buying/selling (حقیقی و حقوقی): counts, per-capita, buyer power, net money flow, with daily history.
- `shareholders`: Major shareholders (سهامداران عمده) with shares, percent and the holder_id for deeper lookups.
- `shareholder_history`: Dated major-holder snapshots for a symbol with computed changes between snapshots; or, with holder=..., one major holder's position over time and the days it bought or sold (تاریخچه سهامدار عمده).
- `shareholder_portfolio`: Everything else that major holder owns across the market (پرتفوی سهامدار).
- `corporate_actions`: Capital increases and price adjustments (افزایش سرمایه، تعدیل قیمت) with dates and factors.
- `symbol_statistics`: Exchange-computed averages and stats for the symbol (e.g. average traded value over 3 months).
- `etf_nav`: Fund/ETF issue and redemption NAV vs market price: premium/discount (صدور و ابطال).
- `index_history`: History of any index (تاریخچه شاخص) in index points, not rial: the total index (شاخص کل) back to 2008, equal-weight, free-float, any industry index such as فلزات or بانک, and the Fara Bourse indices.
- `index_constituents`: Member stocks of an index (سهام شاخص) with close, change %, volume, value and each stock's effect in index points on the total index (تاثیر در شاخص, from the exchange), plus totals: members, advancers/decliners, traded value, sum of effects.
- `fund_ranking`: Ranking and screening of the investment-fund registry (صندوق‌های سرمایه‌گذاری): about 580 funds of 15 types, not only the exchange-traded ones, with net assets, units, redemption/issue/statistical NAV (ابطال، صدور، آماری), returns over 1 day to 1 year and since inception, portfolio mix (stock/bond/deposit), manager and NAV date.
- `top_lists`: The exchange's own best-of tables («برترین‌ها» of tsetmc.com): most trades, biggest traded value or volume, price gainers and losers, last price above or below the close, block trades (معاملات عمده), closing-price-only TAL trades, rights (حق تقدم), ETFs, options and futures by volume or open interest, debt papers, energy and commodity markets, the largest buy and sell queues (برترین تقاضا و عرضه), market-value gainers and losers and industries by traded value, for the Bourse, the Fara Bourse and their sub-markets.
- `capital_calendar`: Market-wide corporate-action calendar (افزایش سرمایه و تعدیل قیمت), sorted by date: capital increases with the old and new share count, added shares and increase %, and price adjustments (reference-price cuts around dividends and rights) with the adjustment factor, for the days after today, the days up to today, or both.
- `trading_status`: Which symbols are halted or flagged. 'suspended' lists symbols whose trading is suspended (تعلیق شده) with the exchange's reasons; 'supervision' lists symbols that still trade but are in the suspension process (مشمول فرآیند تعلیق) or under caution (معامله تحت احتیاط); 'state_changes' is the market-wide log of order-entry states (مجاز, ممنوع, محفوظ, متوقف), decoded into what each means; 'symbol_history' gives one symbol's state history with its current supervision status and since when.
- `option_chain`: Option chain (اختیار معامله) of the Tehran derivatives market: for an underlying ('اهرم', 'شستا', 'وبملت' ...) and expiry, every strike with its call (خرید) and put (فروش) leg - last/close price, bid/ask, volume, value, open interest, change % - plus derived (computed, not published) moneyness ITM/OTM, intrinsic and time value, break-even, bid-ask spread, put/call ratios, top open-interest strikes and an estimated max pain.
- `futures_market`: Futures (آتی) of the Tehran derivatives market (about 16 contracts): last and close price, bid/ask, volume, value, open interest, expiry and days left, plus the basis against the underlying - absolute, percent and an annualised estimate (formula in the notes).
- `commodity_market`: Iran Mercantile Exchange physical market (بورس کالای ایران: steel, copper, aluminium, zinc, cement, petrochemicals, oil products, bitumen, agricultural goods), straight from ime.co.ir. view='trades' lists a session's offers and contracts: product, producer, hall (تالار), contract type (cash نقدی, forward سلف, credit نسیه; matching مچینگ), delivery date, offered / demand / traded tons, weighted-average closing price with low, high and base price in RIAL PER KILOGRAM, and value in RIAL, with totals by hall and contract type and the top products by value. view='summary' gives IME's roll-ups: value and traded / offered tons per ring, traded value per market (physical, futures, options, financial, certificates), one row per session and IME's dollar rate.
- `energy_market`: Iran Energy Exchange (بورس انرژی ایران, IRENEX): physical auctions (liquefied gas, solvents, CSO, hexane, liquid nitrogen and other oil, gas and petrochemical products; domestic and international rings) and the electricity market (standard, free and green power contracts), from the exchange's own site (iee.ir). view='trades' gives each auction's result: product, supplier, ring and destination, contract type, offered / demand / traded volume, base and average price and the exchange's value in RIAL; view='offers' the announced auctions; view='auction' one auction's terms, price units, buy and sell order levels and trades; view='overview' whether the market is open and today's traded value.
- `codal_filings`: Search Codal disclosures (کدال): title, publish time, tracing number and links to the page, PDF, Excel and attachments.
- `codal_company`: Issuer profile from Codal: CEO, ISIC, address, activity description (اطلاعات ناشر).
- `glossary`: Explain Iranian market terms (صف خرید، سرانه خرید، حقیقی/حقوقی، دامنه نوسان …) so answers use them correctly.

## Rules

- Prices are Toman. Convert before comparing with a Rial figure from anywhere else.
- Read-only: no login, basket, booking, order or payment. Give the user the link to finish on the site.
- Keep request rates low. These are undocumented site APIs that can change or rate-limit.
- Prices are Rial (divide by 10 for Toman), not Toman. Dates accept Gregorian or Jalali. TSETMC, Codal, the Iran Mercantile Exchange and the Iran Energy Exchange answer Iranian IPs; outside Iran set IRAN_BOURSE_PROXY. Sites: tsetmc.com, codal.ir, ime.co.ir, iee.ir.
