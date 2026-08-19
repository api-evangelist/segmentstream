---
name: Segmentstream
description: Use when building marketing attribution models, analyzing campaign performance, optimizing budget allocation across channels, configuring event tracking and data sources, or querying attribution data through natural language via MCP.
metadata:
    mintlify-proj: segmentstream
    version: "1.0"
---

# SegmentStream Skill

## Product summary

SegmentStream is a marketing attribution and optimization platform that consolidates first-party and third-party data to measure advertising ROI and KPIs. It uses machine learning to evaluate the incremental value of each marketing touchpoint across devices and browsers. Agents use SegmentStream to configure projects, define conversions, connect advertising platforms, run attribution reports, and optimize budget allocation.

**Key files and concepts:**
- Projects store all configuration, data, and reports
- Data sources connect advertising platforms (Google Ads, Meta, LinkedIn, etc.)
- Conversions define business outcomes to track and attribute
- Attribution models assign credit to marketing touchpoints (Last-Click, First-Click, Multi-Touch, Visit Scoring)
- Reports query campaign performance by dimension and metric
- Portfolios enable AI-powered budget optimization
- MCP (Model Context Protocol) provides natural language access to all data and configuration

**Primary docs:** https://docs.segmentstream.com

## When to use

Reach for this skill when:
- **Configuring a new project** — setting up data warehouse, event tracking, data sources, conversions, and attribution models
- **Analyzing campaign performance** — running reports, filtering by dimension, comparing attribution models, exporting data
- **Optimizing marketing spend** — creating portfolios, reviewing budget recommendations, applying optimization scenarios
- **Debugging attribution** — inspecting user journeys, checking conversion statistics, verifying data source imports
- **Connecting platforms** — authenticating advertising platforms, CRM systems, or analytics tools
- **Querying data via natural language** — using MCP to ask questions about campaigns, conversions, and performance without navigating the UI
- **Creating custom conversions** — writing SQL to define conversions from CRM or offline data
- **Setting up lead generation tracking** — configuring lead scoring, exporting qualified leads to ad platforms

## Quick reference

### Core configuration workflow

| Step | Action | Key settings |
|------|--------|--------------|
| 1. Create project | Set name, timezone, currency, server location | Project ID, timezone (IANA format), currency (ISO 4217) |
| 2. Connect data warehouse | Choose SegmentStream-hosted or Google BigQuery | BigQuery project ID, dataset ID, location |
| 3. Set up event tracking | GA4 BigQuery Export (recommended) or Adobe Analytics | GA4 property, BigQuery link, export location |
| 4. Connect data sources | Authenticate advertising platforms | Platform type, OAuth/API credentials, account selection |
| 5. Define conversions | Create simple, combined, or custom conversions | Conversion type, matching conditions, value type, deduplication |
| 6. Configure attribution models | Choose algorithm and window | Model name, algorithm (last-click/first-click/multi-touch/visit-scoring), window (days) |
| 7. Create reports | Add dimensions, metrics, filters | Date range, dimensions, conversion metrics, filters |
| 8. Set up optimization | Create portfolio with targets | Portfolio name, goal (ROAS/CPA), granularity, budget constraints |

### Conversion types

| Type | Use case | Availability | Key config |
|------|----------|--------------|-----------|
| Simple (Purchase) | E-commerce purchases from GA4 | All plans | Event type, filters, deduplication, adjustment window |
| Simple (Custom event) | Website events (sign-ups, form fills) | All plans | Event name, matching conditions, value type, deduplication |
| Combined | Merge 2+ simple conversions | All plans | Select conversions to combine, deduplication |
| Custom (SQL) | Offline/CRM conversions | Enterprise | BigQuery SQL query, required fields (id, client_id, created) |
| Lead Scoring | ML-powered lead qualification | Enterprise | Lead/sales data, target conversion, conversion window |
| Conversions Export | Send signals to ad platforms | Enterprise | Select conversion, platform, export schedule |

### Attribution models

| Model | Credit assignment | Best for | Configuration |
|-------|-------------------|----------|---------------|
| Last-Click | 100% to final touchpoint | Direct response, short cycles | Attribution window, significant traffic filter |
| First-Click | 100% to initial touchpoint | Awareness, top-of-funnel | No additional config |
| Multi-Touch | Credit if session increased conversion probability | Holistic view, long journeys | Significant traffic filter (paid/non-brand) |
| Visit Scoring | Fractional credit based on post-click behavior | Cross-device, cross-channel | Significant traffic filter |

### Report dimensions and metrics

**Traffic metrics:** cost, clicks, impressions, sessions, users, CPC, CPM, CTR

**Conversion metrics:** conversions, conversion_value, converted_users, conversion_rate, CPA, ROAS, AOV

**Dimensions:** campaign_name, ad_platform, channel, country, source_medium, device, custom dimensions

**Filters:** Boolean AST with operators (equals, contains, in, gt, gte, lt, lte, is_set, not_set)

### MCP commands (natural language)

```
"Show me the top 10 campaigns by ROAS for the last 30 days"
"What is the cost data quality score for our Facebook data source?"
"List all conversions configured in the project"
"Show me the user journey for anonymous ID abc123"
"Create a new attribution model with a 60-day window"
"Export this report as a shareable link"
```

## Decision guidance

### When to use each data warehouse option

| Scenario | SegmentStream-hosted | Google BigQuery |
|----------|---------------------|-----------------|
| Quick setup, no infrastructure | ✓ | — |
| Full data control and access | — | ✓ |
| Custom SQL queries on raw data | — | ✓ |
| Enterprise plan required | — | ✓ |
| Default for new projects | ✓ | — |

### When to use each conversion type

| Scenario | Simple | Combined | Custom (SQL) | Lead Scoring |
|----------|--------|----------|--------------|--------------|
| Track GA4 purchase events | ✓ | — | — | — |
| Track GA4 custom events | ✓ | — | — | — |
| Merge multiple conversions | — | ✓ | — | — |
| CRM/offline conversions | — | — | ✓ | — |
| ML-powered lead qualification | — | — | — | ✓ |

### When to use each attribution model

| Scenario | Last-Click | First-Click | Multi-Touch | Visit Scoring |
|----------|-----------|------------|-------------|---------------|
| Direct response campaigns | ✓ | — | — | — |
| Awareness/top-of-funnel | — | ✓ | — | — |
| Long customer journeys | — | — | ✓ | ✓ |
| Cross-device tracking | — | — | — | ✓ |
| Exclude direct/organic | ✓ | — | ✓ | ✓ |

### When to use MCP vs UI

| Task | MCP | UI |
|------|-----|-----|
| Query reports by natural language | ✓ | — |
| One-off analysis questions | ✓ | — |
| Configure new conversions | — | ✓ |
| Edit attribution models | — | ✓ |
| Connect data sources | — | ✓ |
| Explore user journeys | ✓ | ✓ |
| Run saved reports with overrides | ✓ | ✓ |

## Workflow

### Setting up a new SegmentStream project

1. **Create the project** — provide name, timezone (IANA format), currency (ISO 4217 code), and server location
2. **Choose data warehouse** — use SegmentStream-hosted for quick start, or Google BigQuery for enterprise control
3. **Set up event tracking** — connect GA4 via BigQuery Export (recommended) or Adobe Analytics
4. **Connect data sources** — authenticate each advertising platform (Google Ads, Meta, LinkedIn, etc.) and select accounts
5. **Define conversions** — create simple conversions for GA4 events, or custom conversions for CRM/offline data
6. **Configure attribution models** — create models with appropriate algorithm, window, and filters for your business
7. **Create reports** — add dimensions, metrics, and filters; save reports for recurring use
8. **Set up optimization** — create a portfolio with campaigns/channels as targets, set goal (ROAS/CPA), and review recommendations
9. **Implement SDK** (optional) — install SegmentStream SDK to improve user stitching and attribution quality

### Running an attribution report

1. **Navigate to Attribution** — go to the Attribution tab in the main menu
2. **Select or create a report** — choose a saved report or start from scratch
3. **Set date range** — use date filters above the chart; optionally compare two periods
4. **Add dimensions** — select how to group data (campaign, channel, country, etc.)
5. **Add metrics** — select traffic metrics (cost, clicks) and conversion metrics (conversions, ROAS, CPA)
6. **Apply filters** — filter by dimension values (e.g., "Ad platform equals Facebook Ads AND Country is UK")
7. **Choose attribution model** — select which model to use for conversion credit
8. **Review and export** — examine the table, sort by metric, export to CSV or Google Sheets
9. **Save the report** — click Save or Save as new to reuse the configuration

### Creating a custom conversion from CRM data

1. **Prepare BigQuery table** — load CRM data into BigQuery with required fields (id, client_id, created timestamp)
2. **Navigate to Conversions** — go to Settings > Conversions
3. **Click + ADD** — select Custom conversion
4. **Write SQL query** — query the BigQuery table; include required columns (id, client_id, created) and optional columns (value, currency, is_qualified, params, user_id)
5. **Test the query** — validate syntax and preview results
6. **Configure optional features** — enable predicted value (if target conversion exists), set adjustment window for CRM updates
7. **Save** — SegmentStream processes the conversion and backfills historical data
8. **Use in reports** — add the conversion to reports and attribution models

### Optimizing budget with portfolios

1. **Navigate to Optimization** — go to the Optimization tab
2. **Create portfolio** — click + CREATE PORTFOLIO, enter name, select goal (ROAS/CPA), set granularity (campaign/channel)
3. **Add targets** — select campaigns or channels to optimize
4. **Set constraints** — optionally set total budget limit or daily budget limit
5. **Review recommendations** — view per-target budget allocation and marginal metrics
6. **Apply changes** — apply recommended budgets to your ad platforms (manual or via API)
7. **Monitor performance** — check portfolio history and adoption metrics over time

### Querying data via MCP

1. **Connect MCP** — add SegmentStream MCP server to your AI tool (URL: https://mcp.segmentstream.com/mcp)
2. **Authenticate** — complete OAuth flow with your SegmentStream account
3. **Ask questions** — use natural language to query reports, conversions, user journeys, or configuration
4. **AI handles tool calls** — the assistant automatically calls the right tools (list_active_projects, run_report, get_user_journey, etc.)
5. **Get results** — receive data in table, JSON, or narrative format

## Common gotchas

- **Attribution window too short** — if set to 7 days but customers take 30 days to convert, credit will be lost. Review customer journey length before setting window.
- **Untracked clicks** — if ad URLs lack UTM parameters or tracking code is incomplete, clicks won't match sessions. Check data source notifications for untracked clicks.
- **Conversion deduplication issues** — by default, Purchase conversions deduplicate by order ID. If you want to count repeat purchases, enable "Deduplicate by User" instead.
- **Direct traffic attribution** — SegmentStream never attributes to direct visits unless the entire journey is direct. This is by design (non-direct attribution).
- **GA4 event parameters missing** — custom conversions require event parameters to be sent to GA4. If a parameter is missing, the conversion won't fire. Test in GA4 first.
- **BigQuery permissions** — if using custom conversions with external BigQuery projects, the SegmentStream service account needs BigQuery Data Viewer and BigQuery User roles.
- **Timezone mismatches** — project timezone affects report date ranges. If reports show unexpected dates, check project timezone matches your business location.
- **Data source authentication expired** — ad platform OAuth tokens expire. If a data source stops importing, reconnect via disconnect_data_source then connect_data_source.
- **Conversion value currency mismatch** — if a custom conversion specifies a currency different from the project currency, it will be converted. Verify currency codes match expectations.
- **Attribution model backfill delay** — when creating or updating an attribution model, SegmentStream runs a 1-year backfill. Reports won't show data until backfill completes (typically 15-30 minutes).

## Verification checklist

Before submitting work on a SegmentStream project:

- [ ] **Data sources connected** — verify all advertising platforms are authenticated and importing data (check data source logs for errors)
- [ ] **Event tracking active** — confirm GA4 BigQuery Export or Adobe Analytics is sending events (check event count in conversion statistics)
- [ ] **Conversions defined** — ensure at least one conversion is configured and has recent data (check conversion statistics for last 7 days)
- [ ] **Attribution models created** — verify at least one attribution model exists and has completed backfill (check model list, no "processing" status)
- [ ] **Reports accessible** — run a test report with dimensions, metrics, and filters; verify data appears and numbers are reasonable
- [ ] **Filters working** — apply a filter (e.g., "Ad platform equals Facebook") and confirm row count changes
- [ ] **Exports functional** — test exporting a report to CSV or Google Sheets
- [ ] **User journey traces** — if debugging attribution, query a user journey and verify sessions and conversions appear
- [ ] **Portfolio ready** — if using optimization, confirm portfolio shows "Ready" status and has targets with recent data
- [ ] **MCP connected** (if applicable) — test MCP by asking "List my projects" and verify response includes your project

## Resources

**Comprehensive navigation:** https://docs.segmentstream.com/llms.txt

**Critical documentation pages:**
- [Quick start guide](https://docs.segmentstream.com/quick-start) — step-by-step project setup
- [Attribution overview](https://docs.segmentstream.com/attribution/overview) — how to run and customize reports
- [MCP getting started](https://docs.segmentstream.com/mcp/getting-started) — connect AI tools for natural language queries

---

> For additional documentation and navigation, see: https://docs.segmentstream.com/llms.txt