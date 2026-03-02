# pine-code

Current version: v6.1

## Release Notes

### v6.1 (2026-03-02)
- Added advanced timeframe combo selectors for long/short filters, blockers, and triggers.
- Added `EMA Trail` support in evaluator logic and initial SL modes (Chart, HTF1, HTF2, HTF3).
- Added liquidity sweep signals/conditions (`Liquidity Sweep`, `Bullish Swing Sweep`, `Bearish Swing Sweep`) with chart markers and dashboard visibility.
- Expanded TP/Exit timeframe and indicator options for multi-timeframe and sweep-aware exits.
- Added optional 09:30 trigger carryover (pre-open latch memory + post-open grace bars) to reduce missed open-session entries.
- Standardized MAE enforcement/log metadata wording and updated strategy version footer to `v6.1`.