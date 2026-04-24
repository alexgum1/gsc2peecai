# Contributing to gsc-peec-ai-visibility

Thanks for helping improve this skill. Corrections and additions are welcome — especially anything that reflects how the MCPs actually behave in practice vs. how the docs say they should.

## What's in scope

- **Corrections** — anything in `SKILL.md` or the reference files that's wrong, outdated, or misleading.
- **GSC filter improvements** — better heuristics for identifying conversational queries worth tracking in Peec.ai.
- **Low-performer threshold refinements** — if the current visibility/mention thresholds produce too many false positives or miss real underperformers, open a PR with evidence.
- **MCP gotchas** — if the GSC MCP, Peec.ai MCP, or SISTRIX MCP behaves differently from what the skill assumes, document it. Raw tool call JSON is the most useful evidence.
- **Alternative SEO tools** — if you've adapted Phase 2 (the live dashboard) to work with a different search volume source (Semrush, Ahrefs, Keyword.io MCP, etc.), a note or a PR describing the adapter is welcome.
- **Language support** — the W-question filter currently covers German. If you add support for another language's question words, please include examples and a note about the impression threshold.

## What's out of scope

- Marketing of specific tools, agencies, or consulting services.
- Features that require a paid Peec.ai plan tier without a clear note about the requirement.
- Speculation about Peec.ai's roadmap.

## How to contribute

### Via pull request

1. Fork the repo and branch from `main`.
2. Edit the relevant file (`SKILL.md`, `references/*.md`, `evals/evals.json`).
3. Open a PR with a short description of what you observed that motivated the change.

### Via issue

If you're not comfortable with PRs, open an issue describing:
- Which file and section you're referring to
- What you observed (ideally with a tool call + response snippet)
- What you'd expect instead

## Style notes

- Write instructions for an AI agent, not a human reader. Prefer "fetch X, then do Y" over general advice.
- Explain the *why* behind rules — agents that understand the reasoning handle edge cases better than agents following rigid instructions.
- Keep `SKILL.md` under 500 lines. If content grows beyond that, move appendix material into `references/`.
- Date-stamp behavioural claims that may change ("as of April 2026…").

## Attribution

Contributors are attributed via Git history. Every merged PR carries the author's commit trail on GitHub.
