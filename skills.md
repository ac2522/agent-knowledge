# Skills

## What a skill is
- A skill is just a detailed prompt with clear goals in a markdown file — human language, no code [Sentry](https://www.youtube.com/watch?v=li0SaBt9RDM)
- Versioned, source-controlled, file-based context the agent opens/closes on demand — turns a zero-shot task into few-shot by shipping worked examples [HuggingFace](https://www.youtube.com/watch?v=JomVvNDjGb8)
- Skills authored by domain experts hand the agent (and other engineers) that expertise "for free" [DeepMind](https://www.youtube.com/watch?v=7gujZrJ9L5I)
- Triggered by mention: e.g. when an ADR is referenced, the ADR skill loads how to find/operate on affected code [Safe Intelligence](https://www.youtube.com/watch?v=504PvfXou5Y)

## Less is more — skills can actively hurt
- 10k lines of docs-derived skills produced WORSE results than 553 lines of hand-written "common gotchas" — cover the landmines, not the whole product [WorkOS](https://www.youtube.com/watch?v=vy7o1g2iHY8)
- One measured skill dropped a task from 97% correct to 77% — skills can degrade performance; every skill must earn its place [WorkOS](https://www.youtube.com/watch?v=vy7o1g2iHY8)
- Big skill set sends the model on goose chases checking many things; lean gotchas keep it focused and cut run time (68min → 6min evals) [WorkOS](https://www.youtube.com/watch?v=vy7o1g2iHY8)
- Each thing you add to an agent risks making it worse — large prompts/skills can degrade frontier models; prune relentlessly, get out of the model's way (newer models perform worse with over-instruction; Codex GPT-5.3 system prompt ~1/3 the size of GPT-5's) [Cline](https://www.youtube.com/watch?v=yUmS-F9IX90)
- Knowledge/context banks rot fast — stale on every model release + feature change; aggressively prune (don't just append) or old entries cause context rot and degrade the live agent [Lovable](https://www.youtube.com/watch?v=KA5kPbdkK2E)
- Reconciliation: "less is more" is about quality/leanness, NOT "no skills." Sources that advocate many skills [HuggingFace](https://www.youtube.com/watch?v=JomVvNDjGb8) [DeepMind](https://www.youtube.com/watch?v=7gujZrJ9L5I) still demand each be lean, expert-authored, evaluated, and curated — the failure mode is dumping whole docs, not having skills at all.

## What to put in a skill
- Models already know how to code; skills should encode product-specific intricacies and gotchas, not re-teach general competence [WorkOS](https://www.youtube.com/watch?v=vy7o1g2iHY8)
- Guide, don't prescribe: specific conditional rules ("in the Next.js proxy you can't call redirects") beat dumping summarized docs [WorkOS](https://www.youtube.com/watch?v=vy7o1g2iHY8)
- Ship verification inside the skill: bundle benchmark/test scripts so the agent self-checks its output (e.g. kernel correctness + speedup) instead of trusting it [HuggingFace](https://www.youtube.com/watch?v=JomVvNDjGb8)
- Structure exploration into fixed modes + structured output, not prose: "Catch Me Up" comprehension skill uses fixed modes (architecture, conventions, feature trace, syntax, testing, history) returning org charts/tables [Sentry](https://www.youtube.com/watch?v=li0SaBt9RDM)
- Onboarding-to-unfamiliar-code pattern: state your role ("I am a new contributor") + a specific yes/no question to verify, not just "explore this repo" [Sentry](https://www.youtube.com/watch?v=li0SaBt9RDM)
- Personalization via reusable playbooks/memory: encode the user's own method (e.g. how a firm reviews a contract) so the agent does it the right way, not just any way [Flinn AI](https://www.youtube.com/watch?v=7CrPrHgoEYk)

## Designing skills around existing systems
- Expose a platform's existing guardrails + APIs as agent skills so the agent uses the same safe paths humans do — don't build bespoke "magic" for the agent. Infrastructure-as-code operations become skills (skills.md as implementation detail); the agent is an alternative interface, not a magic layer [Pragmatic Engineer / Kelsey Hightower](https://newsletter.pragmaticengineer.com/p/kubernetes-and-retiring-at-the-top)
- Skills + a guardrailed CLI beat MCP for most work; MCP's real value is auth, not tool surface [DeepMind](https://www.youtube.com/watch?v=7gujZrJ9L5I)
- Vendor-published skills as integration aid: Google ships official coding-agent skills for its Gemini APIs incl. the Live API — install vendor skills when wiring agents to finicky real-time/streaming APIs; steers the agent where docs alone fail [DeepMind-Schaeff](https://www.youtube.com/watch?v=Bc6Ojl2XS1w) (2026-06)
- Anthropic ships an MCP-app scaffolding skill in the modelcontextprotocol repo — runnable through any agent CLI to generate working MCP-app templates incl. handlers and tool-visibility config (model-only / model+app / app-only invocation) [MCP Apps](https://www.youtube.com/watch?v=_xIwFcnHqp4) (2026-06)
- Same generic loop (work → push → feedback → iterate) but swap skills to change the loop's focus: ADR skill, PRD skill, UI skill (skips checks, forces fast browser iteration), test skill (selects tests by coverage + changed files vs running whole suite) [Safe Intelligence](https://www.youtube.com/watch?v=504PvfXou5Y)
- Make your agent itself easy for OTHER coding agents to build/test (a "pseudo-RL" loop): expose a CLI, write AGENTS.md, add skills, wire CI/CD so long-running agents can change and end-to-end test it in a parallel thread [Cline](https://www.youtube.com/watch?v=yUmS-F9IX90)

## Discovering which skills to write
- Mine your own prompt history for repeated patterns, then codify them into a skill (had Claude classify 116 sessions into 6 categories to surface what to automate) [Sentry](https://www.youtube.com/watch?v=li0SaBt9RDM)
- Make your product agent-native: figure out what agents reliably get wrong, write those gotchas down (watch for client-side JS that hides context from page-summarizing crawlers) [WorkOS](https://www.youtube.com/watch?v=vy7o1g2iHY8)
- Instrument your own usage to find where the leverage actually is — in a large mature codebase the dominant use is comprehension, not generation (measured split: 67% comprehension, 2% code gen) [Sentry](https://www.youtube.com/watch?v=li0SaBt9RDM)

## Measuring whether a skill helps (eval each one)
- You only know a skill helps or hurts by measuring — run scenarios with/without it and compare pass rates; otherwise you're adding noise blind. Evals on non-deterministic code are the feedback signal that caught the 95%-deletion win ("trust is a pass rate, a hash, a delta score") [WorkOS](https://www.youtube.com/watch?v=vy7o1g2iHY8)
- Eval each skill, then sweep models against it — compare accuracy vs token cost to drop to a cheaper/open model for skills you run often [HuggingFace](https://www.youtube.com/watch?v=JomVvNDjGb8)
- LLM-generated, auto-run evals validate a new knowledge entry actually resolves the specific cases that spawned it before it ships [Lovable](https://www.youtube.com/watch?v=KA5kPbdkK2E)
- Blank-injection holdout: for a small % where context would be injected, inject nothing; compare success of injected vs could-have-injected cohorts to measure each entry's real production value [Lovable](https://www.youtube.com/watch?v=KA5kPbdkK2E)

## Skill governance & lifecycle
- Skills sprawl in large orgs — curate "Darwinian" so only the best survive; many redundant skills is a liability [DeepMind](https://www.youtube.com/watch?v=7gujZrJ9L5I)
- Two-tier governance: project-maintained skills live in the project (robust, maintainer-owned); experimental/YOLO skills live in a separate repo [HuggingFace](https://www.youtube.com/watch?v=JomVvNDjGb8)
- Skill author owns writing the skill's eval/test; emerging pattern: let agents design those tests (meta-eval) [DeepMind](https://www.youtube.com/watch?v=7gujZrJ9L5I)
- Cluster solved-problem solutions before storing so entries generalize, not one page per exact prompt [Lovable](https://www.youtube.com/watch?v=KA5kPbdkK2E)
- Skill-based harness leaked context as it grew (model "forgot"/skipped tasks); moving orchestration to deterministic code fixed it — skills for knowledge, code for control flow [WorkOS](https://www.youtube.com/watch?v=vy7o1g2iHY8)

## Self-improvement / agent feedback
- Give the agent a "vent" tool to report tooling/docs/schema/platform friction, fired only when genuinely frustrated — tuning the threshold beats an external reviewer forced to comment every turn (overfits to noise) [Lovable](https://www.youtube.com/watch?v=KA5kPbdkK2E)
- The agent has far more root-cause context than the end user (it lived the failure across turns), so its self-reported friction is more actionable than user complaints [Lovable](https://www.youtube.com/watch?v=KA5kPbdkK2E)
- Vent reports surface bugs invisible to normal monitoring (e.g. copy tool silently failing on filenames with spaces); volume spikes act as emergent incident-detection [Lovable](https://www.youtube.com/watch?v=KA5kPbdkK2E)
- Highest-signal training samples come from the stuck→unstuck transition; then ask "what should we have injected at the start to skip the friction?" — turn that into a new skill/knowledge entry [Lovable](https://www.youtube.com/watch?v=KA5kPbdkK2E)
- Retro agent parses Claude/Codex JSONL transcripts to detect doom loops (same tool call repeated, excessive parallel tools) and writes lessons into per-stack markdown memory files [WorkOS](https://www.youtube.com/watch?v=vy7o1g2iHY8)
- Skill maintenance loop: point the agent at its own session logs to improve the skill, or run skills through a prompt optimizer (GEPA) as a "skills gym"; version skills like dotfiles [OpenClaw](https://www.youtube.com/watch?v=pmoDeA3RBZY) (2026-06)
- Harness-engineering discipline: when the agent errs, fix the harness/skill so it can fix itself — never hand-fix the output; every failure is a system bug + data for the next run [WorkOS](https://www.youtube.com/watch?v=vy7o1g2iHY8)
