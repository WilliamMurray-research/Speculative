# **Baseline vs Speculative Decoding**

## **1. Purpose**

This document defines how baseline decoding is measured and how it will be compared against speculative decoding. The baseline establishes the accuracy of **Gemma 3 1B** when used alone, without drafter guidance or acceptance rules.

---

## **2. Baseline Configuration**

The baseline uses **target‑only decoding** with fixed, deterministic parameters:

- **Model:** Gemma 3 1B  
- **Temperature:** 0  
- **Top‑p:** 1.0 (disabled)  
- **Max tokens:** fixed across all runs  
- **Tokenizer:** shared with drafter  
- **Prompt format:** identical to speculative runs  
- **Seed:** fixed for reproducibility  

This ensures the baseline is stable, repeatable, and directly comparable.

---

## **3. Baseline Procedure**

1. Load evaluation dataset.  
2. For each prompt:  
   - run target‑only decoding  
   - record the output  
   - evaluate correctness  
3. Aggregate accuracy across all items.  
4. Log outputs for qualitative inspection.

This produces the baseline accuracy metric used for all comparisons.

---

## **4. Speculative Comparison Framework**

Speculative decoding is evaluated under multiple configurations:

- varying **draft length K**  
- varying **acceptance rules**  
- varying **drafter sampling parameters**

Each speculative run is compared directly against the baseline using:

- accuracy delta  
- acceptance rate  
- fallback rate  
- error type classification  
- qualitative output differences  

---

## **5. Metrics**

### **Primary**
- **Accuracy (%)** — proportion of correct answers

### **Secondary**
- **Acceptance rate (%)**  
- **Fallback rate (%)**  
- **Average accepted sequence length**

### **Qualitative**
- hallucination frequency  
- reasoning stability  
- factual consistency  

---

## **6. Expected Baseline Behaviour**

Gemma 3 1B should:

- outperform the drafter alone  
- produce stable, deterministic outputs  
- show consistent reasoning patterns  
- occasionally hallucinate or mis‑reason on harder items  

This baseline defines the minimum performance speculative decoding must meet or exceed.

---

## **7. Expected Speculative Behaviour**

Speculative decoding may:

- improve accuracy when acceptance rules filter weak tokens  
- reduce hallucinations via token verification  
- degrade accuracy if acceptance rules are too permissive  
- vary significantly with K and acceptance thresholds  

The experiment is designed to test these hypotheses.

---

## **8. Result Reporting**

All results will be stored under:

```
docs/results/baseline_vs_speculative.md
```

with:

- tables  
- accuracy comparisons  
- acceptance statistics  
- qualitative examples  
- ablation summaries  

---

