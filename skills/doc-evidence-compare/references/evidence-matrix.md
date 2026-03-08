# Evidence Matrix

Use this as the default response shape for `$doc-evidence-compare`.

## Top Summary

Start with 2-4 short lines covering:

- what topic was checked
- whether docs and code align overall
- the main mismatch or gap, if any

## Required Matrix

Use a Markdown table with these columns in this order:

| Claim / 问题点 | Doc Evidence | Code Evidence | Status | Gap / Notes | Confidence |
|---|---|---|---|---|---|

## Cell Rules

### `Claim / 问题点`

State one testable point per row.

Examples:

- `退款和支付是否存在明确关联关系`
- `重试策略是否在文档和代码中同时存在`
- `某字段是否只有文档说明但无实现使用`

### `Doc Evidence`

- every non-empty cell must include file references
- if docs were searched but nothing supports the claim, write `none found after doc search`

### `Code Evidence`

- every non-empty cell must include file references
- if code was searched but nothing supports the claim, write `none found after code search`

### `Status`

Allowed values only:

- `documented_and_implemented`
- `documented_only`
- `implemented_only`
- `contradictory`
- `not_found`

### `Gap / Notes`

Use this to:

- explain the mismatch
- label `inference`
- distinguish `文档没写` from `我没找到`
- call out partial coverage or naming drift

Recommended phrases:

- `文档未说明，但代码中有直接实现证据`
- `代码未找到直接实现证据`
- `inference: field naming suggests the same concept`
- `searched docs and code with aliases, no evidence found`

### `Confidence`

Use:

- `high`
- `medium`
- `low`

## Recommendation Block

After the matrix, add a short recommendation block with:

1. whether documentation should be added or corrected
2. whether code should be aligned to documented behavior
3. what to inspect next if confidence is not high

Keep this block short. The matrix is the main deliverable.
