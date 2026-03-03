# OR + HTF Reversal Sequence (v02 Connector Stack)

This is the easiest, copy-ready setup for your current stack.

## Quick-Start (1 minute)

### 1) Add scripts in this exact order
1. `Lux IFVG - Strategy Compatible`
2. `RTH Opening Range - Strategy Compatible`
3. `HTF Reversal - Strategy Compatible`
4. `NQ Scalper Pro v02 - External Connectors`

Why order matters: indicators must load first so the strategy `input.source` dropdowns show all connector fields.

### 2) Chart settings
- Symbol: `CME_MINI:MNQ1!`
- Timeframe: `1m`
- Session: regular RTH workflow (`09:30–16:00 ET`)

### 3) Indicator defaults
- **Lux IFVG - Strategy Compatible**: `ATR Length=200`, `ATR Multiplier=0.25`
- **RTH Opening Range - Strategy Compatible**: `Opening Range Duration=5`, `Extension1=1.0`, `Extension2=2.0`
- **HTF Reversal - Strategy Compatible**: `HTF=15`, `Engulfing=On`, `Pin Bars=On`, `RSI Length=14`, `Divergence Lookback=5`

---

## Strategy Wiring (copy this exactly)

### Engine
- `Order Size (Contracts) = 3`
- `Allow Long = On`
- `Allow Short = On`

### Long side
- `Enable L Filter 1 = On`
  - `L Filter 1 Source = HTF Reversal: LC RSI Bull Bias`
  - `Condition = Is True (1/na)`
- `Enable L Filter 2 = On`
  - `L Filter 2 Source = Lux IFVG: LC Bullish Active`
  - `Condition = Is True (1/na)`
- `Enable L Filter 3 = Off`
- `Enable L Blocker = On`
  - `L Blocker Source = Lux IFVG: LC Bearish Trigger`
  - `Condition = Occurred Within X Bars`
  - `L Blocker Bars = 6`
- `Long Entry Trigger Source = RTH Opening Range: LC OR Break Above`
  - `Condition = Is True (1/na)`

### Short side
- `Enable S Filter 1 = On`
  - `S Filter 1 Source = HTF Reversal: LC RSI Bear Bias`
  - `Condition = Is True (1/na)`
- `Enable S Filter 2 = On`
  - `S Filter 2 Source = Lux IFVG: LC Bearish Active`
  - `Condition = Is True (1/na)`
- `Enable S Filter 3 = Off`
- `Enable S Blocker = On`
  - `S Blocker Source = Lux IFVG: LC Bullish Trigger`
  - `Condition = Occurred Within X Bars`
  - `S Blocker Bars = 6`
- `Short Entry Trigger Source = RTH Opening Range: LC OR Break Below`
  - `Condition = Is True (1/na)`

### Risk / Exits
- `Stop Loss Mode = External Level`
  - `Long SL Source = Lux IFVG: LC Bull IFVG Bottom`
  - `Short SL Source = Lux IFVG: LC Bear IFVG Top`
  - `Fixed SL Ticks = 40` (fallback only)
- `Enable TP1 = On`, `% = 40`
  - `TP1 Long Level = RTH Opening Range: LC OR Ext1 High`
  - `TP1 Short Level = RTH Opening Range: LC OR Ext1 Low`
- `Enable TP2 = On`, `% = 35`
  - `TP2 Long Level = RTH Opening Range: LC OR Ext2 High`
  - `TP2 Short Level = RTH Opening Range: LC OR Ext2 Low`
- `Enable TP3 = On`, `% = 25`
  - `TP3 Long Level = Lux IFVG: LC Bear IFVG Top`
  - `TP3 Short Level = Lux IFVG: LC Bull IFVG Bottom`
- `Enable Break-Even = On`
  - `BE Trigger (1/na) = RTH Opening Range: LC OR Mid Reclaim Bull`
  - `Offset Ticks = 2`
- `Enable Trail = On`
  - `Trail Long Level = RTH Opening Range: LC OR Mid`
  - `Trail Short Level = RTH Opening Range: LC OR Mid`
  - `Trail Trigger (1/na) = RTH Opening Range: LC OR In Range`

---

## Important UI behavior (new)

The strategy input rows now auto-enable to reduce confusion:
- **Row 2 (Target row)** only enables for: `Greater Than`, `Less Than`, `Crosses Over`, `Crosses Under`
- **Row 3 (Lookback row)** only enables for: `Occurred Within X Bars`, `Occurred Within X Days`
- For blockers/filters, disabling the section also disables those extra rows.

So for your blocker example (`Occurred Within X Bars`), row 3 is active and row 2 is inactive by design.

---

## Full connector dictionary (indicator name first)

### Lux IFVG - Strategy Compatible
- `Lux IFVG: LC Bullish Trigger`
- `Lux IFVG: LC Bearish Trigger`
- `Lux IFVG: LC Bullish Active`
- `Lux IFVG: LC Bearish Active`
- `Lux IFVG: LC Bull IFVG Top`
- `Lux IFVG: LC Bull IFVG Bottom`
- `Lux IFVG: LC Bull IFVG Mid`
- `Lux IFVG: LC Bear IFVG Top`
- `Lux IFVG: LC Bear IFVG Bottom`
- `Lux IFVG: LC Bear IFVG Mid`

### RTH Opening Range - Strategy Compatible
- `RTH Opening Range: LC OR Break Above`
- `RTH Opening Range: LC OR Break Below`
- `RTH Opening Range: LC OR Mid Reclaim Bull`
- `RTH Opening Range: LC OR Mid Reclaim Bear`
- `RTH Opening Range: LC OR In Range`
- `RTH Opening Range: LC OR High`
- `RTH Opening Range: LC OR Low`
- `RTH Opening Range: LC OR Mid`
- `RTH Opening Range: LC OR Ext1 High`
- `RTH Opening Range: LC OR Ext1 Low`
- `RTH Opening Range: LC OR Ext2 High`
- `RTH Opening Range: LC OR Ext2 Low`

### HTF Reversal - Strategy Compatible
- `HTF Reversal: LC HTF Bull Pattern Trigger`
- `HTF Reversal: LC HTF Bear Pattern Trigger`
- `HTF Reversal: LC HTF Bull Divergence Trigger`
- `HTF Reversal: LC HTF Bear Divergence Trigger`
- `HTF Reversal: LC RSI Bull Bias`
- `HTF Reversal: LC RSI Bear Bias`
- `HTF Reversal: LC RSI Value`
- `HTF Reversal: LC HTF Open`
- `HTF Reversal: LC HTF High`
- `HTF Reversal: LC HTF Low`
- `HTF Reversal: LC HTF Close`

---

## Sanity Checks
- Long/short arrows print only when OR break triggers and active filters pass.
- Opposing IFVG trigger inside blocker lookback suppresses new entries.
- SL/TP levels bind to selected external connector levels.

## Related docs
- Base preset: `supporting docs/mnq-v02-input-mapping-checklist.md`
- Variants: `supporting docs/mnq-v02-input-mapping-variants.md`
- Full setup: `supporting docs/external-connectors-setup.md`