# pine-code

Current version: v6.2

## Release Notes

### v6.2 (2026-03-02)
- Added an on-chart `Policy Mode` label to display active firm/news compliance state in real time.
- Added compliance reference documentation under `supporting docs/` (MFFU, Tradify, Lucid, 365-day schedule, and historical summary).
- Added companion indicator for visuals/dashboard: [`indicators/visual-companion-dashboard.pine`](indicators/visual-companion-dashboard.pine).
- **Compliance References:**
- Compliance docs index: [MFFU](supporting%20docs/MyFundedFutures%20Compliance%20Overview%20for%20Apex%20Protocol%20Deployment.md) | [Tradify](supporting%20docs/Tradify%20Compliance%20Overview%20for%20Apex%20Protocol%20Deployment.md) | [Lucid](supporting%20docs/Lucid%20Trading%20Compliance%20Overview%20for%20Apex%20Protocol%20Deployment.md) | [365-day schedule](supporting%20docs/Prop%20Firm%20Compliance%20Schedule%20%28365-Day%20Outlook%29.md) | [Historical](supporting%20docs/Historical%20Prop%20Firm%20Compliance%20%28March%202025%20%E2%80%93%20February%202026%29.md)

### v6.1 (2026-03-02)
- Added advanced timeframe combo selectors for long/short filters, blockers, and triggers.
- Added `EMA Trail` support in evaluator logic and initial SL modes (Chart, HTF1, HTF2, HTF3).
- Added liquidity sweep signals/conditions (`Liquidity Sweep`, `Bullish Swing Sweep`, `Bearish Swing Sweep`) with chart markers and dashboard visibility.
- Expanded TP/Exit timeframe and indicator options for multi-timeframe and sweep-aware exits.
- Added optional 09:30 trigger carryover (pre-open latch memory + post-open grace bars) to reduce missed open-session entries.
- Standardized MAE enforcement/log metadata wording and updated strategy version footer to `v6.1`.