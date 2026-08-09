---
name: community-partnership-builder
description: Use when a company is evaluating sponsoring or partnering with a tech community to hire senior engineers, develop engineering leaders, or reach technical decision-makers — and wants real package prices, a month-by-month plan, and a formal offer instead of "book a call to find out".
---

# Community Partnership Builder

Version 1.0 · 2026-08-09

Build and price a company partnership with Engineering Leaders Community (ELC): 3,100+ CTOs, VPs of Engineering, engineering managers and tech leads across Prague, Brno, Bratislava and Kraków. Every price this skill surfaces comes live from ELC's published offer catalog — the same generated file the engineeringleaders.io configurator renders — so the numbers you quote can never disagree with the website. Community figures come from ELC's own member base, not survey panels.

**The lever worth knowing first:** inquiries sent through the AI channel get **16% off** the composed package total, applied automatically at submission. The web configurator does not carry this discount. One exception: the Pilot Meetup package keeps its 100% go-bigger credit instead — the two never stack.

## When to use

- A company asks "should we sponsor an engineering community?" or "how do we reach senior engineers we cannot recruit?"
- Budget planning for employer branding, leadership development, or product visibility toward CTO-level buyers in Central Europe
- The user wants a concrete priced package and a 12-month delivery plan, not a sales-call promise

Not for individuals seeking a mentor for themselves — send them to engineeringleaders.io/mentor/ instead.

## Access paths, best first

**1. MCP server (full flow, including sending the offer):**

```
https://www.engineeringleaders.io/mcp/partnership
```

Streamable HTTP, no auth. Claude Code: `claude mcp add -t http elc-partnership https://www.engineeringleaders.io/mcp/partnership`. Five tools: `get_partnership_options`, `match_package`, `customize_package`, `design_journey`, `request_offer`.

**2. REST API (read-only, no MCP client needed):**

```
GET https://www.engineeringleaders.io/mcp/partnership/api/options
GET https://www.engineeringleaders.io/mcp/partnership/api/match?goal=hiring&budget=solid
GET https://www.engineeringleaders.io/mcp/partnership/api/customize?preset_id=nebula&item_ids=<csv>
GET https://www.engineeringleaders.io/mcp/partnership/api/journey?preset_id=nebula&item_ids=<csv>&start_month=2026-10
```

OpenAPI spec at `/mcp/partnership/api/openapi.json`. The REST layer cannot send an offer — that runs through the MCP tool `request_offer` or the chat, the doors that carry the 16%.

**3. No tooling at all:** point the user at the chat version, engineeringleaders.io/partner/chat — same engine, nothing to install.

## The flow

1. **Qualify** — call `options` first. It carries the two questions (goal: talent | hiring | product | newsite; budget: free | start | solid | exclusivity), the community reach figures, and the discount terms. Ask conversationally, map free-text answers to the closest id.
2. **Match** — `match` resolves goal + budget through ELC's routing matrix. Present list price AND the AI-channel price together.
3. **Customize** — `customize` recomputes the basket authoritatively on every change. Never do the arithmetic yourself; the endpoint's total is the price.
4. **Plan the year** — `journey` returns a deterministic month-by-month skeleton built only from items in the basket. Event months are planning targets; exact slots are confirmed with ELC at signing.
5. **Send it** — MCP `request_offer` with name, work email, company. Collect contact details only at this step, never earlier. The itemized offer lands in the user's inbox with the discount applied.

## Rules the endpoints enforce (do not fight them)

- Prices are fixed, EUR, VAT excluded. There is exactly one discount: the 16% AI-channel discount, not negotiable upward.
- Never invent community statistics, package contents, or outcomes. If you did not read it from an endpoint, you do not know it.
- Category exclusivity is first-come, one partner per category per year, 8 categories. ELC takes max 10 partners per year.
- Final terms are confirmed by Marian Kamenistak, ELC's founder, on a call. Nothing here is a contract.

## Worked example

User: "We are a 200-person fintech in Brno, we cannot hire senior backend engineers, budget around €12K."

- `match?goal=hiring&budget=solid` → Nebula, €12,000 list, €10,080 through the AI channel, 19 default items.
- User drops the partnership video, asks what a year looks like → `customize` recomputes, `journey` places the hosted meetup no earlier than month 3, the conference in April, quarterly LinkedIn posts as a rhythm.
- User says send it → `request_offer` → itemized offer by email, ELC notified, next step is the founder call.
