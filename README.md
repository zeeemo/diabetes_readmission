# Overview

I examined 10 years of data (1999-2008) comprised from 130 US hospitals that represents individuals with diabetes and whether or not they were readmitted to hospitals. 

---

# Executive Summary

I analyzed 10 years of US hospital data for patients with diabetes in order to determine whether or not, based on a series of features, if I could accurately predict whether they would be readmitted to the hospital. In my analysis, I used a few different models including Logistic Regression, KNearestNeighbors, and RandomForest. Overall, Logistic Regression worked best because it performed consistently on my training and test sets but I was only able to accurately predict whether a patient with a diabetic diagnosis would be readmitted with around a .62 accuracy. Through parameter tuning I was able to achieve an almost exact same score using RandomForest with a .617 score on my training and a .618 score on my testing datasets. This leads me to believe there is more room for improvement by more closely examining the relationship between my independent and dependent variables.

To view this project from beginning to end, please view the notebooks in the following order. MY EDA notebook is the most expansive due to the large amount of cleaning required with the dataset.

- [Capstone_EDA](./Capstone_EDA.ipynb)
- [model_one_logreg](./model_one_logreg.ipynb)
- [model_two_knn](./model_two_knn.ipynb)
- [model_three_rf](./model_three_rf.ipynb)

Additionally, there are separate notebooks I used for parameter tuning - however I only made permanent changes to my actual models as my score improved and remained stable. Those are listed below for reference.

- [logreg_tuning](./logreg_tuning.ipynb)
- [knn_tuning](./knn_tuning.ipynb)
- [rf_tuning](./rf_tuning.ipynb)

---

# DATA

I found this dataset through the University of California, Irvine Machine Learning Repository. The repository contains over 470 machine learning datasets for students, educators, and researchers to use and learn from. Because of the size of the dataset and personal family connection to Diabetes, I felt it would be a great project to demonstrate what I have learned over the past 12 weeks in the Data Science Immersive at General Assembly.

To check out some of the sources related to this project in particular, please visit the following:

[UCI Machine Learning Repository](https://archive.ics.uci.edu/ml/index.php)
[Diabetes Dataset](https://archive.ics.uci.edu/ml/datasets/Diabetes+130-US+hospitals+for+years+1999-2008)

The research behind the dataset comes from Virginia Commonwealth University. For more information about their research, please visit the following.

[Impact of HbA1c Measurement on Hospital Admission Rates](https://www.hindawi.com/journals/bmri/2014/781670/)

The diagnostic codes used in three of the columns can be found below. For my analysis, I looped through the columns and placed each observation in a category related to the overall diagnosis. Diabetes Mellitus is 250.0 which placed it in category 3 in my dataset.

[ICD-9-CM Vol. 1 Diagnostic Codes](https://www.findacode.com/code-set.php?set=ICD9)

---

# Data Dictionary

> As you look at the original data, please reference this data dictionary for any related questions. This data dictionary represents the original csv before I began cleaning.

> In the original dataset, there are ~100,000 rows of data with 55 columns. There are a variety of missing values within. Columns are all numeric/nominal to begin with.

|Feature|Type|Description|Missing Percentage|
|---|---|---|---|
|Encounter ID|Numeric|Unique identifier of an encounter|0%|
|Patient number|Numeric|Unique identifier of a patient|0%|
|Race|Nominal|Values: Caucasian, Asian, African American, Hispanic, and other|2%|
|Gender|Nominal|Values: male, female, and unknown/invalid|0%|
|Age|Nominal|Grouped in 10-year intervals: 0, 10), 10, 20), …, 90, 100)|0%|
|Weight|Numeric|Weight in pounds.|97%|
|Admission type|Nominal|Integer identifier corresponding to 9 distinct values, for example, emergency, urgent, elective, newborn, and not available|0%|
|Discharge disposition|Nominal|Integer identifier corresponding to 29 distinct values, for example, discharged to home, expired, and not available|0%|
|Admission source|Nominal|Integer identifier corresponding to 21 distinct values, for example, physician referral, emergency room, and transfer from a hospital|0%|
|Time in hospital|Numeric|Integer number of days between admission and discharge|0%|
|Payer code|Nominal|Integer identifier corresponding to 23 distinct values, for example, Blue Cross/Blue Shield, Medicare, and self-pay|52%|
|Medical specialty|Nominal|Integer identifier of a specialty of the admitting physician, corresponding to 84 distinct values, for example, cardiology, internal medicine, family/general practice, and surgeon|53%|
|Number of lab procedures|Numeric|Number of lab tests performed during the encounter|0%|
|Number of procedures|Numeric|Number of procedures (other than lab tests) performed during the encounter|0%|
|Number of medications|Numeric|Number of distinct generic names administered during the encounter|0%|
|Number of outpatient visits|Numeric|Number of outpatient visits of the patient in the year preceding the encounter|0%|
|Number of emergency visits|Numeric|Number of emergency visits of the patient in the year preceding the encounter|0%|
|Number of inpatient visits|Numeric|Number of inpatient visits of the patient in the year preceding the encounter|0%|
|Diagnosis 1|Nominal|The primary diagnosis (coded as first three digits of ICD9); 848 distinct values|0%|
|Diagnosis 2|Nominal|Secondary diagnosis (coded as first three digits of ICD9); 923 distinct values|0%|
|Diagnosis 3|Nominal|Additional secondary diagnosis (coded as first three digits of ICD9); 954 distinct values|1%|
|Number of diagnoses|Numeric|Number of diagnoses entered to the system|0%|
|Glucose serum test result|Nominal|Indicates the range of the result or if the test was not taken. Values: “>200,” “>300,” “normal,” and “none” if not measured|0%|
|A1c test result|Nominal|Indicates the range of the result or if the test was not taken. Values: “>8” if the result was greater than 8%, “>7” if the result was greater than 7% but less than 8%, “normal” if the result was less than 7%, and “none” if not measured.|0%|
|Change of medications|Nominal|Indicates if there was a change in diabetic medications (either dosage or generic name). Values: “change” and “no change”|0%|
|Diabetes medications|Nominal|Indicates if there was any diabetic medication prescribed. Values: “yes” and “no”|0%|
|24 features for medications|Nominal|For the generic names: metformin, repaglinide, nateglinide, chlorpropamide, glimepiride, acetohexamide, glipizide, glyburide, tolbutamide, pioglitazone, rosiglitazone, acarbose, miglitol, troglitazone, tolazamide, examide, sitagliptin, insulin, glyburide-metformin, glipizide-metformin, glimepiride-pioglitazone, metformin-rosiglitazone, and metformin-pioglitazone, the feature indicates whether the drug was prescribed or there was a change in the dosage. Values: “up” if the dosage was increased during the encounter, “down” if the dosage was decreased, “steady” if the dosage did not change, and “no” if the drug was not prescribed|0%|
|Readmitted|Nominal|Days to inpatient readmission. Values: “<30” if the patient was readmitted in less than 30 days, “>30” if the patient was readmitted in more than 30 days, and “No” for no record of readmission.|0%|

---

# Deliverables

> Attached to my project you will find:

- A Jupyter notebook that contains separate notebooks for each model and one for my EDA/Cleaning
- A README markdown file which provides an introduction, overview, and information related to my project
- My presentation slideshow as a .pdf with sources attached
- Files for my data, code, and images used throughout both the notebook and presentation

---

# If I had more data...

> The largest category of missing data is weight. Because Diabetes is commonly associated with weight, I would have liked this information in order to explore the relationship between my target variable and other information in the dataset like types of medication provided as well as the types of diagnoses and whether there was a relationship between the two.

> It also would have been great to have more data that wasn't solely represented as nominal/numeric. While most of the columns were pulled in as strings and converted to numbers, none of the columns were able to be classified as ordinal. This created a lot of dummy columns for me. While useful, it also would have been helpful to have some columns with ordinal data to work with.

---

# Citations

>Beata Strack, Jonathan P. DeShazo, Chris Gennings, Juan L. Olmo, Sebastian Ventura, Krzysztof J. Cios, and John N. Clore, “Impact of HbA1c Measurement on Hospital Readmission Rates: Analysis of 70,000 Clinical Database Patient Records,” BioMed Research International, vol. 2014, Article ID 781670, 11 pages, 2014.

>Additional links within the README for UCI Machine Learning Repository, Research article from VCU, and Diagnostic codes used in analysis

---

# Misc

> If I have inaccurately cited or sourced any information in this project, please contact me so I can correct it. 