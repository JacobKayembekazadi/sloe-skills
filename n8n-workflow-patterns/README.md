# n8n-workflow-patterns

**Proven architectural patterns for building n8n automation workflows.**

Version: 1.0 | Updated: 2026-03-03

---

## What this skill does

Documents the 5 core n8n workflow patterns used in production — with decision frameworks for choosing the right pattern, error handling standards, and real lessons from live automations.

## When to use it

- Designing a new n8n workflow from scratch
- Choosing between webhook vs. scheduled vs. event-driven patterns
- Wiring n8n to external APIs (HubSpot, Supabase, Resend, OpenAI)
- Building AI agent workflows with n8n
- Debugging workflow failures in production

## The 5 patterns

1. **Webhook Processing** — receive → validate → transform → respond
2. **HTTP API Integration** — poll or push to external services
3. **Database Operations** — Supabase/Postgres CRUD patterns
4. **AI Agent Workflows** — LLM calls, async processing, callback pattern
5. **Scheduled Tasks** — cron, timezone handling, idempotency

## Usage

> *"Use the n8n-workflow-patterns skill to design this automation"*

> *"I need to build a [webhook/scheduled/AI] workflow in n8n"*

---

[← Back to Sloe Skills](../README.md)
