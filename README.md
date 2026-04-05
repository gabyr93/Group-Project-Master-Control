# MasterControl MX Lead Progression — Group Project

**Team:** Corinn, Josh, Joel, and Gaby

**Course:** IS 6813 — Spring 2026

---

## Project Summary

MasterControl's MX product (Manufacturing Solutions) converts leads at 12.7%, significantly below its QX product at 19.7%. This project analyzed 16,644 historical QAL (Qualified Account Lead) records to identify which account profiles and contact characteristics drive higher MX conversion — and to build a model that scores and ranks individual leads by their likelihood of success.

The goal: help MasterControl Sales and Marketing focus their resources on the leads most likely to progress to SQL, SQO, or Won.

---

## What We Found

The biggest insight from this project is that **how a lead engages matters more than what company they come from.** Industry and territory matter less than expected once engagement signals are accounted for. The strongest predictors of MX conversion are:

- **Priority/engagement type:** The single most important predictor, with 3–5x more influence on conversion than industry or territory
- **Job title:** Who the lead is matters nearly as much as how they engaged
- **Campaign channel:** SEO, Direct/Inbound, and Events significantly outperform Email and External Demand Gen for MX

A second key finding: **missing account data is not noise, it is a warning sign.** Leads with incomplete site or manufacturing enrichment convert at 0.88%, versus 18.84% for fully enriched leads — a 21x difference.

---

## Ideal Customer Profile

Based on our EBM model (ROC-AUC of 0.86), here is who MasterControl should prioritize and deprioritize for MX outreach:

| Attribute | Target | Deprioritize |
|---|---|---|
| Industry | Medical Device, Blood & Biologics | Non-Life Science, Unknown |
| Priority | P1 – Contact Us, P1 – Video/Live Demo | P1 – Webinar Demo, Priority 2 |
| Territory | Americas | APAC & Oceania, Japan |
| Account Tier | Small | Large |
| Channel | Direct/Inbound, Events | Email, External Demand Gen |
| Job Title | Founders, Production/Plant/Engineering Managers | QA Directors, Quality Specialists |

**Important note on job titles:** QA Directors and Quality Specialists are not bad leads — they are likely strong fits for QX. We recommend rerouting them rather than discarding them, turning an MX loss into a QX opportunity.

---

## Model Performance

We compared multiple classification models. The **Explainable Boosting Machine (EBM)** was selected as the final model with a **ROC-AUC of 0.86**, the best performance tested, while remaining fully interpretable. Unlike black-box models, the EBM produces a shape function for each feature, meaning Sales can understand exactly why any individual lead scored high or low.

To illustrate the model's practical utility, we scored two hypothetical leads:

- **Sarah** (Production Manager, Medical Device, Americas, P1 – Contact Us, SEO): **97.2% predicted conversion probability: 7.52x above baseline**
- **David** (QA Director, Unknown industry, APAC, No Priority, Online Ads): **2.5% predicted conversion probability: 0.19x baseline**

---

## Business Value

At a 0.10 probability threshold, the model identifies approximately 105 conversions per 1,000 leads pursued. At an average MX contract value of $70,000, that represents roughly **$7.3 million in expected lifetime revenue per 1,000 leads** with outreach costs that are almost negligible in comparison even at $1,000 per lead.

Increasing the MX progression rate from 12.7% to 16–18% would represent a ~50% improvement in MX sales success and meaningfully close the gap with QX.

---

## Repository Contents

- `Business Problem Statement Group1.pdf` — project scope and objectives
- `EDA Master Control.Rmd` / `.html` — full exploratory data analysis notebook
- `Modeling Master Control.Rmd` / `.html` — full modeling notebook including EBM, random forest, decision tree, and cost analysis
- `Master Control Presentation Group1.pdf` — final presentation slides

> **Note:** Project data is not included in this repository per data privacy guidelines. To reproduce the analysis, the original `Mastercontrol QAL Performance.csv` file must be loaded locally.
