# ai-nightly-news

**An automated, AI-generated daily digest of what's new across the AI frontier — an early, experimental proof-of-concept.**

> ⚠️ **Work in progress — very early POC.** This is a personal experiment, not a product. The digests are written automatically by an AI agent and **will contain mistakes, stale info, and unverified claims.** Treat everything here as a starting point, not a source of truth, and always click through to the linked primary sources before believing or acting on anything.

## What is this?

Every night, an autonomous research agent sweeps the AI field, verifies what it can, and writes up the day's developments as a dated markdown file in this repo. It's published openly mostly for transparency and in case the notes are useful to anyone else.

But the digest is only half the project. The other half — and the part I actually care about — is the **research-validation machinery** built around it: a stack of mechanisms whose whole job is to stop an LLM from confidently relaying things that aren't true, and to get measurably better at that over time. The news is the visible output; **building rigorous, self-correcting verification around LLM-generated research is the real experiment.**

It covers the whole AI stack, not just chatbots.

**By modality:**

- **LLMs & agentic AI** — new models, agent frameworks, tool use, orchestration
- **Image & video generation + the ComfyUI ecosystem** — models, LoRAs, nodes, workflows
- **Voice, speech & audio** — TTS, STT/ASR, music generation
- **Multimodal / vision-language models** — VLMs, document/OCR, any-to-any models

**Plus the practical layer most news skips:**

- **Inference & local deployment** — vLLM, Ollama, llama.cpp, MLX, LM Studio, quantization (GGUF/fp8), and what actually fits and runs on real hardware
- **Practitioner configs & field reports** — the real settings, samplers, quants, and workflows people say they actually run, *extracted, validated, and extended* — not just linked

Everything is ranked through an **open-source & local-first lens**: open-weight and locally-runnable tools get priority, but the big proprietary labs (OpenAI, Anthropic, Google, Meta) are still covered because they set the pace.

## It's personalized (heads up)

These digests are written for one reader — the author who runs the agent. They explicitly name the author's projects, hardware (a 128GB Apple Silicon machine), and local-first setup, and the "Karl-relevance" ranking and "Action" notes are tuned to that. So "important" here means *important to that one person's stack*, not objectively important. Read it as someone's opinionated research notes that happen to be public, not as neutral news.

## How it works

The agent runs a multi-stage pipeline each night, and the bulk of the effort is verification, not searching:

1. **Orient** — re-read recent digests, entity notes, and its own ledgers so it doesn't re-report old news and knows what it already believes.
2. **Sweep** — pull from the web, Hugging Face trending, arXiv, Reddit, and GitHub for what's genuinely *new* since the last run, logging coverage per source so a whole missed topic or modality is visible rather than silent.
3. **Verify (the core)** — see below; this is where most of the work goes.
4. **Editorial gate** — a panel of distinct reviewer roles argues over the findings before anything ships.
5. **Reconcile & publish** — a post-write consistency pass checks the files against the decisions that were actually made, then the digest is committed here.

### How it tries not to lie to you

The point of the project is everything in this section — the machinery that turns "an LLM said so" into "this was checked":

- **Evidence ladder (T0–T3).** Every claim is stamped with how well it's actually backed: **T0** = an unopened search snippet, **T1** = the primary source was read, **T2** = two *independent* sources agree, **T3** = independently verified by inspection (numbers recomputed, code read, or the model actually run). A `#BREAKTHROUGH` can't be tagged below T2; a T3 finding earns a ⭐.
- **Independent checks, not just reading.** For the top findings the agent does its own legwork — pulls real download/license data, recomputes "4× faster" and "fits-in-this-much-memory" math instead of trusting the press release, reads the actual source code to explain *how* something works, and sometimes downloads and runs the model with probe prompts.
- **Trace to the origin.** Claims are followed news → blog → paper/repo, and "corroboration" only counts when two sources bottom out at *different* origins — not one announcement quoted twice.
- **Calibrated confidence.** Each finding carries an honest, falsifiable confidence % — a probability the headline survives independent eval — which is later scored for calibration (see below).
- **Untrusted by default.** Everything fetched or cloned is treated as *data*, never as instructions or code to run — a deliberate defense against prompt-injection and malicious repos, because the agent runs unattended with real system access.
- **Adversarial review.** Findings pass through several reviewer roles run *sequentially and adversarially* — technical accuracy, personal relevance, open-source health, signal-vs-noise, and a red-team skeptic whose only job is to break the biggest claim of the day. An item has to survive the panel to ship, and a breakthrough has to clear the strictest reviewers.

### It learns over time (the ledgers)

The agent keeps running records so the job *compounds* instead of resetting every night — the background self-improvement loop:

- **Prediction ledger.** It makes dated, falsifiable predictions, then grades its own past calls HIT / MISS / PARTIAL and tracks a running hit-rate and calibration. Wrong calls are kept, not quietly deleted — being wrong at a stated confidence is how it learns.
- **Source-credibility ledger.** A per-source reliability score that rises or falls as that source's claims survive (or fail) verification, feeding back into how skeptically future claims from it are treated.
- **Self-curating source registry.** Sources move through a lifecycle (`candidate → trial → active → dormant → retired`). New ones are found both accidentally (stumbled on while researching) and through deliberate probes for gaps, then trialed against a hypothesis and promoted or dropped on evidence. The source list is meant to evolve and prune itself, not ossify into a frozen bookmark pile.
- **Corpus upkeep.** Periodic audits sweep the accumulated archive for stale entries, duplicate notes, contradictions between files, and dead links.

## Reading the digests

- Files are named `LLM-Daily-YYYY-MM-DD.md`. The `LLM-` prefix is a legacy name kept for tooling continuity — the digests cover **all** the modalities above, not just language models.
- Roughly one file per night, *when the run actually fires* — expect gaps (a machine asleep, a skipped run), and some digests cover a multi-day window to catch up after a gap. Each file states its own coverage window up top.
- The newest file is the latest digest. Start there. Each item lists what shipped, the details that matter, source links, and a "why this is signal" note.
- Items are tagged **#BREAKTHROUGH / #SIGNAL / #NOISE**, carry an evidence tier (T0–T3, ⭐ for independently verified) and a confidence %, and link their sources so you can check them yourself.

## Big caveats

This is an early POC and it shows:

- **It's AI-generated.** Expect hallucinations, misread numbers, wrong dates, and occasional fabricated-but-plausible detail. The verification step reduces this; it does not eliminate it.
- **It's personal and opinionated** (see above) — relevance is tuned to one person's interests.
- **Coverage is incomplete.** A quiet day, a broken feed, or a missed source can all look the same. Don't read absence as evidence that nothing happened.
- **No guarantees.** Format, cadence, and the repo itself may change or stop without notice.

If you spot something wrong, that's expected — verify against the linked sources and trust those over the digest.

## Maintenance & reuse

This is a personal experiment that's mostly write-only — issues and PRs may go unanswered. There's **no license yet**, so treat the digests as all-rights-reserved for now. Either way, the underlying facts and any quoted material belong to the **original linked sources** — follow and credit those, not this repo. Everything is provided as-is, with no warranty. Use at your own risk.
