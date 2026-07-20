# **Speculative Decoding for Accuracy: Gemma 3 (270M → 1B)**  
*An experimental two‑stage decoding pipeline exploring accuracy‑driven token verification*

---

## **Overview**

This repository contains a minimal but carefully structured experiment using **speculative decoding** with two Gemma 3 models:

- **Drafter:** Gemma 3 270M  
- **Target:** Gemma 3 1B  

The purpose of this project is **not** to reduce inference latency.  
Instead, it investigates a different hypothesis:

> **Speculative decoding can improve accuracy by expanding the search space and using a stronger model to verify candidate tokens.**

The project is intentionally small, but built with research discipline and reproducibility in mind.

---

## **Motivation**

Traditional autoregressive decoding explores only a single continuation at each step. This limits the model’s ability to consider alternative reasoning paths or factual interpretations.

Speculative decoding introduces a second model - the **drafter** - that proposes multiple candidate tokens. The **target** model then evaluates these candidates in parallel and accepts only those that align with its own distribution.

This creates a hybrid of:

- **exploration** (drafter proposes multiple paths)  
- **verification** (target filters for semantic consistency)

The central question is whether this hybrid can produce **more accurate outputs** than the target model alone.

---

## **Hypothesis**

Speculative decoding can act as a lightweight form of **guided search**, improving accuracy by:

- increasing diversity of candidate continuations  
- allowing the target model to reject low‑quality drafts  
- accepting only high‑agreement tokens between both models  

This may reduce hallucinations, improve reasoning consistency, and produce more reliable outputs.

---

## **Architecture**

### **Stage 1 - Drafter (Gemma 3 270M)**  
Generates *K* speculative tokens using nucleus sampling.  
Small size encourages diversity.

### **Stage 2 - Target (Gemma 3 1B)**  
Evaluates the drafter’s proposals in a single forward pass.  
Tokens are accepted only if they meet strict probability‑based criteria.

### **Fallback**  
If a drafted token is rejected, the target model’s own next token is used.

---

## **Acceptance Rules**

To bias toward accuracy rather than speed, acceptance is intentionally conservative:

- Draft token must rank within the **top N** tokens under the target model.  
- Draft token probability must be at least **X%** of the target’s top token probability.  
- Optional: draft token must fall within the target’s **top‑p** nucleus.

These thresholds are tunable and task‑dependent.

---

## **Why Gemma 3?**

Gemma 3 offers:

- consistent tokenizer across model sizes  
- high‑quality small models suitable for drafter roles  
- strong reasoning performance at 1B scale  
- open weights ideal for reproducible research  

This makes it a natural fit for multi‑model decoding experiments.

---

## **Experiment Goals**

- Build a clean, readable speculative decoding pipeline  
- Test whether accuracy improves over baseline 1B decoding  
- Explore how draft length *K* affects correctness  
- Evaluate different acceptance rules  
- Provide a reproducible foundation for further research

---

## **Non‑Goals**

- Production‑grade inference  
- Latency optimization  
- Multi‑token prediction (MTP) from Gemma 4  
- Large‑scale benchmarking

---

## **Planned Experiments**

- **Factual QA:** Does speculative decoding reduce hallucinations?  
- **Reasoning tasks:** Does it improve logical consistency?  
- **Ablations:** Rank‑based vs probability‑ratio vs top‑p acceptance  
- **Diversity vs accuracy:** How does *K* affect correctness?

---

## **Repository Structure**

```
docs/
  experiment_design.md
  methodology.md
  acceptance_rules.md
  evaluation_plan.md
  architecture.md
  config.md
  results/
    baseline_vs_speculative.md
  ablations/
    draft_length.md
    acceptance_thresholds.md
  roadmap.md

/src
  drafter.py
  target.py
  speculative.py
  acceptance.py
  evaluation.py

/notebooks
  speculative_decoding_demo.ipynb

/data
  eval_questions.json

CHANGELOG.md
README.md
LICENSE
```

---

## **Running the Pipeline**

```bash
python src/speculative.py --prompt "Explain quantum tunneling."
```

---

## **Contributing**

This project is intentionally small but built with care.  
Contributions, ideas, and discussions are welcome.

---

## **License**

MIT License 

---

