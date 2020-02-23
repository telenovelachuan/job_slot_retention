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

The random forest achieved . accuracy on testing set, with 

Some other factors I would investigate for retention analysis include:

- Usage of job slots, such as ratio of Job_Listings / Job_Slots, and also the trend of job slot change by employers.

- Number of candidates hired. This is a metric to compare candidate quality with other platforms, and a factor that would influence customers renewal decisions.

- Renewal time series, which focuses on seasonalities of renewal behavior.

###### Recommendations for platform algorithm

1. adaptive bidding

It could be observed in the dataset that, non-renewal employers typically distribute more densely in high cost-per-applicant values than those who renewed.
![nonrn_cost_per_applicant](https://github.com/telenovelachuan/job_slot_retention/blob/master/reports/figures/nonrn_cost_per_applicant.png)

It could be something like a lose-lose: few applicants apply for job -> low application performance -> bids increase -> stop renewal.
Thus for low performance customers, maybe we should take a look at the reason behind before increasing their bid. For example, some employers just don't make full use of job listings.

![violin_job_listing_cost_per_applicant](https://github.com/telenovelachuan/job_slot_retention/blob/master/reports/figures/violin_job_listing_cost_per_applicant.png)

High Job_Listings bins would generally decrease cost per applicant, and that's the merit of job slots.
Recommending customers to take full advantage of these platform features would hopefully increase their performance without additional cost.
And it's more reasonable to increase the bids for low performance from other reasons, such as poor job description, attractiveness of industry, location, company, etc. 


2. Other statistical analysis

For those with Renewal_Flag of 1, they seem to present a far more distribution density in longer contract duration and more job slots.
![nonrn_day_diff](https://github.com/telenovelachuan/job_slot_retention/blob/master/reports/figures/nonrn_day_diff.png)

Thus persuading them into trying more job slots would hopefully increase their market value and application/app start click rate.
![nonrn_Job_Slots](https://github.com/telenovelachuan/job_slot_retention/blob/master/reports/figures/nonrn_Job_Slots.png)
