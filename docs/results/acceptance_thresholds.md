# **Ablation Study: Acceptance Thresholds**

## **1. Purpose**

This ablation examines how varying **acceptance thresholds** affects speculative decoding accuracy.  
Thresholds determine how strict the target model must be before accepting a drafter token.  
They directly influence reliability, fallback frequency, and reasoning stability.

This study complements **acceptance rules** and **draft length**.

---

## **2. Hypothesis**

Stricter thresholds should:

- reduce hallucinations  
- increase fallback usage  
- improve factual accuracy  
- stabilize reasoning chains  

Relaxed thresholds should:

- increase acceptance rate  
- increase diversity  
- risk accuracy degradation  

Hybrid thresholds may offer the best balance.

---

## **3. Threshold Types**

### **Rank Threshold**
Draft token must be within the target’s **top N** tokens.  
Test values: N ∈ {3, 5, 10}

### **Probability Ratio Threshold**
Draft token probability must be ≥ α × top‑token probability.  
Test values: α ∈ {0.5, 0.7, 0.9}

### **Top‑p Nucleus Threshold**
Draft token must fall within the target’s nucleus.  
Test values: p ∈ {0.9, 0.95}

### **Hybrid Thresholds**
Combinations of the above for stricter acceptance.

---

## **4. Experimental Setup**

### Controlled Variables
- Models  
- Prompt format  
- Max tokens  
- Dataset  
- Seeds  
- Draft length K

### Independent Variables
- rank threshold  
- probability ratio threshold  
- top‑p threshold

### Dependent Variables
- accuracy  
- acceptance rate  
- fallback rate  
- error types

---

## **5. Procedure**

1. Run baseline target‑only decoding.  
2. For each threshold configuration:  
   - run speculative decoding  
   - log acceptance decisions  
   - compute accuracy  
3. Compare results across thresholds and against baseline.  
4. Inspect qualitative differences in reasoning and factual stability.

---

## **6. Metrics**

- **Accuracy (%)**  
- **Acceptance rate (%)**  
- **Fallback rate (%)**  
- **Average accepted sequence length**  
- **Error classification:** hallucination, reasoning error, factual inconsistency

---

## **7. Expected Outcomes**

### **Rank Threshold**
- N = 3 → strict, high accuracy  
- N = 5 → balanced  
- N = 10 → permissive, possible accuracy drop

### **Probability Ratio**
- α = 0.9 → strict, stable reasoning  
- α = 0.7 → moderate  
- α = 0.5 → permissive, more diversity

### **Top‑p**
- p = 0.9 → strict nucleus  
- p = 0.95 → more permissive

### **Hybrid**
- highest reliability  
- lowest acceptance rate  
- best for reasoning tasks

---

## **8. Analysis Plan**

### Quantitative
- accuracy vs threshold  
- acceptance rate vs threshold  
- fallback rate vs threshold  
- correlation between strictness and accuracy

### Qualitative
- inspect reasoning chains  
- identify threshold‑sensitive failure modes  
- compare accepted vs rejected token patterns

---

## **9. Limitations**

- small dataset limits statistical significance  
- thresholds interact strongly with K  
- strict thresholds may reduce diversity excessively  
- results may be domain‑specific

---

## **10. Related Documents**

- **experiment_design.md**  
- **methodology.md**  
- **acceptance_rules.md**  
- **draft_length.md**  

---
