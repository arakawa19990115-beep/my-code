# Report structure (Stage 4)

Use this structure for both the HTML report and as the basis for the chat summary. Translate
headers to the user's language; keep the structure itself.

```
# [事業アイデアの名前 / 一言タイトル]

## エグゼクティブサマリー
- 3-5 lines: what the idea is, overall verdict (有望 / 条件付きで有望 / 重大な懸念あり),
  the single biggest risk, the single most important next action.

## 初期提案
- 背景・課題
- 想定顧客
- 提供価値
- 事業モデル仮説
- 未解決の論点（(仮説) と明記した項目を含む）

## レビュー観点別評価
For each of the four lenses (収益性 / 法務 / 顧客ニーズ / 競合差別化):
- 評価: [score]
- 強み
- リスク・懸念
- 提言

## 総合評価と論点の整理
- Where reviewers agreed (a risk flagged by 2+ reviewers independently is a strong signal —
  call this out explicitly)
- Where reviewers are in tension with each other (e.g. profitability wants a higher price,
  customer needs wants a lower one) — name the tension, don't paper over it
- Your synthesized read: is this fundable/buildable as-is, viable with specific changes, or
  blocked on something that needs resolving first

## 次のアクション（優先順位付き）
- A single prioritized list merging all four reviewers' recommendations — not four separate
  lists. Put the cheapest, highest-information action first (usually a validation step) unless
  there's a blocking legal/regulatory question that has to be resolved before anything else.
```

## Notes for the HTML version

- Score the four dimensions somewhere visible near the top (e.g. a small 4-item scorecard) so a
  reader gets the shape of the evaluation before reading prose — this is a natural fit for a
  dataviz-style stat row; consult the `dataviz` skill's conventions if building anything chart-like
  beyond simple text/number tiles.
- Visually distinguish anything marked (仮説)/(assumption) in the initial proposal section, so a
  reader can tell at a glance what was stated by the user versus filled in.
- The "次のアクション" list is often what the user actually acts on — give it clear visual weight,
  not just another paragraph at the bottom.
