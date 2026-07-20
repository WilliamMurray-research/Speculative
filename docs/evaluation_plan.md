# **Evaluation Plan**

## **1. Objectives**

The evaluation aims to determine whether speculative decoding using:

- **Drafter:** Gemma 3 270M  
- **Target:** Gemma 3 1B  

produces **more accurate outputs** than target‑only decoding.  
Accuracy is measured across factual and reasoning tasks with objective correctness criteria.

---

## **2. Evaluation Dataset**

The dataset consists of a small, curated set of prompts covering:

- factual questions  
- short reasoning tasks  
- logic puzzles  
- simple math word problems  

Each item includes:

- a prompt  
- a ground‑truth answer  
- an evaluation function for correctness  

Dataset will be added in **v0.0.2**.

---

## **3. Baseline Evaluation**

The baseline uses **target‑only decoding** with fixed parameters:

- deterministic sampling (temperature = 0)  
- shared tokenizer  
- fixed max tokens  
- consistent prompt formatting  

Baseline accuracy establishes the reference point for all speculative comparisons.

---

## **4. Speculative Evaluation**

Speculative decoding is evaluated under multiple configurations:

### **4.1 Draft Length (K)**
Test values:  
- K = 1  
- K = 2  
- K = 3  
- K = 5  

### **4.2 Acceptance Rules**
Evaluate each rule independently and in hybrid form:

- **Rank threshold**  
- **Probability ratio**  
- **Top‑p nucleus**  
- **Hybrid combinations**

### **4.3 Drafter Sampling**
Test drafter top‑p values:  
- 0.8  
- 0.9  
- 0.95  

---

## **5. Metrics**

### **Primary Metric**
- **Accuracy (%)** — proportion of correct outputs

### **Secondary Metrics**
- **Draft acceptance rate (%)**  
- **Fallback rate (%)**  
- **Average accepted sequence length**  
- **Error type classification:**  
  - hallucination  
  - reasoning error  
  - factual inconsistency  

### **Diagnostic Metrics**
- acceptance rule activation frequency  
- distribution of accepted vs rejected tokens  
- per‑task performance breakdown  

---

## **6. Evaluation Procedure**

1. Load dataset.  
2. Run baseline target‑only decoding.  
3. For each speculative configuration:  
   - run full decoding  
   - log acceptance decisions  
   - log fallback events  
   - compute accuracy  
4. Compare speculative accuracy vs baseline.  
5. Perform ablations:  
   - varying K  
   - varying acceptance thresholds  
   - varying drafter sampling  
6. Summarize results in `docs/results/`.

---

## **7. Analysis Framework**

### **Quantitative**
- accuracy deltas vs baseline  
- acceptance rate correlations  
- threshold sensitivity curves  
- K‑value ablation plots  

### **Qualitative**
- inspect outputs with large accuracy deltas  
- categorize error types  
- identify patterns in rejected tokens  
- analyze reasoning chain stability  

---

## **8. Expected Outcomes**

- Strict acceptance rules should improve accuracy.  
- Larger K may increase diversity but reduce acceptance rate.  
- Hybrid rules may outperform single‑criterion rules.  
- Some tasks may show no improvement — important for falsifiability.

---

## **9. Limitations**

- Small dataset limits statistical significance.  
- Small model sizes limit generalization.  
- Accuracy improvements may be domain‑specific.  
- No latency measurements (accuracy‑focused experiment).

---

