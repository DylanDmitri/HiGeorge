Going to use a single table to hold the relevant data, with a single side-table to the company names as they can get long.
I don't think the problem is complex enough the warrant multiple tables.

If we didn't group by building, would want to normalize the zipcodes and adresses as they repeat a lot and can be larger.
But grouping by building removes that need at the cost of data granularity.

```
Table "RentByBuilding"
  varchar(10) zipcode
  varchar(100) address
  date posted_at            // at month precision
  int rooms                 // number of rooms in this building
  bigint tot_price          // total price of all rooms
index "Buildings" on zipcode
index "Buildings" on (zipcode, posted_at)
```

Would also make a view, broken down my month and zipcode.

```
SELECT
  zipcode, posted_at, sum(rooms) as volume, sum(price)/sum(rooms) as avg_price
FROM RentByBuilding
GROUP BY zipcode, posted_at
```

```
Table "RentByZipMonth"
  varchar(10) zipcode
  date posted_at
  int volume                 // total rooms
  int avg_price              // 
```



