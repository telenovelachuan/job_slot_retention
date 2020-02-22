###### SQL
1. Total Contract Value
```
select loc.`State_Name`, sum(Total_Contract_Value) as `Total_Contract_Value`, DATE_FORMAT(EndDate,'%Y-%m') as `month`
from slot_performance_data slot join location_data loc on slot.`City_ID`=loc.`City_ID`
group by loc.`State_Name`, DATE_FORMAT(EndDate,'%Y-%m')
```
![sql_1](https://github.com/telenovelachuan/job_slot_retention/blob/master/sql_1.png)

