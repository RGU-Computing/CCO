# Stage 2 Evaluation - Step 1

This folder contains the competency question generation and CCO evaluation resources for Stage 2 Step 1.

## Overview

A two-step LLM-assisted evaluation was conducted to examine CCO's support for representing regulatory structures across four compliance domains: education funding, finance, healthcare, and data protection.

## Files

- `Generation.ipynb` - Jupyter notebook for generating competency questions using Qwen3-235B-A22B, with CCO schema.
- `CQs Evaluation.csv` - LLM assessment results for all 1,587 competency questions, including judgement and reason
- `Human_Evaluation.csv` - Human reviewer evaluation results for a stratified random sample of 240 competency questions

## Folders

- `LLM generated CQs/` - Contains the generated competency questions per domain after deduplication and review
- `dataset/` — Contains the regulatory documents used for competency question generation

