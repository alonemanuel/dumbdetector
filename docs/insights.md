# Insights

- **[2026-04-05 23:10]** No "Downdetector for AI quality" exists. Uptime monitoring, enterprise LLM observability, and head-to-head benchmarking all exist — but nothing lets everyday users crowdsource-report "Claude feels dumb today" and see if others agree.

- **[2026-04-05 23:10]** Pain point is massively validated. Multiple viral HN/Reddit/GitHub threads about model quality regressions. Anthropic postmortems confirmed infrastructure bugs degraded quality (Aug-Sep 2025, Jan 2026) with weeks of silence before acknowledgment.

- **[2026-04-05 23:10]** Existing landscape covers uptime (IsDown.app), relative ranking (LMSYS Chatbot Arena), enterprise LLM monitoring (Langfuse, Confident AI, Traceloop, Arize, Braintrust), and official status pages — none address subjective day-to-day quality as felt by users.

- **[2026-04-05 23:15]** Two dimensions of "dumb": beyond subjective quality ("is the model dumb"), there's token burn rate ("is the model wasteful"). Models eating tokens too fast — verbose responses, runaway agent loops, unnecessary tool calls — is a distinct, measurable degradation that directly correlates with cost. A spike in both signals simultaneously is a very strong regression indicator.

- **[2026-04-07 16:30]** Deep research completed: 13 distinct mechanisms identified for how models "become dumb" even though they are "just weights." See docs/research-model-degradation.md for the full taxonomy.
