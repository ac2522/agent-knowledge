# Archive

Superseded, contradicted, or uncorroborated-after-3-months entries, kept for recovery. Original topic noted per entry.

## Skills

- Skill-of-skills structure: a top-level skill + sub-skills (create/observe/...), each sub-skill fired by natural-language trigger words ("evaluate my agent", "help me build a data set"); inspect the installed SKILL.md to see its best-practice checklist + trigger phrases (low-confidence src) [Foundry](https://www.youtube.com/watch?v=iOXM3zE-2dk) (2026-05) — archived 2026-08: single low-confidence source, never corroborated.
- Optimizer loops need a human stop button: an eval/optimize "observe skill" prompt-optimizer regresses chasing a perfect score (7→8→5...), so checkpoint after each step — the agent reports the result and asks "want me to continue?" while a human picks the best version (low-confidence src) [Foundry](https://www.youtube.com/watch?v=iOXM3zE-2dk) (2026-05) — archived 2026-08: single low-confidence source, never corroborated.

## Harnesses

- One full K8s pod per agent task — "wasteful but a better abstraction" (a whole computer beats a locked-down sandbox for agent power), paired with read/write state-sync across harness/repo/chat client. (low-confidence src — vendor framing) [ACP](https://www.youtube.com/watch?v=VaS2h-dY1-4) (2026-05) — archived 2026-08-07: single vendor-framed source, uncorroborated after 3 months, and the sandboxing section has since converged on microVMs as the boundary that survives guest root.
