# Ikonik Dental Analytics & Patient No-Show Prediction

> **Disclaimer:** *This project utilizes a dummy dataset provided for a technical assessment. Explicit permission has been obtained from the respective organization to showcase this analysis for portfolio purposes.*

##  Project Overview
Patient "No-Shows" (missed appointments) represent a major operational bottleneck in healthcare clinics, leading to wasted practitioner hours and uncollected revenue. 

In this end-to-end case study, I analyzed multi-sheet clinic operations data (covering patient demographics, appointment logs, treatments, cashier payments, and marketing spend) to uncover the root causes of **RM338,360 in revenue loss** and built a predictive **Machine Learning model** to flag high-risk appointments before they happen.

---

##  Tech Stack & Tools Used
* **Microsoft Excel:** Data cleaning, multi-table reconciliation, VLOOKUP/XLOOKUP, and operational modeling.
* **Tableau:** Interactive dashboarding for revenue tracking, branch performance, and marketing ROI analysis.
* **Python (Jupyter Notebook):** Data wrangling (`pandas`, `numpy`), Machine Learning (`scikit-learn`), and visualization (`matplotlib`, `seaborn`).

---

##  Key Analytical Workflow

### 1. Data Merging & Cleaning
* Consolidated relational tables (`ADMIN_CS_Appointments`, `MASTER_Patients`, and `CASHIER_Payments`) using `PatientID`.
* Handled missing values (`NaN`) resulting from left joins—imputing uncollected monetary metrics as `0` and categorical unrecorded fields as `'None'`.
* Engineered the binary target variable `Is_No_Show` (`1` for No-Show, `0` for Attended).

### 2. Predictive Modeling & The "Imbalanced Data" Challenge
* **Initial Baseline (Standard Random Forest):** Achieved a high overall Accuracy of **91.02%**, but suffered from a low Recall of **24%** on the minority class (`No-Show`) due to severe class imbalance (2,534 Attended vs. 295 No-Shows in the test set).
* **Optimized Model (Balanced Random Forest):** Applied `class_weight='balanced'` to penalize false negatives heavily.
  * **Recall (No-Show) surged from 24% to 75%** (Accuracy adjusted to **67.16%**).
  * **Business Justification:** In clinic operations, the cost of a **False Negative** (missing an actual No-Show, leaving an empty dental chair and losing hundreds of Ringgit) is significantly higher than the cost of a **False Positive** (sending an automated WhatsApp reminder to a patient who was already planning to attend).

### 3. Feature Importance Analysis
Extracted feature importance scores from the Random Forest classifier to identify the primary drivers behind patient No-Shows:
1. **`ReminderSent_Y` (~0.44 Importance Score):** Whether a patient received an appointment reminder was the single strongest predictor of attendance.
2. **`channel_None` (~0.40 Importance Score):** Walk-in patients or bookings without a tracked marketing channel showed the second-highest risk of missing appointments.
3. **Branch Location (`Puchong` & `Shah Alam`):** Specific branches exhibited distinct No-Show patterns compared to others.
4. **`WaitTime_Min` (<0.01 Importance Score):** Contrary to common assumptions, clinic waiting time had almost zero impact on whether a patient showed up for their scheduled slot.

---

##  Strategic Business Recommendations
1. **Mandatory Automated Reminders (SOP):** Implement an automated WhatsApp/SMS reminder system 24 hours prior to appointments. Since `ReminderSent` is the #1 driver, this low-cost intervention directly protects clinic revenue.
2. **Targeted Manual Follow-Ups:** Require front-desk staff to record acquisition channels for all patients and conduct manual confirmation calls specifically for high-risk segments (such as untraceable/`None` channel bookings).
3. **Branch-Specific Operational Audits:** Review booking confirmation workflows at the Puchong and Shah Alam branches to standardize patient follow-up procedures.
