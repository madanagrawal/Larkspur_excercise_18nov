# PITCH.md

Six lines and a lever. Your words. The last two are scored.

Built: An autonomous flight disruption resolution agent on Claude 3.5 Sonnet integrated with Altura booking lookup, OpsFeed live flight status, deterministic policy evaluation, and two decoupled tools (next_available_day and fare_rules) running over Model Context Protocol (stdio). 
Does: Identifies cancellations and delays, determines exact policy entitlements (waivers, meal vouchers, discounted hotel rates), searches upcoming open-seat inventory forward by date, and escalates out-of-scope passengers (groups, unaccompanied minors) to human queues. 
Number: 2,810 schema tokens per turn across 11 tools over MCP, completing all 5 Stage-1 disruption tickets in 18 turns (56,355 fresh input tokens) with 5/5 returning verified resolution summaries. 
Safety check: Two-phase commit gating irreversible rebooking confirmations behind client-minted confirmation tokens (preventing hallucinated booking writes), refusal to process refunds without human escalation, and unassisted routing of group reservation G2HL9V to human queue GRP1. 
Next: Implement prompt caching to lower the 2,810-token per-turn schema tax, add dedicated tone guardrails and legal-threat escalation rules for abusive passengers (R8KD3F), and expand next_available_day to check multi-passenger party capacity. 
Still broken: Abusive passengers threatening legal action (R8KD3F) receive routine voucher offers instead of immediate de-escalation or human handoff; next_available_day checks single-seat inventory only; and reaching the 8-turn ceiling terminates the loop without automated human queue dispatch.
Lever: <cost | speed | intelligence>

## Priya asked

Costs:
Wrong:
Runs it:
Left out:
