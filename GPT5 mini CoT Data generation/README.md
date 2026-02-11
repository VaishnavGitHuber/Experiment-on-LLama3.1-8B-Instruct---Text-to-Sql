# Chain-of-Thought (CoT) Data Generation for Text-to-SQL

This repository contains the code, prompts, and results for generating **Chain-of-Thought (CoT) reasoning data** using **GPT-5 Mini** for Text-to-SQL tasks on the California school database.

---

## Objective

1. To study and analyze the reasoning capabilities of GPT-5 Mini.
2. To generate high-quality Chain-of-Thought (CoT) data for small-model training (Supervised Fine-Tuning and Direct Preference Optimization).
3. To evaluate the alignment of generated reasoning outputs with the original **ExCoT: Text-to-SQL** study.

---

## Experimental Setup

- **Platform:** Google Colab (TPU v4, free 60 hours/month)
- **Model:** GPT-5 Mini (API-based ChatGPT system)
- **Temperature:** 1.0 for diverse reasoning
- **Token Limit:** 
  - 2000 tokens for Non-CoT and standard CoT techniques  
  - 3000 tokens for Divide-and-Conquer CoT technique
- **Prompt Dataset:** Stored as individual `.txt` files in `NoCoT_Prompts/`
- **Output Directory:** `NoCoT_Output/` with one-to-one mapping from input prompts

---

## Methodology

1. **Environment Setup**  
   Initialize the OpenAI API and create directories for inputs and outputs.

2. **Prompt Selection**  
   Select relevant question files from the dataset (`question50.txt` to `question88.txt`) using regular expressions.

3. **Input Preparation**  
   Read each prompt file and preserve the text for accurate model input.

4. **CoT Generation**  
   Send prompts to GPT-5 Mini via API to generate detailed step-by-step reasoning outputs.

5. **Output Storage**  
   Save the generated responses in a dedicated output directory with clear naming for traceability.

6. **Error Handling**  
   Log any errors during processing to ensure smooth pipeline execution.

7. **Dataset Completion**  
   Collect all outputs into a structured dataset ready for analysis, evaluation, and training.

---

## Results

| Strategy                | Simple EX | Simple VA | Moderate EX | Moderate VA | Challenging EX | Challenging VA | Total EX | Total VA |
|------------------------|-----------|-----------|-------------|-------------|----------------|----------------|----------|----------|
| No CoT                 | 53.7      | 100       | 43.33       | 96.67       | 60             | 100            | 50.56    | 98.88    |
| Simple CoT             | 53.7      | 98.15     | 43.33       | 96.67       | 60             | 100            | 50.56    | 97.75    |
| Divide & Conquer CoT   | 53.7      | 100       | 43.33       | 96.67       | 40             | 80             | 49.44    | 97.75    |

**Observations:**
- Simple and No-CoT strategies show similar execution accuracy (~50–51%), with CoT slightly lowering validation accuracy.  
- Divide & Conquer CoT improves reasoning for simple/moderate prompts but struggles with challenging prompts (EX drops to 40%).  
- Overall validation accuracy remains high (~97–98%), showing the model maintains correctness even with varied execution.

---

## Files in this Repository

- `NoCoT_Prompts/` – Input prompts for Text-to-SQL tasks.  
- `NoCoT_Output/` – Generated CoT outputs from GPT-5 Mini.  
- `cot_generation.py` – Python script for automated CoT data generation.  
- `README.md` – This file.

---

## License

This project is for **research and educational purposes only**.
