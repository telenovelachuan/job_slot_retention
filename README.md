## SQL

###### Total Contract Value
```
select loc.`State_Name`, sum(Total_Contract_Value) as `Total_Contract_Value`, DATE_FORMAT(EndDate,'%Y-%m') as `month`
from slot_performance_data slot join location_data loc on slot.`City_ID`=loc.`City_ID`
group by loc.`State_Name`, DATE_FORMAT(EndDate,'%Y-%m')
```
![sql_1](https://github.com/telenovelachuan/job_slot_retention/blob/master/reports/figures/sql_1.png)

###### Second transaction
```
select employer_id, startdate, job_slots, click_market_value from 
(select @rank := IF(@current_employer = employer_id, @rank + 1, 1) AS employer_rank,
		@current_employer := employer_id,
       employer_id,
       startdate,
       job_slots, click_market_value
       from slot_performance_data
        order by employer_id, startdate
) row_numbered
where employer_rank = 2
```
![sql_2](https://github.com/telenovelachuan/job_slot_retention/blob/master/reports/figures/sql_2.png)


## Metric Design

The below metrics are used to measure the quality of services.

- application click conversion rate: Applications / Apply_Start_Clicks
- daily application: Applications / days_in_the_contract
- daily app start click: Apply_Start_Clicks / days_in_the_contract
- applications per slot: Applications / Job_Slots
- app start click per slot: Apply_Start_Clicks / Job_Slots
- cost per applicant: Total_Contract_Value / Applications

They vary with: 

###### Job_Slots

Job_Slots only contains 2 values(15, 50). Higher Job_Slot obviously increases daily Applications/app start clicks. While it does not impact conversion rate and applications per slot so much.

![Job_Slots_violin](https://github.com/telenovelachuan/job_slot_retention/blob/master/reports/figures/Job_Slots_violin.png)

###### Total_Contract_Value

In terms of Total_Contract_Value, try to split it into 4 quantile bins for violin plot.
Higher value bins typically demonstrated more distribution density than lower value bins for applications/app start click both per day and per slot, as well as for conversion rate.
It shows that generally higher contract values contain higher application rate and conversion rate.

![Total_Contract_Value_scatter](https://github.com/telenovelachuan/job_slot_retention/blob/master/reports/figures/Total_Contract_Value_scatter.png)

![Total_Contract_Value_violin](https://github.com/telenovelachuan/job_slot_retention/blob/master/reports/figures/Total_Contract_Value_violin.png)

###### Click_Market_Value

Similarly, larger Click_Market_Value bins expands distribution density of more daily applications/start clicks.
![Click_Market_Value_violin_1](https://github.com/telenovelachuan/job_slot_retention/blob/master/reports/figures/Click_Market_Value_violin_1.png)

Interestingly, it does not help with the conversion rate.
![Click_Market_Value_violin_2](https://github.com/telenovelachuan/job_slot_retention/blob/master/reports/figures/Click_Market_Value_violin_2.png)

But there seems that it has a big impact on the cost per applicant.
![Click_Market_Value_violin_3](https://github.com/telenovelachuan/job_slot_retention/blob/master/reports/figures/Click_Market_Value_violin_3.png)

High Click_Market_Value bins contain significantly lower cost per applicant, which is a good signal - making a big contract would reduce your average cost on getting a single applicant!

More details of metrics design can be fount 
[HERE](https://github.com/telenovelachuan/job_slot_retention/blob/master/metric_design.ipynb)


## Retention Analysis

###### Factors for retention

To find out the best factors for predicting retention(Renewal_Flag), first take a look at the Pearson correlation matrix.

![pearson_corr](https://github.com/telenovelachuan/job_slot_retention/blob/master/reports/figures/pearson_corr.png)

The most correlated features with Renewal_Flag are:

- day diff 0.64
- cost per applicant -0.2
- Click_Market_Value 0.12
- start_click_per_slot 0.12

It seems the duration of the contract plays a vital role in renewal. Then to use decision tree for feature importance.
Below is the calculated feature importance by a random forest that contains 500 trees.

![rf_feat_imp](https://github.com/telenovelachuan/job_slot_retention/blob/master/reports/figures/feature_importance_rf.png)

Day_diff(contract duration in days) largely outperforms all other features, with Click_Market_Value, end_month and start month following after.

Boosting method is also tried. The feature importance calculated by a Gradient Boosting forest is shown below.

![gb_feat_imp](https://github.com/telenovelachuan/job_slot_retention/blob/master/reports/figures/feature_importance_gb.png)

Top features are generally the same, except that the importance of day_diff increased from .38 to .67.

Some assumptions here for the classification models:

- available samples are indenpendent, and ideally, they're identically distributed
- training set and testing set are strongly correlated, though overfitting may happen
- the target variable, Renewal_Flag, is dependent on all other given variables for prediction

###### Prediction performance

The random forest achieved .93 accuracy on testing set, with main parameters to be trees, gini criterion, min_samples_leaf retrieved using a self implemented grid search.
If more time would be available, I would fine tune boosting parameters a little bit, together with cross validation.

Some other factors I would investigate for retention analysis include:

- Usage of job slots, such as ratio of Job_Listings / Job_Slots, and also the trend of job slot change by employers.

- Number of candidates hired. This is a metric to compare candidate quality with other platforms, and a factor that would influence customers renewal decisions.

- Renewal time series, which focuses on seasonalities of renewal behavior.

###### Recommendations for platform algorithm

- Adaptive bidding

It could be observed in the dataset that, non-renewal employers typically distribute more densely in high cost-per-applicant values than those who renewed.
![nonrn_cost_per_applicant](https://github.com/telenovelachuan/job_slot_retention/blob/master/reports/figures/nonrn_cost_per_applicant.png)

It could be something like a lose-lose: few applicants apply for job -> low application performance -> bids increase -> stop renewal.
Thus for low performance customers, maybe we should take a look at the reason behind before increasing their bid. For example, some employers just don't make full use of job listings.

![violin_job_listing_cost_per_applicant](https://github.com/telenovelachuan/job_slot_retention/blob/master/reports/figures/violin_job_listing_cost_per_applicant.png)

High Job_Listings bins would generally decrease cost per applicant, and that's the merit of job slots.
Recommending customers to take full advantage of these platform features would hopefully increase their performance without additional cost.
And it's more reasonable to increase the bids for low performance from other reasons, such as poor job description, attractiveness of industry, location, company, etc. 

- Other statistical insights

For those with Renewal_Flag of 1, they seem to present a far more distribution density in longer contract duration and more job slots.
![nonrn_day_diff](https://github.com/telenovelachuan/job_slot_retention/blob/master/reports/figures/nonrn_day_diff.png)

Thus persuading them into trying more job slots would hopefully increase their market value and application/app start click rate.
![nonrn_Job_Slots](https://github.com/telenovelachuan/job_slot_retention/blob/master/reports/figures/nonrn_Job_Slots.png)

More details of retention analysis can be fount 
[HERE](https://github.com/telenovelachuan/job_slot_retention/blob/master/retention_analysis.ipynb)





