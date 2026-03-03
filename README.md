# Sloe Skills

**Agent skills built in production, not in theory.**

Each skill is a structured markdown file that tells an AI agent exactly how to do a specific task — methodology, patterns, decision frameworks, and real lessons from live client work.

Built by [Jacob Kayembe Kazadi](https://github.com/JacobKayembekazadi) — AI solutions consultant and AI-native builder.

---

## Install

```bash
# OpenClaw
npx openclaw skill add https://github.com/JacobKayembekazadi/sloe-skills/tree/main/<skill-name>

# Manual — copy SKILL.md to your agent's skills directory
curl -O https://raw.githubusercontent.com/JacobKayembekazadi/sloe-skills/main/<skill-name>/SKILL.md
```

---

## Skills

| Skill | Description | Status |
|-------|-------------|--------|
| [shopify-theme-converter](./shopify-theme-converter) | Convert any web project into a complete, merchant-operable Shopify store. Performance gates, mobile standards, tracking events, Klaviyo wiring, client handoff docs. | ✅ v3.0 |
| system-architect | 14-layer system architecture from any product idea — state, journeys, failures, security, resilience, observability, experience. | 🔜 |
| cold-email | B2B cold email sequences that don't read like cold emails. Positioning, subject line patterns, follow-up cadence. | 🔜 |
| founder-sales | Sales conversation playbooks for founders selling their own product. Discovery, objection handling, closing. | 🔜 |

More shipping weekly.

---

## What Makes These Different

Most prompt libraries are theoretical. These are built from real sessions.

`shopify-theme-converter v3` was used to build and ship a merchant-operable Shopify theme in one session:
- Client feedback integrated same day
- Full mobile responsive pass
- Klaviyo BIS + pixel events wired
- Dashboard-only operation post-handoff — no developer dependency
- Every lesson learned added to the skill's `Lesson Log`

Skills get versioned when real gaps are found. v3 exists because v2 had 8 documented blind spots from a live delivery.

---

## Skill Format

```
skill-name/
└── SKILL.md
```

Frontmatter:

```yaml
---
name: skill-name
version: 1.0
description: What it does and when to use it.
updated: YYYY-MM-DD
---
```

---

## Contributing

Found a gap in production? Open a PR with the fix and a real example.

---

## License

MIT.

---

*Built with [OpenClaw](https://openclaw.ai)*
