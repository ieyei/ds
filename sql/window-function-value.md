
[sqlite window function](https://www.sqlitetutorial.net/sqlite-window-functions/)

syntax LAG
```sql
LAG(expression [,offset[, default ]]) OVER (
    PARTITION BY expression1, expression2,...
    ORDER BY expression1 [ASC | DESC], expression2,...
)
```

syntax LEAD
```sql
LEAD(expression [,offset[, default ]]) OVER (
    PARTITION BY expression1, expression2,...
    ORDER BY expression1 [ASC | DESC], expression2,...
)
```

```sql
WITH TEST(yyyymm, amt, id) AS
(
SELECT '201901'  , 100  ,1
UNION ALL SELECT '201902', 200, 2 
UNION ALL SELECT '201903', 300, 3 
UNION ALL SELECT '201904', 400, 4 
UNION ALL SELECT '201905', 500, 1
UNION ALL SELECT '201906', 600, 2 
UNION ALL SELECT '201907', 700, 3
UNION ALL SELECT '201908', 800, 4
UNION ALL SELECT '201909', 900, 1
UNION ALL SELECT '201910', 100, 2 
UNION ALL SELECT '201911', 200, 3 
UNION ALL SELECT '201912', 300, 4
)
select yyyymm, amt,id,
	LAG(amt, 1) over (partition by id
                        order by yyyymm) as lag1,
	LAG(amt, 1,0) over (partition by id
                        order by yyyymm) as lag2,   
	LAG(amt, 2,0) over (partition by id
                        order by yyyymm) as lag3,                        
  LEAD(amt,1,0) over (partition by id
                      order by yyyymm) as lead1,
  LEAD(amt,2,0) over (partition by id
                      order by yyyymm) as lead2                        
FROM TEST
```
lag1: 현재 row에서 1(offset) row 이전 값(default 값이 없으므로 첫번째 값은 null)  
lag2: 현재 row에서 1(offset) row 이전 값(default 값이 0이므로 첫번째 값은 0)  
lag3: 현재 row에서 2(offset) row 이전 값  
lead1: 현재 row에서 1(offset) row 이후 값  
lead2: 현재 row에서 2(offset) row 이후 값


|yyyymm|amt|id|lag1|lag2|lag3|lead1|lead2|
|---|---|---|---|---|---|---|---|
201901|100|1||0|0|500|900
201905|500|1|100|100|0|900|0
201909|900|1|500|500|100|0|0
201902|200|2||0|0|600|100
201906|600|2|200|200|0|100|0
201910|100|2|600|600|200|0|0
201903|300|3||0|0|700|200
201907|700|3|300|300|0|200|0
201911|200|3|700|700|300|0|0
201904|400|4||0|0|800|300
201908|800|4|400|400|0|300|0
201912|300|4|800|800|400|0|0

