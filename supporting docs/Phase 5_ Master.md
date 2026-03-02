### **📋 COPY AND PASTE THIS TO GEMINI:**

**Prompt:**

**Role:** You are an elite algorithmic trading developer specializing in Pine Script v6 and direct-market-access (DMA) institutional routing.

**Task:** Write **ONLY** the Pine Script v6 code for **Phase \[5\]** of the "God Mode" Multi-Timeframe NQ Scalping Engine. You already have the Master Blueprint framework. **Do NOT write the code for any other phases, and do NOT provide placeholder comments for the rest of the script.** Your output should strictly contain the specific module requested.

**Execution Environment:** The code must be optimized for Tradovate's cloud-based matching engine via TradingView webhooks, targeting strict compliance with MFFU, Lucid Trading, and Tradeify rule sets.

**Critical Constraints & Prop Firm Rules You MUST Implement (Apply those relevant to this Phase):**

**1\. The Fractional Scaling Trap (Strict Integer Math):** Tradovate’s API explicitly rejects fractional futures contracts. You are strictly forbidden from using `qty_percent` parameters in `strategy.exit()`. For dynamic exits and Tradeify's massive 15+ mini-contract scaling limits, you must calculate precise integer values natively using `math.max(1, math.round(qtyInput * ratio))` and pass absolute whole-number integers into `strategy.close(qty=...)`. Allow asynchronous `f_eval` triggers to scale out smoothly.

**2\. The Microscalping Filter (10-Second Temporal Delay):** To prevent flagging Tradeify’s microscalping bans, the "Waterfall SL" or trailing stop logic must include a temporal minimum delay condition: `if (time - entry_time) > 10000`. The algorithm must hold the macro HTF stop-loss for a minimum of 10 seconds before cascading the trailing stop tightly behind micro-liquidity pools.

**3\. Compliance Engine (Bar-Time Death Trap, News, & Lucid Disarm):** The MFFU 16:09 EST daily flatten must NOT be bound to `process_orders_on_close = true` bar-close dependencies. It must be built as an independent tick-based `alertcondition()` using `calc_on_every_tick = true`. For the News Killzone Array, use a statically populated Unix timestamp matrix (`var int[] newsTimestamps = array.new_int()`) with a ±120-second buffer to block new entries and flatten trades; `request.economic()` is explicitly banned. You must also include the Lucid Trading "Safety Disarm" which tracks current-session PnL and freezes the bot (`safetyDisarm := true`) if it reaches 48% of the profit target to protect the 50% consistency rule.

**4\. Institutional Execution Handicaps:** Strategy declarations must simulate Tradovate market-order latency. Strictly use the Master Blueprint's cloud-routed handicaps: `initial_capital = 50000`, `slippage = 2`, `commission_value = 2.50`, `use_bar_magnifier = true`, and `process_orders_on_close = true`.

**5\. Zero-Repainting MTF Tuples (The Institutional Standard):** To avoid lookahead bias and request limits, you must pack all 16 structural variables into a single array tuple per timeframe inside `request.security()`. You must use the absolute safest zero-repainting configuration: `lookahead=barmerge.lookahead_on` combined with a strict \`\` historical offset applied to *every single calculation* inside the tuple to evaluate only the previous closed HTF bar.

**6\. Cycle Memory & State Tracking (Ban `ta.barssince`):** You are strictly forbidden from using `ta.barssince()`, as it crashes the MTF engine by forcing deep historical scans. Use single-threaded native `var` declarations for state tracking. Establish immutable macro-anchors when a structural event occurs, and use forward-rolling counters that increment natively on every bar.

**7\. The `f_eval` Master Parser & Waterfall Risk Flow:** The `f_eval` function must act as a universal translator using `switch` blocks to route UI timeframe strings ("HTF 1", "HTF 2", "HTF 3") to their respective tuple arrays (`h1_`, `h2_`, `h3_`), applying a `dir` argument (1 for Long, \-1 for Short). For Phase 6 risk, the Waterfall SL must evaluate HTF 3, then HTF 2, then HTF 1 against `maxSlTicks`. If all fail, execute the `failsafeAct` ("Abort" or "Snap"), and enforce a hard `maxLossTrade` Max Adverse Excursion (MAE) dollar limit to override all structural stops.

**8\. Non-Repainting Visuals:** Bypass `plot()` buffer limits. Draw Entry, SL, and TP lines dynamically using `line.new()` and `line.set_x2()`, permanently freezing them on the bar the trade closes to create a flawless visual audit trail.

**Output:** Provide ONLY the fully optimized Pine Script v6 code block for **Phase \[5** with brief inline technical comments explaining how the constraints were satisfied.

