### Key Implementation Steps for Master Prompt
- **Core Objective**: The master prompt guides a multi-agent AI system in GitHub Codespaces to refactor your Pine Script v6 strategy by externalizing indicators, reducing script complexity to address token, memory, and performance issues.
- **Approach**: It strips internal indicator logic, creates external indicator(s) that plot necessary values/signals, and modifies the strategy to use `input.source()` for connecting to these plots (similar to LuxAlgo's Lucid Connectors but using standard Pine Script mechanics).
- **Agent Structure**: Utilizes up to 10 specialized subagents for parallel tasks like analysis, code generation, testing, and integration to ensure thorough, error-free implementation.
- **Expected Outcome**: A lighter strategy script with dynamic connections to chart-added indicators, allowing flexible use of external signals for filters, entries, stops, trails, etc., without recompiling the full indicator code.

#### How It Mimics Lucid Connectors
In standard Pine Script v6, there's no built-in "placeholder" system like LuxAlgo's, but we achieve similar functionality via `input.source()` inputs. External indicators plot values (e.g., 1 for bullish trigger, NaN otherwise), and the strategy selects these plots in its settings. This "populates" fields dynamically when adding the indicator to the chart.

#### Usage Instructions
Copy the master prompt below into your main AI agent in GitHub Codespaces. Provide your existing strategy code as input. The system will delegate tasks to subagents, output refactored scripts, and suggest testing steps.

#### Master Prompt
```
You are the Master Agent overseeing a team of up to 10 specialized subagents in GitHub Codespaces. Your goal is to refactor an existing Pine Script v6 trading strategy to address issues with max tokens, memory, and performance. The user wants to strip out all internal indicator code and add a feature to use outputs from an external indicator added to the chart. This external indicator will provide plots for signals and values, which the strategy will access via input.source() inputs. These can then be used as filter conditions, entry triggers, stop loss (SL), take profit (TP), trailing stops, break-even (BE), etc., similar to LuxAlgo's Lucid Connectors (where placeholders expose trigger booleans from toolkits).

Key Requirements:
- Externalize all indicators: Move calculation logic to one or more separate indicator scripts that plot all necessary values (e.g., boolean signals as 1/NaN, levels as floats).
- Modify strategy: Replace internal calculations with input.source() for each required value. Ensure inputs are titled clearly (e.g., "Bullish Entry Trigger", "Stop Loss Level").
- Populate fields: When the strategy is added to the chart with the external indicator, users can select plots in the strategy's input settings to "populate" the fields dynamically.
- Handle booleans: For triggers/filters, external plots should use 1 for true, NaN for false; strategy checks with conditions like 'if input_var == 1'.
- Performance optimization: Minimize code in strategy to reduce compilation tokens and runtime load.
- Compatibility: Use Pine Script v6 features like dynamic requests if needed, but keep it simple.
- Testing: Validate on sample data; ensure no loss of functionality.

Subagents and Their Roles (use max 10; delegate tasks in parallel where possible):
1. Code Analyzer: Parse the existing strategy code. Identify all indicators (e.g., RSI, MA), their calculations, and usages (e.g., in entries, exits, filters).
2. Dependency Mapper: Map dependencies between indicators and strategy logic. List all required outputs (signals, values) to externalize.
3. Indicator Designer: Design and generate separate indicator script(s) – preferably one consolidated indicator with multiple plots for all values.
4. Input Generator: Create input.source() declarations for the strategy, matching each external plot.
5. Logic Refactorer: Replace internal logic in the strategy with references to the new inputs.
6. Error Checker: Scan for syntax errors, v6 compatibility issues, and logical inconsistencies.
7. Optimizer: Optimize code for performance (e.g., remove unused vars, simplify conditions).
8. Tester: Write test cases; simulate backtests in Codespaces (use sample data if needed).
9. Documenter: Generate documentation on how to add/use the external indicator and connect inputs.
10. Integrator: Combine all outputs into final scripts; version control in Codespaces.

Workflow:
1. Receive existing strategy code from user.
2. Delegate analysis to Subagents 1-2.
3. Based on analysis, have Subagents 3-5 generate new code.
4. Use Subagents 6-7 to refine.
5. Test with Subagent 8.
6. Document and integrate with Subagents 9-10.
7. Output: Refactored strategy script, external indicator script(s), setup guide, and any warnings.

Communicate via structured messages (e.g., JSON for task delegation). Ensure all subagents collaborate in Codespaces environment. If issues arise, escalate to me for decision.

Start by confirming receipt of code and outlining plan.
```

---
### Comprehensive Guide to Refactoring Your Pine Script v6 Strategy with External Indicators

This detailed survey expands on the master prompt's implementation, drawing from Pine Script v6 documentation and best practices for externalizing indicators. It covers the rationale, step-by-step mechanics, code examples, potential challenges, and optimization strategies. The goal is to create a modular system where your strategy becomes lightweight, relying on chart-added indicators for computations—directly addressing token limits (Pine Script has compilation constraints around ~65k tokens), memory usage in runtime, and performance bottlenecks from complex internal loops or calculations.

#### Rationale and Benefits
TradingView's Pine Script v6, released on December 10, 2024, introduces enhancements like dynamic `request.*()` functions, improved boolean evaluation for faster condition checks, and better loop handling within local scopes. These are ideal for streamlined strategies but don't directly solve script bloat from embedded indicators. By externalizing:

- **Token/Memory Reduction**: Internal indicators (e.g., multiple TA functions like `ta.rsi()`, `ta.sma()`) inflate script size. Moving them out can cut tokens by 50-80%, based on complexity.
- **Performance Gains**: Calculations run in separate indicators, reducing strategy runtime load. v6's lazy boolean evaluation further speeds up condition checks using external inputs.
- **Flexibility Like Lucid Connectors**: LuxAlgo's system exposes boolean placeholders (e.g., `{bullish_choch}`) from toolkits to backtesters. In standard Pine, `input.source()` achieves this by letting users select external plots in settings, "populating" strategy fields dynamically. No auto-detection, but manual selection mimics connector behavior.
- **Modularity**: Update indicators independently without recompiling the strategy. Supports multi-indicator setups if needed.

Limitations from LuxAlgo docs: Only triggers (booleans) are typically exposed, but in Pine, you can plot floats for levels (SL/TP). Ensure external plots are visible/selectable.

#### Step-by-Step Implementation Mechanics
1. **Analyze Existing Strategy**:
   - Identify indicators: Scan for `ta.*()` calls, custom functions computing values.
   - Map usages: Note where outputs feed into `strategy.entry()`, `strategy.exit()`, filters (e.g., `if rsi > 70`), SL/TP calculations (e.g., `close - atr*2`).

2. **Create External Indicator Script**:
   - Consolidate into one indicator for simplicity, with multiple `plot()` calls.
   - For booleans: Plot 1 for true, `na` for false.
   - For levels: Plot float values (e.g., SL level).
   - Use v6 features: Dynamic requests if pulling external data (e.g., `request.security()`).

   Example Indicator Script (assuming original has RSI and MA):
   ```pinescript
   //@version=6
   indicator("External Indicators Bundle", overlay=true)
   
   // RSI Calculation
   rsi_length = input.int(14, "RSI Length")
   rsi_val = ta.rsi(close, rsi_length)
   rsi_overbought = rsi_val > 70 ? 1 : na  // Boolean signal
   plot(rsi_overbought, "RSI Overbought Trigger", color.red, style=plot.style_linebr)
   
   // MA Calculation
   ma_length = input.int(50, "MA Length")
   ma_val = ta.sma(close, ma_length)
   ma_cross_up = ta.crossover(close, ma_val) ? 1 : na
   plot(ma_cross_up, "MA Cross Up Trigger", color.green, style=plot.style_linebr)
   plot(ma_val, "MA Level", color.blue)  // For use in TP/SL if needed
   ```

3. **Modify Strategy to Use Inputs**:
   - Add `input.source()` for each plot.
   - Replace internal logic: E.g., `if rsi > 70` becomes `if rsi_input == 1`.
   - For SL/TP/Trail/BE: Use inputs in `strategy.exit()` or conditions.
   - Handle NaN: Use `nz()` or checks.

   Example Refactored Strategy:
   ```pinescript
   //@version=6
   strategy("Optimized Strategy", overlay=true)
   
   // Inputs for external plots
   rsi_ob_input = input.source(na, "RSI Overbought Trigger")
   ma_cross_input = input.source(na, "MA Cross Up Trigger")
   ma_level_input = input.source(close, "MA Level for TP/SL")
   
   // Entry Trigger (using external)
   long_condition = ma_cross_input == 1 and rsi_ob_input != 1  // E.g., cross up without overbought
   
   if long_condition
       strategy.entry("Long", strategy.long)
   
   // SL/TP using external level
   sl_level = ma_level_input - (close * 0.02)  // Example adjustment
   strategy.exit("Exit Long", "Long", stop=sl_level, limit=ma_level_input + (close * 0.05))
   
   // Trailing/BE example (add more inputs if needed)
   // Assume external plots a trail offset
   ```

4. **Setup on Chart**:
   - Add external indicator first.
   - Add strategy; in settings > Inputs, select the indicator's plots (e.g., "RSI Overbought Trigger" from the dropdown).
   - This "populates" the fields, allowing use in filters/entries.

5. **Handling Advanced Features**:
   - **Trailing Stops**: Input a trail offset plot; use in `strategy.exit(trail_offset=...)`.
   - **Break-Even**: Input a BE trigger plot; condition to move SL to entry.
   - **Filters**: Chain multiple inputs (e.g., `if filter1_input == 1 and filter2_input == 1`).
   - v6-Specific: Use dynamic `request.security()` in external indicator for multi-timeframe data without static limits.

#### Potential Challenges and Solutions
- **Input Limitations**: Max 64 inputs in Pine; consolidate plots if exceeding.
- **NaN Handling**: Use `na(input_var) ? fallback : input_var`.
- **Performance Testing**: Backtest on historical data; monitor compile time.
- **Compatibility**: Ensure external indicator uses `@version=6`; test on various symbols/timeframes.
- **Security Calls**: If original has `request.security()`, move to external to further lighten strategy.

| Component | Original Approach | Refactored Approach | Benefit |
|-----------|-------------------|---------------------|---------|
| Indicators | Embedded `ta.rsi()`, etc. | Separate script with plots | Reduces strategy tokens by moving computations |
| Entries/Filters | Internal conditions | `input.source()` checks (e.g., ==1) | Dynamic; changeable without code edits |
| SL/TP/Trail | Calculated inline | Based on input levels | Flexible for external optimizations |
| Performance | High memory from loops | Lazy eval in v6 + external offload | Faster runtime, lower lag |

#### Multi-Agent System in GitHub Codespaces
In Codespaces, set up with tools like LangChain or Auto-GPT for subagents. Each subagent can run in parallel containers. Max subagents (10) ensure division of labor: Analysis (2 agents), Generation (3), Validation (3), Finalization (2). This prevents bottlenecks in complex refactors.

#### Example Full Workflow Output
After running the prompt:
- External Indicator: As above.
- Refactored Strategy: As above.
- Guide: "1. Add indicator to chart. 2. Add strategy. 3. In strategy inputs, select plots from indicator dropdowns."

This approach ensures your strategy is future-proof, leveraging v6's efficiencies while emulating Lucid's connectivity.

#### Key Citations
- [TradingView Blog: Pine Script v6 Release](https://www.tradingview.com/blog/en/pine-script-v6-has-landed-48830) 
- [TradingView Pine Script Docs: Release Notes](https://www.tradingview.com/pine-script-docs/release-notes) 
- [TradingView Support: Applying Indicators to Others](https://www.tradingview.com/support/solutions/43000474048-how-to-apply-an-indicator-or-strategy-to-another-indicator) 
- [TradingView Blog: External Inputs for Strategies](https://www.tradingview.com/blog/en/use-an-input-from-another-indicator-with-your-strategy-19584) 
- [LuxAlgo Docs: Lucid Connectors](https://docs.luxalgo.com/docs/ai-backtesting/backtesters/strategy-scripting/lucid-connectors) 
- [TradingView Blog: More External Inputs](https://www.tradingview.com/blog/en/more-external-inputs-for-scripts-38014)