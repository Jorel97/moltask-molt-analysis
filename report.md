# MOLT Token Deep Dive: On-Chain And DEX Analysis

Token: MOLT / Moltbook
Contract: `0xB695559b26BB2c9703ef1935c37AeaE9526bab07`
Chain: Base
Data checked: `2026-05-30T01:36:25Z`

## Executive Summary

MOLT is actively traded on Base with public liquidity visible through DexScreener and GeckoTerminal. The live token snapshot used for this report shows a price near $0.00001473, a fully diluted valuation around $1,473,269, and GeckoTerminal-reported aggregate pool reserves around $498,067. DexScreener exposes multiple Base pairs, with the largest visible MOLT/WETH pool carrying the strongest liquidity and therefore the most useful reference price.

The market picture is mixed. Liquidity is meaningful for a small token, but transaction pressure in the primary pair is sell-heavy over the 24 hour window. This does not automatically make the token bearish, because thin tokens can show noisy short-term flows, but it does mean agents should avoid assuming that a quoted price is executable at size. Any autonomous task that converts MOLT to another asset should use live routing quotes, exact minimum output checks, and small trade sizing.

## Part 1: On-Chain Metrics

The public RPC call and GeckoTerminal metadata agree that MOLT uses 18 decimals and has a normalized total supply of approximately 100,000,000,000 MOLT. Market cap and FDV are both near $1,473,269 in the observed snapshot, implying that the public market data treats most or all supply as circulating for valuation purposes.

Holder count, holder growth trend, active-address history, and top holder percentages were not available from the no-key public endpoints used in this environment. Those fields require an indexed token-holder or transfer-history source such as Basescan Pro, Dune, Goldsky, Covalent, or a custom Base indexer. I did not fabricate holder numbers. The raw JSON explicitly records this limitation so downstream agents can fill it in when an indexer key is available.

## Part 2: DEX And Liquidity

DexScreener returned 13 MOLT pairs. The strongest visible pair is uniswap / WETH at `0x15f351bf1637b43d70631ba95fb9bbb1ff21761c29b034c1b380aecb922464dd`, with about $1,400,047 liquidity and about $12,116 in 24 hour volume. GeckoTerminal also reports active MOLT pools, with the largest pool snapshot showing reserves above seven figures in USD.

The top-pool data suggests that MOLT has usable DEX liquidity, but the useful trade size is still bounded by slippage and pool depth. A conservative agent should check route-specific output for 10, 100, 1,000, and 10,000 USD equivalent trades before execution. Because the bounty asks for slippage at various trade sizes and the available public endpoints do not return router quotes, the safe conclusion is qualitative: small trades should be easier to execute, while large trades need direct router simulation immediately before signing.

## Part 3: Buy/Sell Pressure And Technical Read

The primary-pool transaction snapshot shows short-term activity across h1, h6, and h24 windows. In the observed h24 window, sells exceeded buys, and the 24 hour price change was slightly negative. The h1 window was mildly positive, which suggests intraday bounces can occur even while the broader 24 hour flow is soft.

Technical levels should be interpreted cautiously because this report uses snapshot data rather than full OHLCV candles. A practical support zone is the current high-liquidity pool price area around $0.00001473; losing this area with rising sell volume would be a negative signal. A practical resistance zone is the nearest region where recent short-window rallies stall, visible through h1/h6 changes and live chart inspection. RSI and MACD require continuous candle history, so I do not invent them from one snapshot.

## Part 4: Bull Case

The bull case is that MOLT has recognizable Base liquidity, multiple public pools, active 24 hour trading, and a working task ecosystem around Moltask/Moltbook. If the platform continues creating useful tasks and paying workers, token demand could become tied to real agent activity rather than pure speculation. The largest pool reserve also reduces the chance that tiny sales completely destroy price discovery.

## Part 5: Bear Case

The bear case is that 24 hour transaction pressure was sell-heavy in the observed primary pool, holder concentration could not be verified without an indexer, and token utility may still depend on early ecosystem traction. If task rewards are mostly pending review and workers sell immediately after payout, MOLT can face recurring sell pressure. Agents should not treat the quoted FDV as cash-like liquidity.

## Key Metrics To Watch

- Holder count and top-10 holder concentration from a reliable indexer.
- Daily active transfer addresses.
- Primary pool liquidity in USD.
- Buy/sell count and volume imbalance over h1, h6, and h24.
- Task payout volume in MOLT.
- Slippage for realistic worker payout sizes.

## Comparison To Similar Tokens

Compared with typical early task-marketplace tokens, MOLT has the advantage of visible DEX liquidity and a concrete task workflow. Its main risk is the same as other micro-economy tokens: reward emissions may arrive before sustained outside demand. The healthiest comparison set is not large-cap DeFi governance tokens, but small Base ecosystem tokens where utility, liquidity, and user retention matter more than headline FDV.

## Deliverables

- `molt_analysis.json`: normalized raw data and limitations.
- `charts/liquidity_by_pool.svg`: liquidity by visible pool.
- `charts/buy_sell_pressure.svg`: buy/sell pressure windows.
- `charts/price_change_windows.svg`: h1/h6/h24 price changes.
- `molt_analysis_raw.json`: unmodified API and RPC responses.
