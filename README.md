# principles

Research → principle catalog → compliant agent artifacts.

## What you get

- **to-principles** — turn a research round into a classified principle catalog with a human gate.
- **principled-write** — lint or rewrite a markdown artifact against the catalog and voice rules.
- **principles-render** — generate compliance artifacts (must-card, lint rules, role prompts) from the catalog.

## Why

Agents write slop. Model weights hold habits, not rules, and a model cannot be ordered to obey what it can forget mid-session. This pack treats principles as an external rule layer: files a tool reads, lints, and rewrites against. The model cannot violate a rule it never sees, so the rules live outside the prompt and inside the artifact pipeline. The catalog draws on published engineering research; each unit keeps a verbatim quote and a `file:line` source tag.

## How it works

```
research (papers, books, round notes)
   │  to-principles
   ▼
principle catalog (class, verified, deduped, source-tagged)
   │  principled-write                    │  principles-render
   ▼                                      ▼
lint / rewrite of each artifact    compliance artifacts:
(must obey the catalog rules)      must-card.md, lint-rules.md,
                                   role-prompts/*.md, spec-template.md
```

## Install

1. Copy the skill folders into your agent skills directory:

   ```bash
   cp -r skills/to-principles ~/.agents/skills/
   cp -r skills/principled-write ~/.agents/skills/
   cp -r skills/principles-render ~/.agents/skills/
   ```

2. `principles-render` runs `render.mjs` with Node.js. Install Node 18 or newer, then:

   ```bash
   node ~/.agents/skills/principles-render/render.mjs --out ./rendered
   ```

3. In pi, invoke a skill as `/skill:to-principles`, `/skill:principled-write`, or `/skill:principles-render`.

## Quick example

Lint a PRD against the catalog:

```bash
python ~/.agents/skills/principled-write/scripts/ste-lint.py PRD.md
```

Rewrite it in place:

```bash
/skill:principled-write path=PRD.md artifact-type=PRD output-mode=rewrite
```

Regenerate compliance artifacts after a catalog change:

```bash
node ~/.agents/skills/principles-render/render.mjs --out ./rendered
```

## Honest status

- `principles-render` expects a catalog of 100 principles at `~/.agents/skills/to-violations/PRINCIPLES.md`; the catalog ships outside this pack.
- `principled-write` rewrites are not self-certified. A human or a verification tool must confirm compliance.
- The shipped `rendered/` corpus in this repo is a snapshot; the live catalog grows over time.
- `to-principles` runs one subagent at a time and stops for a human decision at its merge gate.

## License

MIT. See [LICENSE](LICENSE).
