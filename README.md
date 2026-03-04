<div align="center">

<h1>🧠 sloe-skills</h1>
<p><strong>Agent skill packs built in production, not in theory.</strong></p>

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](https://opensource.org/licenses/MIT)
[![Skills](https://img.shields.io/badge/Skills-3_published-blue.svg)](#skills)
[![Built by Sloe Labs](https://img.shields.io/badge/Built_by-Sloe_Labs-black.svg)](https://sloelabs.com)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)

</div>

---

## What is this?

Structured markdown files that tell AI agents *exactly* how to execute complex, multi-step tasks — without hallucinating steps, skipping edge cases, or forgetting hard-won lessons.

Most prompt libraries are written speculatively. These come from real sessions with real consequences — client deliverables, production deployments, live systems.

> The Shopify skill has a v1, v2, and v3. v3 exists because v2 had 8 documented blind spots found in production.

---

## Skills

| Skill | Version | Description |
|-------|---------|-------------|
| [`shopify-theme-converter`](./shopify-theme-converter/) | v3 | Convert any web app (React, Next.js, Vue) into a production Shopify 2.0 theme |
| [`system-architect`](./system-architect/) | v2 | Full-stack architecture analysis with risk scoring and recommendations |
| [`n8n-workflow-patterns`](./n8n-workflow-patterns/) | v1 | Production-tested n8n automation patterns for business ops |

---

## How to use

Drop a skill into your agent context before the task:

```
Read shopify-theme-converter/SKILL.md, then convert this React app into a Shopify theme.
```

Works with Claude, GPT-4, Gemini, and any instruction-following model.

---

## Stack

![Markdown](https://img.shields.io/badge/Markdown-000000?style=flat&logo=markdown&logoColor=white)
![Claude](https://img.shields.io/badge/Claude-D4A96A?style=flat&logo=anthropic&logoColor=white)
![OpenAI](https://img.shields.io/badge/GPT--4-412991?style=flat&logo=openai&logoColor=white)
![Gemini](https://img.shields.io/badge/Gemini-4285F4?style=flat&logo=google&logoColor=white)

---

## Contributing

Skills improve through use. Found a blind spot? Open an issue or PR with the lesson.

See [CONTRIBUTING.md](CONTRIBUTING.md) for the format.

---

<div align="center">
<sub>Built by <a href="https://sloelabs.com">Sloe Labs</a> · MIT License</sub>
</div>