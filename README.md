# principles

Research → principle catalog → compliant agent artifacts.

## What you get

Three skills, one pipeline:

- **to-principles** — turn a research round into a classified principle catalog with a human gate at the merge step.
- **principled-write** — lint or rewrite a markdown artifact against the catalog and the voice rules.
- **principles-render** — generate compliance artifacts from the catalog with `render.mjs` (requires Node.js 18+).

## Why

Agents write slop. Model weights hold habits, not rules: a model cannot be ordered to obey what it can forget mid-session. This pack treats principles as an external rule layer — files a tool reads, lints, and rewrites against. The catalog draws on published engineering research (239 principles extracted from publications); each unit keeps a verbatim quote and a `file:line` source tag. Rules live outside the prompt so the model cannot violate a rule it never sees.

Scope: the three skills above and the artifact pipeline. Non-scope: model training, prompt-only rule systems, verification of rewrites (a human or a verification tool must confirm compliance).

## How it works

```
research (papers, books, round notes)
   │  to-principles
   ▼
principle catalog (class, verified, deduped, source-tagged)
   │  principled-write                    │  principles-render
   ▼                                      ▼
lint / rewrite of each artifact    rendered compliance artifacts:
(must obey the catalog rules)      must-card, lint-rules,
                                   role-prompts, cp-corpus
```

## Install

Prerequisites: Node.js 18 or newer (for `principles-render` only).

```bash
cp -r skills/to-principles ~/.agents/skills/
cp -r skills/principled-write ~/.agents/skills/
cp -r skills/principles-render ~/.agents/skills/
node ~/.agents/skills/principles-render/render.mjs --out ./rendered
```

In pi, invoke a skill as `/skill:to-principles`, `/skill:principled-write`, or `/skill:principles-render`.

## Usage

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

Note: `principles-render` expects a catalog of 100 principles at `~/.agents/skills/to-violations/PRINCIPLES.md`; the catalog ships outside this pack.

## Troubleshooting

- `render.mjs` fails with a missing catalog — point it at `~/.agents/skills/to-violations/PRINCIPLES.md` or copy the catalog into place first.
- A rewrite still reads like slop — `principled-write` rewrites are not self-certified; run a verification pass or a human review.

## Contributing

File issues in the repo tracker. Run the lint check before submitting a doc change:

```bash
python ~/.agents/skills/principled-write/scripts/ste-lint.py README.md
```

## License

MIT. See [LICENSE](LICENSE).
