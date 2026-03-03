# NQ Scalper Pro v02 - External Connectors Setup

## Files Added/Updated
- Strategy: `strategies/NQ Scalper Pro v02 - External Connectors.pine`
- Strategy-compatible indicators (recommended for v02 wiring):
  - `indicators/strategy-compatible/lux-ifvg-strategy-compatible.pine`
  - `indicators/strategy-compatible/rth-opening-range-strategy-compatible.pine`
  - `indicators/strategy-compatible/htf-reversal-strategy-compatible.pine`
- Existing originals were also left in place and still include connector plots:
  - `indicators/lux-ifvg.pine`
  - `indicators/rth-opening-range.pine`
  - `indicators/htf-reversal.pine`

## Chart Setup Order
1. Add one or more connector-capable indicators to chart:
  - Lux IFVG - Strategy Compatible
  - RTH Opening Range - Strategy Compatible
  - HTF Reversal - Strategy Compatible
2. Add `NQ Scalper Pro v02 - External Connectors` strategy.
3. Open strategy settings → Inputs.
4. For each `... Source` field, select the desired plot from one of the added indicators.

## Connector Plot Naming Convention
All connector plots are prefixed with `LC`.

### Lux IFVG connectors
- Triggers: `LC Bullish Trigger`, `LC Bearish Trigger`
- States: `LC Bullish Active`, `LC Bearish Active`
- Levels: `LC Bull IFVG Top/Bottom/Mid`, `LC Bear IFVG Top/Bottom/Mid`

### RTH Opening Range connectors
- Triggers: `LC OR Break Above`, `LC OR Break Below`, `LC OR Mid Reclaim Bull`, `LC OR Mid Reclaim Bear`, `LC OR In Range`
- Levels: `LC OR High`, `LC OR Low`, `LC OR Mid`, `LC OR Ext1 High/Low`, `LC OR Ext2 High/Low`

### HTF Reversal connectors
- Triggers: `LC HTF Bull Pattern Trigger`, `LC HTF Bear Pattern Trigger`, `LC HTF Bull Divergence Trigger`, `LC HTF Bear Divergence Trigger`
- Bias: `LC RSI Bull Bias`, `LC RSI Bear Bias`
- Levels: `LC HTF Open`, `LC HTF High`, `LC HTF Low`, `LC HTF Close`, `LC RSI Value`

## How to Wire in Strategy
- Filters / Entry / Blockers:
  - Use trigger plots (`1/na`) with condition `Is True (1/na)`.
  - Use level plots with `Greater Than`, `Less Than`, `Crosses Over`, `Crosses Under`.
- Stop Loss:
  - `Stop Loss Mode = External Level` and map `Long SL Source` / `Short SL Source`.
- Take Profit:
  - Map TP level fields to level plots.
  - Optional trigger-based TP uses `TPx Trigger (1/na)`.
- Break-Even / Trail:
  - Map trigger source(s) to `1/na` plots.
  - Map trail long/short levels to any level plot.

## Notes
- This v02 strategy intentionally contains no internal indicator calculations to reduce tokens/memory/runtime load.
- For trigger series, use plots that output `1` on event and `na` otherwise.
- If a field appears inactive, verify the selected source plot is from an indicator currently on chart.

## Ready Preset
- For a ready-to-apply MNQ wiring profile, use:
  - `supporting docs/mnq-v02-input-mapping-checklist.md`
- For additional profile variants, use:
  - `supporting docs/mnq-v02-input-mapping-variants.md`
