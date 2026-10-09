# Tiered PR Review

How AI models can do pull request review — with each model tier doing the part it's best at.

A pull request comes in. A cheap model triages it and checks style. A stronger model works through the logic. A frontier model takes only the risky calls — auth, migrations, concurrency. One consolidated review comes out, with a cost receipt for every step.

## How it works

```
PR diff → Triage (low-cost) → Router / decision model (low-cost)
        → Quick pass (low) | Standard review (medium) | Deep review (frontier)
        → Synthesizer (medium) → Cost ledger
```

| Stage | Model tier | Job |
|---|---|---|
| Triage | Low-cost | Classify the change, score risk 1–5 |
| Router (decision model) | Low-cost | Risk 1–2 → quick pass, 3 → standard, 4–5 → deep; enforces per-PR budget |
| Quick pass | Low-cost | Style, formatting, typos, doc nits |
| Standard review | Medium | Logic bugs, edge cases, test coverage, API misuse |
| Deep review | Frontier | Security, auth, migrations, race conditions, architecture |
| Synthesizer | Medium | One severity-ranked review comment, approve / request-changes verdict |
| Cost ledger | — | Per-step receipt: model, tokens, cost; actual vs all-frontier baseline |

The key idea: a cheap decision model gates all downstream spend. You stop paying frontier prices to review typo fixes.

## Documents

- [`docs/how-ai-models-review-pull-requests.pdf`](docs/how-ai-models-review-pull-requests.pdf) — one-page overview: how the models do the work.
- [`docs/reference-architecture-github-actions.pdf`](docs/reference-architecture-github-actions.pdf) — full reference architecture for automating this with GitHub Actions.

## Roadmap

- [x] Concept and one-pager
- [x] GitHub Actions reference architecture
- [ ] Pipeline package (`triage → router → reviewers → synthesizer → ledger`), mock-first with live adapters
- [ ] GitHub Action: label-gated (`ai-review`), posts review comments back to the PR
- [ ] Demo run on sample PRs with measured cost comparison

## License

MIT
