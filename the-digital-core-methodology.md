# The Digital Core Methodology

*A configuration layer for collaborative research with language models, with a candidate algebraic framework as case study.*

**Wisdom Happy**
*Playful Sincerity Research*
*Draft 2026-04-24 — provisional, pre-peer-review*

**Keywords:** Human–AI collaboration · Agent harnesses · LLM verification · Institutional memory · Configuration as epistemology · Tools for thought.

---

## Author's Note

I am the sole author of this paper. I wrote it in collaboration with Claude (Anthropic), running inside Claude Code (Anthropic's CLI agent harness), shaped by the configuration layer this paper describes. Substantial portions of the prose were drafted by Claude under my direction; I edited, structured, and bear final responsibility for what is claimed. Where the paper makes a first-person observation about what the methodology feels like from the inside, that observation is mine — though Claude often articulated it before I did, and I kept the articulation when it captured what I'd seen.

The convention I am following — sole human author, AI collaborator named in acknowledgments and described throughout the body — matches the convention I use for other Playful Sincerity Research artifacts. It is not a hedge. It is a deliberate naming choice: when an AI system is doing something more than pattern-matching but less than independent authorship, the honest move is to name *the system* (Playful Sincerity Digital Core, henceforth PSDC) rather than the underlying model. PSDC is what produces work no individual component could produce alone. It deserves attribution; I deserve responsibility.

---

## Abstract

Over three weeks in March–April 2026, working alone with an AI collaborator inside a structured environment of my own design, I produced 403 computational verification checks across six independent tool chains for a candidate algebraic framework that spans nine classical mathematical domains. The results are provisional. There has been one external reviewer and no formal peer review at the time of writing. What follows is not a paper about the mathematics. It is about the configuration layer that made the mathematics possible: the **Playful Sincerity Digital Core (PSDC)** — a configuration-on-harness architecture of persistent rules, on-demand skills, delegated subagents, deterministic hooks, and a running chronicle and memory layer, sitting atop Claude Code (Anthropic). I describe (i) the five artifact types comprising PSDC, (ii) the generate-verify-refine research loop they enable, (iii) the IVNA case study that tested them, (iv) the broader intellectual lineage this work sits inside (Engelbart, Licklider, Clark, Hutchins, Alexander, second-order cybernetics, and the Zettelkasten tradition), and (v) what this methodology does and does not show about lone researchers in 2026.

---

## 1. Introduction

The claim is narrow. One researcher, working alone with a language model for three weeks inside a configuration layer of my own construction, produced provisional results in abstract algebra at a velocity that would plausibly have taken months for a traditional lone researcher. The Lean 4 formalization covers only first-order virtual numbers. The "division by zero" framing the work originally used turns out, on closer inspection, to be misleading, and I walked it back. Despite those caveats, I think the question this work raises is real:

*When a researcher is embedded in a sufficiently structured AI environment, does "lone researcher" still mean what it meant in 2023?*

I do not know. I am writing this paper to describe what I built, what I did, what I observed, and how it fits into a sixty-year intellectual lineage I think most of the current AI discourse has forgotten — so that other researchers, especially in academia, can decide for themselves whether to build something similar.

My honest view, after three weeks of intensive use across one mathematical case study and several adjacent research projects: a language model can be a real research partner for a researcher who has the discipline to build a verification layer around it. Without that layer, the model will generate plausible mathematics, plausible citations, and plausible proofs, and a researcher who trusts it gets a plausible paper full of errors. With the layer, the generation-plus-verification loop becomes something that moves faster than I think a single human or a naïve human-LLM pair can move.

**Roadmap.** Section 2 places this work in its intellectual neighborhood — agent harnesses, memory systems, research-pipeline automation, the cognitive-scaffolding lineage, and systems-design patterns. Section 3 describes PSDC itself: the five artifact types and how they compose. Section 4 describes the research loop. Section 5 is the case study. Section 6 discusses three broader themes that the prior-art survey surfaced and the paper makes explicit. Section 7 is honest about what this is and is not evidence for. Section 8 closes with open questions.

Every non-trivial claim below is either computationally verified and reproducible from a public repository, internally consistent but not externally reviewed, or provisional hypothesis. I mark which is which.

---

## 2. Background and Related Work

PSDC sits at the intersection of five active research and engineering communities. None of them, in my reading, captures the full pattern. This section names where PSDC's components have prior art and where the integration is, as far as I have been able to verify, novel.

### 2.1 Agent harnesses and frameworks

An *agent harness* is the software layer that gives an LLM access to tools — file systems, shell, web, subagents — and orchestrates tool calls across a multi-turn task. Claude Code (Anthropic) is the harness PSDC runs on top of [1]. Comparable harnesses include Cursor [2] and Aider as IDE-integrated agents; AutoGen (Microsoft) [3] and LangGraph (LangChain) [4] as multi-agent orchestration frameworks; SWE-Agent (Princeton/Stanford, NeurIPS 2024) [5], OpenHands/OpenDevin [6], and Cognition's Devin [7] as task-specialized autonomous engineers; and CrewAI [8] as a role-based multi-agent system.

The distinction PSDC introduces is that it is *not* a harness. It is a configuration layer that operates on top of a fixed harness. The harness handles tool dispatch and execution; PSDC shapes how the model reasons within the harness — through always-loaded behavioral rules, persistent memory and chronicle files, and deterministic shell hooks that fire at session events. This is closer to the relationship between an operating system and an application than between two applications. To my knowledge, no published harness or framework treats the configuration layer as an independent, version-controllable, durable object in this way.

### 2.2 LLM memory and continuity systems

Persistent memory is an active research area. Letta (formerly MemGPT) [9] introduced an OS-style virtual-memory analogy with core and archival memory. Mem0 [10] extracts and indexes facts from interactions automatically. CoALA (Princeton) [11] proposes a four-quadrant memory taxonomy (working, episodic, semantic, procedural) for language agents. Voyager (NVIDIA) [12] demonstrated lifelong skill accumulation in Minecraft. Stanford's Generative Agents [13] used natural-language memory traces with reflection cycles for social simulation.

Every modern memory system I am aware of uses learned summarization or neural compression to manage memory growth. PSDC uses **hand-authored markdown files** — daily chronicle entries written by me (with substantial AI drafting assistance), and per-topic memory files under a lightweight `MEMORY.md` index. The choice is deliberate. It is mechanically wasteful at the token level — uncompressed prose, no learned compression — and it is exactly what the Zettelkasten tradition has prescribed for sixty years [14, 15]. Niklas Luhmann's 90,000 paper notes remained navigable for decades; the same property — re-findability through human-readable structure — is what I am preserving here. PSDC trades token efficiency for narrative coherence and full audit trail. The chronicle is not just storage; it is a record of *why*, in prose, that another researcher could read.

### 2.3 AI-assisted research pipelines

Sakana AI's *The AI Scientist* (2024) [16] and *AI Scientist v2* (2025) demonstrated end-to-end autonomous research pipelines: hypothesis, experiment, paper drafting. GPT Researcher [17] and Stanford's Storm [18] automate literature synthesis and report generation.

These systems are designed for autonomy. PSDC is designed for the opposite: human-in-the-loop research where the AI is the velocity multiplier and verification is load-bearing. The model in PSDC does not produce a paper; it produces a *candidate* paragraph, formula, or argument that I check, push back on, or formalize. The verification layer is what keeps the speed from being hallucinated speed. None of the autonomous-research systems I have seen treats verification this way; they verify outputs at the end, not the inputs at every step.

### 2.4 Cognitive scaffolding and the extended mind

The deepest lineage PSDC sits inside is the one current AI discourse most often forgets. J.C.R. Licklider's *Man-Computer Symbiosis* (1960) [19] explicitly described a system in which "men set the goals, formulate the hypotheses, determine the criteria, and perform the evaluations" while computers "do the routinizable work that must be done to prepare the way for insights and decisions in technical and scientific thinking." Doug Engelbart's *Augmenting Human Intellect: A Conceptual Framework* (1962) [20] introduced the H-LAM/T system — Human using Language, Artifacts, Methodology, and Training — and argued that intellectual capability is a function of the whole augmented system, not the human alone.

PSDC is, structurally, an instantiation of H-LAM/T. *Language* is shared natural-language convention plus the markdown formats. *Artifacts* are the chronicle, memory, and rule files. *Methodology* is the generate-verify-refine loop. *Training* is the configuration of the AI itself, plus the practitioner's accumulated practice. I did not set out to implement Engelbart; I built the system that worked, and only when researching prior art did I realize how exactly it matches the 1962 specification.

The philosophical scaffolding for why this works comes from Andy Clark and David Chalmers' *The Extended Mind* (1998) [21] and Edwin Hutchins' *Cognition in the Wild* (1995) [22]. Clark and Chalmers argued that cognitive processes are not bounded by skin and skull — when a notebook does the same job a memory does, the notebook is part of the cognitive system. Hutchins documented how distributed cognitive systems (a navigation team coordinating across instruments, charts, and crew) achieve performance no individual member could achieve alone. PSDC extends both arguments to a human-AI dyad: the chronicle, memory, rules, and skills are *part of the cognition*, not external aids to it.

The contemporary lineage on the human side is the tools-for-thought community — Bret Victor's *Inventing on Principle* and Dynamicland [23], Andy Matuschak's working notes [24], Roam Research, Obsidian, and the broader Zettelkasten revival [25]. PSDC's claim is that the same patterns extend to human-AI cognition, and that the AI's full participation requires the same kind of externalized, version-controlled, human-readable structure.

### 2.5 Systems design and pattern languages

PSDC's component vocabulary — rules, skills, agents, hooks, chronicle, memory — is a small pattern language in Christopher Alexander's sense [26]. Each pattern names a recurring problem in the human-AI workflow and a structural solution; the patterns compose. Donella Meadows' *Thinking in Systems* [27] gives the leverage-points framework: small structural changes (a behavioral rule, a Stop hook, a checkpoint) can shift system behavior more than large additions of content. Second-order cybernetics — particularly Heinz von Foerster's insistence that the observer is part of the system being observed [28] — is the operating philosophy: the chronicle is the system observing itself, and the breath rule is the system pausing to ask whether the observation is still right. Pattie Maes' *Concepts and Experiments in Computational Reflection* (1987) [29] anticipates the architectural pattern: the system carries an explicit model of itself that it reasons about and modifies.

Andrej Karpathy's recent work on "Software 3.0" [30] frames this period as one in which behavioral configuration of language models becomes the new programming. PSDC is one instance of what such configuration looks like when it is treated as serious infrastructure rather than a one-off prompt.

---

## 3. The Digital Core

PSDC has three structural layers. The **engine** is Claude — the language model that does the reasoning. The **harness** is Claude Code, which gives the model files, shell, web, subagents, and other tools. The **configuration** is PSDC: the rules, skills, agents, hooks, chronicle, and memory I have authored on top of the harness to shape how the model works.

The engine and harness are Anthropic's. The configuration layer is mine. It consists of five kinds of artifact.

**Rules.** Always-loaded markdown instructions that shape how the model reasons. The current public set [31] includes rules for semantic logging (maintain a running narrative of what is happening and why), epistemic verification (do not claim a task complete until it has been observed working), model selection (route subagent tasks to cheaper models when possible), breath (pause periodically to check whether the current trajectory is still right), honesty (state what is true about where things are; never fabricate evidence), and safety scans on shell commands and fetched web content. From the inside, rules feel like durable biases. They are not features the model turns on; they are conditions it works under by default. Without them, the model drifts. With them, it mostly stays on-task and mostly catches its own mistakes. The rule layer is the least-discussed and, in my experience, the most consequential part of PSDC.

**Skills.** On-demand workflows invoked by slash command. Examples include `/debate` (spawn three adversarial sub-instances to stress-test a claim), `/play` (creative exploration with three voices — thread-follower, paradox-holder, pattern-spotter), `/plan-deep` (recursive decomposition of a large task), `/research-papers` and `/research-books` (multi-database literature search with raw-source preservation), and `/think-deep` (a meta-skill that composes research, play, structure, challenger, and synthesis into a single workflow). Skills keep complexity out of the main conversation until it is needed.

**Agents.** Delegated sub-instances of the model spawned via Claude Code's built-in `Agent` tool, with scoped context and, often, smaller and cheaper models. More than half of subagent work in recent IVNA sessions was routed to Haiku (smallest), with Sonnet reserved for reasoning-heavy tasks and Opus reserved for post-debate synthesis. Model routing is itself a rule. From the inside, delegating to a cheaper version of the model feels closer to scheduling a task than to talking to a different entity — but the context isolation is real, and it is what lets work scale beyond a single conversation's attention span.

**Hooks.** Deterministic shell scripts that run when the model's turn ends. They check: has the chronicle been updated recently? has a breath-pause been taken? is the right model being routed for the current subagent task? The hooks emit reminders the model is then required to act on. This is self-regulation that does not depend on the model remembering to regulate itself, which matters because it frequently does not. Hooks are the *enforcement* layer for rules that require proactive action.

**Chronicle and memory.** A per-day markdown log of what happened, why, and what it means (`chronicle/YYYY-MM-DD.md`), plus a persistent memory system at `memory/`. The chronicle is the narrative layer — what was decided and why, in prose. Memory is the cross-conversation fact layer — what needs to carry forward. Between them, they close the gap that the model's statelessness leaves open. When a new conversation begins, the model's context is not empty; it is the prior work, summarized for it by the work itself.

A sanitized public release of PSDC is available [31]; the full private system contains additional project-specific skills, rules, and memory not appropriate for public distribution.

---

## 4. The Research Loop: Generate–Verify–Refine

The single most important part of the methodology is not the model. It is the verification layer around it.

The model's main risk as a research partner is *confident fabrication*. It generates plausible output by default; the plausibility is not evidence of correctness. Without a verification layer, a researcher who trusts the model's outputs ends up with work that looks right and is not. PSDC encodes verification as the load-bearing step in every research loop.

**Domain-aware verification.** Each research domain has a profile that declares what can be computed, what can be proved, and which tool applies. For IVNA, the profile specified: algebraic identities are checked in SymPy; axiom consistency is checked in Z3; symbolic results are cross-verified in Wolfram; core theorems are formalized in Lean 4. The profile is a rule the model follows.

**The loop.** The model generates a candidate claim. It writes code (or Lean) to test the claim. It runs the test. If the test fails, the generation was wrong, and the model refines. If the test passes, the model logs the pass and moves on. The pattern is not novel; it is the standard scientific-computing pattern. What is novel is making it the *default* — enforced by rules and hooks across an entire research project, including the meta-step of auditing the verification suite itself.

**Six tool chains.** The IVNA verification suite uses Python (IVNA's own reference implementation), SymPy (symbolic algebra), Z3 (SMT solver for axiom consistency), Wolfram Engine (independent cross-verification), Lean 4 (machine-checked proofs), and a meta-verification layer that audits the suite itself. No single chain is trusted alone. Agreement across independent tools is the signal we rely on.

**Adversarial debate.** For claims that resist computation — philosophical framings, literature interpretations, naming decisions — we use `/debate`. Three instances of the model with different lenses (at least one adversarial steel-manner) argue the claim. The convergence, or the remaining disagreement, is itself evidence about the claim's robustness. As a single instance, the model tends to converge too quickly toward agreement; as three instances with assigned positions, it produces more honest resistance.

**Multi-session decomposition.** Work that exceeds a single conversation's capacity is split into parallel session briefs — self-contained markdown files that each drive a separate conversation. IVNA's Phase 4 ran five such sessions in parallel: EXPLORE (open questions), VERIFY (triple-check the unification table), SEARCH (prior art across 15+ databases), RESTRUCTURE (paper organization), and DEBATE (adversarial stress test). They reconverged at a written synthesis.

**Research-agent outputs as primary data.** Every research subagent's full output is saved as a dated markdown file and cross-referenced in the synthesis. Anyone reading the final paper should be able to trace a claim back to the specific agent that produced it. This hedges against a failure mode the model is prone to: summarizing research in a way that is more coherent than the research itself, and thereby smoothing over real disagreements in the sources.

---

## 5. Case Study: IVNA

**What IVNA is.** Indexed Virtual Number Algebra is a candidate algebraic framework in which certain classically "undefined" expressions are given consistent algebraic meaning by carrying nonzero index labels. The original framing — "dividing by zero" — is technically misleading. A recent internal `/think-deep` session established this clearly enough that the paper's current revision walks it back. More accurately: IVNA introduces objects `0_x` and `∞_y` (indexed zeros and indexed infinities) on which multiplication is defined by `0_x · ∞_y = xy`. The indices `x` and `y` must be nonzero scalars in the underlying field. The operation is *not* a division by the scalar zero. It is a product of two indexed objects whose labels happen to include the words zero and infinity.

With this corrected framing, one product rule — `0_x · ∞_y = xy` — appears, in different disguises, across nine classical mathematical domains: first-order derivatives; Riemann integration; Dirac delta normalization, sifting, and scaling; conditional probability (Bayes' theorem as `0_a / 0_b = a/b`); removable singularities in complex analysis; residue extraction; compound growth and scaling symmetry; algebraic blow-up correspondence with exceptional divisors in **P¹**; and KL divergence and renormalization limits. The observation that these nine instances share one algebraic rule appears to be novel; PSDC's prior-art search across 31 queries in 7 different search strategies found no source presenting them as a unified algebraic pattern. Individual connections within the list — between infinitesimal calculus and the Dirac delta, for example — are well known. The unified algebraic framing is the new part.

**Verification record.** As of the 2026-04-06 run of the IVNA verification suite: IVNA-native tests, 198 checks, 0 failures; nonstandard-analysis embedding tests, 70 checks, 0 failures; classical correspondence tests, 93 checks, 0 failures; Wolfram cross-verification, 42 checks, 0 failures. The Lean 4 formalization contains 11 axioms and 12 derived theorems with zero `sorry`. A meta-verification layer adds 18 checks of the suite itself. The grand total is 403 checks, 0 formal failures.

**What that count does and does not mean.** It does not mean IVNA has been proved correct. It means that across six independent tools, no tool has yet found a contradiction in the axioms or their claimed consequences. Domains 3 through 9 in the unification table are currently verified as *classical correspondences*: SymPy or Wolfram computes the classical result, and IVNA notation is shown to produce the same answer. They do not yet demonstrate a case where IVNA computes a novel result that classical analysis cannot.

**The meta-audit — the load-bearing story.** On 2026-04-06, a scheduled audit of the verification suite discovered that the derivative verification function, `ivna_derivative()`, was a passthrough that returned its input unchanged. The model had written it. Twenty derivative tests had been "passing" by computing nothing. We rewrote the function as a genuine `A-VT + A8` pipeline; the corrected version passes 30 of 30 unit tests. The audit also found that prior verification counts had been inflated by about 270 classical-math checks that were not IVNA-native; the authoritative count was revised downward to 403.

I tell this story because it is the methodology doing what it is supposed to do. A circular test will pass silently. Neither the model nor I would have caught it by inspection, because there was nothing visibly wrong. The only way to catch it was to audit the tests themselves, and the audit was not triggered by suspicion — it was a scheduled step in the loop. This is exactly the kind of failure mode the model is prone to, and the methodology's main job is to catch it before it ships.

**External review.** IVNA has had one external reviewer to date: Kiel Howe, a Stanford physics PhD who works on renormalization, who reviewed the paper on 2026-04-13. His most incisive question — *"why do I need the subscripts? If I just define `inf*` and `0*` with their product equal to one, can't I leave the coefficients explicit?"* — turned out to be formally correct at every level of our current formalization. The Lean 4 axioms encode multiplication of two indexed objects as coefficient-multiplication, which is exactly what Kiel identified. The paper's next revision must either rigorously defend the indexed notation as more than notational convenience or reframe the contribution as a unifying algebraic vocabulary rather than a new number system. I think the reframing is the honest move.

**What is genuinely provisional.** The "killer theorem" — a result IVNA proves that nonstandard analysis cannot — has not been found. Until it is, IVNA's status is notational, pedagogical, and organizational. Whether that counts as a mathematical contribution is for peer review to determine.

**On provenance and speed.** The core ideas in IVNA come from my own research, which predates this AI collaboration by about nine years. The model's contribution in this case study was not to invent the mathematics. It was to formalize it: write the verification code across six tool chains, formalize the axioms in Lean 4, run the prior-art search across fifteen-plus databases, draft the paper, and stress-test specific claims through adversarial debate. The active AI-assisted work on IVNA took roughly 24–36 hours, spread across two days in early April 2026 inside a three-week collaboration window. Attributing the ideas to me and the formalization velocity to the methodology is, I believe, the honest split.

---

## 6. Discussion: Three Broader Themes

The prior-art survey in §2 surfaced three themes the methodology embodies but had not, before this paper, named explicitly. I name them here because I think they are the parts most worth carrying forward, regardless of whether PSDC itself catches on.

### 6.1 The 1960s vision, finally instantiable

I did not set out to implement Licklider or Engelbart. I built the system that worked under daily stress. Only when I researched the prior art for this paper did I realize that the H-LAM/T architecture from 1962 maps almost element-for-element onto PSDC: the chronicle is the artifact layer, the rules and skills are the methodology layer, the model itself is the language layer, and my accumulated practice is the training layer. Licklider's symbiosis claim — humans set goals, formulate hypotheses, evaluate; machines do the routinizable work — is an exact description of what generate-verify-refine looks like in practice.

This is not a claim of intellectual borrowing. It is a claim that the 1960s vision *required* sufficiently capable underlying components to instantiate. Until language models capable of meaningful tool use existed, the configuration layer had nothing to configure. Now it does. PSDC is one early instantiation; I expect many more, and I expect most of them to converge on the same architectural pattern, because the pattern is dictated by the structure of the problem: stateless engine + harness + persistent configuration + human-readable artifacts. Any team that takes the problem seriously will reinvent something in this neighborhood.

### 6.2 Hand-authored memory as a deliberate choice

Every modern memory system I am aware of uses learned compression to manage memory growth. PSDC does not. The chronicle and memory files are written by hand (with substantial AI drafting assistance, but the prose is human-readable and human-edited, never neurally summarized). This is, on the face of it, inefficient. It uses more tokens. It scales with operator effort, not just compute.

It is also the design choice that makes everything else work. A chronicle entry written in prose, read six weeks later, can be understood. A neurally summarized vector cannot. When a future session needs to know *why* a decision was made, a summary written by a human (or a human-AI pair with the human reviewing) is usable in a way no learned compression has yet matched. This is the same property Luhmann's Zettelkasten relied on: human-readable structure persists across decades of use, while compressed indexes lose specificity over time.

The trade is real. PSDC consumes more tokens than Mem0 would. The trade is worth it for the same reason version-controlled source code is worth it over a binary blob: human-readable structure is the substrate of long-term audit, and audit is the substrate of trust. For research work, that trade dominates.

### 6.3 Verification-narrative integration

The strongest claim I want to make about PSDC is one the prior-art survey supported by failing to find a precedent. Existing systems handle verification as an output: they run tests, they produce a pass/fail. They handle narrative as a separate output: they generate a paper, a report, a summary. *They do not integrate the two.* The chronicle in PSDC is not a log of what was done. It is the record of *why* a verification step was taken, what alternatives were considered, what surprised the operator, what was checked because the system flagged it, and what was deferred. The narrative *is* the proof that the verification was real and not theatrical.

The meta-audit of IVNA's `ivna_derivative()` passthrough function (§5) is the canonical example. Without the chronicle, the bug would have shipped. The chronicle did not catch the bug; the scheduled audit caught the bug. But the audit was a scheduled step *because* the chronicle from prior weeks had documented the verification suite's growth and flagged the meta-verification step as worth running. The narrative produced the audit; the audit caught the bug; the chronicle now contains the bug-and-fix story for future reference. That is the integration I mean.

I do not yet know if "verification-narrative integration" is the right name for this. I do know that it is what makes the methodology worth describing.

---

## 7. What This Is Evidence For, and What It Is Not

**What the case study provisionally supports.** A single academic researcher, working with a sufficiently capable language model in a structured environment that emphasizes computational verification, can move faster through the early exploration of a mathematical framework than a traditional solo researcher could. The verification layer is what keeps the speed from being hallucinated speed. The meta-audit is what keeps the verification layer from being hallucinated verification.

**What it does not support.** It does not show that AI systems will replace mathematical research teams. It does not show that IVNA is a correct or important framework; peer review has not happened. It does not show the methodology generalizes to every research domain; IVNA is one case, and mathematics is an unusually friendly domain for computational verification.

**On academic research partnerships.** I think this pattern matters most for academic researchers in heavily computational or formalizable fields — mathematics, theoretical computer science, theoretical physics, formal linguistics, some corners of biology and chemistry — who do not have large research groups and who are willing to treat the AI-plus-verification stack as a collaborator with real strengths and real failure modes. The strengths are breadth, speed, and patience. The failure modes are confident fabrication, premature convergence, and (as the meta-audit showed) writing tests that do not test what they claim to. All three failure modes can be mitigated if the researcher treats verification as load-bearing rather than decorative.

**On the methodology being tried elsewhere.** PSDC is currently in use across several other Playful Sincerity Research projects: a graph-based memory architecture, a candidate physics framework, a long-running formal-language project, and others. I withhold architectures and findings from those projects here. I mention them only to report that the methodology is being tried beyond the case this paper documents, and that it has not yet collapsed under the broader load.

**On the digital core as a category.** PSDC is one instance; the category is worth naming. A *digital core*, as I use the term, is a configuration and knowledge layer that sits on top of an agentic system — shaping how the agent reasons in-session, and carrying institutional memory across sessions and across the humans who operate the system. It includes the agent's operating rules, its workflows, the record of prior decisions, and the persistent facts carried forward. In the bus-problem sense — what survives if any particular person stops being the operator — the digital core is what survives. Individual pieces exist in many systems today: config files, knowledge bases, runbooks. What is new is the integration: a coherent layer treating persistent memory, methodology, and continuity as one thing, built specifically for collaboration with language models. I expect digital cores, in this sense, to become common — for individual researchers, for small teams, for institutions — because what they compensate for is general.

---

## 8. Open Questions

- Is there a theorem IVNA proves that nonstandard analysis does not? The single most important open question about the mathematics, and the one most likely to determine whether the framework deserves mathematical-contribution status.
- Does the methodology scale beyond mathematics to empirical sciences, where verification is noisier and slower? My intuition is yes, with adaptations — the verification layer has to absorb statistical, not just symbolic, evidence — but I have not tested this.
- What is the right form of external validation for work produced this way? Traditional peer review, collaborative replication by another AI-plus-human pair, formal theorem provers — each gives different guarantees. The right answer is probably *all three, in sequence*, but I do not yet know how to formalize that.
- Can the methodological patterns in PSDC be extracted into a form other researchers can adopt without re-doing the months of environment-building the system represents? The sanitized public release [31] is an attempt at this; whether it lands depends on whether the patterns are as portable as I think they are.
- How does the configuration layer evolve as model capability evolves? PSDC's specific rules and skills are calibrated to the failure modes of present-day language models. As the failures change, the configuration will need to change. The pattern — rules + skills + agents + hooks + chronicle + memory — should outlast the specific contents.

**Reproduction.** The IVNA repository is public. The PSDC Meta System is public [31]. Every verification check cited in §5 can be re-run from the IVNA repository.

**A closing note.** This paper is itself produced using the methodology it describes. A `/think-deep` session scoped it. A `/research` round produced the prior-art survey in §2. Agent outputs are saved alongside this draft. The chronicle for 2026-04-19 logged the original drafting decisions; the chronicle for 2026-04-24 logs this revision. The recursion is intentional: the work documents the tool that produced the work. What I hope a careful reader takes away is not that I am a capable researcher, though I may or may not be, but that the environment I built around the language model made the work possible in a way the research community has not fully registered yet.

---

## Acknowledgments

This paper, like the IVNA work it describes, was developed in collaboration with the **Playful Sincerity Digital Core** — a research methodology system I have built on top of Claude Code (Anthropic). Substantial drafting assistance came from Claude (Anthropic) operating within this methodology; I am the author and bear final responsibility for the claims, but PSDC and the underlying model were essential to the work's velocity and to the prose's clarity. Kiel Howe (Stanford) provided the first external review of IVNA on 2026-04-13; his question about the subscripts is reshaping the framework. Errors are mine. Any successes are the methodology working the way it is supposed to.

---

## References

[1] Anthropic. *Claude Code overview.* https://code.claude.com/docs/en/overview ; Anthropic, *Building agents with the Claude Agent SDK.* https://www.anthropic.com/engineering/building-agents-with-the-claude-agent-sdk

[2] Cursor (Anysphere). *Cursor — The AI Code Editor.* https://cursor.com/

[3] Wu, Q., Bansal, G., et al. *AutoGen: Enabling Next-Gen LLM Applications via Multi-Agent Conversation.* Microsoft Research, 2024. https://github.com/microsoft/autogen

[4] LangChain. *LangGraph — durable, stateful agent orchestration.* https://github.com/langchain-ai/langgraph

[5] Yang, J., Jimenez, C. E., et al. *SWE-agent: Agent-Computer Interfaces Enable Automated Software Engineering.* NeurIPS 2024. https://arxiv.org/abs/2405.15793

[6] All-Hands AI. *OpenHands (formerly OpenDevin) — Code Less, Make More.* https://github.com/All-Hands-AI/OpenHands

[7] Cognition AI. *Introducing Devin, the first AI software engineer.* 2024. https://cognition.ai/blog/introducing-devin

[8] crewAI Inc. *CrewAI — Framework for orchestrating role-playing, autonomous AI agents.* https://github.com/crewAIInc/crewAI

[9] Packer, C., Wooders, S., et al. *MemGPT: Towards LLMs as Operating Systems.* 2023. https://arxiv.org/abs/2310.08560 ; Letta. https://github.com/letta-ai/letta

[10] Mem0 AI. *Mem0 — The Memory Layer for Personalized AI.* https://github.com/mem0ai/mem0 ; Mem0 paper: https://arxiv.org/abs/2504.19413

[11] Sumers, T. R., Yao, S., Narasimhan, K., Griffiths, T. L. *Cognitive Architectures for Language Agents.* Princeton, 2023. https://arxiv.org/abs/2309.02427

[12] Wang, G., Xie, Y., et al. *Voyager: An Open-Ended Embodied Agent with Large Language Models.* NVIDIA, 2023. https://voyager.minedojo.org/

[13] Park, J. S., O'Brien, J. C., et al. *Generative Agents: Interactive Simulacra of Human Behavior.* Stanford, ACM UIST 2023. https://arxiv.org/abs/2304.03442

[14] Luhmann, N. *Communicating with Slip Boxes.* (Trans. by M. Kuehn.) Discussion of the Zettelkasten method behind Luhmann's ~90,000-note research output.

[15] Ahrens, S. *How to Take Smart Notes.* 2017. The contemporary canonical treatment of the Zettelkasten tradition.

[16] Lu, C., Lu, C., Lange, R. T., et al. *The AI Scientist: Towards Fully Automated Open-Ended Scientific Discovery.* Sakana AI, 2024. https://sakana.ai/ai-scientist/ ; *AI Scientist v2*, 2025.

[17] GPT Researcher. *Autonomous research agent for comprehensive online research.* https://github.com/assafelovic/gpt-researcher

[18] Shao, Y., Jiang, Y., et al. *Assisting in Writing Wikipedia-like Articles From Scratch with Large Language Models (STORM).* Stanford, NAACL 2024. https://github.com/stanford-oval/storm

[19] Licklider, J. C. R. *Man-Computer Symbiosis.* IRE Transactions on Human Factors in Electronics, 1960. https://groups.csail.mit.edu/medg/people/psz/Licklider.html

[20] Engelbart, D. C. *Augmenting Human Intellect: A Conceptual Framework.* SRI International, 1962. https://www.dougengelbart.org/pubs/augment-3906.html

[21] Clark, A., Chalmers, D. *The Extended Mind.* Analysis 58(1), 1998. https://consc.net/papers/extended.html

[22] Hutchins, E. *Cognition in the Wild.* MIT Press, 1995.

[23] Victor, B. *Inventing on Principle.* CUSEC 2012. https://vimeo.com/906418692 ; Dynamicland. https://dynamicland.org

[24] Matuschak, A. *Working notes.* https://notes.andymatuschak.org

[25] Roam Research, Obsidian, and the broader tools-for-thought community. The Zettelkasten revival in personal knowledge management 2018–present.

[26] Alexander, C., Ishikawa, S., Silverstein, M. *A Pattern Language: Towns, Buildings, Construction.* Oxford University Press, 1977.

[27] Meadows, D. *Thinking in Systems: A Primer.* Chelsea Green, 2008. *Leverage Points: Places to Intervene in a System.* The Sustainability Institute, 1999.

[28] von Foerster, H. *Cybernetics of Cybernetics, or, the Control of Control and the Communication of Communication.* Future Systems Inc., 1995. The canonical statement of second-order cybernetics.

[29] Maes, P. *Concepts and Experiments in Computational Reflection.* OOPSLA 1987. https://dl.acm.org/doi/10.1145/38765.38821

[30] Karpathy, A. *Software 2.0* (Medium, 2017) and subsequent talks on Software 3.0 framing language-model configuration as the new programming paradigm.

[31] *Playful Sincerity Digital Core — Meta System.* Sanitized public release. https://github.com/Playful-Sincerity/claude-system

---

*Word count (body): ~5,200*
