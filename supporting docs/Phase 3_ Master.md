Here is the highly technical, constraint-heavy prompt you can copy and paste directly to Gemini 3.1 Pro. It integrates the QA resolutions from our previous conversation, the explicit Prop Firm rules from the Master Blueprint, and the rigorous memory optimization syntax (like banning `ta.barssince()` and using macro-anchors) dictated by the latest Pine Script v6 research.

---

### **📋 COPY AND PASTE THIS TO GEMINI:**

**Prompt:**

**Role:** You are an elite algorithmic trading developer specializing in Pine Script v6 and direct-market-access (DMA) institutional routing.

**Task:** Write **ONLY** the Pine Script v6 code for **Phase \[3\]** of the "God Mode" Multi-Timeframe NQ Scalping Engine. You already have the Master Blueprint framework. **Do NOT write the code for any other phases, and do NOT provide placeholder comments for the rest of the script.** Your output should strictly contain the specific module requested.

**Execution Environment:** The code must be optimized for Tradovate's cloud-based matching engine via TradingView webhooks, targeting strict compliance with MFFU, Lucid Trading, and Tradeify rule sets.

**Critical Constraints & Prop Firm Rules You MUST Implement (Apply those relevant to this Phase):**

**1\. The Fractional Scaling Trap (Strict Integer Math):** Tradovate’s API explicitly rejects fractional futures contracts. You are strictly forbidden from using `qty_percent` parameters in `strategy.exit()`. For dynamic exits and Tradeify's 15 mini-contract scaling limits, you must calculate precise integer values natively using `math.max(1, math.round(qtyInput * ratio))` and pass absolute whole-number integers into `strategy.close(qty=...)`. Allow asynchronous `f_eval` triggers to scale out smoothly.

**2\. The Microscalping Filter (10-Second Temporal Delay):** To prevent flagging Tradeify’s microscalping bans, the "Waterfall SL" or trailing stop logic must include a temporal minimum delay condition: `if (time - entry_time) > 10000`. The algorithm must hold the macro HTF stop-loss for a minimum of 10 seconds before cascading the trailing stop tightly behind micro-liquidity pools.

**3\. The Bar-Time Death Trap & Compliance Engine:** The MFFU 16:09 EST daily flatten must NOT be bound to `process_orders_on_close = true` bar-close dependencies, which causes latency liquidations. It must be built as an independent tick-based `alertcondition()` using `calc_on_every_tick = true`, or the strategy trigger must be shifted to 16:08 EST. For the News Killzone Array, use a hardcoded Unix timestamp matrix (e.g., NFP, FOMC) to block new entries and flatten trades; external API calendars are banned to prevent webhook timeouts.

**4\. Institutional Execution Handicaps:** Strategy declarations must simulate Tradovate market-order latency. Strictly use the Master Blueprint's cloud-routed handicaps: `initial_capital = 50000`, `slippage = 2`, `commission_value = 2.50`, `commission_type = strategy.commission.cash_per_contract`, `use_bar_magnifier = true`, and `process_orders_on_close = true`. Also set `max_lines_count=500` and `max_boxes_count=500` to respect memory limits.

**5\. Zero-Repainting MTF Tuples (The Institutional Standard):** To avoid lookahead bias and memory limits, you must pack all 16 structural variables (Close, High, Low, Smart Trail, States, EMA, ATR, Swings, Exhaustions) into a single array tuple per timeframe inside `request.security()`. You must use the absolute safest zero-repainting configuration: `lookahead=barmerge.lookahead_on` combined with a strict \`\` historical offset applied to *every* calculation to evaluate only the previous closed HTF bar.

**6\. Cycle Memory & State Tracking (Ban `ta.barssince`):** You are strictly forbidden from using `ta.barssince()`, as it crashes the MTF engine by forcing deep historical scans. Use single-threaded native `var` declarations for state tracking. Establish immutable macro-anchors (`bar_index` and `price`) the moment a structural event occurs. Use forward-rolling counters (`var int count = na; count := condition ? 0 : count + 1`) and calculate distances natively via `bar_index - anchorBar`.

**7\. The `f_eval` Master Parser Routing:** The `f_eval` function must act as a universal translator using `switch` blocks to route UI timeframe strings ("HTF 1", "HTF 2", "HTF 3") to their respective tuple arrays. It must accept a `dir` argument (1 for Long, \-1 for Short) to automatically align directional logic across timeframes.

**8\. Non-Repainting Visuals:** Bypass `plot()` buffer limits. Draw Entry, SL, and TP lines dynamically using `line.new()` and `line.set_x2()`, permanently freezing them on the bar the trade closes to create a flawless visual audit trail.

**Output:** Provide ONLY the fully optimized Pine Script v6 code block for **Phase \[3\]** with brief inline technical comments explaining how the constraints were satisfied.

