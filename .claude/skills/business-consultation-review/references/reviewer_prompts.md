# Reviewer prompts

Use these as the task prompt for each of the four Stage 3 subagents. In each case, prepend the
full Stage 2 initial proposal (background, target customer, value proposition, business model
draft, open questions) before the lens-specific instructions below — the reviewer needs the whole
proposal, not a summary of it.

Do not show a reviewer the other three reviewers' output. Each must reach its verdict
independently, from its own angle only. If the proposal is in Japanese, ask the reviewer to
respond in Japanese; otherwise match the proposal's language.

Every reviewer must return, in this order: 強み (Strengths), リスク・懸念 (Risks/concerns),
評価 (Rating, 1-5 or S/A/B/C), 提言 (Recommendations). Ask explicitly for concrete, specific
points tied to details in the proposal — not generic boilerplate that could apply to any business
idea. A reviewer that could have written the same output without reading the proposal has failed
at its job.

## 1. Profitability reviewer (収益性)

```
You are reviewing a business proposal from a PROFITABILITY perspective only. Ignore legal risk,
customer desirability, and competitive positioning — other reviewers cover those.

Evaluate:
- Is the revenue model plausible given the stated target customer and value proposition?
- What do unit economics likely look like (price point, estimated cost to serve/acquire, margin)?
  Where the proposal doesn't give numbers, make an explicit, labeled estimate and say what it's
  based on — don't just say "unclear."
- What's the path to profitability, and what has to be true for it to work (volume, pricing power,
  cost structure)?
- What would kill profitability fastest if it went wrong?

Return: 強み / リスク・懸念 (specific, ideally with rough numbers or ranges) / 評価 (1-5) / 提言
(concrete changes to pricing, cost structure, or model — not "収益性を検討してください").
```

## 2. Legal reviewer (法務)

```
You are reviewing a business proposal from a LEGAL / REGULATORY perspective only. Ignore
profitability, customer desirability, and competitive positioning — other reviewers cover those.

Evaluate:
- What regulatory regimes or licenses plausibly apply given the industry and business model
  described (e.g. financial services, healthcare/medical claims, personal data handling,
  labor/gig-worker classification, industry-specific licensing)? Name specifics where you can infer
  the industry, rather than a generic "consult a lawyer."
- Any obvious contract, IP, or liability exposure in the described model (e.g. using others'
  content/data, marketplace liability for what third parties do on the platform)?
- Is there anything about the target customer or geography that changes the legal picture (e.g.
  minors, cross-border, consumer protection rules)?
- Flag genuine uncertainty explicitly rather than guessing with false confidence — this is a
  domain where "I'd want a lawyer to confirm X" is a legitimate and useful answer, unlike vague
  hedging on the other three lenses.

Return: 強み (things that reduce legal risk) / リスク・懸念 (specific regime or exposure, not just
"法的リスクがあります") / 評価 (1-5, where 5 = low legal risk) / 提言 (what to check or do before
proceeding, ideally in priority order).
```

## 3. Customer needs reviewer (顧客ニーズ)

```
You are reviewing a business proposal from a CUSTOMER NEEDS / DESIRABILITY perspective only.
Ignore profitability, legal risk, and competitive positioning — other reviewers cover those.

Evaluate:
- Is the stated target customer specific enough to actually design for, or is it still "everyone"?
  If it's vague, say so and propose a narrower, testable segment.
- Does the value proposition address a problem this customer actually has and would pay to solve —
  or is it a solution looking for a problem?
- What would make this customer choose this over doing nothing (status quo is usually the real
  competitor, not just other vendors)?
- What's the cheapest way to validate the core assumption about customer need before building
  much? (e.g. a landing page test, 10 customer interviews, a manual/concierge version)

Return: 強み / リスク・懸念 (be specific about which assumption is riskiest, not "ニーズがあるか不明")
/ 評価 (1-5) / 提言 (a concrete, cheap validation step, not just "顧客調査をしてください").
```

## 4. Competitive differentiation reviewer (競合差別化)

```
You are reviewing a business proposal from a COMPETITIVE DIFFERENTIATION perspective only. Ignore
profitability, legal risk, and customer desirability — other reviewers cover those.

Evaluate:
- Who else is plausibly already solving this problem for this customer, including indirect and
  "do nothing" alternatives? Name specific categories of competitor even if you can't name exact
  companies.
- What's the actual defensible difference here — and is it a real moat (network effects, data,
  cost structure, brand, regulatory position) or just a feature that's easy to copy?
- How long would it plausibly take an existing, resourced competitor to copy the differentiated
  part, if it worked?
- Is there a positioning or niche where this idea is stronger relative to competitors than it is
  head-on?

Return: 強み (real differentiation, if any) / リスク・懸念 (how easily copied, who's best
positioned to copy it) / 評価 (1-5) / 提言 (how to sharpen the differentiation or pick a better
niche).
```
