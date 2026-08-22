---
name: business-consultation-review
description: Multi-agent pipeline that takes a vague or early-stage business idea/consultation (新規事業のアイデア相談、事業計画のブラッシュアップ、ビジネスプラン壁打ち) and turns it into a structured, reviewed report. An interviewer agent organizes the idea into an initial proposal, four reviewer agents independently score it on profitability (収益性), legal risk (法務), customer needs (顧客ニーズ), and competitive differentiation (競合差別化), and a report-builder agent synthesizes everything into one HTML report plus a chat summary. Use this whenever the user brings a rough business idea, asks for a "壁打ち"/"ブラッシュアップ"/multi-perspective review of a business plan, or wants feedback on a new service/product concept from several angles at once — even if they only describe it in a sentence or two.
---

# Business Consultation Review (ビジネス相談ブラッシュアップ)

## What this skill does

Takes a business idea that is often vague ("こういう事業をやりたい" level of detail) and runs it
through a small pipeline of specialized reviewers so the user gets back something sharper than
what they put in: a structured proposal, four independent expert critiques, and one synthesized
report they can act on.

Think of it as convening a short internal review board: one person structures the pitch, four
specialists poke at it from their own angle, and one person writes up the findings. Each stage
should genuinely add a distinct perspective rather than restating the previous stage — that's the
entire value of doing this as a multi-step pipeline instead of one pass.

## Pipeline overview

1. **Intake** — make sure there's enough to work with; ask only if truly necessary.
2. **Interviewer** — organize the raw idea into a structured initial proposal, then check the
   direction with the user before spending four reviewers' worth of work on it.
3. **Reviewers (×4, parallel)** — profitability / legal / customer needs / competitive
   differentiation, each producing an independent structured verdict, researched with web search
   where that sharpens the answer.
4. **Report builder** — synthesize the proposal + four reviews into one cohesive report.
5. **Deliverable** — render the report as an HTML artifact, and give the user a concise summary
   in chat.

Work through these in order. Don't skip the reviewer stage or collapse it into the report builder
— the reviewers need to reach their conclusions independently, without seeing each other's
opinions, or their "four perspectives" become one blended, watered-down opinion, which defeats the
point of asking from four angles.

## Stage 1 — Intake

Most consultations will have *something* concrete (a product idea, an industry, a target user),
even if underspecified. Default to proceeding with reasonable, explicitly-stated assumptions
rather than interrogating the user before you've done any work — that's slower and less useful
than showing them a first pass they can correct.

Only stop and ask (via AskUserQuestion) when the input is so thin that any proposal you write
would be pure invention — e.g. a single word with no domain, no customer, no problem. In that
case ask 1-2 targeted questions (e.g. "どんな課題を持つ誰向けの事業ですか？" / "既にお持ちのイメージや制約はありますか？"),
not a long intake form.

If the user writes in Japanese, do the whole pipeline in Japanese (agent prompts, the report, the
summary). Otherwise use the user's language. The stage names and structure below are
language-agnostic — translate the section headers, not the underlying structure.

## Stage 2 — Interviewer: build the initial proposal

Acting as the interviewer, turn the raw consultation into a structured initial proposal. Do this
yourself (no subagent needed — it's mostly organizing what the user already said plus filling
reasonable gaps). Use this structure:

- **背景・課題 (Background / Problem)** — what problem is this solving, and for whom?
- **想定顧客 (Target customer)** — be specific; a segment, not "everyone"
- **提供価値 (Value proposition)** — why would this customer choose this over alternatives?
- **事業モデル仮説 (Business model draft)** — how it makes money: pricing, channel, unit economics
  if there's any basis for a guess
- **未解決の論点 (Open questions)** — the things you had to assume or that genuinely need the
  user's input; carry these forward, don't hide them

Mark anything you filled in rather than the user stating it as **(仮説)** / **(assumption)** so
downstream reviewers and the final report can flag it as something to validate rather than as
settled fact. This is important: a plausible-sounding fabricated detail that reads as confirmed
will mislead every later stage.

**Before moving on, show this initial proposal to the user and confirm the direction** (a short
AskUserQuestion, or just asking in chat if the proposal needs more nuance than a multiple-choice
question allows — e.g. "この方向性で4つの観点のレビューに進めてよいですか？ずれている点があれば教えてください").
Four parallel reviewers plus a synthesis is real work; it's worth one checkpoint here to make sure
it's aimed at the right idea before spending it. If the user corrects something, revise the
proposal and confirm again rather than pushing ahead on a proposal you know is off. Only skip this
checkpoint if the user has already been very explicit and detailed about their idea — the check
is for resolving ambiguity, not a rubber stamp on every run.

## Stage 3 — Reviewers: four independent evaluations

Spawn four subagents in parallel using the Agent tool, all in the same response (foreground —
`run_in_background: false` — since the report builder needs all four before it can proceed and
there's nothing else useful to do meanwhile). Use `general-purpose` as the subagent type, and make
sure the subagents have web search available — each reviewer should look things up rather than
reason from priors alone: the profitability reviewer benchmarking real pricing/CAC figures for
comparable businesses, the legal reviewer checking actual regulatory regimes or licensing
requirements for the relevant industry/jurisdiction, the customer-needs reviewer checking whether
the described problem shows up in real user complaints/forums/reviews, and the competitive
reviewer naming actual existing competitors rather than a generic category. A verdict grounded in
a couple of real, cited findings is worth far more than a longer one reasoned entirely from
priors — push each reviewer to search rather than just assert.

Give each one **only** the Stage 2 proposal plus the lens-specific prompt from
`references/reviewer_prompts.md` — not the other reviewers' output. Independence is the point:
if one reviewer's take leaks into another's context, the four "perspectives" converge into one
opinion, and you lose exactly the thing this pipeline is for.

Each reviewer must return a structured verdict:
- **強み (Strengths)** — what holds up from this angle
- **リスク・懸念 (Risks / concerns)** — specific, not generic ("収益性が低いかもしれない" is weak;
  "客単価◯円想定に対し類似業態のCACが◯円を超えるため、初期は赤字が続く可能性" is useful)
- **評価 (Rating)** — a simple score, e.g. 5段階 (1-5) or S/A/B/C, so the report builder can
  spot which dimension is weakest at a glance
- **提言 (Recommendations)** — concrete next steps or changes, not just "要検討"

See `references/reviewer_prompts.md` for the exact prompt to give each of the four lenses
(profitability, legal, customer needs, competitive differentiation) — use them as-is rather than
improvising the framing, since each is written to keep that reviewer focused on its own angle and
not drift into generic feedback.

## Stage 4 — Report builder: synthesize

Once all four reviews are back, write the unified report yourself (no subagent — synthesis
benefits from you having full context of everything so far). Use the structure in
`references/report_template.md`. The core job here is genuine synthesis, not concatenation:

- Call out where reviewers agree (a risk two or more flagged independently is probably real)
- Call out real tensions between perspectives (e.g. the customer-needs reviewer wants a lower
  price than the profitability reviewer's viable price point) — these tensions are often the most
  valuable output of the whole exercise, so surface them explicitly rather than smoothing them over
- Turn the four sets of recommendations into one prioritized list of next actions, not four
  separate to-do lists

## Stage 5 — Deliverable: HTML report + chat summary

1. Load the `artifact-design` skill before writing the HTML (this is required by the Artifact
   tool's own instructions — it calibrates how much design effort a page like this warrants).
2. Write the report from Stage 4 as a self-contained HTML file per `artifact-design` and the
   `Artifact` tool's requirements (light/dark aware, no external requests, etc.), then publish it
   with the `Artifact` tool. Pick a favicon emoji and a title that names the business idea, not a
   generic "Business Report".
3. In chat, give a concise summary — not a repeat of the full report:
   - One or two lines on what the idea is
   - Overall read (is this promising, promising-with-changes, or has a serious blocker)
   - The single most important risk and the single most important next action
   - The link to the published artifact for the full report

Keep the chat summary short enough to read in a few seconds — the HTML report is where the detail
lives.
