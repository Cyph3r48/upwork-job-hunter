# Upwork Job Hunter v2.0

Agentic n8n workflow that scouts, scores, and triages freelance job postings —
built for my own use, running in production daily.

## Pipeline

1. **Schedule trigger** — weekday mornings
2. **Scrape** Upwork job search via Apify (`trudax/upwork-scraper`), filtered by keyword sets
3. **Filter/code nodes** — dedupe, budget and keyword qualification
4. **LLM scoring** — Anthropic chat node scores each posting for fit and writes a
   structured verdict (apply / skip + reasoning)
5. **Memory write** — decision records persisted to a memory-store API
   (`/v1/memory/store`), so the agent learns from past apply/skip outcomes
6. **Webhook response** — results surface for human review

## Use it

Import `workflow.json` into n8n:

```bash
n8n import:workflow --input=workflow.json
```

Set credentials for Apify and Anthropic, and set `YOUR_MEMORY_HOST` to your
memory endpoint (or drop that node).

Node types used: scheduleTrigger, httpRequest, code, filter, switch,
lmChatAnthropic, webhook, respondToWebhook — 20 nodes total.