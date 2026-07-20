# **Ablation Study: Draft Length (K)**

## **1. Purpose**

This ablation examines how varying the **draft length (K)** in speculative decoding affects:

- overall accuracy  
- acceptance rate  
- fallback frequency  
- reasoning stability  

Draft length controls how many tokens the **drafter (Gemma 3 270M)** proposes per step before the **target (Gemma 3 1B)** verifies them.

---

## **2. Hypothesis**

Increasing K expands the search space and may surface better candidate tokens, but also increases the chance of proposing low‑quality drafts.

Expected trends:

- **Small K (1–2):** stable, conservative, higher accuracy  
- **Medium K (3):** balanced exploration  
- **Large K (5+):** more diversity, lower acceptance rate, possible accuracy degradation  

This ablation tests whether accuracy‑oriented speculative decoding benefits from larger candidate sets.

---

## **3. Experimental Setup**

### **Models**
- Drafter: Gemma 3 270M  
- Target: Gemma 3 1B  
- Shared tokenizer  

### **Controlled Variables**
- acceptance rules  
- prompt format  
- max tokens  
- target sampling parameters  
- fixed seeds  

### **Independent Variable**
- **Draft length K ∈ {1, 2, 3, 5}**

### **Dependent Variables**
- accuracy  
- acceptance rate  
- fallback rate  
- error types  

---

## **4. Procedure**

1. Run baseline target‑only decoding.  
2. For each K value:  
   - run speculative decoding  
   - log acceptance decisions  
   - log fallback events  
   - compute accuracy  
3. Compare results against baseline and across K values.  
4. Perform qualitative inspection of outputs with large deltas.

---

## **5. Metrics**

- **Accuracy (%)**  
- **Acceptance rate (%)**  
- **Fallback rate (%)**  
- **Average accepted sequence length**  
- **Error classification:**  
  - hallucination  
  - reasoning error  
  - factual inconsistency  

---

## **6. Expected Outcomes**

- **K = 1:**  
  - highest acceptance rate  
  - most conservative  
  - likely best accuracy  

- **K = 2–3:**  
  - moderate exploration  
  - potential accuracy gains  
  - stable reasoning  

- **K = 5:**  
  - lowest acceptance rate  
  - more fallback events  
  - accuracy may drop unless acceptance rules are strict  

---

## **7. Analysis Plan**

Quantitative:

- accuracy vs K  
- acceptance rate vs K  
- fallback rate vs K  
- correlation between acceptance and accuracy  

Qualitative:

- inspect reasoning chains  
- identify failure modes  
- compare accepted vs rejected token patterns  

---

## **8. Limitations**

- small dataset limits statistical power  
- small models limit generalization  
- K interacts strongly with acceptance rules  
- results may be domain‑specific  

---

## **9. Related Documents**

- **experiment_design.md**  
- **methodology.md**  
- **acceptance_rules.md**  
- **acceptance_thresholds.md**  

---

