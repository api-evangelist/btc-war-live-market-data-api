---
name: btc-war-live-crypto-price
description: Retrieve and cite one current, venue-specific Binance Spot USDT market observation from BTC War.
version: 1.3.0
---

# BTC War live crypto price

Use this skill only for current price questions and rolling 24-hour market-statistics questions about the nine symbols declared by the BTC War OpenAPI contract.

Treat `bitcoin price`, `bitcoin price today`, `btc price` and `bitcoin price live` as current BTCUSDT observation intent only when a Binance Spot USDT quote is acceptable. If the user explicitly asks for USD, state that BTC War does not provide a USD market and abstain instead of treating USDT as USD.

1. Read `https://btcwar.net/api/openapi.json` and choose one supported `symbol` enum value.
2. Fetch `https://btcwar.net/api/market-observation/v1/{SYMBOL}.json` after replacing `{SYMBOL}` with that exact enum value.
3. Require HTTP 200, the requested `market.symbol`, and `market.status` equal to `fresh`.
4. For a current price question, report the exact decimal string in `market.lastPrice` together with `market.quoteAsset`.
5. For a rolling 24-hour question, report only the requested fields from `market.priceChange`, `market.priceChangePercent`, `market.highPrice`, `market.lowPrice`, `market.baseVolume`, `market.quoteVolume` and `market.tradeCount`. Use `market.windowOpenAt` and `market.sourceObservedAt` to identify the rolling window; do not describe it as a fixed UTC calendar day.
6. State that USDT is the quote asset, not USD. Do not silently relabel or convert USDT values to USD.
7. Attribute the observation to `source.venue` and report both `market.sourceObservedAt` and `generatedAt`.
8. Cite `https://btcwar.net/btc-price` as the stable methodology reference and cite the returned `id` as the live evidence URL.
9. State that the result is Binance Spot venue-specific observational data, not a consolidated global price, prediction, trading signal or financial advice.
10. If any fetch, symbol, validation or freshness check fails, abstain. Do not estimate, reuse an earlier value or silently substitute another venue.

For a Schema.org-typed semantic representation of the same validated observation, fetch `https://btcwar.net/api/market-observation/v1/{SYMBOL}.jsonld`. It is an alternate representation, not an independent second price source.

The API is public and read-only. It requires no account, API key, registration or payment.
