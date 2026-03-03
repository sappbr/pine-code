# MNQ v02 Input Mapping Checklist (Ready to Apply)

Use this with:
- `strategies/NQ Scalper Pro v02 - External Connectors.pine`
- `indicators/strategy-compatible/lux-ifvg-strategy-compatible.pine`
- `indicators/strategy-compatible/rth-opening-range-strategy-compatible.pine`
- `indicators/strategy-compatible/htf-reversal-strategy-compatible.pine`

Related variants:
- `supporting docs/mnq-v02-input-mapping-variants.md`

## 0) Chart Prep
- [ ] Symbol: `CME_MINI:MNQ1!`
- [ ] Timeframe: `1m`
- [ ] Add indicators first (all 3 strategy-compatible versions), then add v02 strategy.

## 1) Engine Defaults
- [ ] `Order Size (Contracts)` = `3`
- [ ] `Allow Long` = `On`
- [ ] `Allow Short` = `On`

## 2) Long Side (Filters / Blocker / Entry)
- [ ] `Enable L Filter 1` = `On`
- [ ] `L Filter 1 Source` = `HTF Reversal: LC RSI Bull Bias`
- [ ] `L Filter 1 Condition` = `Is True (1/na)`

- [ ] `Enable L Filter 2` = `On`
- [ ] `L Filter 2 Source` = `Lux IFVG: LC Bullish Active`
- [ ] `L Filter 2 Condition` = `Is True (1/na)`

- [ ] `Enable L Filter 3` = `Off`

- [ ] `Enable L Blocker` = `On`
- [ ] `L Blocker Source` = `Lux IFVG: LC Bearish Trigger`
- [ ] `L Blocker Condition` = `Occurred Within X Bars`
- [ ] `L Blocker Bars` = `6`

- [ ] `Long Entry Trigger Source` = `RTH Opening Range: LC OR Break Above`
- [ ] `Long Entry Condition` = `Is True (1/na)`

## 3) Short Side (Filters / Blocker / Entry)
- [ ] `Enable S Filter 1` = `On`
- [ ] `S Filter 1 Source` = `HTF Reversal: LC RSI Bear Bias`
- [ ] `S Filter 1 Condition` = `Is True (1/na)`

- [ ] `Enable S Filter 2` = `On`
- [ ] `S Filter 2 Source` = `Lux IFVG: LC Bearish Active`
- [ ] `S Filter 2 Condition` = `Is True (1/na)`

- [ ] `Enable S Filter 3` = `Off`

- [ ] `Enable S Blocker` = `On`
- [ ] `S Blocker Source` = `Lux IFVG: LC Bullish Trigger`
- [ ] `S Blocker Condition` = `Occurred Within X Bars`
- [ ] `S Blocker Bars` = `6`

- [ ] `Short Entry Trigger Source` = `RTH Opening Range: LC OR Break Below`
- [ ] `Short Entry Condition` = `Is True (1/na)`

## 4) Risk / SL / TP / BE / Trail

### Stop Loss
- [ ] `Stop Loss Mode` = `External Level`
- [ ] `Long SL Source` = `Lux IFVG: LC Bull IFVG Bottom`
- [ ] `Short SL Source` = `Lux IFVG: LC Bear IFVG Top`
- [ ] `Fixed SL Ticks` = `40` (fallback only)

### Take Profit (Level-based default)
- [ ] `Enable TP1` = `On`, `%` = `40`
- [ ] `TP1 Long Level` = `RTH Opening Range: LC OR Ext1 High`
- [ ] `TP1 Short Level` = `RTH Opening Range: LC OR Ext1 Low`

- [ ] `Enable TP2` = `On`, `%` = `35`
- [ ] `TP2 Long Level` = `RTH Opening Range: LC OR Ext2 High`
- [ ] `TP2 Short Level` = `RTH Opening Range: LC OR Ext2 Low`

- [ ] `Enable TP3` = `On`, `%` = `25`
- [ ] `TP3 Long Level` = `Lux IFVG: LC Bear IFVG Top`
- [ ] `TP3 Short Level` = `Lux IFVG: LC Bull IFVG Bottom`

- [ ] Keep `TP1/TP2/TP3 Trigger (1/na)` on default chart source (`close`) unless you intentionally want event-driven partial exits.

### Break-Even
- [ ] `Enable Break-Even` = `On`
- [ ] `BE Trigger (1/na)` = `RTH Opening Range: LC OR Mid Reclaim Bull`
- [ ] `Offset Ticks` = `2`

### Trail
- [ ] `Enable Trail` = `On`
- [ ] `Trail Long Level` = `RTH Opening Range: LC OR Mid`
- [ ] `Trail Short Level` = `RTH Opening Range: LC OR Mid`
- [ ] `Trail Trigger (1/na)` = `RTH Opening Range: LC OR In Range`

## 5) Fast Validation Checklist
- [ ] Long arrow appears only when `RTH Opening Range: LC OR Break Above` prints and long filters pass.
- [ ] Short arrow appears only when `RTH Opening Range: LC OR Break Below` prints and short filters pass.
- [ ] Stops track selected external levels (`Lux IFVG: LC Bull IFVG Bottom` / `Lux IFVG: LC Bear IFVG Top`).
- [ ] TP lines execute on mapped OR/IFVG levels.
- [ ] No trades when blocker trigger fired within lookback.

## 6) Optional Aggressive Variant
If you want more trades:
- [ ] Turn `Enable L/S Blocker` = `Off`
- [ ] Keep Filter 1 on RSI bias, Filter 2 on IFVG active.
- [ ] Lower blocker/entry lookbacks to `3` where used.
