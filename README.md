# Text-to-SQL LLM Evaluation: Zero-Shot vs Few-Shot Prompting

This repository contains an end-to-end evaluation of the **Qwen2.5-Coder-7B-Instruct** Large Language Model on the Text-to-SQL task, using the **Spider** dataset.

## Project Objective
The goal of this project is to evaluate the model's ability to translate Natural Language queries into accurate SQL code. The project compares two different prompting strategies:
1. **Zero-Shot Prompting:** Providing only the database schema and the question.
2. **Few-Shot Prompting:** Providing 5 carefully selected, diverse examples covering `JOIN`, `GROUP BY`, `WHERE`, `COUNT`, and `ORDER BY` prior to the question.

## Dataset
This project uses the **Spider Dataset**, a large-scale complex and cross-domain text-to-SQL dataset.
The data is loaded from two different sources in the code:
1. **HuggingFace:** The `xlangai/spider` dataset is used directly via the `datasets` library to load the queries.
2. **Kaggle:** The actual SQLite databases (`.sqlite`) and schema definitions (`tables.json`, `dev.json`) are required to evaluate the Execution Accuracy. You can download this data from Kaggle here: [Yale University's Spider 1.0 NLP Dataset](https://www.kaggle.com/datasets/jeromeblanchet/yale-universitys-spider-10-nlp-dataset). 

## Evaluation Metric
Unlike simple string matching, this project evaluates the generated SQL using **Execution Accuracy**. Both the Gold SQL (ground truth) and the LLM-predicted SQL are executed against the actual SQLite databases. A prediction is marked correct *only* if the resulting data matches exactly.

## Key Findings
- **Zero-Shot Accuracy:** 70.21% (726 / 1034 correct)
- **Few-Shot Accuracy:** 70.60% (730 / 1034 correct)
- **Statistical Significance:** A McNemar's test (p-value = 0.78) revealed that the 0.39% improvement brought by Few-Shot prompting is *not* statistically significant for this specific model.

An in-depth error analysis showed that the model primarily struggles with complex, multi-table `JOIN`s (responsible for 59% of the shared errors) and advanced aggregations.

## Repository Contents
* `nlp-project.ipynb`: The complete Python notebook containing the environment setup, prompt engineering, generation logic, and statistical tests.
* `Text_to_SQL_Project_Report.pdf`: A PDF report summarizing the methodology and findings.
* `zero_shot_results.zip`: The results of the zero-shot prompting to show if each generated query is correct or not.
* `few_shot_results_v2.zip`: The results of the few-shot prompting to show if each generated query is correct or not.

## Tools & Technologies Used
* **LLM:** Qwen2.5-Coder-7B-Instruct
* **Libraries:** HuggingFace Transformers, BitsAndBytes (4-bit quantization), PyTorch, Pandas
* **Statistical Testing:** statsmodels (McNemar's Test)
