# **Experiment Design**

## **1. Purpose**

This experiment investigates whether **speculative decoding** - traditionally used for speed—can instead be applied to **improve accuracy** in language model generation. The design evaluates a two‑stage decoding pipeline using:

- **Drafter:** Gemma 3 270M  
- **Target:** Gemma 3 1B  

The central question:  
**Does multi‑model token verification produce more accurate outputs than the target model alone?**

---

## **2. Hypothesis**

Speculative decoding can act as a lightweight form of **guided search**, improving accuracy by:

- expanding the candidate token space via a small drafter  
- allowing the stronger target model to filter low‑quality drafts  
- accepting only high‑agreement tokens between both models  

This may reduce hallucinations and improve reasoning consistency.

---

## **3. Experimental Variables**

### **Independent Variables**
- **Draft length (K):** number of speculative tokens proposed per step  
- **Acceptance criteria:**  
  - rank threshold  
  - probability ratio threshold  
  - top‑p nucleus threshold  
- **Sampling strategy:** drafter top‑p values

### **Dependent Variables**
- **Accuracy** on evaluation tasks  
- **Token acceptance rate**  
- **Fallback frequency** (target-only tokens)

### **Controlled Variables**
- Shared tokenizer  
- Prompt format  
- Evaluation dataset  
- Target model sampling parameters

---

## **4. Models**

### **Drafter (Gemma 3 270M)**
Chosen for diversity and speed. Generates speculative tokens.

### **Target (Gemma 3 1B)**
Chosen for stronger reasoning and factual grounding. Verifies drafter tokens.

---

## **5. Tasks and Evaluation Domains**

The experiment focuses on domains where accuracy is measurable:

- **Factual QA**  
- **Short reasoning tasks**  
- **Logic puzzles**  
- **Simple math word problems**

Each task has a clear, objective correctness criterion.

---

## **6. Procedure**

1. Provide prompt to both models using shared tokenizer.  
2. Drafter generates *K* speculative tokens.  
3. Target evaluates all *K* tokens in a single forward pass.  
4. Apply acceptance rules:  
   - If accepted → append draft token  
   - If rejected → append target’s own next token  
5. Continue until max tokens or EOS.  
6. Compare output accuracy against baseline target-only decoding.

---

## **7. Metrics**

- **Accuracy (%)** across evaluation set  
- **Draft acceptance rate (%)**  
- **Average accepted sequence length**  
- **Error type classification:**  
  - hallucination  
  - reasoning error  
  - factual inconsistency  
- **Comparison vs baseline 1B decoding**

---

## **8. Expected Outcomes**

- Strict acceptance rules should maintain or improve accuracy.  
- Larger K may increase diversity but reduce acceptance rate.  
- Probability-ratio acceptance may outperform rank-only acceptance.  
- Some tasks may show no improvement - important for falsifiability.

---

## **9. Limitations**

- Small model sizes limit generalization.  
- Evaluation dataset is intentionally small.  
- Results may vary across domains.  
- No latency measurements (accuracy-only experiment).

---

## **10. Reproducibility**

All code, acceptance rules, and evaluation data are included in the repository.  
Experiments are deterministic when run with fixed seeds.

