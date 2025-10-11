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

