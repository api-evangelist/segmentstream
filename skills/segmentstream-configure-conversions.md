---
name: Configure conversions and query performance
description: >-
  Use the SegmentStream MCP server to define a conversion (event or SQL-based)
  and read its statistics and geographic breakdown.
api: mcp/segmentstream-mcp.yml
transport: https://mcp.segmentstream.com/mcp
operations:
  - list_active_projects
  - list_conversions
  - get_conversion
  - create_conversion
  - list_conversion_fields
  - get_conversion_statistics
  - get_conversions_by_country
---

# Configure conversions and query performance

Conversions are the events SegmentStream attributes marketing spend against.
Use the MCP server to inspect, create, and analyze them.

## Auth
OAuth 2.0 authorization-code + PKCE (browser sign-in). Creating/updating
conversions requires a user role with configuration permissions.

## Steps
1. `list_active_projects` — pick the project.
2. `list_conversions` — see what is already defined; `get_conversion` for detail.
3. `list_conversion_fields` — discover the data fields available for a new conversion.
4. `create_conversion` — define a purchase/custom event conversion, or a SQL
   conversion against raw BigQuery data (offline/CRM conversions).
5. `get_conversion_statistics` — read totals and historical trends to validate it fires.
6. `get_conversions_by_country` — break performance down geographically.

## Rules
- Prefer `update_conversion` over creating duplicates when refining an existing one.
- Validate a new conversion with `get_conversion_statistics` before exporting it to
  ad platforms — a misconfigured conversion pollutes downstream bidding.
