Retention Analysis

I retained all previous metrics as below:

- application click conversion rate: Applications / Apply_Start_Clicks
- daily application: Applications / days_in_the_contract
- daily app start click: Apply_Start_Clicks / days_in_the_contract
- applications per slot: Applications / Job_Slots
- app start click per slot: Apply_Start_Clicks / Job_Slots
- market profit: Click_Market_Value - Total_Contract_Value

###### Factors for retention

To find out the best factors for predicting retention(Renewal_Flag), first take a look at the Pearson correlation matrix.

![pearson_corr](https://github.com/telenovelachuan/job_slot_retention/blob/master/reports/figures/pearson_corr.png)

The most correlated features with Renewal_Flag are:

- day_diff 0.64
- market profit 0.13
- start_click_per_slot 0.12
- start_year -0.11

It seems the duration of the contract plays a vital role in renewal. Then to use decision tree for feature importance.
Below is the calculated feature importance by a random forest that contains 500 trees.

![rf_feat_imp](https://github.com/telenovelachuan/job_slot_retention/blob/master/reports/figures/feature_importance_rf.png)

Day_diff(contract duration in days) largely outperforms all other features, with end_month, Click_Market_Value and start month following after.

Boosting method is also tried. The feature importance calculated by a Gradient Boosting forest is shown below.

![gb_feat_imp](https://github.com/telenovelachuan/job_slot_retention/blob/master/reports/figures/feature_importance_gb.png)

Top features are generally the same, except that the importance of day_diff increased from .49 to .69.

Some assumptions here for the classification models:

- available samples are indenpendent, and ideally, they're identically distributed
- training set and testing set are strongly correlated, though overfitting may happen
- the target variable, Renewal_Flag, is dependent on all other given variables for prediction

###### Prediction performance

