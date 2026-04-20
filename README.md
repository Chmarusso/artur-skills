# artur-skills

These are the Claude Code agent skills I use every day. Sharing them here so I can re-install them on any machine and so others can grab whatever looks useful.

Some are checked into this repo; others are installed from their official upstream repos.

## Skills in this repo

| Skill | What it does |
|---|---|
| `make-pr` | Create or update a PR for the current branch with a business-focused description |
| `fix-tests` | Run tests stopping at first failure, fix it, repeat until all pass |
| `apply-code-review` | Fetch PR review comments and apply the suggested changes |
| `use-railway` | Operate Railway infrastructure (projects, services, envs, domains, logs) |
| `railway-deploy` | Push code to Railway (`railway up`, deploy, ship) |
| `railway-status` | Check Railway deployment status and uptime |
| `micro-lesson` | Turn the current session into a compact lesson: essence, analogy, business value, future uses, alternatives |

### Install

**Recommended — with [`npx skills`](https://github.com/vercel-labs/skills):**

```bash
# install a single skill
npx skills add Chmarusso/artur-skills --skill micro-lesson

# or install several at once
for skill in make-pr fix-tests apply-code-review micro-lesson; do
  npx skills add Chmarusso/artur-skills --skill $skill
done
```

`npx skills` handles discovery, download, and placement in `~/.claude/skills/` for you. It also makes updates a one-liner later.

**Available skills:** `make-pr`, `fix-tests`, `apply-code-review`, `micro-lesson` (plus `use-railway`, `railway-deploy`, `railway-status` — but see note below).

> The Railway skills (`railway-deploy`, `railway-status`) have generic `name:` fields (`deploy`, `status`) inherited from the official Railway plugin. If you want them namespaced, install the full Railway plugin instead: `/plugin marketplace add railway-railway-skills` in Claude Code.

**Alternative — clone and symlink:**

```bash
git clone https://github.com/Chmarusso/artur-skills.git
cd artur-skills
mkdir -p ~/.claude/skills
for skill in make-pr fix-tests apply-code-review micro-lesson; do
  ln -s "$PWD/$skill" ~/.claude/skills/$skill
done
```

Verify in Claude Code with `/help` or by triggering a skill (e.g. `/make-pr`).

## Skills installed from official repos

These aren't vendored here — install them directly from upstream so they stay current.

| Skill | Source | Install |
|---|---|---|
| `find-skills` | [vercel-labs/skills](https://github.com/vercel-labs/skills) | `npx skills add vercel-labs/skills --skill find-skills` |
| `remotion-best-practices` | [remotion-dev/skills](https://github.com/remotion-dev/skills) | `npx skills add remotion-dev/skills --skill remotion-best-practices` |
| `playwright-skill` | [lackeyjb/playwright-skill](https://github.com/lackeyjb/playwright-skill) | `/plugin marketplace add lackeyjb/playwright-skill` then `/plugin install playwright-skill@playwright-skill` |
| `landing-page-copywriter` | [onewave-ai/claude-skills](https://github.com/onewave-ai/claude-skills) | `npx skills add onewave-ai/claude-skills --skill landing-page-copywriter` |
| `write-a-prd` | [mattpocock/skills](https://github.com/mattpocock/skills) | `npx skills add mattpocock/skills --skill to-prd` (renamed upstream) |
| `prd-to-plan` | [mattpocock/skills](https://github.com/mattpocock/skills) | `npx skills add mattpocock/skills --skill prd-to-plan` |

> Note: `write-a-prd` appears to have been renamed to `to-prd` in Matt Pocock's current repo. If you need the original `write-a-prd` filename, use a fork like [mateuszmidor/mattpocock_skills](https://github.com/mateuszmidor/mattpocock_skills) that preserves the old name.

## Skill structure reference

Each skill is a directory containing at minimum a `SKILL.md` with frontmatter:

```markdown
---
name: skill-name
description: When Claude should invoke this skill.
---

Instructions for Claude...
```

See the [Agent Skills docs](https://code.claude.com/docs/en/skills) for the full spec.
