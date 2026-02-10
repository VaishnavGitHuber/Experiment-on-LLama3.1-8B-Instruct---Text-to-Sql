# Database Verification Report – California Schools (BIRD Dataset)

## Overview
This repository documents the **manual verification** of the *California Schools* database used in the **BIRD text-to-SQL benchmark**.  
The objective of this study is to ensure the correctness and reliability of question–SQL pairs before conducting model evaluation and training (Base Model, SFT, and DPO).

High-quality ground truth is essential, as incorrect or ambiguous SQL queries can significantly affect evaluation metrics such as **Execution Accuracy (EX)** and **Validation Accuracy (VA)**.

---

## Dataset Reference
- **Dataset:** BIRD (Benchmark for Investigating Relational Database Reasoning)
- **Database:** California Schools
- **Original reported accuracy:** **92.96%** (validated by the BIRD development team and domain experts)

---

## Verification Methodology
- Each **natural language question** was manually reviewed.
- Corresponding **SQL queries** were executed on the database.
- Queries were verified for:
  - Correct SQL syntax
  - Correct execution behavior
  - Unambiguous ground-truth outputs

---

## Verification Results

### Overall Accuracy
- **Correct queries:** **97.75%**
- **Incorrect / ambiguous queries:** 2

### Identified Issues
The following queries were found to be problematic:
- **Question 53**
- **Question 72**

**Issue:**  
Instead of producing a single expected output, these SQL queries returned **multiple rows**, making the ground-truth answers ambiguous and unsuitable for reliable evaluation.

---

## Accuracy by Question Type

| Question Type | Correct / Total | Accuracy |
|--------------|----------------|----------|
| Simple       | 53 / 54        | 98.00%   |
| Moderate     | 29 / 30        | 96.67%   |
| Challenging  | 5 / 5          | 100.00%  |

---

## Implications for Experiments
- Ambiguous queries can **artificially lower Validation Accuracy (VA)** even when the generated SQL is logically correct.
- Queries **53** and **72** should be:
  - Excluded from evaluation, or
  - Corrected to ensure a single unambiguous output.

This verification ensures that subsequent experiments—**Base Model Evaluation, SFT, and DPO**—are conducted on a **reliable and high-quality benchmark**.

---

## Next Steps
- Exclude or correct ambiguous queries.
- Use the verified dataset for:
  - Non-quantized base model evaluation
  - DPO experiments
  - Fair comparison across prompting strategies

---

## Author
**Vaishnav Krishna P**  
Computer Science Researcher
