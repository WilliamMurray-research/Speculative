# **Results Templates**

These templates give you a consistent, rigorous way to report results across baseline runs, speculative runs, and ablation studies. They make your repo look like a real experimental project rather than an ad‑hoc code dump.

---

## **1. Baseline Results Template**

```
# Baseline Results — Target-Only Decoding

## Summary
- Model: Gemma 3 1B
- Temperature: 0.0
- Top-p: 1.0
- Max Tokens: <value>
- Dataset Size: <N>

## Accuracy
- Correct: <X>
- Incorrect: <Y>
- Accuracy: <Z>%

## Error Breakdown
- Hallucinations: <count>
- Reasoning Errors: <count>
- Factual Errors: <count>

## Observations
- <qualitative notes>

## Example Outputs
### Correct
- Prompt: <prompt>
- Output: <output>

### Incorrect
- Prompt: <prompt>
- Output: <output>
- Expected: <ground truth>
```

---

## **2. Speculative Decoding Results Template**

```
# Speculative Decoding Results

## Configuration
- Drafter: Gemma 3 270M
- Target: Gemma 3 1B
- Draft Length (K): <value>
- Acceptance Rules: <rank/prob_ratio/top-p/hybrid>
- Drafter Top-p: <value>

## Accuracy
- Correct: <X>
- Incorrect: <Y>
- Accuracy: <Z>%
- Delta vs Baseline: <Δ>

## Acceptance Statistics
- Acceptance Rate: <value>%
- Fallback Rate: <value>%
- Avg Accepted Sequence Length: <value>

## Error Breakdown
- Hallucinations: <count>
- Reasoning Errors: <count>
- Factual Errors: <count>

## Observations
- <qualitative notes>

## Example Outputs
### Accepted Draft Tokens
- <token sequence>

### Rejected Draft Tokens
- <token sequence>

### Fallback Cases
- <token sequence>
```

---

## **3. Ablation Results Template — Draft Length (K)**

```
# Ablation: Draft Length (K)

## Tested Values
K ∈ {1, 2, 3, 5}

## Summary Table
| K | Accuracy (%) | Acceptance Rate (%) | Fallback Rate (%) | Notes |
|---|--------------|---------------------|-------------------|-------|
| 1 | <value>      | <value>             | <value>           | <text> |
| 2 | <value>      | <value>             | <value>           | <text> |
| 3 | <value>      | <value>             | <value>           | <text> |
| 5 | <value>      | <value>             | <value>           | <text> |

## Observations
- <analysis>
```

---

## **4. Ablation Results Template — Acceptance Thresholds**

```
# Ablation: Acceptance Thresholds

## Tested Thresholds
- Rank: {3, 5, 10}
- Probability Ratio: {0.5, 0.7, 0.9}
- Top-p: {0.9, 0.95}

## Summary Table
| Threshold | Accuracy (%) | Acceptance Rate (%) | Notes |
|----------|--------------|---------------------|-------|
| Rank 3   | <value>      | <value>             | <text> |
| Rank 5   | <value>      | <value>             | <text> |
| Rank 10  | <value>      | <value>             | <text> |
| Ratio 0.9| <value>      | <value>             | <text> |
| Ratio 0.7| <value>      | <value>             | <text> |
| Ratio 0.5| <value>      | <value>             | <text> |
| Top-p 0.9| <value>      | <value>             | <text> |
| Top-p 0.95| <value>     | <value>             | <text> |

## Observations
- <analysis>
```

---

## **5. Combined Summary Template**

```
# Combined Summary — Baseline vs Speculative

## Baseline Accuracy
- <value>%

## Best Speculative Accuracy
- <value>% (Configuration: <details>)

## Accuracy Delta
- <value>%

## Key Findings
- <bullet points>

## Recommendations
- <bullet points>
```

---



