# Misc

## Agent memory & decision traces

- Split agent memory into 3 layers — short-term (session/conversation), long-term (deduped resolved entities), reasoning (decision traces) — each needs different storage/retrieval [[Neo4j](https://www.youtube.com/watch?v=B9h9ovW5H9U)]
- Feed agents past decision traces + precedents (causal chains + outcomes), not just fact docs — shifts agent from answering questions to making/justifying decisions (accept/reject + why) [[Neo4j](https://www.youtube.com/watch?v=B9h9ovW5H9U)]
- Retrieve similar past decisions via hybrid of semantic AND structural similarity (embed the whole reasoning subgraph, not just text) — surfaces precedents plain text/vector search misses [[Neo4j](https://www.youtube.com/watch?v=B9h9ovW5H9U)]
- Agents won't persist decisions/learnings unless explicitly prompted to store them — reusable traces need deliberate write steps, not implicit memory [[Neo4j](https://www.youtube.com/watch?v=B9h9ovW5H9U)]
- Personalization via reusable playbooks/memory encodes the user's own method (e.g. how a firm reviews a contract) so the agent does it the right way, not just any way [[Flinn](https://www.youtube.com/watch?v=7CrPrHgoEYk)]
- Text-to-structure extraction as a cost cascade: cheap NER (spaCy) → mid (GLiNER) → LLM fallback, then a separate merge/dedup/enrich pass — avoid LLM-for-everything [[Neo4j](https://www.youtube.com/watch?v=B9h9ovW5H9U)]

## Decision-making frameworks

- Core AI-engineering move: make implicit human knowledge (how to decide, what matters) explicit for the agent [[Neo4j](https://www.youtube.com/watch?v=abvQEhvRI_c)]
- Generic decision frameworks don't transfer cleanly — every step is heavily domain-specific; expect to specialize per domain [[Neo4j](https://www.youtube.com/watch?v=abvQEhvRI_c)]
- ADR captures *why* a rule exists + how it's enforced + which files/folders it concerns; the lint tool states the rule and points to the doc [[Safe Intelligence](https://www.youtube.com/watch?v=504PvfXou5Y)]
- PRD can be minimal — just why / problem / goal / user journey — for humans 6 weeks later and for agents [[Safe Intelligence](https://www.youtube.com/watch?v=504PvfXou5Y)]
- BDD (Cucumber) as the readable, executable layer spec-driven dev lacks — "one thing harder than reading AI code is reading AI tests"; human-language scenarios linking back to PRDs are reviewable in a way raw AI tests aren't [[Safe Intelligence](https://www.youtube.com/watch?v=504PvfXou5Y)]

## Code review & quality

- Per-language auto-review models fine-tuned on style guides + curated good-code examples; per-product custom review prompts on top [[DeepMind](https://www.youtube.com/watch?v=7gujZrJ9L5I)]
- Calibrate review agents on acceptance history: index past PRs + reviewer comments; suggestion weight rises when developers accept, decays when repeatedly ignored; inject that history twice — into specialist agents AND into the judge filtering merged findings; hard rules always fire, only bug-class suggestions decay [[Qodo](https://www.youtube.com/watch?v=EcqMYoIV57A)] (2026-06)
- AI review holds up at max-rigor codebases: kernel devs feed mailing-list patches into AI-review bots; Linus + maintainers at the 2025 Maintainer Summit called the reviews "really impressive" — extra safety net, not reviewer replacement [[Ryhl-Rust](https://newsletter.pragmaticengineer.com/p/why-rust-is-different-with-alice)] (2026-05)
- AI review's sweet spot = high-frequency trivial mistakes: even a 0.1%-error-rate expert slips across a bajillion sites; catching that class is where review bots pay off [[Ryhl-Rust](https://newsletter.pragmaticengineer.com/p/why-rust-is-different-with-alice)] (2026-05)
- Agents excel at ancillary review chores: "run a benchmark" review reply → agent found an existing benchmark, adapted it, ran it, wrote the analysis script + table — drops the cost of measurement requests in review [[Ryhl-Rust](https://newsletter.pragmaticengineer.com/p/why-rust-is-different-with-alice)] (2026-05)
- Agent-PR review UX: replace alphabetical diff lists with AI-generated commentary that guides the reviewer pedagogically — the craft shifts to reviewing/architecture, so design the review experience [[Hejlsberg](https://newsletter.pragmaticengineer.com/p/typescript-c-and-turbo-pascal-with)] (2026-05)
- Agents as patch-porters across a rewrite: TS team uses AI to migrate the backlog of PRs onto the parallel Go-ported compiler; plus AI PR review (recently much improved) and same-style test generation as toil removal [[Hejlsberg](https://newsletter.pragmaticengineer.com/p/typescript-c-and-turbo-pascal-with)] (2026-05)
- Agents mute the "hack prickle": hack-vs-redesign judgment degrades because the agent absorbs the immediate pain of the hacky fix — OpenCode caught itself shipping hacks where ground-up redesign was right; the landmines remain, the feedback loop is gone [[OpenCode](https://newsletter.pragmaticengineer.com/p/opencode)] (2026-05)
- Codebase-wide cleanup is now cheap — prompt the agent to roll a new pattern through everywhere and kill dead old patterns; tech-debt cleanup that was never worth the typing time now is [[OpenCode](https://newsletter.pragmaticengineer.com/p/opencode)] (2026-05)
- Refactor mercilessly on code you care about — it forces you back into the codebase, the only thing keeping quality high / complexity low (against token-maxing wisdom) [[Pi](https://newsletter.pragmaticengineer.com/p/building-pi-and-what-makes-self-modifying)]
- Agents add recovery/fallback paths instead of failing, producing emergent-state-machine sprawl; they won't refactor it away because they don't feel codebase pain [[Pi](https://newsletter.pragmaticengineer.com/p/building-pi-and-what-makes-self-modifying)]
- Use the agent to answer "why did this change / where did this regress" instantly — replaces manual git blame, GitHub spelunking, cross-timezone Slack questions [[Sentry](https://www.youtube.com/watch?v=li0SaBt9RDM)]
- Overfitting AI-generated unit tests acted as an accidental golden-master net during a ~1M-LOC refactor touching 82% of core — green = "still somewhat close" [[OpenClaw](https://www.youtube.com/watch?v=pmoDeA3RBZY)] (2026-06)

## Provenance & auditability

- Source control for agent-authored code needs character-level provenance, not line-level diffs — attribute human vs LLM-written/influenced code, record model info + identity, map to evals (and back to training); commit chat history alongside PRs; in regulated orgs "what did this agent do at 2pm 3 years ago" must be answerable [[Guild-Meta](https://podcasters.spotify.com/pod/show/mlops/episodes/From-Single-Player-to-Multi-Player-Operating-AI-Agents-at-Scale-e3khpk8)] (2026-06)

## Formal verification × AI

- LLMs × formal verification is a two-way driver: LLMs make proofs economical to write, and vibe-coded volume makes proofs necessary (human review of all generated code is the bottleneck; tests sample inputs, proofs cover the state space) — apply where complete bug absence matters (security, data-corruption-critical algorithms) [[Kleppmann](https://newsletter.pragmaticengineer.com/p/designing-data-intensive-applications)] (2026-04)
- Formal-methods entry path: start with model checking (TLA+/FizzBee), not proof assistants (Isabelle/Rocq/Lean) — proof assistants take months to learn and the learning resources are poor [[Kleppmann](https://newsletter.pragmaticengineer.com/p/designing-data-intensive-applications)] (2026-04)
- Formal proofs as CI regression gates, not one-time verification: S3 wired its strong-consistency proof into check-ins on the index-subsystem code paths — every commit re-proves the consistency model hasn't regressed; "at a certain scale, math has to save you" (can't enumerate combinatoric edge cases) [[S3](https://newsletter.pragmaticengineer.com/p/how-aws-s3-is-built)] (2026-01)
- Pair proofs with continuous empirical verification — proofs assume hardware failure rates; S3 runs dedicated auditor microservices inspecting every byte fleet-wide to answer "what was our actual durability last week" — proof on every check-in AND verify on every request [[S3](https://newsletter.pragmaticengineer.com/p/how-aws-s3-is-built)] (2026-01)

## PR back-pressure & team workflow

- Bottlenecks / back-pressure are now a deliberate engineering primitive: agent PRs arrive faster than humans can review or even than they stay mergeable [[Pi](https://newsletter.pragmaticengineer.com/p/building-pi-and-what-makes-self-modifying)]
- Filter agent contributions cheaply: auto-close PRs from unknown authors, ask for a human-voice issue; agents don't read the bot comment, so it self-selects humans [[Pi](https://newsletter.pragmaticengineer.com/p/building-pi-and-what-makes-self-modifying)]
- "Prompt request" / value of a naive bad implementation: seeing the dumb agent's attempt tells you what was actually wanted faster than reviewing the prompt or the code [[Pi](https://newsletter.pragmaticengineer.com/p/building-pi-and-what-makes-self-modifying)]
- Adoption needs ~2-3 weeks of dedicated time to click; mandates without slack time fail [[Pi](https://newsletter.pragmaticengineer.com/p/building-pi-and-what-makes-self-modifying)]
- AI productivity self-deception at team level: "feels like we're going fast, but we're not moving faster than competitors" — most engineers cash gains as same output for less energy, while the quality-minded few drown in slop PRs and burn out [[OpenCode](https://newsletter.pragmaticengineer.com/p/opencode)] (2026-05)
- Maintainer PR-triage at scale: bare "review &lt;URL&gt;" prompts cleared 100 PRs in 90 min — ~10% merge as-is, ~20% right-problem/wrong-code → agent clean-rooms the fix in project style [[DHH](https://newsletter.pragmaticengineer.com/p/dhhs-new-way-of-writing-code)] (2026-04)
- Duplicate agent-era PRs/issues as a demand signal: semantic-embed + cluster the 6,000-PR pile; heavy duplicate pressure = de-facto roadmap priority [[OpenClaw](https://www.youtube.com/watch?v=pmoDeA3RBZY)] (2026-06)
- Earn agent adoption, don't mandate it: pose challenges only solvable with the new tools ("eliminate the holiday code freeze" → diff-risk-score agent) and run a visible, forkable agent hub so one engineer's high-impact agent compounds; mandating "more PRs / move faster" backfires [[Guild-Meta](https://podcasters.spotify.com/pod/show/mlops/episodes/From-Single-Player-to-Multi-Player-Operating-AI-Agents-at-Scale-e3khpk8)] (2026-06)

## Architecture & human design ownership

- Architecture of an agent must be designed by a human, thoughtfully — don't let models rip through code; spend real time on the state-machine design even when pairing with an agent [[Cline](https://www.youtube.com/watch?v=yUmS-F9IX90)]
- Optimize "speed to understanding" not "speed to outcome" — fast generation that misses the user's implicit intent is useless; capture nuance first [[Flinn](https://www.youtube.com/watch?v=7CrPrHgoEYk)]
- Autonomous agent loops need a verifiable experiment (training run, kernel benchmark) as the fitness signal — without one, self-improvement has nothing to optimize against [[HuggingFace](https://www.youtube.com/watch?v=JomVvNDjGb8)]
- Delegate-vs-grapple rule: use AI when the artifact is the outcome; do it by hand when the thought process is the outcome (writing as thinking) — while still rating AI very productive for unblocking technicalities and stress-testing ideas [[Kleppmann](https://newsletter.pragmaticengineer.com/p/designing-data-intensive-applications)] (2026-04)

## Where coding agents pay off — domain limits

- Off-distribution datapoint from a deep-systems shop (Oxide: de novo Rust OS, hypervisor, kernel C, firmware): team-wide Claude Code use, but "helpful as a polishing tool, less at the epicenter of creation" — "LLMs more valuable in the small than in the large": idiomatic-Rust snippet checks, test-case generation, tedium; "no part of the Oxide stack is vibe coded"; counterweight to exec assumptions of blanket 10-30% productivity gains [[Cantrill-Oxide](https://newsletter.pragmaticengineer.com/p/the-history-of-servers-the-cloud)] (2025-12)
- A writing-intensive culture is "LLM ready" on the consumption side, not generation: a large RFD/doc corpus unlocks document-comprehension tasks previously intractable by hand (cross-corpus glossary: hours by hand, an LLM just turns it out) — value the existing doc corpus as agent context, don't generate more docs [[Cantrill-Oxide](https://newsletter.pragmaticengineer.com/p/the-history-of-servers-the-cloud)] (2025-12)
- Don't extrapolate coding-agent success to adjacent engineering: coding is an unusually good LLM fit (simple grammar, validatable output); hardware EE work gets ~zero value — partly because mature deterministic tooling (EDA rule checks, SI simulation) already covers the mechanizable parts [[Cantrill-Oxide](https://newsletter.pragmaticengineer.com/p/the-history-of-servers-the-cloud)] (2025-12)

## MCP vs CLI

- MCP fills context and is non-composable (combining two servers' outputs forces data through the model); CLI piping lets the model see only the end result and massage freely — "code mode" exposes MCP servers as callable functions to recover composability [[Pi](https://newsletter.pragmaticengineer.com/p/building-pi-and-what-makes-self-modifying)]
- Serve context anthropomorphically: models were RL'd on human tool traces, so give them the tools humans use (agentic search over bespoke RAG, CLIs over custom APIs) and pay the latency tax — new agent-only tools may need "a few RL cycles" before models wield them well [[Dust](https://podcasters.spotify.com/pod/show/mlops/episodes/MCP--Agents--the-40M-Bet-on-Multiplayer-AI-e3kmopj)] (2026-06)
- Why bash/CLI-in-a-sandbox dodges MCP-style context rot: Unix tools (cat/sed/grep/awk) are effectively baked into model weights — model just tries one and recovers from "not found", so no tool defs load; skills sit between (agent reads only each skill's header markdown when scanning, not the body); MCP/skills can carry an intent hint to aid selection, raw bash can't [[Sazabi](https://podcasters.spotify.com/pod/show/mlops/episodes/Logs-Are-All-You-Need-Rethinking-Observability-with-AI-Agents-e3jthog)] (2026-06)
- Prefer an open data layer (e.g. parquet) over a fixed dashboard for agent metrics: the agent can query raw data and build whatever view it needs (gantt, custom viz) [[HuggingFace](https://www.youtube.com/watch?v=JomVvNDjGb8)]
- MCP Apps: a tool result can return a UI resource (bundled HTML/React) the host renders in a sandboxed iframe (no editor settings/API/external access) with app↔server callbacks for live data; Shopify in-chat checkout, Excalidraw, Figma already shipping on it [[MCP Apps](https://www.youtube.com/watch?v=_xIwFcnHqp4)] (2026-06)
- Route dense tool output to an interactive UI instead of through the model — an in-chat flame-graph iframe eliminates the multi-turn "where's the time spent?" loop of having the LLM interpret profiler JSON [[MCP Apps](https://www.youtube.com/watch?v=_xIwFcnHqp4)] (2026-06)
- Custom UI components cut cost, not just friction: a tappable picker vs free-text both improves UX AND removes LLM calls — render structured UI where the interaction is constrained, reserve the LLM for genuinely open-ended turns [[iFood](https://podcasters.spotify.com/pod/show/mlops/episodes/The-Control-vs-Magic-Spectrum-Building-Agents-e3k9n0g)] (2026-06)
- Generative-UI maturity ladder: static components (agent only fills props) → declarative descriptors (agent emits JSON/YAML → render engine maps to design-system components; Netflix server-driven-UI precedent) → fully generative (model writes HTML/CSS/JS at runtime); declarative = today's sweet spot for flexibility vs consistency vs token cost. Counterpoint to Cloudflare's "skip the DSL, generate real code in a sandbox" stance (see harnesses.md sandboxes) [[Postman-GenUI](https://www.youtube.com/watch?v=hCMrEfPG2Yg)] (2026-06)
- Treat runtime LLM-generated UI as untrusted third-party code — needs a distribution model with boundary/containment/sandbox, not direct rendering; MCP Apps is the current best delivery mechanism (auth, tool calling, UI↔agent message passing, sandboxed iframe). Signal: Anthropic ships its first-party Visualizer via MCP Apps instead of a bespoke renderer [[Postman-GenUI](https://www.youtube.com/watch?v=hCMrEfPG2Yg)] (2026-06)
- Shared-artifact collaboration as the post-chat interface: Excalidraw MCP app creates a canvas both human and agent edit (prompt the agent AND click/drag directly) — UI as a co-owned workspace, not just output visualization [[Postman-GenUI](https://www.youtube.com/watch?v=hCMrEfPG2Yg)] (2026-06)

## Retrieval & conflict handling

- Surface conflicts, don't hide them: when code in main disagrees with a Slack thread, let the agent see both and weight by source authority (e.g. CTO's correction) rather than silently picking one [[Unblocked](https://www.youtube.com/watch?v=BiG2ssibKGc)]
- Don't cache "good answers" for latency — a correct answer goes stale on a ~24h clock as the system changes, so you start confidently serving lies [[Unblocked](https://www.youtube.com/watch?v=BiG2ssibKGc)]
- Social/expert graph as a retrieval pivot: resolve "who is asking" → their repos, PR history, collaborators → scope retrieval to relevant code instead of the whole corpus [[Unblocked](https://www.youtube.com/watch?v=BiG2ssibKGc)]

## Economics & pricing

- Token-hunger breaks subscription pricing; quota throttling per user/team is the current blunt defense against one power user spawning 100 agents [[DeepMind](https://www.youtube.com/watch?v=7gujZrJ9L5I)]

## Languages & type systems as agent leverage

- Strict compiler as agent feedback loop: Rust refactor workflow = "fix compiler errors until it stops shouting" — agents run the same loop; type system makes certain bug classes unshippable, so Rust-like languages are promising agent targets [[Ryhl-Rust](https://newsletter.pragmaticengineer.com/p/why-rust-is-different-with-alice)] (2026-05)
- Gradual + erasable typing is the codegen sweet spot: annotate only where there's no context, let inference flow the rest — forcing annotations everywhere makes the model repeat itself, track more state, and err more; fewer tokens = more efficient generation (why AI targets TS over plain JS) [[Hejlsberg](https://newsletter.pragmaticengineer.com/p/typescript-c-and-turbo-pascal-with)] (2026-05)
- Locality as an agent-friendly language/codebase property: explicit per-file imports make a file's external protocol extractable in isolation → smaller context windows, easy per-module summaries; globals / #include-style scope force whole-program grokking [[Hejlsberg](https://newsletter.pragmaticengineer.com/p/typescript-c-and-turbo-pascal-with)] (2026-05)
- Cargo-cult risk in weakly-checked domains: AI editing kernel makefiles mirrored unnecessary C flags onto the Rust side ("any human would ask why") — plausible-but-wrong output thrives where there's no compiler-grade feedback (build configs, scripts) [[Ryhl-Rust](https://newsletter.pragmaticengineer.com/p/why-rust-is-different-with-alice)] (2026-05)
- Token efficiency as a framework-choice criterion: concise code (Rails) pays twice — fewer generated tokens and human-verifiable output [[DHH](https://newsletter.pragmaticengineer.com/p/dhhs-new-way-of-writing-code)] (2026-04)

## Making products/codebases agent-native

- Make your product agent-native: figure out what agents reliably get wrong, write those gotchas down; watch for client-side JS that hides context from page-summarizing crawlers [[WorkOS](https://www.youtube.com/watch?v=vy7o1g2iHY8)]
- Verbose "enterprise" patterns (heavy DDD, 2000s design patterns) return as agent guardrails: they produce reliable modular code but were dropped for typing cost — the agent types the boilerplate for free, and agents are tireless juniors without training wheels who need exactly those rails [[OpenCode](https://newsletter.pragmaticengineer.com/p/opencode)] (2026-05)
- Design system + pattern library for consistent agent-built UIs: document the rules ("one primary button per page"), build components with previews/snippets agents can see and review against, compose bottom-up; ban inline styles via lint [[Safe Intelligence](https://www.youtube.com/watch?v=504PvfXou5Y)]
- Consumer agents largely ship untested; a quick standardized safety-baseline exam before deploying an agent to act on your inbox/accounts is a cheap guardrail [[DeepMind](https://www.youtube.com/watch?v=Ubwb6NzegyA)]

## Model landscape (watch)

- Text-diffusion serving economics: ~2,000 tok/s at AR-quality parity (edge on code) but lower throughput at large batch → too costly for big hosted models; sweet spot is batch≈1 (on-device, robotics, single-user interactive loops; already deployed inside Alphabet) — expect diffusion coding models there first [[DeepMind-diffusion](https://www.youtube.com/watch?v=r305-aQTaU0)] (2026-06)
- Diffusion gives a pre-settable latency budget: cap denoise steps for a known worst-case latency regardless of output length; the model adaptively finishes easy outputs early (4 passes for 100 memorized tokens vs 31 for hard prose); step budget = tunable speed/quality knob [[DeepMind-diffusion](https://www.youtube.com/watch?v=r305-aQTaU0)] (2026-06)
- In-place editing is a native diffusion primitive — surgical bug-fix/add-docs edits conditioned bidirectionally on surrounding code, vs AR token-by-token regeneration or string-replace [[DeepMind-diffusion](https://www.youtube.com/watch?v=r305-aQTaU0)] (2026-06)
