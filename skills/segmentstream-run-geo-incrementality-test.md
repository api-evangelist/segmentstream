---
name: Read geo-incrementality experiment results
description: >-
  Use the SegmentStream MCP server to list geo-lift experiments and read the
  statistical results of an incrementality test measuring the real sales impact
  of paid media.
api: mcp/segmentstream-mcp.yml
transport: https://mcp.segmentstream.com/mcp
operations:
  - list_active_projects
  - list_experiments
  - get_experiment
  - list_experiment_custom_parameter_keys
---

# Read geo-incrementality experiment results

Geo tests run controlled test/control markets to measure the incremental sales
impact of ad spend. Use the SegmentStream MCP server to read their results.

## Auth
OAuth 2.0 authorization-code + PKCE (browser sign-in). Read-only.

## Steps
1. `list_active_projects` — select the project.
2. `list_experiments` — enumerate active and completed geo-lift tests.
3. `list_experiment_custom_parameter_keys` — discover the segmentation options
   available for slicing the chosen experiment.
4. `get_experiment` — read the experiment's statistical findings (lift, confidence,
   incremental conversions/revenue) for the test vs. control markets.

## Rules
- Report the confidence/significance alongside the point estimate — never quote
  lift without its statistical qualifier.
- Distinguish completed from still-running experiments; do not treat interim
  numbers as final.
