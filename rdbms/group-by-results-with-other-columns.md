# Group By results, with other columns

Group by reduces the set with distinct values on the grouped column. Some kind of aggregation operation must be done on other columns \(i.e sum, max etc.\) 

It is not possible to get the values of the other columns in the same group by query. A nested query should be performed to get the job done.

```text
> select * from threhold;
===============================================================
|key                |date                |threshold            |
===============================================================
|key_abc            |2020-08-03          |55                   |
---------------------------------------------------------------
|key_abc            |2020-08-02          |86                   |
---------------------------------------------------------------
|key_xyz            |2020-08-03          |43                   |
---------------------------------------------------------------
|key_xyz            |2020-08-01          |23                   |
----------------------------------------------------------------

> select t.key, t.date t.threshold from threshold t inner join 
    (select key, max(date) max_date from threshold 
    group_by key) g on g.key = t.key and g.max_date = t.date;
==============================================================
|key                |date                |threshold           |
==============================================================
|key_abc            |2020-08-03          |55                  |
--------------------------------------------------------------
|key_xyz            |2020-08-03          |43                  |
---------------------------------------------------------------
```

In the above table, to get the threshold value per key for the latest date, we need to perfom a nested query and join by the grouped and aggregated columns.



