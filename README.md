# Telco Customer Churn Analysis

This repository contains an end-to-end exploratory analysis of a telco customer churn dataset. The goal is to identify key factors driving customer attrition and recommend strategies to reduce churn.

Dataset
Customer Churn.csv

7,043 customer records

21 features including demographics, account information, services, charges, and churn label

Contents
TCA.ipynb – Jupyter notebook performing data cleaning, visualization, and comparative analysis

Customer Churn.csv – Raw dataset

README.md – Project overview and instructions

Key Findings
Overall Churn Rate: 26.5%

Senior Citizens churn at 36.5%, vs. 21.0% for non-seniors

Tenure: High churn in months 1–2; drops below 10% after 24 months

Contract Type:

Month-to-Month: 42.0% churn

One-Year: 11.0%

Two-Year: 3.0%

Internet Service: Fiber optic users churn most (31%), DSL moderate (22.5%), none lowest (10%)

Add-On Services: Subscribers to security, backup, support, and streaming services exhibit 5–10 percentage points lower churn

PhoneService: Multiple-line users churn slightly more (29%) vs. single-line (25%); no-service customers churn least (20%)

Payment Method: Electronic check (33% churn) vs. automatic methods (17–24%)

Recommendations
Promote longer-term (1–2 year) contracts

Upsell bundled add-on services

Target senior citizens with retention initiatives

Incentivize automatic payment enrollment

Reward early-tenure customers to reduce 1–2 month churn spikes
