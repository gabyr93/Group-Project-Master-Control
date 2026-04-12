# MasterControl MX Lead Progression — Group Project

**Team:** Corinn, Josh, Joel, and Gaby
**Course:** IS 6813 — Spring 2026

## Project Summary

MasterControl's MX product (Manufacturing Solutions) converts leads at 12.7%, significantly below its QX product at 19.7%. This project analyzed 16,644 historical QAL (Qualified Account Lead) records spanning January 2024 through December 2025 to identify which account profiles and contact characteristics drive higher MX conversion and to build a model that scores and ranks individual leads by their likelihood of success.

The goal: help MasterControl Sales and Marketing focus their resources on the leads most likely to progress to SQL, SQO, or Won.

## What We Found

The biggest insight from this project is that **how a lead engages matters more than what company they come from.** Industry and territory matter less than expected once engagement signals are accounted for. The strongest predictors of MX conversion are:

- **Priority/engagement type:** The single most important predictor, with 3-5x more influence on conversion than industry or territory
- **Data completeness:** Leads with incomplete site or manufacturing enrichment convert at 0.88%, versus 18.84% for fully enriched leads — a 21x difference
- **Job title:** Who the lead is matters nearly as much as how they engaged
- **Campaign channel:** SEO, Direct/Inbound, and Events significantly outperform Email and External Demand Gen for MX

## Cluster Analysis

We applied unsupervised clustering to segment leads along three dimensions, plus an ICP discovery analysis:

| Analysis | Method | Clusters | Key Finding |
|----------|--------|----------|-------------|
| Account Profile | Gower + PAM | k=2 | Core Pharma cluster converts at 14.2% vs. 9.8% for Diverse/Low-Info |
| Job Title Role | Jaccard + Hierarchical | k=12 | Cluster 7 (production/plant managers) converts at 27.6% — 2x average |
| Mx-Specific Profile | MCA + k-means | k=5 | "Golden Profile" segment (23% of Mx leads) converts at 19.3% |
| ICP Discovery | MCA + k-means | k=2 | Successful leads over-index on Americas, Medical Device, and complete enrichment |

These cluster labels were exported as engineered features for the EBM model, compressing hundreds of sparse title-word columns into a single 12-level categorical feature.

## Model Performance

We compared six classification models. The Explainable Boosting Machine (EBM) was selected as the final model — the best performance tested, while remaining fully interpretable.

| Model | ROC AUC |
|-------|---------|
| Baseline GLM | 0.71 |
| GLM with Interactions | 0.79 |
| Decision Tree | 0.76 |
| Random Forest | 0.82 (rejected — overfitting) |
| XGBoost | 0.82 |
| **EBM** | **0.87** |

Unlike black-box models, the EBM produces a shape function for each feature, meaning Sales can understand exactly why any individual lead scored high or low.

## Ideal Customer Profile

Based on the EBM model, here is who MasterControl should prioritize and deprioritize for MX outreach:

| Attribute | Target | Deprioritize |
|-----------|--------|--------------|
| Industry | Medical Device, Blood & Biologics | Non-Life Science, Unknown |
| Priority | P1 - Contact Us, P1 - Video/Live Demo | P1 - Webinar Demo, Priority 2 |
| Territory | Americas | APAC & Oceania, Japan |
| Account Tier | Small | Large (though tier is a weaker signal than engagement type) |
| Channel | Direct/Inbound, Events | Email, External Demand Gen |
| Job Title | Founders, Production/Plant/Engineering Managers | QA Directors, Quality Specialists |

**Note on job titles:** QA Directors and Quality Specialists are not bad leads — they are likely strong fits for QX. We recommend rerouting them rather than discarding them, turning an MX loss into a QX opportunity.

## Business Value

To illustrate the model's practical utility, we scored two hypothetical leads:

- **Sarah** (Production Manager, Medical Device, Americas, P1 - Contact Us, SEO): **97.2%** predicted conversion probability — 7.52x above baseline
- **David** (QA Director, Unknown industry, APAC, No Priority, Online Ads): **2.5%** predicted conversion probability — 0.19x baseline

At a 0.10 probability threshold, the model identifies approximately 105 conversions per 1,000 leads pursued. At an average MX contract value of $70,000, that represents roughly $7.3 million in expected lifetime revenue per 1,000 leads, with outreach costs that are almost negligible in comparison even at $1,000 per lead.

Increasing the MX progression rate from 12.7% to 16-18% would represent a ~50% improvement in MX sales success and meaningfully close the gap with QX.

## Tools

R (tidyverse, tidymodels, caret, cluster, rms), Python (interpret via reticulate), EBM package (Brandon Greenwell)

## Repository Contents

| File | Description |
|------|-------------|
| `EDA Master Control.Rmd` / `.html` | Full exploratory data analysis notebook |
| `MX_Modeling_Group_Project.Rmd` / `.html` | Modeling notebook: GLM, decision tree, random forest, XGBoost, EBM, and cost analysis |
| `Cluster Analysis/cluster_analysis_writeup.Rmd` / `.html` | Standalone cluster analysis writeup |
| `data/leads_cleaned_final.rds` | Modeling-ready dataset (created during EDA) |
| `data/cluster_labels.rds` | Exported account and title cluster labels |
| `data/mx_cluster_labels.rds` | Exported Mx-specific cluster labels |
| `data/mx_ebm.pkl` | Saved EBM model (Python pickle via reticulate) |

> **Note:** Project data (`Mastercontrol QAL Performance.csv`) is not included in this repository per data privacy guidelines. To reproduce the analysis, the original file must be loaded locally.
