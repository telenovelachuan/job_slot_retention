Metric design

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

High Click_Market_Value bins contain significantly lower cost per applicant, which is a good signal - making a big contract would reduce your cost on getting a single applicant!


