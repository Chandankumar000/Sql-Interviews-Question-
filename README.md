# Sql-Interviews-Question-

# Data given.

      create table call_details  (
      call_type varchar(10),
      call_number varchar(12),
      call_duration int
      );

          insert into call_details
          values ('OUT','181868',13),('OUT','2159010',8)
          ,('OUT','2159010',178),('SMS','4153810',1),('OUT','2159010',152),('OUT','9140152',18),('SMS','4162672',1)
          ,('SMS','9168204',1),('OUT','9168204',576),('INC','2159010',5),('INC','2159010',4),('SMS','2159010',1)
          ,('SMS','4535614',1),('OUT','181868',20),('INC','181868',54),('INC','218748',20),('INC','2159010',9)
          ,('INC','197432',66),('SMS','2159010',1),('SMS','4535614',1);
          
          
### useing this see all details of data
Select * from call_details 

##  find out call_number whose use both incoming and outcoming and duration of outgoing calling is greater than incoming call. In three different approch. 

1. CTE and where clause

                    with cte as(
                    select call_number,sum(case when call_type='OUT' then call_duration else 0 end) as out_call,
                    sum(case when call_type='INC' then call_duration else 0 end) as INC_call
                    from call_details
                    group by call_number
                    )
                    select call_number from cte 
                    where out_call > inc_call


2.Magic of having clause

                    select call_number
                    from call_details
                    group by call_number
                    having sum(case when call_type='OUT' then call_duration else 0 end) > sum(case when call_type='INC' then call_duration else 0 end)

3. CTE and inner join

                    with cte1 as(
                    select call_number,sum(call_duration) as out_dur
                    from call_details
                    where call_type='OUT'
                    group by call_number
                    )
                    ,cte2 as(
                    select call_number,sum(call_duration) as inc_dur
                    from call_details
                    where call_type='Inc'
                    group by call_number
                    )
                    Select a.call_number from cte1 a join cte2 b on a.call_number=b.call_number
                    where out_dur > inc_dur
