# Hypothesis Statement

---

## 1. Research Question

Does accuracy-oriented speculative decoding, using a lightweight drafter model and strict acceptance rules, improve factual accuracy and reasoning stability compared to target-only decoding?

---

## 2. Hypotheses

### Null Hypothesis (H₀)
Speculative decoding **does not** produce higher factual accuracy or more stable reasoning than target-only decoding when evaluated under identical prompts, datasets, and deterministic target-model decoding conditions.

Formally:

$$
\text{Accuracy}_{\text{spec}} \le \text{Accuracy}_{\text{baseline}}
$$

and

$$
\text{ErrorRate}_{\text{spec}} \ge \text{ErrorRate}_{\text{baseline}}
$$

where “error rate” includes hallucinations, reasoning errors, and factual inconsistencies.

---

### Alternative Hypothesis (H₁)
Speculative decoding **does** produce higher factual accuracy and more stable reasoning than target-only decoding, provided that:

*   draft length \(K\) is moderate (e.g., 2–3)
*   acceptance thresholds are strict (rank, probability ratio, nucleus membership)
*   drafter sampling is controlled (top-p $\le$ 0.9)

Formally:

$$
\text{Accuracy}_{\text{spec}} > \text{Accuracy}_{\text{baseline}}
$$

and

$$
\text{ErrorRate}_{\text{spec}} < \text{ErrorRate}_{\text{baseline}}
$$

---

## 3. Operational Definitions

### Speculative Decoding
A decoding method where a lightweight drafter proposes \(K\) tokens and a stronger target model verifies or rejects them using acceptance rules.

### Accuracy
Percentage of outputs matching ground-truth answers in a fixed evaluation dataset.

### Reasoning Stability
Reduction in:
*   multi-step reasoning errors
*   contradictory statements
*   logically invalid inference chains

### Fallback Event
A step where the target rejects the drafter’s token and substitutes its own.

### Acceptance Rules
Thresholds applied to drafter tokens:
*   **rank threshold**
*   **probability ratio threshold**
*   **top-p nucleus threshold**
*   **hybrid combinations**

See [acceptance rules](ca://s?q=Draft_acceptance_rules_document) for details.


---

## 4. Assumptions

*   Drafter and target share the same tokenizer.
*   Target decoding is deterministic (temperature = 0).
*   Dataset is fixed and identical across all runs.
*   Seeds are fixed to ensure reproducibility.
*   No latency or throughput optimisations are considered (accuracy-only focus).
*   Drafter sampling uses controlled diversity (top-p $\le$ 0.9).

---

## 5. Evaluation Criteria

The hypothesis is supported if:

1.  **Accuracy improves** under speculative decoding for at least one configuration of \(K\) and acceptance thresholds.
2.  **Error rates decrease**, especially hallucinations and reasoning errors.
3.  **Fallback frequency correlates** with improved accuracy (i.e., strict acceptance rules outperform permissive ones).
4.  **Reasoning chains show fewer contradictions** and more stable token-level transitions.

These criteria align with your ablations:

*   **draft length**
*   **acceptance thresholds**

---

## 6. Scope

This hypothesis applies to:

*   small-scale models (Gemma 3 270M + Gemma 3 1B)
*   accuracy-oriented decoding
*   factual and reasoning-based evaluation datasets
*   controlled experimental conditions

It **does not** address:

*   latency improvements
*   throughput scaling
*   large-model speculative behaviour
*   multi-drafter architectures

---
