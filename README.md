# global-tech-salary-dataset
This dataset is constructed by merging and harmonizing 3 independent compensation sources: Stack Overflow Developer Survey (2023), Levels.fyi Salary Data (2018–2021), and Glassdoor Job Postings (~2019–2020).  It is designed to support salary prediction, compensation fairness analysis, labor market research, and multi-source domain adaptation studies.

---

The final dataset contains **~107,000 records** across roles, industries, locations, and experience levels.

All salary values are:
-  Normalized to **2023 USD**
-  Annual and numeric
-  Processed with **fairness-aware data preparation**

---

## At a Glance

| Property | Value |
|--------|------|
| Total Records | ~107,000 |
| Sources | Stack Overflow, Levels.fyi, Glassdoor |
| Salary Range | $8K – $580K (post-normalization) |
| Target Variable | `salary` (USD, 2023 normalized) |
| Geographic Coverage | Global (US-dominant) |
| Reference Year | 2023 |
| License | MIT |

---

## Quick Start

```python
import pandas as pd

df = pd.read_csv("global_tech_salary_2023.csv")

print(df.head())
print(df.columns)
````

---

## Motivation

Most public salary datasets suffer from:

* **Temporal drift** → salaries collected across years without inflation adjustment
* **Population bias** → US-heavy, senior-role dominated datasets
* **Fairness blindness** → ignoring structural factors like location

This dataset addresses all three:

* Salaries normalized to **2023 USD**
* Multi-source integration for **global coverage**
* Designed for **fairness-aware modeling**

---

## Data Sources

### 1. Stack Overflow Developer Survey 2023

* Source: [https://survey.stackoverflow.co](https://survey.stackoverflow.co/datasets/stack-overflow-developer-survey-2023.zip)
* Coverage: ~89,000 respondents (global)
* Used: ~46,000 records after filtering
* Salary: `ConvertedCompYearly` (USD)
* Strengths: Global diversity, skills, education
* Limitations: Self-reported, no company data
* Assigned Year: 2023

---

### 2. Levels.fyi Salary Data (2018–2021)

* Source: [https://www.levels.fyi](https://www.levels.fyi)
* Dataset repo: [https://github.com/Yashas-153/Data-Science-Stem-Salaries](https://github.com/Yashas-153/Data-Science-Stem-Salaries)
* Used: ~61,000 records
* Salary: total compensation (base + stock + bonus)
* Strengths: High-quality US tech compensation
* Limitations: US-dominant, equity-heavy salaries
* Year Range: 2018–2021

---

### 3. Glassdoor Job Postings Dataset

* Source repo: [https://github.com/PlayingNumbers/ds_salary_proj](https://github.com/PlayingNumbers/ds_salary_proj)
* Used: ~450–700 records (after filtering)
* Salary: Parsed from ranges (e.g., `$53K–$91K`)
* Strengths: Industry + job descriptions
* Limitations: Small size, estimated salaries
* Assigned Year: ~2019–2020

---

## Schema

| Column                      | Type   | Description                         |
| --------------------------- | ------ | ----------------------------------- |
| salary                      | float  | Final salary (USD, 2023 normalized) |
| log_salary                  | float  | log1p(salary)                       |
| original_salary             | float  | Pre-normalized salary               |
| experience                  | float  | Years of experience                 |
| education                   | string | Education level                     |
| role                        | string | Job role/title                      |
| company                     | string | Employer                            |
| location                    | string | Country / city                      |
| industry                    | string | Industry                            |
| skills_structured           | string | Tech stack (semicolon-separated)    |
| skills_text                 | string | Job description text                |
| data_source                 | string | Source dataset                      |
| sample_weight               | float  | Imbalance correction weight         |
| salary_within_source_zscore | float  | Domain-adaptive normalized salary   |

---

## Preprocessing Pipeline

### 1. Temporal Normalization

* Median-based scaling within Levels.fyi
* Region-specific CPI adjustment (2021 → 2023)
* Stack Overflow treated as 2023 baseline
* Glassdoor adjusted with conservative inflation factor

---

### 2. Outlier Handling

* Per-source 99th percentile clipping
* Global 1st–99th percentile filtering

---

### 3. Dataset Imbalance Correction

```
weight = total_records / (num_sources × source_count)
```

---

### 4. Domain Adaptation Support

* `salary_within_source_zscore` enables source-invariant modeling

---

### 5. Schema Alignment

* Structured vs unstructured skills separated
* Missing features preserved intentionally

---

## Known Limitations

1. **Population heterogeneity**
   Sources represent different labor markets

2. **US bias (~65%)**
   Model may underperform globally

3. **Total vs base compensation mismatch**
   Levels.fyi includes equity

4. **High missingness (40–70%)**
   Features differ across sources

5. **Glassdoor sample size is small**
   Not suitable as primary training source

6. **Experience is low granularity**
   Integer-binned values

---

## Fairness Note

Significant salary disparities exist across locations:

* Same role → large salary gaps across regions
* Example: Software Engineer

  * Mountain View (~$260K)
  * Austin (~$165K)

These reflect **market conditions**, not purely merit.

Models trained on this dataset will inherit these biases unless explicitly mitigated.

---

## Recommended Use Cases

* Salary prediction (ML models)
* Fairness-aware modeling
* Labor market research
* Domain adaptation experiments
* Feature engineering for skills

---

## Repository Structure

```
Global Tech Salary Dataset (Fairness-Aware, 2023 Normalized).ipynb
employee_salary_dataset_final.csv
employee_salary_audit.csv
README.md
LICENSE
```

---

## Data Usage Notice

This dataset is derived from publicly available sources:

* Stack Overflow
* Levels.fyi
* Glassdoor

Users must comply with original source licenses when using this dataset.

---

## Citation

```bibtex
@dataset{global_tech_salary_2023,
  title   = {Global Tech Salary Dataset 2023: A Fairness-Aware,
             Temporally Normalized Multi-Source Benchmark},
  year    = {2023},
  url     = {https://github.com/fareehanezam/global-tech-salary-dataset}
}
```

---

## License

This dataset is released under the MIT License.

You are free to use, modify, and distribute this dataset for any purpose, including commercial use, without restriction. Attribution is appreciated but not required.

---

## Contributing

Issues and pull requests are welcome.
Please report data inconsistencies with justification.

---

## Changelog

| Version | Date | Notes           |
| ------- | ---- | --------------- |
| v1.0.0  | 2026 | Initial release |

---

## Final Note

Built with transparency and reproducibility in mind.
All preprocessing steps are documented and reproducible.

```
```
