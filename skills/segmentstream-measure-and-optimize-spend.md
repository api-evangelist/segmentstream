---
name: Measure attribution and optimize ad spend
description: >-
  Connect to a SegmentStream workspace over MCP, pull a cross-channel
  attribution report, and turn marginal-ROAS budget recommendations into an
  optimized spend plan.
api: mcp/segmentstream-mcp.yml
transport: https://mcp.segmentstream.com/mcp
operations:
  - list_active_projects
  - get_project
  - run_report
  - run_report_timeseries
  - list_portfolios
  - get_portfolio_optimization
  - get_portfolio_history
---

# Measure attribution and optimize ad spend

Use the SegmentStream MCP server (`https://mcp.segmentstream.com/mcp`, Streamable
HTTP) to measure marketing performance and produce a budget-reallocation plan.
Access is **read-only** for reporting.

## Auth
OAuth 2.0 authorization-code + PKCE (S256). On first call the client opens a
browser sign-in; approve access. The agent inherits the signed-in user's role.

## Steps
1. `list_active_projects` — pick the target project; note its `id`, timezone, currency.
2. `get_project` — confirm configured data sources, conversions, and attribution models.
3. `run_report` — pull an aggregated attribution report (spend, conversions, ROAS)
   broken down by the channel/campaign dimensions you care about.
4. `run_report_timeseries` — get the per-date trend to spot momentum or decay.
5. `list_portfolios` — find the budget-optimization portfolio for this project.
6. `get_portfolio_optimization` — read the marginal-ROAS recommendations and the
   suggested budget reallocation across channels.
7. `get_portfolio_history` — validate that past recommendations improved adoption /
   prediction accuracy before you trust the new plan.

## Rules
- Never write budgets back blindly — present the recommended reallocation for
  human approval (this MCP surface is analysis, not ad-account mutation).
- Respect the project timezone and currency when summarizing numbers.
- If a report is empty, check `get_data_source_logs` / `list_incidents` before concluding.
