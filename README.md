# trading-indicators

This folder contains TradingView Pine v5 indicators used to inspect NIFTY option market direction. The primary script is `market-direction.pine` which shows ATM and ITM option candles and computes a basic market-direction status using yesterday+today value sums.

Files
- `market-direction.pine` — Pine v5 indicator. Plots ATM/ITM option candles (when provided), computes per-instrument status and a global status, and shows a small table with key fields.

Quick usage
1. Copy the contents of `market-direction.pine` into TradingView Pine Editor and click Add to Chart.
2. The script requires option contract symbols as inputs. Because Pine does not allow runtime-built symbol strings to be used with `request.security`, the indicator offers two modes:
	 - Auto-detect (suggestion mode): compute likely ATM/ITM contract symbols for the next weekly expiry and show them in the table. You must copy the suggested symbol strings into the manual inputs for the script to fetch option OHLC.
	 - Manual mode: paste exact option contract symbols into the four manual symbol inputs in the script.

Inputs
- `Auto-detect symbols from NSE:NIFTY` — toggle suggestion mode on/off
- `ATM Call Symbol (manual)` — static symbol used by `request.security` for ATM call
- `ATM Put Symbol (manual)` — static symbol used by `request.security` for ATM put
- `ITM Call Symbol (manual)` — static symbol used by `request.security` for ITM call
- `ITM Put Symbol (manual)` — static symbol used by `request.security` for ITM put
- `Show ATM Candles` / `Show ITM Candles` — toggle plotting of candles

Notes about auto-detect and TradingView limitations
- Pine Script cannot pass runtime (series) strings into `request.security`. That means the script cannot programmatically request option symbols whose names are constructed at runtime.
- To work around this, the script computes suggestion strings for ATM/ITM option symbols (based on the underlying NIFTY daily open and nearest 50 strike) and displays them in the table for copy/paste.

Status logic
- The indicator computes simple status categories per the spec using (yesterday + today value) comparisons:
	- CALL PANIC
	- SELL PANIC
	- CALL PROFIT BOOKING
	- PUT PROFIT BOOKING
	- SIDEWAYS

Troubleshooting
- If you see parse errors: ensure you're on Pine v5. Helper functions must be defined at top-level and chained ternary expressions can be fragile — the script already uses if/else ladders.
- If the suggestion strings are empty or incorrect, confirm the chart symbol is `NSE:NIFTY` and daily data is available in your TradingView subscription/market data.

Enhancements (ideas)
- Display the computed suggestion strings as a separate table or label for easier copy/paste (I can add this).
- Allow a small UI button or indicator to auto-fill manual inputs (not possible in Pine; external helper required).

If you want, I can update the Pine script to show suggestion strings in the table or add an extra table row with the computed symbol suggestions. Tell me which display you prefer (table row per instrument, or a compact label list).

