

[sqlite online](https://sqliteonline.com/)  
[sqlite-window-functions](https://www.sqlitetutorial.net/sqlite-window-functions/)  
[oracle window function](http://www.gurubee.net/lecture/2674)

```sql
WITH test(yyyymm,amt) AS
(
SELECT '200801'  , 100  
UNION ALL SELECT '200802', 200 
UNION ALL SELECT '200803', 300 
UNION ALL SELECT '200804', 400 
UNION ALL SELECT '200805', 500 
UNION ALL SELECT '200806', 600 
UNION ALL SELECT '200808', 800 
UNION ALL SELECT '200809', 900 
UNION ALL SELECT '200810', 100 
UNION ALL SELECT '200811', 200 
UNION ALL SELECT '200812', 300 
)
select * from test
```
