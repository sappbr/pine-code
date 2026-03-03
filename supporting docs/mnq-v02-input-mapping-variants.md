# MNQ v02 Input Mapping Variants

Use with:
- `strategies/NQ Scalper Pro v02 - External Connectors.pine`
- `indicators/strategy-compatible/lux-ifvg-strategy-compatible.pine`
- `indicators/strategy-compatible/rth-opening-range-strategy-compatible.pine`
- `indicators/strategy-compatible/htf-reversal-strategy-compatible.pine`

---
## Variant A — Higher Frequency / Fewer Filters

### 1) Engine
- [ ] `Order Size (Contracts)` = `2` or `3`
- [ ] `Allow Long` = `On`
- [ ] `Allow Short` = `On`

### 2) Long Side
- [ ] `Enable L Filter 1` = `On`
- [ ] `L Filter 1 Source` = `HTF Reversal: LC RSI Bull Bias`
- [ ] `L Filter 1 Condition` = `Is True (1/na)`
- [ ] `Enable L Filter 2` = `Off`
- [ ] `Enable L Filter 3` = `Off`
- [ ] `Enable L Blocker` = `Off`
- [ ] `Long Entry Trigger Source` = `RTH Opening Range: LC OR Break Above`
- [ ] `Long Entry Condition` = `Is True (1/na)`
- [ ] `L Entry Bars` = `1`

### 3) Short Side
- [ ] `Enable S Filter 1` = `On`
- [ ] `S Filter 1 Source` = `HTF Reversal: LC RSI Bear Bias`
- [ ] `S Filter 1 Condition` = `Is True (1/na)`
- [ ] `Enable S Filter 2` = `Off`
- [ ] `Enable S Filter 3` = `Off`
- [ ] `Enable S Blocker` = `Off`
- [ ] `Short Entry Trigger Source` = `RTH Opening Range: LC OR Break Below`
- [ ] `Short Entry Condition` = `Is True (1/na)`
- [ ] `S Entry Bars` = `1`

### 4) Risk / Exits
- [ ] `Stop Loss Mode` = `External Level`
- [ ] `Long SL Source` = `RTH Opening Range: LC OR Low`
- [ ] `Short SL Source` = `RTH Opening Range: LC OR High`
- [ ] `Fixed SL Ticks` = `28`

- [ ] `Enable TP1` = `On`, `%` = `50`
- [ ] `TP1 Long Level` = `RTH Opening Range: LC OR Ext1 High`
- [ ] `TP1 Short Level` = `RTH Opening Range: LC OR Ext1 Low`

- [ ] `Enable TP2` = `On`, `%` = `30`
- [ ] `TP2 Long Level` = `RTH Opening Range: LC OR Ext2 High`
- [ ] `TP2 Short Level` = `RTH Opening Range: LC OR Ext2 Low`

- [ ] `Enable TP3` = `On`, `%` = `20`
- [ ] `TP3 Long Level` = `Lux IFVG: LC Bear IFVG Mid`
- [ ] `TP3 Short Level` = `Lux IFVG: LC Bull IFVG Mid`

- [ ] `Enable Break-Even` = `On`
- [ ] `BE Trigger (1/na)` = `RTH Opening Range: LC OR Mid Reclaim Bull`
- [ ] `Offset Ticks` = `1`

- [ ] `Enable Trail` = `On`
- [ ] `Trail Long Level` = `RTH Opening Range: LC OR Mid`
- [ ] `Trail Short Level` = `RTH Opening Range: LC OR Mid`
- [ ] `Trail Trigger (1/na)` = `RTH Opening Range: LC OR In Range`

### 5) Notes
- This variant increases signal frequency and churn.
- Expect more entries and potentially more false starts around chop.

---
## Variant B — Prop-Safe Conservative

### 1) Engine
- [ ] `Order Size (Contracts)` = `1` or `2`
- [ ] `Allow Long` = `On`
- [ ] `Allow Short` = `On`

### 2) Long Side
- [ ] `Enable L Filter 1` = `On`
- [ ] `L Filter 1 Source` = `HTF Reversal: LC RSI Bull Bias`
- [ ] `L Filter 1 Condition` = `Is True (1/na)`

- [ ] `Enable L Filter 2` = `On`
- [ ] `L Filter 2 Source` = `Lux IFVG: LC Bullish Active`
- [ ] `L Filter 2 Condition` = `Is True (1/na)`

- [ ] `Enable L Filter 3` = `On`
- [ ] `L Filter 3 Source` = `HTF Reversal: LC HTF Bull Pattern Trigger`
- [ ] `L Filter 3 Condition` = `Occurred Within X Bars`
- [ ] `L Filter 3 Bars` = `8`

- [ ] `Enable L Blocker` = `On`
- [ ] `L Blocker Source` = `Lux IFVG: LC Bearish Trigger`
- [ ] `L Blocker Condition` = `Occurred Within X Bars`
- [ ] `L Blocker Bars` = `10`

- [ ] `Long Entry Trigger Source` = `RTH Opening Range: LC OR Break Above`
- [ ] `Long Entry Condition` = `Is True (1/na)`
- [ ] `L Entry Bars` = `2`

### 3) Short Side
- [ ] `Enable S Filter 1` = `On`
- [ ] `S Filter 1 Source` = `HTF Reversal: LC RSI Bear Bias`
- [ ] `S Filter 1 Condition` = `Is True (1/na)`

- [ ] `Enable S Filter 2` = `On`
- [ ] `S Filter 2 Source` = `Lux IFVG: LC Bearish Active`
- [ ] `S Filter 2 Condition` = `Is True (1/na)`

- [ ] `Enable S Filter 3` = `On`
- [ ] `S Filter 3 Source` = `HTF Reversal: LC HTF Bear Pattern Trigger`
- [ ] `S Filter 3 Condition` = `Occurred Within X Bars`
- [ ] `S Filter 3 Bars` = `8`

- [ ] `Enable S Blocker` = `On`
- [ ] `S Blocker Source` = `Lux IFVG: LC Bullish Trigger`
- [ ] `S Blocker Condition` = `Occurred Within X Bars`
- [ ] `S Blocker Bars` = `10`

- [ ] `Short Entry Trigger Source` = `RTH Opening Range: LC OR Break Below`
- [ ] `Short Entry Condition` = `Is True (1/na)`
- [ ] `S Entry Bars` = `2`

### 4) Risk / Exits
- [ ] `Stop Loss Mode` = `External Level`
- [ ] `Long SL Source` = `Lux IFVG: LC Bull IFVG Bottom`
- [ ] `Short SL Source` = `Lux IFVG: LC Bear IFVG Top`
- [ ] `Fixed SL Ticks` = `40`

- [ ] `Enable TP1` = `On`, `%` = `45`
- [ ] `TP1 Long Level` = `RTH Opening Range: LC OR Ext1 High`
- [ ] `TP1 Short Level` = `RTH Opening Range: LC OR Ext1 Low`

- [ ] `Enable TP2` = `On`, `%` = `35`
- [ ] `TP2 Long Level` = `RTH Opening Range: LC OR Ext2 High`
- [ ] `TP2 Short Level` = `RTH Opening Range: LC OR Ext2 Low`

- [ ] `Enable TP3` = `On`, `%` = `20`
- [ ] `TP3 Long Level` = `Lux IFVG: LC Bear IFVG Top`
- [ ] `TP3 Short Level` = `Lux IFVG: LC Bull IFVG Bottom`

- [ ] `Enable Break-Even` = `On`
- [ ] `BE Trigger (1/na)` = `RTH Opening Range: LC OR Mid Reclaim Bull`
- [ ] `Offset Ticks` = `2`

- [ ] `Enable Trail` = `On`
- [ ] `Trail Long Level` = `RTH Opening Range: LC OR Mid`
- [ ] `Trail Short Level` = `RTH Opening Range: LC OR Mid`
- [ ] `Trail Trigger (1/na)` = `RTH Opening Range: LC OR In Range`

### 5) Notes
- This variant intentionally trades less and blocks more adverse setups.
- Better suited to rule-constrained evaluation/funded workflows.
