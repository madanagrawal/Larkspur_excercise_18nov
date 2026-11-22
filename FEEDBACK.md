# Client review: Larkspur disruption-care agent

**To:** Larkspur_excercise_18nov  
**From:** Larkspur client review agent, on behalf of Priya Raghavan  
**Re:** the disruption-care agent you walked us through in our last session  
**Generated:** 2026-11-22 21:45

## Priya's note

> Our vendor says we should just be using your best model.
>
> Why aren't we?
>
> Priya Raghavan, Larkspur Airlines

She sent that before this session opened. She means it. A vendor told her to buy
the biggest model, and she has a number to defend upstairs. Her four questions from
day one are still open. Naming a model answers none of them.

## Still open from day one

| Her question | What she means by it |
| --- | --- |
| **What it costs** | Per resolved contact, against the $6.90 a human contact costs us. |
| **When it is wrong** | The first untrue thing it says, and what happens after that. |
| **Who runs it** | In June, after you have left. |
| **What you left out** | The scope you cut, and why. |

## What the review agent found

Larkspur pointed a review agent at your repository. It read the
code. It did not run your agent, and the only file it changed is this one. Each
item below names the file and the line it is about.

**1. PITCH.md Number claims 18 turns and 56,355 input tokens for 5 tickets; the only committed run is readout-trace.json at 4 turns, 17604 in.**

readout-trace.json records one run: 4 API turns, 5 tool calls, 17604 tokens in, 1104 out, 17.0s. Nothing in the repository holds a run of all five Stage-1 tickets, and no bench output is in the material. The 18, the 56,355 and the 5/5 verified summaries have no artifact under them that I can open.

Run python3 bench.py --label after --stage 1 --runs 3, then python3 bench.py --compare after after, and paste the compare output with the turn and fresh-token totals.

**2. PITCH.md says '2,810 schema tokens per turn across 11 tools'; the static scan of agent.py finds 9 schemas, and the diff adds none to EXTRA_TOOLS.**

EXTRA_TOOLS is still 0 declared and tool_list() now returns build_tools() + mcp_client.tools(). The two extra tools PITCH.md names, next_available_day and fare_rules, come from the MCP side, so the count of 11 depends on what mcp_client.tools() returns at run time. Nothing committed shows that list or where 2,810 was computed.

Run python3 run.py --tool-tax and python3 run.py --show-tools and paste both outputs.

**3. The PITCH.md Next line plans prompt caching; agent.py has no cache_control anywhere and readout-trace.json shows 0 read, 0 written.**

All four turns of the committed run paid for the full prefix: 17604 tokens in with a cache hit ratio of None. The system argument is runtime_preamble() + SYSTEM_PROMPT + TONE_ADDENDUM rebuilt on every call, and the tools list is resent on every turn. A bigger model would be billed for exactly this same prefix, so the per-turn cost Priya is asking about is set by the loop structure, not by the model name.

Run python3 run.py --all --trace and paste the totals footer so the per-turn input tokens sit next to the 2,810 schema figure.

**4. agent.py run_agent() still has MAX_TOOL_CALLS = 8 and PITCH.md Still broken says reaching it ends the loop with no handoff.**

The while condition is response.stop_reason == "tool_use" and turns < MAX_TOOL_CALLS, and your diff leaves the cap and the exit path untouched. When the cap hits, the function returns text_of(response), which is empty if the last turn was a tool_use block. The customer gets a blank reply and no human receives the case. No model upgrade changes that branch.

Run python3 run.py R8KD3F --trace and paste the final turn showing what the customer sees and whether escalate_to_human was called.

**5. No evals/cases.json exists, and PITCH.md names R8KD3F as failing with 'routine voucher offers' without a recorded run.**

The repository holds no eval cases, so the claimed failure and the claimed passes both rest on your say-so. TONE_ADDENDUM is still 0 characters, which is consistent with the abusive-passenger case being unhandled. Your diff did tighten search_alternatives from 'search' to a 312-character description, and changed the loop to append response.content. Neither change has a before-and-after run committed.

Run python3 eval_harness.py and paste the pass and fail lines, including whichever case covers R8KD3F.

## Your four answers

The four lines under `## Priya asked` in your PITCH.md are still empty. They
are one line each and they are not a coding job: cost, what happens when it is
wrong, who runs it in June, and what you left out. Whoever on your side is not
editing agent.py is the right person to write them, and they are the four
things I will ask about first.

## Before our next meeting

> Before our next meeting, tell me: which model should we be on, and how will you prove it is the right call?
>
> Priya Raghavan, Larkspur Airlines

Bring two things. A recommendation, and the measurement behind it. If the model is
not the problem, say so, and bring the number that shows it.

## What this review read

- `agent.py (238 lines)`
- `PITCH.md`
- `TEAM.md`
- `readout-trace.json`
- given files that differ from the shipped pack: `verify.py`, `eval_harness.py`, `bench.py`, `readout.py`, `setup.py`, `pod_sync.py`, `run.py`, `review.py`, `support/__init__.py`, `support/client.py`, `support/data.py`, `support/models.py`, `support/tools.py`, `support/trace.py`, `support/offline.py`, `support/mock_backend.py`, `support/mcp_client.py`, `support/mcp_server.py`, `support/mcp_selftest.py`, `support/access.py`, `support/keyscan.py`, `support/review.py`

Reviewer: `claude-sonnet-5-5`. Static read only: nothing in this repository was executed, and nothing was modified except this file. Larkspur Airlines is a fictional training scenario. Confidential, do not distribute.
