# Apple Ads playbook — an agent skill

An agent skill (Claude Code, Codex and other SKILL.md-compatible agents) that runs Apple Ads
(Apple Search Ads, "ASA") for an app: setup, structure, keywords, bids and breakeven,
brand defense, custom product pages, placements, reading results, incrementality, scaling
and audits. It is written for the agent of an app developer: what to do, in what order, and
how to judge and test Apple's recommendations.

Built on ten years of public Apple Ads work by **Thomas Petit ([@Thomasbcn](https://x.com/Thomasbcn))**:
his RevenueCat guide, conference talks (ASO Conference, Growth Phestival, App Growth
Annual), podcasts (Sub Club, Mobile UA Show, MDM), slide decks and posts. Every source is
listed in `apple-ads-playbook/references/sources.md`.

## Install

```bash
# Claude Code
cp -R apple-ads-playbook ~/.claude/skills/
# Codex and other agents that read ~/.agents/skills
cp -R apple-ads-playbook ~/.agents/skills/
```

Then ask your agent, for example: "Set up ASA for my app", "Audit my Apple Ads account",
"Should I switch to Maximize Conversions?", "What is my breakeven bid?"

## Contents

- `SKILL.md`: rules by area, plus four workflows (new app, weekly loop, audit, "should we try X?")
- `references/sources.md`: sources and credit
- `references/outdated-advice.md`: old advice still found online, and what to do instead

## Notes

Unofficial: not affiliated with or endorsed by Thomas Petit unless he says so. Platform
facts are dated November 2026; Apple changes Apple Ads often, so recheck dated items. The
agent should recommend changes that spend money and let you approve them.

## License

MIT (see `LICENSE`). The ideas credited in `references/sources.md` remain Thomas Petit's; please keep the credit when you reuse this.
