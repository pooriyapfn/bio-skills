# MolDesk Biotech Skills

Reusable research workflows built on the MoleculeDesk `moldesk` CLI. Each skill has its own `SKILL.md`, with challenge-specific rules in `references/` and worked examples in `examples/`.

## MolDesk Binder Campaign

Use `moldesk-binder-campaign` to plan or run a binder campaign for a noninfectious human oncology target. Give the agent the **issue** (target or challenge URL), **total budget** (including currency), and **allowed compute** (for example, rented GPU only). The agent chooses models, interprets scores, and decides which candidates advance. It asks for essential information that the request and brief leave open.

- [Skill](skills/moldesk-binder-campaign/SKILL.md)
- [MolDesk commands and scores](skills/moldesk-binder-campaign/references/moldesk-tools.md)
- [KRAS pMHC benchmark profile](skills/moldesk-binder-campaign/references/proteinbase-kras-g12d.md) and [worked structure and assay review](examples/kras-g12d-pmhc/review.md)

The first benchmark is the archived Anthropic × Adaptyv KRAS G12D pMHC challenge. The worked example uses public crystal structures and fictional assay measurements. [Proteinbase challenge](https://proteinbase.com/competitions/anthropic-adaptyv-2026/challenges/pmhc)

To try the draft, check `moldesk --version` and copy `skills/moldesk-binder-campaign/` into `~/.agents/skills/` for Codex or `~/.claude/skills/` for Claude Code.

> Use `moldesk-binder-campaign` for the archived Proteinbase KRAS G12D pMHC challenge. Cap total spend at $20 USD. Use only rented Runpod GPUs for model jobs and keep local work to CPU inspection and bookkeeping. Return up to 10 candidates with evidence across the three challenge conditions.
