# Archive

Superseded, contradicted, or uncorroborated-after-3-months entries, kept for recovery. Original topic noted per entry.

## Skills

- Skill-of-skills structure: a top-level skill + sub-skills (create/observe/...), each sub-skill fired by natural-language trigger words ("evaluate my agent", "help me build a data set"); inspect the installed SKILL.md to see its best-practice checklist + trigger phrases (low-confidence src) [Foundry](https://www.youtube.com/watch?v=iOXM3zE-2dk) (2026-05) — archived 2026-08: single low-confidence source, never corroborated.
- Optimizer loops need a human stop button: an eval/optimize "observe skill" prompt-optimizer regresses chasing a perfect score (7→8→5...), so checkpoint after each step — the agent reports the result and asks "want me to continue?" while a human picks the best version (low-confidence src) [Foundry](https://www.youtube.com/watch?v=iOXM3zE-2dk) (2026-05) — archived 2026-08: single low-confidence source, never corroborated.
- For brownfield migrations go docs-first: write docs over the legacy code (e.g. 2M LOC undocumented COBOL) and migrate from the docs, not the source (low-confidence src) [Cognition](https://www.youtube.com/watch?v=_xQnSNlBP_w) (2026-05) — archived 2026-08-11: low-confidence entry at the ~3-month mark with no corroboration since. Recoverable if a migration source repeats it.

## Harnesses

- One full K8s pod per agent task — "wasteful but a better abstraction" (a whole computer beats a locked-down sandbox for agent power), paired with read/write state-sync across harness/repo/chat client. (low-confidence src — vendor framing) [ACP](https://www.youtube.com/watch?v=VaS2h-dY1-4) (2026-05) — archived 2026-08-07: single vendor-framed source, uncorroborated after 3 months, and the sandboxing section has since converged on microVMs as the boundary that survives guest root.

## Harnesses

- MCP "tasks" v1 (SEP-1686, Nov 2025): a tool call returns a handle with get-status/cancel/get-result, but the client CANNOT push new info into a running task (workaround: extra tools on the same server), and `tasks/list` is unfilterable [MLOps-English](https://podcasters.spotify.com/pod/show/mlops/episodes/The-Next-Programming-Language-Is-English-e3lnahg) (2026-07) — archived 2026-08-09: obsolete as fact. The v2 spec (Jul 2026) replaces the stateful elicitation tunnel with a client→server update endpoint and drops `tasks/list`; the superseding source states no client ever shipped v1, so the v1 asymmetry is not a constraint anyone still designs around [Temporal-Davis](https://www.youtube.com/watch?v=s4r6nk5WsZw) (2026-08).
