# shopify-theme-converter

**Convert any web project into a complete, merchant-operable Shopify store.**

Version: 3.0 | Updated: 2026-03-03

---

## What this skill does

Takes any source (React, Next.js, Vue, HTML, Gemini single-file export) and produces a deployment-ready Shopify theme package — not just a theme folder, but everything the client needs to go live and self-operate forever.

**Output:**
```
deployment-package/
├── theme/              ← Upload directly to Shopify
├── SETUP-CHECKLIST.md  ← Step-by-step for client
├── dependencies.md     ← App → section mapping
├── assets-manifest.md  ← Images to upload
└── menus.json          ← Navigation structure
```

## When to use it

- Client has a design/website and wants it on Shopify
- You're building a custom Shopify theme from scratch
- You need to audit an existing Shopify theme for gaps
- Client will self-operate post-handoff (the main design constraint)

## What v3 adds over v2

v2 produced a working theme. v3 produces a **merchant-operable** theme — one where the client never needs to call you after handoff.

Key additions: checkout limitations briefing, Lighthouse performance gate, Theme Check validation, app blocks in all key sections, metafield → Customizer wiring, localization block, settings schema audit checklist.

## Usage

Tell your agent:
> *"Use the shopify-theme-converter skill to convert [project/URL]"*

or for auditing:
> *"Audit this Shopify theme using the shopify-theme-converter skill"*

## Lesson Log

Real findings from production deliveries. This is what makes v3 different from v2.

See the full log at the bottom of [SKILL.md](./SKILL.md).

---

[← Back to Sloe Skills](../README.md)
