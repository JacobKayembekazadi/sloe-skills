# Contributing to Sloe Skills

## The core principle

Skills improve through production use, not speculation. The most valuable contribution is a lesson learned from a real project.

## How to contribute

### Adding a lesson to an existing skill

If you ran a skill and found something that should be documented:

1. Fork the repo
2. Open `<skill-name>/SKILL.md`
3. Scroll to `## Lesson Log`
4. Add an entry:

```markdown
**YYYY-MM-DD — [brief project description, no client names]:**
- What you found
- Why it matters
- The fix
```

5. Open a PR with the title: `lesson(skill-name): brief description`

### Fixing a methodology gap

If a phase is wrong, incomplete, or missing something important:

1. Open an issue first describing the gap
2. Reference your real-world example (anonymised)
3. Propose the fix
4. Submit a PR once the approach is agreed

### Adding a new skill

New skills must meet the bar:
- You've used it on at least one real project
- It has a complete phased workflow (not just a prompt)
- It has at least one Lesson Log entry from production

Submit as a PR. Skill will be reviewed for methodology quality, not just correctness.

## What NOT to contribute

- Client names, company data, or confidential information
- Speculative content ("this probably works")
- Skills that duplicate existing ones without meaningful differentiation
- Generic prompts without workflow structure

## Skill quality bar

A good skill answers:
- When exactly should I use this? (not just a vague description)
- What are the decision points? (not a linear checklist)
- What breaks in production? (Lesson Log)
- What's the output format? (clear deliverable)

---

Questions? Open an issue or reach out on [Twitter/X](https://x.com/jacobkazadi).
