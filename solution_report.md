# AI Solution Design Report: Claim Fraud Detection

## Task 1: Business Domain
**Domain Selected:** Insurance

## Task 2: Define the Business Problem
* **The Problem:** The insurance company is struggling with high operational costs and slow payout times due to the manual review of every incoming claim for potential fraud.
* **Stakeholders:** Claims Adjusters (internal users), Policyholders (external customers), and the Fraud Investigation Unit.
* **Current Manual Process:** Every claim is manually reviewed by an adjuster who checks the policyholder's history, the claim details, and attached documents to decide if it looks suspicious. 
* **Limitations:** Looking at the sample KPI data, the current process requires upwards of 500+ manual processing hours per month and leads to average resolution times of over 30 hours. Furthermore, human fatigue leads to an error rate of 6-11%, meaning legitimate claims are delayed and fraudulent claims slip through.

## Task 3: Identify the AI Task Type
* **Task Type:** Classification (specifically, Binary Classification) or Anomaly Detection.
* **Why it's suitable:** We need the model to output a distinct category for each claim. By treating this as a binary classification problem, the model can look at the input features and predict a probability score between 0 (Legitimate) and 1 (Fraudulent). Claims scoring above a certain threshold are flagged for human review.

## Task 4: Data Requirement Plan
* **Type of Data:** Primarily structured historical data, with potential for unstructured data (claim descriptions).
* **Input Features:** Claim amount, policyholder tenure, past claims count, location of incident, time of incident, and payment method.
* **Target Variable:** `is_fraud` (Binary: 1 for known fraud, 0 for legitimate).
* **Data Collection Method:** Extracted from the internal Claims Management SQL database, utilizing historical data from the last 3-5 years where fraud was confirmed or denied by human investigators.
* **Data Quality Risks:** The biggest risk is class imbalance. Fraud is relatively rare compared to legitimate claims, so the model might naturally bias toward predicting "not fraud" just to achieve high baseline accuracy.

## Task 5: Model Recommendation
* **Recommended Model:** Feed-Forward Neural Network (FNN).
* **Why it's appropriate:** Since our primary data is structured (tabular features like amounts, counts, and categorical IDs), a feed-forward neural network is highly effective at finding complex, non-linear relationships across many variables that a human adjuster couldn't easily spot. (Gradient Boosting like XGBoost would also be a strong traditional machine learning alternative).

## Task 6: Evaluation Plan
* **Technical Metrics:** We will prioritize **Recall** and the **F1-Score**. High recall ensures we catch as much actual fraud as possible, even if it means a few false positives. Pure accuracy is a bad metric here due to class imbalance.
* **Business Metrics:** Reduction in `manual_processing_hours` and a drop in `average_resolution_time_hours` (as seen in the KPI logs). We also want to track the Customer Satisfaction Score (CSAT) to ensure faster payouts are making users happier.
* **Human Validation Process:** The AI will not automatically reject claims. It will act as a triage system. High-risk claims are routed to the Fraud Investigation Unit, while low-risk claims are fast-tracked for automated payout.

## Task 7: Responsible AI Considerations
* **Bias in Data:** The model might learn to associate certain zip codes or demographic proxies with fraud, leading to unfair profiling of specific neighborhoods.
* **Incorrect Predictions:** A false positive means a legitimate customer's payout is frozen while being investigated, which can cause extreme financial distress.
* **Over-reliance on AI:** Adjusters might develop "automation bias," simply rubber-stamping the AI's flags without doing their own independent investigation.
* **Mitigation:** Strict human-in-the-loop policies for all rejected claims, and regular audits of the model's predictions to check for demographic bias.

---

## Task 8: Final Solution Summary
**Problem:** High manual processing hours and slow resolution times due to human review of all insurance claims for fraud.
**Proposed AI Solution:** An automated triage system that scores incoming claims for fraud probability.
**Required Data:** 3-5 years of structured historical claim records labeled with `is_fraud`.
**Model Recommendation:** Feed-Forward Neural Network (FNN) for tabular data classification.
**Expected Business Impact:** A sharp decrease in the 500+ monthly manual processing hours. Low-risk claims will be resolved in minutes instead of 30+ hours, improving customer satisfaction, while investigators can focus solely on high-risk flags.
**Risks and Mitigation:** Demographic bias and false positives disrupting legitimate users. Mitigated by using the AI strictly as an advisory tool for human investigators, never as an automated rejection engine.
