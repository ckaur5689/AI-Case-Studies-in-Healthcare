# AI-Case-Studies-in-Healthcare
Case studies examining AI successes and failures in real-world applications, notably in Healthcare.

# Case Study 1

Real-World life example of application of Machine Learning classification in Healthcare:

# Patient at risk of Urgent and Emergency Care (UEC) Hospitalisation in next 12 months. 
In the real patient level data, it is a relatively small proportion of the total patient population who would be admitted to hospital due via. the UEC route, hence this is a minority class that the model would be predicting. 

The remaining patients (majority class) are relatively healthy, any health conditions they have are being managed well in Primary Care (GP) and would not experience a UEC event in the next 12 months.

Predicting the minority class is meaningful as this allows Acute Hospitals to predict demand of their services and flex capacity but more importantly enables GPs to pro-actively target their practice’s more at risk patients with providing additional checks, referrals to community based services or advice on lifestyle choices;  thus reducing the risk of hospitalisation due to UEC which can be very disruptive prolonged event in a  patient’s life. It also puts patients at increase of infections from other hospital patients and is one of the costliest forms of healthcare provision in the NHS. 

This is quite often what Risk Stratification tools used in healthcare are attempting to predict e.g. John Hopkins Risk Stratification Model 
[https://www.hopkinsacg.org/risk-stratification-101-what-is-it-and-how-is-it-used/](https://www.hopkinsacg.org/risk-stratification-101-what-is-it-and-how-is-it-used/)

The imbalance in the classes would mean the “Risk of hospitalisation in the next 12 months” would be difficult to predict as the model does not have enough data to learn and therefore to predict the minority class. There would be more Type II errors (False negatives) i.e. predicting a patient would not be at risk of hospitalisation (majority class) when they actually are (minority class). This would render the model outputs useless, and GPs would be missing patients they could have targeted for prevention and no robust way of predicting demand into Acute Hospitals.

📍Therefore, correcting for the imbalance (stratified sampling or oversampling) is essential to carry out first. 

By oversampling the minority class, to correct for the class imbalance, there is more data made available, for the class of interest for the model to learn and test against. However the trade-off is overfitting and the model created might only show good performance on the local patient population that it was trained on. If the model was applied to a different local patient population with for example different social demographics, then there is a risk of the model not being generalisable enough and performance dropping significantly. 

# IBM Watson Oncology Model

Interesting case study which aligns well to the above case of the dangers of an overfitted model; IBM’s Watson Oncology model was first developed using patient’s data from Memorial Sloan Kettering to predict the most effective cancer treatment for individual patients for specific cancers. While this hospital was highly regarded for cancer treatment worldwide, some raised concerns that Watson's results may be biased, as they largely drew from a wealthier subset of patients. Page 10 IBM Watson: [https://healthark.ai/wp-content/uploads/2023/11/IBM-Watson-From-healthcare-canary-to-a-failed-prodigy_1.pdf](https://healthark.ai/wp-content/uploads/2023/11/IBM-Watson-From-healthcare-canary-to-a-failed-prodigy_1.pdf)

IBM later used synthetic patient data, but the recommendations of which cancer treatment was based on preferences from a few specialists for each cancer type, rather than using established guidelines or evidence. This introduced biasness in the model, thus introducing another shortcoming of the model.  [IBM's Watson recommended 'unsafe and incorrect' cancer treatments | STAT](https://www.statnews.com/2018/07/25/ibm-watson-recommended-unsafe-incorrect-treatments/)

# Case Study 2

Th Optum algorithm involving an AI / machine learning model used by Optum (a subsidiary of UnitedHealth Group) for population health management—that is, to identify which patients should be offered “high-risk care management” programs (extra attention, care coordination, home visits, etc

🧠 1. What the algorithm was designed to do

Optum’s model was a risk-prediction system used by hospitals and insurers to rank patients by future healthcare risk—essentially:

	“Which patients are likely to need the most care in the future?”

It assigned a risk score to each patient. Those with the highest scores were enrolled in intensive “care management” programs aimed at reducing complications and hospitalizations.

According to Optum, this system was used for tens of millions of patients across the U.S., often under product names like Impact Pro or Impact Intelligence (depending on the client).

⚙️ 2. The modeling approach (type of ML)

The algorithm was based on supervised machine learning using regression and decision-tree ensembles (typical of healthcare risk-adjustment models).
Specifically:

	•	Type: Predictive modeling / regression (likely a proprietary variant of gradient boosted trees or random forest regression).
	•	Target variable: The predicted healthcare cost (spending) for each patient in the next year.
	•	Inputs (features):
	•	Age, gender, diagnoses (ICD codes)
	•	Lab results and prior utilisation (hospitalizations, ED visits)
	•	Prescriptions and comorbidities
	•	Past healthcare spending
	•	Output: A numeric score estimating expected future cost.

This kind of model is common in actuarial analytics—insurers use it to forecast costs, adjust risk pools, and plan care budgets.

However… this choice of target variable is what led to the problem.

⚠️ 3. The core flaw — proxy label bias

The model did not directly predict future medical need or disease severity.
Instead, it predicted future spending, assuming that cost = health need.

That’s a proxy label, and in healthcare it’s a dangerous shortcut, because cost is strongly influenced by social and systemic inequities:
	•	Black patients and lower-income patients often spend less on healthcare (due to barriers to access, under-insurance, historical bias, or mistrust).
	•	Therefore, for two equally sick patients—one white, one Black—the model “learned” that the Black patient would likely generate less cost → lower predicted “risk.”

In other words:

	The model mistook under-utilisation of care for lower medical need.

That meant that Black patients were far less likely to be flagged for high-risk care management—even though their actual disease burden was equal or higher.

📊 4. Quantified impact (from the Science study)

The flaw was discovered by Obermeyer et al., Science (2019):

	“Dissecting racial bias in an algorithm used to manage the health of populations” (Science 366, 447–453).

Findings:
	•	The algorithm’s bias was systematic and large.
	•	Among patients assigned the same risk score:
	•	Black patients were significantly sicker (had more chronic conditions and higher disease severity).
	•	As a result:
	•	Only 18% of the patients identified for extra care were Black.
	•	If the model had been unbiased, 46% would have been Black.
	•	The bias affected millions of patients nationwide.

🧩 5. Why it happened (technical chain)

Of course — here’s the information from sections 5 and 6 of the Optum algorithm explanation rewritten in UK English bullet-point form.

⚠️ 5. Why the bias happened – technical chain

	•	Design choice: the algorithm’s target variable was future healthcare cost rather than actual medical need.
→ This meant that existing inequalities in healthcare access were baked into the model.
	•	Training data: based on historical insurance claims, which already reflected racial and socio-economic disparities.
	•	Feature correlations: variables linked to spending (such as postcode or previous service use) reinforced the assumption that lower cost equals lower risk.
	•	Deployment context: the risk score was used to select patients for high-risk care management programmes.
→ Result: many Black patients were incorrectly ranked as lower risk.
	•	Validation metric: the model was assessed using cost prediction accuracy (e.g., correlation or RMSE) rather than real-world health outcomes or fairness indicators.

Overall consequence:
A technically sound cost-prediction model produced systematically biased clinical decisions because its design objective was mis-aligned with healthcare equity.

🧮 6. The type of machine-learning model

	•	Model type: supervised regression – predictive modelling of continuous outcomes.
	•	Algorithm family: ensemble methods such as gradient-boosted trees, random forests, or traditional generalised linear models.
	•	Training data: historical claims records covering millions of patients.
	•	Features (inputs): demographics, diagnoses (ICD codes), lab results, previous hospital and GP visits, prescribed medicines, and total past spending.
	•	Label / target variable: the following year’s total healthcare cost per patient.
	•	Loss function: mean-squared or mean-absolute error on cost prediction.
	•	Evaluation metric: R² or RMSE on predicted cost, not on health outcomes.
	•	Deployment output: a numeric risk score used to rank patients by predicted cost (interpreted as risk).
	•	Implementation environment: typical actuarial analytics tools – SAS, Python (scikit-learn/XGBoost), or Optum’s proprietary platforms.

Summary:
The Optum algorithm used a standard supervised regression framework, but by optimising for future cost rather than future illness, it encoded structural bias and misrepresented genuine patient need.

It was likely implemented in SAS, Python (scikit-learn / XGBoost), or proprietary actuarial platforms used by Optum.

🧭 7. How it was corrected

After the Science study:
	•	Optum collaborated with researchers and redesigned the model to use direct health outcomes (e.g., number of chronic conditions, hospitalization likelihood) rather than cost.
	•	Adjusted to include social determinants of health and fairness constraints.
	•	Bias reduced dramatically—roughly an 84% reduction in racial disparity when retrained on need-based labels.

🩺 8. Broader lesson for healthcare AI

This case became the canonical example of “proxy-label bias” in applied machine learning.

Core lesson:

	A model can be “accurate” on paper but ethically and clinically harmful if its label encodes systemic bias.

So:
	•	Always validate models for equity across subgroups.
	•	Carefully audit what outcome the algorithm truly learns.
	•	Prefer health outcomes or clinical measures of need over economic proxies.
	•	Monitor models post-deployment for drift and fairness.

# How was the Optum algorithm fixed?

🩺 How the Optum Algorithm Was Fixed

After the 2019 Science study revealed serious racial bias in Optum’s risk-prediction model, researchers and Optum’s own data scientists collaborated to identify and correct the causes. The solutions focused on three main areas: changing the target variable, improving fairness checks, and re-validating the model’s purpose.

1. Replacing the Target Variable
	•	The original model predicted future healthcare cost as a proxy for medical risk.
	•	This was replaced with a measure of true health need – such as:
	•	Number and severity of chronic conditions
	•	Rates of hospitalisation or emergency admissions
	•	Laboratory results or clinical outcome scores
	•	By training the model to predict actual health outcomes rather than cost, it could better identify patients who were genuinely unwell, regardless of how much had been spent on their care.

Effect: This change removed the cost bias that had previously penalised groups with lower healthcare spending, particularly Black patients.

2. Adding Fairness and Subgroup Validation
	•	The revised model was explicitly tested for performance across demographic groups (race, gender, income level, and age).
	•	Analysts compared:
	•	Whether patients with similar clinical need received similar risk scores across groups
	•	Whether enrolment rates in care programmes matched actual disease prevalence
	•	If disparities were detected, the model was adjusted or reweighted to improve equity.

Effect: This step ensured that the algorithm’s predictions did not systematically favour or disadvantage any subgroup.

3. Redefining the Evaluation Metrics
	•	The previous system evaluated success based on cost-prediction accuracy (e.g., R² or RMSE on cost).
	•	The updated approach evaluated models using clinical and fairness metrics, including:
	•	Accuracy in predicting hospitalisations or complications
	•	Alignment between predicted and actual health status across patient groups
	•	Bias and disparity indices to track fairness over time

Effect: The model’s goal shifted from financial efficiency to clinical equity and accuracy.

4. Improved Data Governance and Monitoring
	•	Optum introduced more robust data governance processes to review model inputs, training data, and outcomes.
	•	Regular audits and post-deployment monitoring were added to detect bias drift — where a model might gradually become less fair as healthcare patterns change.

Effect: This helps maintain fairness and reliability throughout the model’s lifecycle, not just at deployment.

✅ Overall Impact

When retrained and revalidated using these methods, the algorithm’s racial bias was reduced by roughly 80–85%.
Black patients with equal disease burden were far more likely to be correctly identified as high-risk and offered appropriate care-management support.

In summary:

	Optum’s algorithm was fixed by moving from cost-based prediction to need-based prediction, embedding fairness testing and equity monitoring into every stage of the model’s design and deployment.

