This is a handover of the project from one AI agent to another.

PROJECT: Minter v3
https://github.com/danieldoderlein/Minter-3


Roles:
- Architect: ChatGPT (design, sequencing, validation logic, guardrails)
- Product Owner: Daniel (the user) (decide scope, constraints, acceptance criteria and orchestrate between Architect and Developer)
- Developer: Antigravity (Agentic AI Developer) (implementation only, no architectural decisions)

Details about the project:
- See README.md

Architecture:
- See ARCHITECTURE.md

Decisions made:
- See DECISIONS.md

Plan:
- See PLAN.md


Constraints:
- No drift from ARCHITECTURE.md
- Determinism must hold
- Tests must pass
- Keep layers isolated

Execution Model:
- ChatGPT defines scope, constraints, acceptance criteria
- ChatGPT asks Daniel for approval
- ChatGPT produces prompts (markdown)for Daniel to copy and paste to Antigravity
- Antigravity implements according to prompt in adherance with its rules and documents.
- No feature expansion unless explicitly approved
- No architecture changes without architect and user approval

Constraints:
- No drift from ARCHITECTURE.md
- Determinism must hold
- Tests must pass
- Keep layers isolated

Goal for this thread:
<single architectural objective>

Non-goals:
<explicitly state what we are NOT doing>

Prompt output example:
# Antigravity Prompt: Add MediumTermTrendFollowV1 + Initial Robustness Scan

Repo: Minter-3
Constraints:
- No architecture drift, keep layers isolated.
- Determinism must hold, tests must pass.
- Research-only strategy, no live/paper logic inside.
- Reuse existing daily rollup utilities/patterns from other daily strategies.

## 1) Implement strategy
Create:
src/minter/research/strategies/medium_term_trend_follow_v1.py

Class:
MediumTermTrendFollowV1 (ExecutionStrategy)

Params (defaults conservative):
- regime_sma_days: 200
- breakout_lookback_days: 100
- atr_lookback_days: 14
- atr_stop_mult: 3.0
- trailing_atr_mult: 3.0
- max_hold_days: 20
- exit_on_regime_flip: true

Entry (long only, once per day):
- Regime gate: close_today > SMA(regime_sma_days)
- Trend trigger: close_today > max(close over prior breakout_lookback_days)

Exit (first hit wins):
- If exit_on_regime_flip and close_today < SMA(regime_sma_days): exit
- Trailing stop: track highest_close_since_entry; if close_today <= highest_close_since_entry - trailing_atr_mult*ATR: exit
- Hard stop: if close_today <= entry_price - atr_stop_mult*ATR: exit
- Time stop: holding_days >= max_hold_days: exit

Other rules:
- One position per symbol at a time
- Deterministic, no RNG usage
- Warmup: do not trade until enough daily history exists for SMA + breakout + ATR

Wiring:
- Export in strategies/__init__.py if you maintain exports.

## 2) Tests
Add tests under tests/research/strategies/:
- determinism: same input twice yields identical trades/summary
- warmup: no trades before sufficient history
- regime gate: no entry when below SMA200
- trailing stop: triggers after peak then drawdown beyond threshold
- time stop: exits at max_hold_days

Keep tests small and deterministic.

## 3) Initial robustness scan (small grid)
After implementation, run robustness harness for universes:
- mega_cap
- index_etfs

Realism profiles:
- optimistic, realistic, pessimistic

Seed:
- 42

Parameter grid (small, not huge):
- breakout_lookback_days: 60, 100, 150
- trailing_atr_mult: 2.5, 3.0, 3.5
- max_hold_days: 10, 20, 30

Fixed:
- regime_sma_days=200
- atr_stop_mult=3.0
- atr_lookback_days=14

Output:
reports/medium_term_trend_follow_v1_initial_<date>/
- manifest.json (commands)
- summary.md

summary.md must include TEST metrics:
- median_pf
- median_return
- worst_decile_return
- percent_profitable
- coverage_ge_10
- effective spread_bps

Rank by pessimistic TEST median_pf, then worst_decile_return, and note stability across nearby params.

## Acceptance
- ruff/mypy/pytest pass
- No engine changes
- Report generated as specified