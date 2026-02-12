# DPO Preference Pair Construction for Text-to-SQL

## Overview

This project focuses on constructing high-quality **contrastive preference pairs** for **Direct Preference Optimization (DPO)** in a Text-to-SQL task.

The methodology for preference pair construction is inspired by the research paper:

> **ExCoT: Optimizing Reasoning for Text-to-SQL with Execution Feedback**

Execution-based filtering is applied to ensure reliable and high-quality supervision for DPO training.

---

## Objective

- Construct `(prompt, chosen, rejected)` contrastive pairs for DPO.
- Use **ChatGPT-5 mini (CoT output)** as the reference model.
- Apply execution-based filtering to remove noisy samples.
- Evaluate LLaMA3.1 8B (4-bit quantized) performance before DPO training.

---

## Preference Pair Construction Strategy

For each question, the following filtering rules were applied:

### Filtering Rules

1. **If ChatGPT generates an incorrect answer**  
   → The question is removed from the experiment.

2. **If ChatGPT is correct and LLaMA is incorrect**  
   → Construct DPO preference pair:
   - `chosen = GPT output`
   - `rejected = LLaMA output`

3. **If both GPT and LLaMA are correct**  
   → Not used for DPO (no contrast signal).

4. **If both are incorrect**  
   → Removed from the dataset.

This approach ensures:
- High-quality contrastive supervision
- Execution-grounded filtering
- Reduced noise in DPO training

---

## Evaluation Metrics

Two evaluation metrics are used:

### Validation Accuracy (VA)
Measures logical correctness of the SQL query.

### Execution Accuracy (EA)
Measures whether the SQL query executes correctly and produces the expected results.

> Execution Accuracy is considered the primary metric.

---

## Accuracy Results  
(Considering Only Records Where ChatGPT Output is Correct)

| Strategy            | Total Records | ChatGPT Correct | LLaMA VA (%)      | LLaMA EA (%)      |
|---------------------|--------------|-----------------|-------------------|-------------------|
| Non-CoT             | 89           | 45              | 77.78% (35/45)    | 35.56% (16/45)    |
| Simple CoT          | 45           | 45              | 77.78% (35/45)    | 37.78% (17/45)    |
| Divide & Conquer    | 44           | 44              | 61.36% (27/44)    | 27.27% (12/44)    |

---

## Key Observations

### 1. CoT Slightly Improves Execution Accuracy
Simple CoT shows a small improvement in EA compared to Non-CoT.

### 2. Divide & Conquer Underperforms
This strategy results in lower VA and EA compared to the other approaches.

### 3. Gap Between VA and EA
Validation Accuracy is significantly higher than Execution Accuracy.

This indicates:
- Structural correctness ≠ semantic correctness
- Execution-based evaluation is essential for Text-to-SQL tasks

### 4. Unexpected Observation

In 1–2 records:
- LLaMA3.1 8B (4-bit) produced correct SQL
- GPT-5 mini produced incorrect SQL

Possible reasons:
- Training data differences
- Model-specific reasoning biases
- Over-reasoning in larger models
- Structured nature of SQL queries

This suggests that larger models are not always superior in structured reasoning tasks.

---

## Why Execution-Based Filtering?

Text-to-SQL tasks are highly execution-sensitive:

- Small logical mistakes break correctness.
- Surface-level similarity is insufficient.
- Semantic correctness must be validated through execution.

Therefore, preference pairs are constructed using execution feedback, following the ExCoT framework.
-- 
