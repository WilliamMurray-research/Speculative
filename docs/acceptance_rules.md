# Acceptance Rules

---

## 1. Purpose

Acceptance rules determine whether a token drafted by the **Gemma 3 270M** model is allowed into the final output or replaced by the **Gemma 3 1B** model’s own next token. These rules are intentionally conservative to bias the pipeline toward **accuracy**, not speed.

---

## 2. Acceptance Philosophy

Speculative decoding traditionally prioritizes throughput. This experiment instead prioritizes **semantic reliability**.

A drafted token is accepted only when the target model’s distribution indicates **strong agreement**. Otherwise, the target model’s own token is used.

This ensures:

*   fewer hallucinations
*   more stable reasoning chains
*   higher factual consistency

---

## 3. Acceptance Criteria

Three acceptance mechanisms are supported. Each can be used independently or combined.

### 3.1 Rank-Based Acceptance

A drafted token is accepted if:

*   its rank under the target model is within the **top N** tokens.

**Rationale:** Tokens ranked highly by the target model are likely semantically aligned with its preferred continuation.

**Configurable Parameter:**
*   `rank_threshold` (e.g., N = 3, 5, 10)

### 3.2 Probability Ratio Acceptance

A drafted token is accepted if:

*   its probability is at least **X%** of the target model’s top token probability.

Formally:
$$
P_{\text{draft}} \geq \alpha \cdot P_{\text{top}}
$$

**Rationale:** This ensures the draft token is not only plausible but competitively probable.

**Configurable Parameter:**
*   `prob_ratio_threshold` (e.g., $\alpha = 0.5, 0.7, 0.9$)

### 3.3 Top-p Nucleus Acceptance

A drafted token is accepted if:

*   it falls within the target model’s **top-p nucleus**.

**Rationale:** Nucleus membership ensures the token is part of the target’s high-confidence distribution.

**Configurable Parameter:**
*   `target_top_p` (e.g., $p = 0.9, 0.95$)

---

## 4. Hybrid Acceptance

Hybrid acceptance rules combine multiple criteria for stricter accuracy bias.

Examples include:

*   **Rank + Probability Ratio**
*   **Probability Ratio + Top-p**
*   **Rank + Top-p + Probability Ratio** (maximally conservative)

Hybrid rules reduce the acceptance rate but increase reliability.

---

## 5. Rejection and Fallback

If a drafted token fails all active acceptance criteria:

*   the target model’s own next token is used
*   the pipeline continues normally
*   the fallback event is logged for analysis

Fallback frequency is a key diagnostic metric.

---

## 6. Configurable Parameters

| Parameter | Description | Typical Values |
| :--- | :--- | :--- |
| `rank_threshold` | Max rank allowed for acceptance | 3–10 |
| `prob_ratio_threshold` | Minimum probability ratio | 0.5–0.9 |
| `target_top_p` | Nucleus threshold | 0.9–0.95 |
| `draft_length_k` | Number of speculative tokens | 1–5 |

All parameters are defined in `acceptance.py`.

---

## 7. Logging

Each acceptance decision logs:

*   drafted token
*   target model rank
*   target model probability
*   probability ratio
*   nucleus membership
*   acceptance or rejection
*   fallback usage

These logs support ablation studies and reproducibility.

---

## 8. Expected Behavior

*   **Strict rules** $\rightarrow$ higher accuracy, lower acceptance rate
*   **Relaxed rules** $\rightarrow$ higher acceptance rate, mixed accuracy
*   **Hybrid rules** $\rightarrow$ best accuracy for reasoning tasks

---

## 9. Limitations

*   Overly strict rules may reduce diversity
*   Some tasks may not benefit from speculative verification
*   Small models limit generalization
