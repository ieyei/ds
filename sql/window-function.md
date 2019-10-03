

[sqlite online](https://sqliteonline.com/)  
[sqlite-window-functions](https://www.sqlitetutorial.net/sqlite-window-functions/)  
[oracle window function](http://www.gurubee.net/lecture/2674)

```sql
WITH TEST(yyyymm,amt) AS
(
SELECT '201901'  , 100  
UNION ALL SELECT '201902', 200 
UNION ALL SELECT '201903', 300 
UNION ALL SELECT '201904', 400 
UNION ALL SELECT '201905', 500 
UNION ALL SELECT '201906', 600 
UNION ALL SELECT '201907', 700 
UNION ALL SELECT '201908', 800 
UNION ALL SELECT '201909', 900 
UNION ALL SELECT '201910', 100 
UNION ALL SELECT '201911', 200 
UNION ALL SELECT '201912', 300 
)
select yyyymm, amt,
	sum(amt) over (order by yyyymm 
                   rows BETWEEN unbounded preceding and current row) amt1,
	sum(amt) over (order by yyyymm 
                   rows BETWEEN unbounded preceding and unbounded following) amt2,
	sum(amt) over (order by yyyymm 
                   rows BETWEEN current row and unbounded following) amt3,
	sum(amt) over (order by yyyymm 
                   rows 1 preceding) amt4,
	sum(amt) over (order by yyyymm
                   rows BETWEEN 1 preceding and 1 following) amt5
FROM TEST
```

amt1 : 첫번째 row ~ 현재 row 합  
amt2 : 첫번째 row ~ 마지막 row 합  
amt3 : 현재 row ~ 마지막 row 합  
amt4 : 이전 row ~ 현재 row 합  
amt5 : 이전 row ~ 다음 row 합  

|yyyymm|amt|amt1|amt2|amt3|amt4|amt5|
|------|------|------|------|------|------|------|
201901|100|100|5100|5100|100|300
201902|200|300|5100|5000|300|600
201903|300|600|5100|4800|500|900
201904|400|1000|5100|4500|700|1200
201905|500|1500|5100|4100|900|1500
201906|600|2100|5100|3600|1100|1800
201907|700|2800|5100|3000|1300|2100
201908|800|3600|5100|2300|1500|2400
201909|900|4500|5100|1500|1700|1800
201910|100|4600|5100|600|1000|1200
201911|200|4800|5100|500|300|600
201912|300|5100|5100|300|500|500
