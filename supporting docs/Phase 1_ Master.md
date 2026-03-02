Here is the highly technical, constraint-heavy prompt tailored for Gemini 3.1 Pro. It integrates the exact v6 syntax optimizations from the Master Blueprint and explicitly enforces the resolutions to the four critical Prop Firm execution conflicts we just audited.

---

### **📋 COPY AND PASTE THIS TO GEMINI:**

**Prompt:**

**Role:** You are an elite algorithmic trading developer specializing in Pine Script v6 and direct-market-access (DMA) institutional routing.

**Task:** Write **ONLY** the Pine Script v6 code for **Phase \[1\]** of the "God Mode" Multi-Timeframe NQ Scalping Engine. You already have the Master Blueprint framework. Do NOT write the code for any other phases, and do not provide placeholder comments for the rest of the script.

**Execution Environment:** The code must be optimized for Tradovate's cloud-based matching engine via TradingView webhooks, targeting compliance with MFFU, Lucid Trading, and Tradeify rule sets.

**Critical Compliance & QA Constraints You MUST Implement for this Phase:**

1. **The Fractional Scaling Trap (Strict Integer Math):** Tradovate’s API explicitly rejects fractional futures contracts. You are strictly forbidden from using `qty_percent` parameters in `strategy.exit()`. If this phase handles dynamic exits and scaling, you must calculate precise integer values natively using `math.max(1, math.round(qtyInput * ratio))` (e.g., handling 15+ mini-contract limits) and pass absolute whole-number integers into `strategy.close(qty=...)`.

2. **The Microscalping Filter (10-Second Temporal Delay):** If this phase includes the "Waterfall SL" or trailing stop logic, you must prevent the algorithm from flagging Tradeify’s microscalping bans. Introduce a temporal minimum delay condition: `if (time - entry_time) > 10000`. The algorithm must hold the macro HTF stop-loss for a minimum of 10 seconds before it is authorized to cascade the trailing stop tightly behind micro-liquidity pools.

3. **The Bar-Time Death Trap (MFFU 16:09 Flatten):** If this phase handles the compliance daily flatten, do not bind it to `process_orders_on_close = true` bar-close dependencies, which will cause latency liquidation at MFFU's 16:10 EST strike. The flatten must either be built as an independent tick-based `alertcondition()` bypassing the strategy block, or the strategy trigger must be shifted to `16:08 EST` with `calc_on_every_tick = true`.

4. **Institutional Execution Handicaps (Strategy Declarations):** If this phase includes the `strategy()` declaration, you must use the Master Blueprint's cloud-routed handicaps: `slippage = 2`, `commission_value = 2.50`, `commission_type = strategy.commission.cash_per_contract`, and `use_bar_magnifier = true`. Do NOT use optimistic Rithmic settings (e.g., 1 tick / 1.25).

**v6 Syntax & Memory Directives:**

* Utilize Pine Script v6 `group` and `inline` parameters for all `input()` declarations to maintain a modular UI without bloating the settings panel.  
* Maintain zero-repainting multi-timeframe arrays. If requesting HTF data, mandate `lookahead=barmerge.lookahead_on` paired with historical \`\` indexing to lock the HTF close.  
* Use `var` for state trackers and rolling counters instead of memory-heavy `ta.barssince()` loops.

**Output:** Provide ONLY the fully optimized Pine Script v6 code block for Phase **\[1\]** with brief inline technical comments explaining how the constraints were satisfied.

---

**How to use this:** Simply replace **\[1\]** with the exact phase you are currently building (e.g., *Phase 6: Waterfall Risk Engine & Visuals* or *Phase 1: UI Architecture & Strategy Declarations*). Gemini will read the constraints and automatically weave the QA resolutions into that specific module's code.

