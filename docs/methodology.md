# **Methodology**

## **1. Overview**

This methodology describes how speculative decoding is implemented and evaluated using a two‑model pipeline:

- **Drafter:** Gemma 3 270M  
- **Target:** Gemma 3 1B  

The drafter proposes candidate tokens; the target verifies them. The methodology ensures reproducibility, clarity, and falsifiability.

---

## **2. Shared Foundations**

### **Tokenizer**
Both models use the **same tokenizer** to ensure alignment of token boundaries and probability distributions.

### **Prompt Format**
All experiments use a consistent prompt template to avoid prompt‑induced variance.

### **Determinism**
Random seeds are fixed for:
- drafter sampling  
- target evaluation  
- dataset ordering  

This ensures reproducible results.

---

## **3. Decoding Pipeline**

### **Step 1 — Drafter Generation**
The drafter produces **K speculative tokens** using nucleus sampling (`top‑p`).  
This encourages diversity and exploration.

### **Step 2 — Target Verification**
The target model evaluates all K tokens in a **single forward pass**, computing:

- token ranks  
- token probabilities  
- probability ratios  
- nucleus membership  

### **Step 3 — Acceptance Decision**
Each drafted token is evaluated against configurable acceptance rules:

- **rank threshold**  
- **probability ratio threshold**  
- **top‑p nucleus threshold**  

If accepted → appended to output.  
If rejected → target model’s own next token is appended.

### **Step 4 — Continue Decoding**
The process repeats until:
- EOS token  
- max token limit  
- task‑specific stopping criteria

---

## **4. Acceptance Rules**

Acceptance rules determine how conservative or permissive the pipeline is.

### **Rank-Based**
Draft token must be within the target’s **top N** tokens.

### **Probability Ratio**
Draft token probability must be ≥ **X%** of the target’s top token probability.

### **Top‑p Nucleus**
Draft token must fall within the target’s **top‑p** distribution.

### **Hybrid**
Combinations of the above for stricter accuracy bias.

---

## **5. Evaluation Procedure**

### **Dataset**
A small, curated set of prompts covering:
- factual QA  
- reasoning tasks  
- logic puzzles  
- simple math problems  

### **Baseline**
Run target‑only decoding to establish baseline accuracy.

### **Speculative Runs**
Run speculative decoding with:
- varying K  
- varying acceptance rules  
- varying drafter sampling parameters

### **Comparison**
Compare speculative outputs against baseline using:
- correctness  
- hallucination rate  
- reasoning consistency  
- token acceptance rate  
- fallback frequency

---

## **6. Metrics**

- **Accuracy (%)**  
- **Draft acceptance rate (%)**  
- **Fallback rate (%)**  
- **Average accepted sequence length**  
- **Error classification:**  
  - hallucination  
  - reasoning error  
  - factual inconsistency  

---

## **7. Experimental Controls**

To ensure validity:

- identical prompts across all runs  
- identical max token limits  
- identical sampling parameters for target model  
- fixed seeds  
- consistent evaluation criteria  

---

## **8. Limitations**

- Small model sizes limit generalization  
- Small dataset limits statistical significance  
- Accuracy improvements may be domain‑specific  
- No latency measurements (accuracy‑focused experiment)

---

## **9. Reproducibility Checklist**

- Shared tokenizer  
- Fixed seeds  
- Logged acceptance decisions  
- Logged fallback events  
- Versioned dataset  
- Versioned code  
- Documented configuration parameters  

---

