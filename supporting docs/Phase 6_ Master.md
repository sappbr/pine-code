Here is the highly technical, constraint-heavy prompt tailored for Gemini 3.1 Pro to specifically generate **Phase 6: Waterfall Risk Engine & Visuals**. It integrates the exact QA resolutions for the Fractional Scaling Trap, the Tradeify Microscalping Filter, the Abort/Snap Waterfall logic, and the zero-repainting visual architectures required for this phase.

---

### **📋 COPY AND PASTE THIS TO GEMINI:**

**Prompt:**

**Role:** You are an elite algorithmic trading developer specializing in Pine Script v6 and direct-market-access (DMA) institutional routing.

**Task:** Write **ONLY** the Pine Script v6 code for **Phase 6: Waterfall Risk Engine & Visuals** of the "God Mode" Multi-Timeframe NQ Scalping Engine. You already have the Master Blueprint framework. **Do NOT write the code for any other phases, and do NOT provide placeholder comments for the rest of the script.** Your output should strictly contain the specific module requested.

**Execution Environment:** The code must be optimized for Tradovate's cloud-based matching engine via TradingView webhooks, targeting strict compliance with MFFU, Lucid Trading, and Tradeify rule sets.

**Critical Constraints & Prop Firm Rules You MUST Implement for Phase 6:**

**1\. The Fractional Scaling Trap (Strict Integer Math):** Tradovate’s API explicitly rejects fractional futures contracts. You are strictly forbidden from using Grok's suggested `qty_percent` parameters in `strategy.exit()`. To handle dynamic exits and Tradeify's massive 15+ mini-contract scaling limits, you must calculate precise integer values natively using `math.max(1, math.round(qtyInput * ratio))`. Pass these absolute whole-number integers directly into `strategy.close(qty=...)` and allow asynchronous `f_eval` triggers to scale out smoothly.

**2\. Tradeify Microscalping Filter (10-Second Temporal Guard):** Tradeify actively bans "microscalping" (holding \>50% of trades for under 10 seconds). Your dynamic Waterfall SL logic must include a temporal minimum delay condition: `if (time - strategy.opentrades.entry_time(strategy.opentrades - 1)) > 10000`. The algorithm must maintain the wider macro HTF stop-loss for a minimum of 10 seconds before it is authorized to step down and cascade the trailing stop tightly behind micro-liquidity pools.

**3\. Institutional Waterfall SL Engine (Abort vs. Snap Logic):** The risk engine must evaluate the MTF Smart Trail arrays sequentially when a trade is armed. It must check the distance to the HTF 3 Smart Trail against the `maxSlTicks` input. If it exceeds the limit, it must step down to HTF 2, then HTF 1\. If all timeframes exceed the tick limit, it must read a `failsafeAct` string variable:

* If **"Abort"**: Execute the Abort Trade Completely sequence (cancel the trade to protect capital).  
* If **"Snap"**: Ignore the structural geometry, hard-cap the risk exactly at `maxSlTicks`, and send the entry order.

**4\. Max Adverse Excursion (MAE) Hard Failsafe:** You must build a dollar-based MAE hard failsafe to override all structural stops. Track real-time MAE using `var` persistence on `strategy.position_avg_price` and per-bar unrealized loss. If the MAE exceeds a user-defined `maxLossTrade` equity percentage (e.g., 2%), immediately force a `strategy.close()` to flatten the trade.

**5\. Zero-Repainting Visuals (Object Management):** You must bypass `plot()` buffer limits and repainting issues for dynamic trade paths. Draw Entry, SL, and TP lines dynamically using `line.new()` and `line.set_x2()`. Ensure lines are only created on `barstate.islastconfirmedhistory` and are permanently frozen on the bar the trade closes to create a flawless visual audit trail. Implement a garbage collection array to delete old lines, respecting `max_lines_count=500`.

**6\. Memory Mandates:** Use single-threaded native `var` declarations for state tracking instead of memory-crashing loops like `ta.barssince()`. Use `group` and `inline` parameters for all UI `input()` declarations.

**Output:** Provide ONLY the fully optimized Pine Script v6 code block for **Phase 6** with brief inline technical comments explaining how the constraints were satisfied.

