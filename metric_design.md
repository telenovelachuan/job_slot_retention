Metric design

The below metrics are used to measure the quality of services.

- application click conversion rate: Applications / Apply_Start_Clicks
- daily application: Applications / days_in_the_contract
- daily app start click: Apply_Start_Clicks / days_in_the_contract
- applications per slot: Applications / Job_Slots
- app start click per slot: Apply_Start_Clicks / Job_Slots
- market profit: Click_Market_Value - Total_Contract_Value

###### Job_Slots

Job_Slots only contains 2 values(15, 50). Applications/app start clicks per day grows with Job_Slot, while applications/app start clicks per slot decreases when slots grow to 50.
![Job_Slots_scatter](https://github.com/telenovelachuan/job_slot_retention/blob/master/reports/figures/Job_Slots_scatter.png)

Besides, the violin plots show that when slots increase to 50, there's a distribution rise for daily application/app start clicks.
![Job_Slots_violin](https://github.com/telenovelachuan/job_slot_retention/blob/master/reports/figures/Job_Slots_violin.png)

###### Total_Contract_Value

In terms of Total_Contract_Value, in the range of [0, 1000], there's an increase in daily applications and applications per slot distributions.
Since higher value contracts typically contains more slots. While the performance seems to decline between [2000, 3000], especially for applications per slot.
![Total_Contract_Value_scatter](https://github.com/telenovelachuan/job_slot_retention/blob/master/reports/figures/Total_Contract_Value_scatter.png)

From the violin plot below, Total_Contract_Value is splitted into 4 quantile bins.

![Total_Contract_Value_violin](https://github.com/telenovelachuan/job_slot_retention/blob/master/reports/figures/Total_Contract_Value_violin.png)

Higher value bins typically demonstrated more distribution density than lower value bins for applications/app start click both per day and per slot, as well as for conversion rate.
It shows that generally higher contract values contain higher application rate and conversion rate.


###### Click_Market_Value

It's obvious that market profit linearly grows with Click_Market_Value
![Click_Market_Value_scatter](https://github.com/telenovelachuan/job_slot_retention/blob/master/reports/figures/Click_Market_Value_scatter.png)

And higher Click_Market_Value bins correspond to more distribution density in daily application/app start click, application per slot and conversion rate.
![Click_Market_Value_violin](https://github.com/telenovelachuan/job_slot_retention/blob/master/reports/figures/Click_Market_Value_violin.png)




