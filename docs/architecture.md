# **Architecture Overview**

## **1. Purpose**

This document describes the architecture of the **accuracy‑oriented speculative decoding pipeline**, detailing how the **drafter** and **target** models interact, how acceptance rules are applied, and how decoding flows from prompt to final output.  
It complements **experiment_design** and **methodology**.

---

## **2. High‑Level Pipeline**

The speculative pipeline consists of three core components:

1. **Drafter (Gemma 3 270M)** — proposes candidate tokens  
2. **Target (Gemma 3 1B)** — verifies or rejects those tokens  
3. **Acceptance Module** — applies **acceptance rules** to decide whether a draft token is appended or replaced

The pipeline is designed for **accuracy**, not speed.

---

## **3. Data Flow**

### **Step 1 — Prompt Ingestion**
- Shared tokenizer encodes the prompt.  
- Both models receive identical token sequences.

### **Step 2 — Drafter Proposal**
- Drafter generates **K speculative tokens** using controlled sampling.  
- These tokens form the candidate set for verification.

### **Step 3 — Target Verification**
- Target evaluates all K tokens in a single forward pass.  
- It produces:
  - token ranks  
  - token probabilities  
  - probability ratios  
  - nucleus membership indicators  

### **Step 4 — Acceptance Decision**
The acceptance module applies configured thresholds:

- rank threshold  
- probability ratio threshold  
- top‑p nucleus threshold  
- hybrid combinations  

If accepted → draft token appended.  
If rejected → target’s own next token appended.

### **Step 5 — Continue Decoding**
The process repeats until:
- EOS  
- max tokens  
- task‑specific stopping criteria

---

## **4. Components**

### **4.1 Drafter**
- Lightweight model  
- Generates diverse candidates  
- Tunable via top‑p sampling  
- Sensitive to **draft length**

### **4.2 Target**
- Stronger model  
- Provides authoritative probability distribution  
- Determines acceptance or fallback  
- Deterministic decoding for baseline comparison

### **4.3 Acceptance Module**
Implements all acceptance logic:

- rank-based  
- probability ratio  
- top‑p nucleus  
- hybrid rules  

Thresholds are explored in **acceptance_thresholds**.

### **4.4 Logging Layer**
Captures:
- accepted tokens  
- rejected tokens  
- fallback events  
- threshold activations  
- per‑step metadata  

Essential for ablations and reproducibility.

---

## **5. Control Parameters**

Key architectural parameters:

| Parameter | Role |
|----------|------|
| `draft_length_k` | Number of speculative tokens proposed per step |
| `rank_threshold` | Max rank allowed for acceptance |
| `prob_ratio_threshold` | Minimum probability ratio |
| `target_top_p` | Nucleus threshold |
| `drafter_top_p` | Controls drafter diversity |

All parameters are documented in **config.md**.

---

## **6. Architectural Goals**

- **Accuracy over speed**  
- **Deterministic baseline comparison**  
- **Transparent acceptance logic**  
- **Reproducible experiments**  
- **Modular design** for easy ablations

---

## **7. Limitations**

- Small models limit generalization  
- Acceptance rules may be overly conservative  
- K and thresholds interact strongly  
- No latency optimizations (intentional)

---

## **8. Related Documents**

- **experiment_design.md**  
- **methodology.md**  
- **acceptance_rules.md**  
- **draft_length.md**  
- **acceptance_thresholds.md**  

---

