# **Configuration Reference**

## **1. Purpose**

This document defines all configuration parameters used in the speculative decoding pipeline.  
It ensures experiments are reproducible, comparable, and easy to modify.  
It complements **architecture** and **methodology**.

---

## **2. Overview**

The pipeline exposes three major configuration domains:

- **Drafter configuration**  
- **Target configuration**  
- **Acceptance configuration**  

All parameters are deterministic when seeds are fixed.

---

## **3. Drafter Configuration**

| Parameter | Description | Typical Values |
|----------|-------------|----------------|
| `drafter_model` | Model used for speculative proposals | gemma-3-270m |
| `drafter_top_p` | Controls diversity of draft tokens | 0.8–0.95 |
| `drafter_temperature` | Usually fixed at 1.0 for exploration | 1.0 |
| `draft_length_k` | Number of speculative tokens per step | 1–5 |

The drafter’s behaviour is explored in **draft_length**.

---

## **4. Target Configuration**

| Parameter | Description | Typical Values |
|----------|-------------|----------------|
| `target_model` | Model used for verification | gemma-3-1b |
| `target_temperature` | Deterministic decoding | 0.0 |
| `target_top_p` | Nucleus threshold for acceptance | 0.9–0.95 |
| `max_tokens` | Hard limit for generation | 128–512 |

The target model defines the authoritative probability distribution.

---

## **5. Acceptance Configuration**

Acceptance rules determine whether a draft token is appended or replaced.  
Full details are in **acceptance_rules**.

### **Rank Threshold**
- `rank_threshold`  
- Accept draft if its rank ≤ N  
- Typical values: 3, 5, 10

### **Probability Ratio Threshold**
- `prob_ratio_threshold`  
- Accept draft if:  
  \[
  P_{\text{draft}} \ge \alpha \cdot P_{\text{top}}
  \]  
- Typical values: 0.5, 0.7, 0.9

### **Top‑p Nucleus Threshold**
- `target_top_p`  
- Accept draft if it lies within the target’s nucleus  
- Typical values: 0.9, 0.95

### **Hybrid Rules**
Combine multiple thresholds for stricter accuracy bias.  
Explored in **acceptance_thresholds**.

---

## **6. Prompt Configuration**

| Parameter | Description |
|----------|-------------|
| `prompt_template` | Shared prompt format for all runs |
| `tokenizer` | Shared tokenizer for drafter and target |
| `stop_sequences` | Optional task‑specific stopping criteria |

Consistency here prevents prompt‑induced variance.

---

## **7. Evaluation Configuration**

| Parameter | Description |
|----------|-------------|
| `dataset_path` | Location of evaluation dataset |
| `seed` | Global random seed |
| `evaluation_fn` | Function that checks correctness |
| `log_path` | Directory for acceptance/fallback logs |

Evaluation design is defined in **evaluation_plan**.

---

## **8. Logging Configuration**

| Parameter | Description |
|----------|-------------|
| `log_acceptance` | Whether to log acceptance decisions |
| `log_fallbacks` | Whether to log fallback events |
| `log_probs` | Whether to store probability distributions |
| `log_tokens` | Whether to store full token sequences |

Logging is essential for ablations and reproducibility.

---

## **9. Recommended Defaults**

These defaults provide a stable starting point:

```
drafter_top_p: 0.9
drafter_temperature: 1.0
draft_length_k: 2

target_temperature: 0.0
target_top_p: 0.9
max_tokens: 256

rank_threshold: 5
prob_ratio_threshold: 0.7
```

These values balance exploration and accuracy.

---

## **10. Related Documents**

- **architecture.md**  
- **methodology.md**  
- **acceptance_rules.md**  
- **draft_length.md**  
- **acceptance_thresholds.md**  

---

