###### SQL
1. Total Contract Value
```
select loc.`State_Name`, sum(Total_Contract_Value) as `Total_Contract_Value`, DATE_FORMAT(EndDate,'%Y-%m') as `month`
from slot_performance_data slot join location_data loc on slot.`City_ID`=loc.`City_ID`
group by loc.`State_Name`, DATE_FORMAT(EndDate,'%Y-%m')
```
![sql_1](https://github.com/telenovelachuan/job_slot_retention/blob/master/sql_1.png)

2. Second transaction
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
![sql_2](https://github.com/telenovelachuan/job_slot_retention/blob/master/sql_2.png)


