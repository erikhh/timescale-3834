# Reproduce Timescale [issue 3834](https://github.com/timescale/timescaledb/issues/3834)

## Start your database version of choice
```
docker-compose up -d pg13
```

## Get psql promt
```
docker exec -it pg13 psql -U postgres gapfill
```

## Query that reproduces issue 3834
```
SELECT
  time_bucket_gapfill('30000 ms', time) AS timestamp,
  ship_id,
  interpolate(avg(nautical_mile_per_hour)) AS speedKnots,
  'speedlog' AS source
  FROM speed_through_water_measurement
  WHERE ship_id IN (858, 1)
  AND time >= '2020-12-01 14:05:00+01' AND time < '2020-12-01 14:10:00+01'
  GROUP BY timestamp, ship_id;
```
