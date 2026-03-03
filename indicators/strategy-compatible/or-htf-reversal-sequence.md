# Quick-Start Card (1 Minute Setup)

## Load Order
1. `Lux IFVG - Strategy Compatible`
2. `RTH Opening Range - Strategy Compatible`
3. `HTF Reversal - Strategy Compatible`
4. `NQ Scalper Pro v02 - External Connectors`

## Chart
- Symbol: `CME_MINI:MNQ1!`
- Timeframe: `1m`

## Indicator Defaults
- **Lux IFVG**: `ATR Length=200`, `ATR Multiplier=0.25`
- **RTH OR**: `OR Duration=5`, `Ext1=1.0`, `Ext2=2.0`
- **HTF Reversal**: `HTF=15`, `Engulfing=On`, `Pin Bars=On`, `RSI Len=14`, `Div Lookback=5`

## Strategy v02 (Fast Wiring)
- **Long**
  - `L Filter 1 Source = LC RSI Bull Bias` (`Is True`)
  - `L Filter 2 Source = LC Bullish Active` (`Is True`)
  - `L Blocker Source = LC Bearish Trigger` (`Occurred Within X Bars`, bars=`6`)
  - `Long Entry Trigger Source = LC OR Break Above` (`Is True`)
- **Short**
  - `S Filter 1 Source = LC RSI Bear Bias` (`Is True`)
  - `S Filter 2 Source = LC Bearish Active` (`Is True`)
  - `S Blocker Source = LC Bullish Trigger` (`Occurred Within X Bars`, bars=`6`)
  - `Short Entry Trigger Source = LC OR Break Below` (`Is True`)
- **Risk/Exit**
  - `SL Mode = External Level`
  - `Long SL = LC Bull IFVG Bottom`, `Short SL = LC Bear IFVG Top`
  - `TP1: 40% -> LC OR Ext1 High/Low`
  - `TP2: 35% -> LC OR Ext2 High/Low`
  - `TP3: 25% -> LC Bear IFVG Top / LC Bull IFVG Bottom`
  - `BE Trigger = LC OR Mid Reclaim Bull`, `Offset=2`
  - `Trail Level = LC OR Mid`, `Trail Trigger = LC OR In Range`

## Sanity Check
- Long/short arrows only print on OR breaks with filters passing.
- Blockers suppress entries after opposing trigger events.
- Stops/TPs bind to selected external levels.

---

# OR + HTF Reversal Sequence (v02 Connector Stack)

This is the practical chart setup guide for your current repository strategy stack.

## 1) What to load on chart (exact order)
1. `Lux IFVG - Strategy Compatible`
   - File: `indicators/strategy-compatible/lux-ifvg-strategy-compatible.pine`
2. `RTH Opening Range - Strategy Compatible`
   - File: `indicators/strategy-compatible/rth-opening-range-strategy-compatible.pine`
3. `HTF Reversal - Strategy Compatible`
   - File: `indicators/strategy-compatible/htf-reversal-strategy-compatible.pine`
4. `NQ Scalper Pro v02 - External Connectors`
   - File: `strategies/NQ Scalper Pro v02 - External Connectors.pine`

Always add indicators first, strategy second, so all `input.source` dropdowns populate immediately.

## 2) Chart symbol + timeframe
- Symbol: `CME_MINI:MNQ1!`
- Execution timeframe: `1m`
- Recommended session use: regular RTH workflow (09:30–16:00 ET)

## 3) Indicator configuration

### A) Lux IFVG - Strategy Compatible
- `ATR Length` = `200`
- `ATR Multiplier` = `0.25`

Primary connector outputs used by strategy:
- Triggers: `LC Bullish Trigger`, `LC Bearish Trigger`
- State: `LC Bullish Active`, `LC Bearish Active`
- Levels: `LC Bull IFVG Top/Bottom/Mid`, `LC Bear IFVG Top/Bottom/Mid`

### B) RTH Opening Range - Strategy Compatible
- `Opening Range Duration (minutes)` = `5`
- `Extension 1 Multiplier` = `1.0`
- `Extension 2 Multiplier` = `2.0`

Primary connector outputs used by strategy:
- Triggers: `LC OR Break Above`, `LC OR Break Below`, `LC OR Mid Reclaim Bull`, `LC OR Mid Reclaim Bear`, `LC OR In Range`
- Levels: `LC OR High`, `LC OR Low`, `LC OR Mid`, `LC OR Ext1 High/Low`, `LC OR Ext2 High/Low`

### C) HTF Reversal - Strategy Compatible
- `High Timeframe` = `15`
- `Show Engulfing Patterns` = `On`
- `Show Pin Bars` = `On`
- `RSI Length` = `14`
- `Divergence Lookback` = `5`

Primary connector outputs used by strategy:
- Triggers: `LC HTF Bull Pattern Trigger`, `LC HTF Bear Pattern Trigger`, `LC HTF Bull Divergence Trigger`, `LC HTF Bear Divergence Trigger`
- Bias: `LC RSI Bull Bias`, `LC RSI Bear Bias`
- Levels: `LC HTF Open/High/Low/Close`, `LC RSI Value`

## 4) Strategy v02 configuration (recommended baseline)

### Engine
- `Order Size (Contracts)` = `3`
- `Allow Long` = `On`
- `Allow Short` = `On`

### Long filters / blocker / entry
- `Enable L Filter 1` = `On`
  - `L Filter 1 Source` = `LC RSI Bull Bias`
  - `Condition` = `Is True (1/na)`
- `Enable L Filter 2` = `On`
  - `L Filter 2 Source` = `LC Bullish Active`
  - `Condition` = `Is True (1/na)`
- `Enable L Filter 3` = `Off`
- `Enable L Blocker` = `On`
  - `L Blocker Source` = `LC Bearish Trigger`
  - `Condition` = `Occurred Within X Bars`
  - `L Blocker Bars` = `6`
- `Long Entry Trigger Source` = `LC OR Break Above`
  - `Condition` = `Is True (1/na)`

### Short filters / blocker / entry
- `Enable S Filter 1` = `On`
  - `S Filter 1 Source` = `LC RSI Bear Bias`
  - `Condition` = `Is True (1/na)`
- `Enable S Filter 2` = `On`
  - `S Filter 2 Source` = `LC Bearish Active`
  - `Condition` = `Is True (1/na)`
- `Enable S Filter 3` = `Off`
- `Enable S Blocker` = `On`
  - `S Blocker Source` = `LC Bullish Trigger`
  - `Condition` = `Occurred Within X Bars`
  - `S Blocker Bars` = `6`
- `Short Entry Trigger Source` = `LC OR Break Below`
  - `Condition` = `Is True (1/na)`

### SL / TP / BE / Trail
- `Stop Loss Mode` = `External Level`
  - `Long SL Source` = `LC Bull IFVG Bottom`
  - `Short SL Source` = `LC Bear IFVG Top`
  - `Fixed SL Ticks` = `40` (fallback)

- `Enable TP1` = `On`, `%` = `40`
  - `TP1 Long Level` = `LC OR Ext1 High`
  - `TP1 Short Level` = `LC OR Ext1 Low`

- `Enable TP2` = `On`, `%` = `35`
  - `TP2 Long Level` = `LC OR Ext2 High`
  - `TP2 Short Level` = `LC OR Ext2 Low`

- `Enable TP3` = `On`, `%` = `25`
  - `TP3 Long Level` = `LC Bear IFVG Top`
  - `TP3 Short Level` = `LC Bull IFVG Bottom`

- `Enable Break-Even` = `On`
  - `BE Trigger (1/na)` = `LC OR Mid Reclaim Bull`
  - `Offset Ticks` = `2`

- `Enable Trail` = `On`
  - `Trail Long Level` = `LC OR Mid`
  - `Trail Short Level` = `LC OR Mid`
  - `Trail Trigger (1/na)` = `LC OR In Range`

## 5) Sequence logic (how this setup behaves)
1. HTF bias + IFVG state filter direction.
2. OR break (`Above`/`Below`) arms entry.
3. Opposing IFVG trigger within blocker lookback blocks low-quality entries.
4. Initial SL from IFVG structure; TP from OR extensions.
5. BE + trail management uses OR reclaim/in-range logic.

## 6) Quick validation checklist
- Long signal appears only when OR breaks up and long filters pass.
- Short signal appears only when OR breaks down and short filters pass.
- Opposing trigger within lookback suppresses entries.
- SL follows IFVG level selection; TPs map to OR extensions.

## 7) Related docs
- Base preset: `supporting docs/mnq-v02-input-mapping-checklist.md`
- Variants: `supporting docs/mnq-v02-input-mapping-variants.md`
- Full connector setup: `supporting docs/external-connectors-setup.md`