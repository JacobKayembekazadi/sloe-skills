# Sloe Skills

**Agent skills built in production, not in theory.**

Structured markdown methodologies that tell AI agents exactly how to execute complex tasks — with decision frameworks, copy-paste patterns, and real lesson logs from live client work.

By [Jacob Kayembe Kazadi](https://github.com/JacobKayembekazadi) — AI solutions consultant, AI-native builder.

---

## Why these are different

Most prompt libraries are written speculatively. These come from real sessions with real clients. Every skill has a **Lesson Log** — documented findings from actual deliveries that make the methodology more accurate over time.

The Shopify skill has a v1, v2, and v3. v3 exists because v2 had 8 documented blind spots found during a live client delivery. That's the difference.

---

## Skills

| Skill | Version | Description |
|-------|---------|-------------|
| [shopify-theme-converter](./shopify-theme-converter) | v3.0 | Convert any web project into a complete, merchant-operable Shopify store |
| system-architect | v2.0 | 14-layer system architecture from any product idea | 
| cold-email | v1.0 | B2B outreach sequences that don't read like cold emails |
| founder-sales | v1.0 | Sales playbooks for founders selling their own product |

*More shipping regularly.*

---

## How to use

### With OpenClaw
Copy `SKILL.md` into your OpenClaw workspace skills directory:
```
~/.openclaw/workspace/skills/<skill-name>/SKILL.md
```
Then tell your agent: *"Use the shopify-theme-converter skill to convert this project"*

### With Claude / any agent
Paste the `SKILL.md` contents into your system prompt or reference it at the start of a session:
*"Follow the methodology in this skill: [paste SKILL.md]"*

### With Cursor / Windsurf
Add `SKILL.md` to your `.cursor/rules` or reference it in your project context.

---

## Skill format

```
skill-name/
├── SKILL.md          # Full methodology — the only required file
└── references/       # Optional supporting docs (patterns, examples, templates)
    ├── patterns.md
    └── examples.md
```

### SKILL.md frontmatter

```yaml
---
name: skill-name
version: 1.0
description: One sentence. What it does and when to use it.
updated: YYYY-MM-DD
---
```

### What every skill contains

- **Quick reference** — when to use it, what it outputs
- **Phased workflow** — step-by-step with decision points
- **Copy-paste patterns** — code/templates ready to use
- **Lesson Log** — real findings from production use

---

## Contributing

Skills improve through real use. If you run a skill on a live project and find a gap:

1. Fork the repo
2. Add your finding to the skill's `## Lesson Log` with the date
3. If it warrants a workflow change, update the relevant phase
4. Open a PR — include what you built, what broke, what you learned

The best contributors are people who shipped something with the skill.

---

## Roadmap

- [ ] `system-architect` — 14-layer architecture framework
- [ ] `cold-email` — B2B outreach methodology
- [ ] `founder-sales` — sales conversation playbooks
- [ ] `n8n-workflow-patterns` — automation design patterns
- [ ] `api-design` — REST/GraphQL/webhook design standards
- [ ] `mobile-responsive` — systematic mobile audit + fix patterns

---

## License

MIT. Use freely, attribution appreciated but not required.

---

*Built with [OpenClaw](https://openclaw.ai) · [Sloe Labs](https://sloelabs.com)*
