#### Objective:
Many patients are frustrated by the continuously rising health care costs in the U.S. and the lack of transparency in the
medical charging system: the prices of procedures and tests are not known up front, differ from location to location, and often
seem disproportionate to the actual level of care received.  A tool to help negotiate this system could allow patients to find
the most economical solution for their healthcare needs.  I built a predictive model that indicates whether a provider will be
expensive or inexpensive based on information about the institution, location and community attributes.

#### Data:
The  negotiations that determine the actual costs to patients pay happen between insurance companies and medical providers 
behind closed doors and are not available to the public. However, the Medicare Provider Utilization and Payment dataset offers
a glimpse into the charges that medical providers bill for their services. The dataset gives information about the services and
costs for all Medicare providers in the country, including the total amount the provider bills to Medicare services. In
addition to the inpatient and outpatient data from the Medicare Provider Utilization and Payment Data, I also used the
Physician and Other Supplier Database to obtain further information about the providers, and 2014 census data and 2014 health
insurance coverage information for specific predictive community attributes.  

![](https://github.com/TorrBorr/health-care-cost-estimator/blob/master/Images/State_Charges_Map.png?raw=true)

<img src="https://github.com/TorrBorr/health-care-cost-estimator/blob/master/Images/Hospital_visit_cost_CO.png?raw=true" width="500">

#### Methods:
My approach was to classify the charges of each provider as above average, approximately average and below average in price.
Predicting a specific price per procedure is difficult due to many factors, including the individual nature of medicine and the
complications and side effects that may occur;  however, predicting how a provider compares to the average is reasonable. These
values are interesting on both a state and community level, so I looked at predicting how a provider's costs compare to both
the national average and the state average since it is often not feasible for patients to leave their local communities for
health care.

Given that this is a multiclass classification problem, I checked the classification algorithms logistic regression, decision
trees, random forest,  SVC,  and KNN to find which was able to provide the best prediction.  Model optimization was based on
the precision of the high cost class (class 3). Predicting that a provider would be average or inexpensive in cost when they
are actually above average in cost would put an unexpected and unacceptable financial burden on the patient.

Data cleaning and model development was done using python Jupyter notebooks with pandas and sklearn.

#### Results:
In both cases, the random forest model provided the predictions with the highest precision. The table below summarizes the
precision for each class and the number of predictions of class 1 that were actually class 3. Decision trees and KNN were close
in precision, with logistic regression and SVM far behind at values of 77% and 49%, respectively.

##### Confusion Matrix for Random Forest Model:
<img src="https://github.com/TorrBorr/health-care-cost-estimator/blob/master/Images/Random_Trees_Confusion_Martix.png?raw=true" width="300">  





| Model              | Class 1 Precision | Class 2 Precision | Class 3 Precision | Class 3 predicted as Class 1 |
| ------------------ |:-----------------:| -----------------:| -----------------:| ----------------------------:|
| KNN                | 0.84              | 0.69              | 0.86              |104                           |
| Random Forest      | 0.85              | 0.70              | 0.87              |36                            |
| Logistic Regression| 0.49              | 0.35              | 0.49              |2004                          |
| SVC                | 0.77              | 0.72              | 0.53              |89                            |
| Decision Trees     | 0.83              | 0.69              | 0.87              |63                            |
##### Table of precisions for each prediction model

The first iteration of the model included 17 factors involving state, hospital type, procedure, and race, ethnicity, income,
population, percent uninsured population, and economic estimators of the county the provider is located in.  However, many of
the included features contributed little to the model and the precision increased a with the removal of 10 features, leaving
the model with only the features shown in the figure below.  The eliminated features were chosen for removal due to their
covariance with other features.

The model that compared each provider's cost to the national average showed the most important feature as the state, which
makes sense given the stark differences in procedure costs between states, possibly driven by policies and other state specific
factors. Other important factors included the county median income, median age, and uninsured %. These factors  are reasonable,
as the cost of healthcare increases with increased age, higher income areas attract medical facilities with access to more
expensive and advanced treatments, and the greater number of uninsured patients a hospital sees, the more costs they have to
distribute to other patients to balance the costs.

##### Feature Importance for Random Forest Model:
<img src="https://github.com/TorrBorr/health-care-cost-estimator/blob/master/Images/Random_trees_feature_importance.png?raw=true" width="500">

In the second model,  each provider's  cost  was compared to the state average rather than the national average.  The same
factors are again seen as the strongest predictors.  Since the impact of the state is partially accounted for in the class
itself, state as a predictor was removed in this analysis.

#### Conclusions and future work:
Whether a procedure will cost less than, greater than or near average can be predicted using a random forest classification
model with 87% precision based on hospital and community specific information. This prediction model could be the basis for a
tool that would allow patients to chose the most economic solution for their healthcare needs rather than waiting to receive
their medical bills to know how expensive a provider is.   An interactive app, currently in development, is the best way to
allow patients to access this information.
