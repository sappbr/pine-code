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
4. For each `... Source` field, select the desired connector from one of the added indicators.

## Connector Plot Naming Convention
All connector plots are prefixed with `LC`. In this guide, mappings use `Indicator Name: LC ...`.

### Lux IFVG connectors
- Triggers: `Lux IFVG: LC Bullish Trigger`, `Lux IFVG: LC Bearish Trigger`
- States: `Lux IFVG: LC Bullish Active`, `Lux IFVG: LC Bearish Active`
- Levels: `Lux IFVG: LC Bull IFVG Top`, `Lux IFVG: LC Bull IFVG Bottom`, `Lux IFVG: LC Bull IFVG Mid`, `Lux IFVG: LC Bear IFVG Top`, `Lux IFVG: LC Bear IFVG Bottom`, `Lux IFVG: LC Bear IFVG Mid`

### RTH Opening Range connectors
- Triggers: `RTH Opening Range: LC OR Break Above`, `RTH Opening Range: LC OR Break Below`, `RTH Opening Range: LC OR Mid Reclaim Bull`, `RTH Opening Range: LC OR Mid Reclaim Bear`, `RTH Opening Range: LC OR In Range`
- Levels: `RTH Opening Range: LC OR High`, `RTH Opening Range: LC OR Low`, `RTH Opening Range: LC OR Mid`, `RTH Opening Range: LC OR Ext1 High`, `RTH Opening Range: LC OR Ext1 Low`, `RTH Opening Range: LC OR Ext2 High`, `RTH Opening Range: LC OR Ext2 Low`

### HTF Reversal connectors
- Triggers: `HTF Reversal: LC HTF Bull Pattern Trigger`, `HTF Reversal: LC HTF Bear Pattern Trigger`, `HTF Reversal: LC HTF Bull Divergence Trigger`, `HTF Reversal: LC HTF Bear Divergence Trigger`
- Bias: `HTF Reversal: LC RSI Bull Bias`, `HTF Reversal: LC RSI Bear Bias`
- Levels: `HTF Reversal: LC HTF Open`, `HTF Reversal: LC HTF High`, `HTF Reversal: LC HTF Low`, `HTF Reversal: LC HTF Close`, `HTF Reversal: LC RSI Value`

## How to Wire in Strategy
- Filters / Entry / Blockers:
  - Use trigger plots (`1/na`) with condition `Is True (1/na)`.
  - Use level plots with `Greater Than`, `Less Than`, `Crosses Over`, `Crosses Under`.
- Stop Loss:
  - `Stop Loss Mode = External Level` and map `Long SL Source` / `Short SL Source` (example: `Lux IFVG: LC Bull IFVG Bottom`, `Lux IFVG: LC Bear IFVG Top`).
- Take Profit:
  - Map TP level fields to level plots (example: `RTH Opening Range: LC OR Ext1 High`, `RTH Opening Range: LC OR Ext1 Low`, `RTH Opening Range: LC OR Ext2 High`, `RTH Opening Range: LC OR Ext2 Low`).
  - Optional trigger-based TP uses `TPx Trigger (1/na)`.
- Break-Even / Trail:
  - Map trigger source(s) to `1/na` plots (example: `RTH Opening Range: LC OR Mid Reclaim Bull`, `RTH Opening Range: LC OR In Range`).
  - Map trail long/short levels to any level plot (example: `RTH Opening Range: LC OR Mid`).

## Notes
- This v02 strategy intentionally contains no internal indicator calculations to reduce tokens/memory/runtime load.
- For trigger series, use plots that output `1` on event and `na` otherwise.
- If a field appears inactive, verify the selected source plot is from an indicator currently on chart.

## Ready Preset
- For a ready-to-apply MNQ wiring profile, use:
  - `supporting docs/mnq-v02-input-mapping-checklist.md`
- For additional profile variants, use:
  - `supporting docs/mnq-v02-input-mapping-variants.md`
