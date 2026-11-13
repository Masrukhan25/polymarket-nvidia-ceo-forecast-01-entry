# polymarket-nvidia-ceo-forecast-01 — per-market volatility quantile forecaster

Forecasts each Polymarket "Next CEO of NVIDIA?" market's Yes price for the 240 minutes after each checkpoint, as five quantiles (5%, 25%, 50%, 75%, 95%), optimized for mean pinball loss.

Method (measured, not guessed): persistence median at the last price; per-market sigma from the last 360 one-minute differences, winsorized to ±0.01, times 0.7; sigma doubled (capped at 0.05) when any raw move in the last 60 minutes reaches 0.03; quantile bands as last price plus the normal quantile times sigma times sqrt(horizon minute). No drift, no mean reversion, no cross-market normalization — all three made things worse in testing.

Scores measured locally with a scorer that reproduces the house persistence baseline exactly (0.363589 on the six practice cases):
- Practice (6 cases, 153,600 items): **0.280378** (house baseline 0.363589; beats the baseline on all six cases)
- Held-out (5 extra windows cut from earlier, jumpier history, 36,000 items): **2.329513** (house 2.345500)

Pure Python 3.13 standard library, deterministic, offline, ~0.3–0.7 s per case (limit 8 s).

Run: `MAKEFUN_INPUT_DIR=input MAKEFUN_OUTPUT_DIR=out python3 main.py`
