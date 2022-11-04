# Data with Danny - Data Exploration
First open the terminal and navigate to the `docker-compose.yml` file folder.

`$cd Desktop/Learning/serious-sql`

`$sudo docker-compose up`

Then navigate to a browser (Chrome seems to work best) and type: `localhost:3000`

Time to get......... <span style="font-size:30px;">serious.</span>

## Select and Sort Data

1 - `OFFSET` can be used to skip a few rows (in this case the first two rows):

```sql
SELECT postal_code, city_id
FROM dvd_rentals.address
ORDER BY city_id DESC
LIMIT 5 OFFSET 2;
```

2 - `NULLS FIRST` is a way to ensure null values are sent to the top. Otherwise they default to the bottom (this is `DESC` agnostic).

```sql
WITH test_data (sample_values) AS (
VALUES
(null),
('0123'),
('_123'),
(' 123'),
('(abc'),
('  abc'),
('bca')
)
SELECT * FROM test_data
ORDER BY 1 NULLS FIRST;
```

## Record Counts and Distinct Values

1 - `DISTINCT` can be used the same as `unique` in pandas:

```sql
SELECT DISTINCT
  rating
FROM dvd_rentals.film_list
```

Can pair this with a different aggregate function such as `COUNT`:

```sql
SELECT
  COUNT(DISTINCT category) AS unique_category_count
FROM dvd_rentals.film_list
```

unique_category_count is known as a column alias.

2 - `GROUP BY` will only return one row for each group requested. So whether it is a `COUNT`, `MEAN`, or whatever, only one row will ever be returned with a `GROUP BY`. `ORDER BY` comes after `GROUP BY`.

```sql
SELECT
  rating, COUNT(*) AS total
FROM
  dvd_rentals.film_list
GROUP BY
  rating
ORDER BY 
  total DESC

RETURNS
rating          total
----------------------
PG-13           223
NC-17           210
PG              194
R               193
G               177
```

Can also use `GROUP BY` to group combinations of certain columns together. Can be used to find the frequency of intersections of columns.

```sql
SELECT
  rating, category, COUNT(*) AS total
FROM
  dvd_rentals.film_list
GROUP BY
  rating, category
ORDER BY 
  total DESC

RETURNS
rating      category        total
----------------------------------
PG-13       Drama           22           
NC-17       Music           20
PG-13       International   19
PG-13       Animation       19
PG          Family          18
```

3 - When doing algebra (specifically division) with integers, SQL default is to do floor division or to drop the values after the decimal (0.3 -> 0, 6.9 -> 6, etc.) This can be circumnavigated by type casting:

```sql
SELECT
  15::NUMERIC / 20;

RETURNS
0.750000000000000000000000
```

4 - Counting number of `NULLS`

```sql
SELECT COUNT(*)
FROM health.user_logs
WHERE systolic IS NULL;
```

## Identifying Duplicate Records

1 - Steps when inspecting a new data set

1) View head
2) Count # records
3) Get unique counts on all columns (can only do it one at a time)
4) Check frequency of each unique value in each column (preferably with %)
5) Check to understand nulls. In the health record, nulls commonly occured for systolic and diastolic when blood pressure was not the measure. Also 0 values occurred in measure_value for blood pressure because they were found in the systolic and diastolic columns.

```sql
--Counts number of nulls values in systolic
SELECT COUNT(*) - COUNT(systolic)
FROM health.user_logs;

--Counts number of times each unique measure was paired with a measure_value of 0
SELECT
  measure,
  COUNT(*)
FROM health.user_logs
WHERE measure_value = 0
GROUP BY 1;
```

2 - Remove duplicates by using the `DISTINCT` clause. This will only return rows which are not repeated.

```sql
SELECT DISTINCT *
FROM health.user_logs;
```

3 - Identifying <u>number</u> of duplicates is done three ways (`SELECT COUNT(DISTINCT *)` won't work)

1) Subqueries (least recommended due to being inside -> out)

```sql
SELECT COUNT(*)
FROM (
SELECT DISTINCT *
FROM health.user_logs
) AS subquery;
```

2) Common Table Expressions (reads sequentially). Is a super temporary table. Is analagous to subquery, but reads better. However, it cannot be used in a `WHERE` clause.

```sql
WITH deduped_logs AS (
SELECT DISTINCT *
FROM health.user_logs
)
SELECT COUNT(*)
FROM deduped_logs;
```

3) Temporary Tables (written in disc, deleted once session is closed, good for large data)

```sql
CREATE TEMP TABLE deduplicated_user_logs AS
SELECT DISTINCT *
FROM health.user_logs;
```

Use temporary table if you could reuse it, or CTE if you don't.

4 - Identifying duplicate rows is a little different. This will output all rows and group those duplicates together. This will NOT exclude non duplicates.

```sql
SELECT
  id,
  log_date,
  measure,
  measure_value,
  systolic,
  diastolic,
  COUNT(*) AS frequency
FROM health.user_logs
GROUP BY
  id,
  log_date,
  measure,
  measure_value,
  systolic,
  diastolic
ORDER BY frequency DESC
```

But if you are wanting to exclude the non-duplicated rows

```sql
WITH duplicate_data AS(
SELECT
  id,
  log_date,
  measure,
  measure_value,
  systolic,
  diastolic,
  COUNT(*) AS record_count
FROM health.user_logs
GROUP BY
  id,
  log_date,
  measure,
  measure_value,
  systolic,
  diastolic
-- ORDER BY frequency DESC   (not necessary now)
)
SELECT id, SUM(record_count)
FROM duplicate_data
WHERE record_count > 1
GROUP BY id
ORDER BY total_count DESC
```

## Summary Statistics

1 - Mean, median, and mode (after deleting some obvious mistakes). Yes median and mode are weird like that and requires `WITHIN GROUP`

```sql
WITH legit_data AS(
SELECT
  measure,
  measure_value
FROM health.user_logs
WHERE measure_value < 10000
)
SELECT
  measure,
  ROUND(AVG(measure_value),2) AS average_value,
  PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY measure_value) AS median_value,
  MODE() WITHIN GROUP (ORDER BY measure_value) AS mode_value
FROM
  legit_data
GROUP BY
  measure
ORDER BY
  average_value DESC;
```

2 - Range is simple `MAX` - `MIN`. Variance is `VAR`. Standard deviation is `STDDEV` 

## Distribution Functions

1 - Can see the distribution of values over a range. This case is splitting all measure_values where the measure is weight into 100 even buckets (27-28 rows/bucket, starting with 2782 rows). 

```sql
-- labels each row with it's respective percentile, i.e. first 28 rows are labeled 1. Next 28 are 2, etc.
WITH percentile_values AS (
SELECT 
  measure_value,
  --splits the rows into 100 buckets by percentile
  NTILE(100) OVER (
    ORDER BY
      measure_value
  ) AS percentile
FROM
  health.user_logs
WHERE 
  measure = 'weight'
)
-- finds min/max from each grouped percentile point, i.e. 
SELECT
  percentile,
  MIN(measure_value),
  MAX(measure_value)
  -- COUNT(*) shows how many are in each bucket
FROM percentile_values
GROUP BY percentile
ORDER BY percentile;
```
OUTPUT:
```
percentile  min         max
1           0           29.029888
2           29.48348    32.0689544
3           32.205032   35.380177
4           35.380177   36.74095
5           36.74095    37.194546
.           .           .
.           .           .
.           .           .
99          133.89095   136.531192
100         136.531192  39642120
```
If we hone in on just the 100 percentile bucket, we can see how they look and group together. Using a ***Window Function*** we can look at each row in the 100th percentile (`ROW_NUMBER`), their rank--counts ties (`RANK`), and their dense rank--doesn't count ties (`DENSE_RANK`). 

```sql
WITH percentile_values AS (
  SELECT
    measure_value,
    NTILE(100) OVER (
      ORDER BY
        measure_value
    ) AS percentile
  FROM health.user_logs
  WHERE measure = 'weight'
)
SELECT
  measure_value,
  -- these are examples of window functions below
  ROW_NUMBER() OVER (ORDER BY measure_value DESC) as row_number_order,
  RANK() OVER (ORDER BY measure_value DESC) as rank_order,
  DENSE_RANK() OVER (ORDER BY measure_value DESC) as dense_rank_order
FROM percentile_values
WHERE percentile = 100
ORDER BY measure_value DESC
```
OUTPUT:
```sql
measure_value     row_number 	rank  dense_rank
39642120          1 	        1     1
39642120          2 	        1 	  1
576484 	          3 	        3 	  2
200.487664 	      4 	        4 	  3
190.4 	          5 	        5 	  4
```

Once outliers are detected, we can create a temp table where they are dropped. 

```sql
CREATE TEMP TABLE clean_weight_logs AS (
  SELECT *
  FROM health.user_logs
  WHERE measure = 'weight'
    AND measure_value > 0 
    AND measure_value < 201
);
```

### Histogram
Can easily make histogram of the cleaned data (clean_weight_logs) using the `floor()` function. Floor chops off anything in the decimal place. We divide the value by 10 t0 turn 62 into 6.2, floor chops off the .2, multiply the value back into 60, and group and count the number of 60s, 70s, etc.

```sql
  SELECT
    floor(measure_value/10.00)*10 as bin_floor, 
    COUNT(measure_value)
  FROM
    clean_weight_logs
  WHERE
    measure = 'weight'
  GROUP BY bin_floor

-- or we can use the WIDTH BUCKET function

SELECT
  WIDTH_BUCKET(measure_value, 0, 200, 50) AS bucket, --min, max, width of bucket
  AVG(measure_value) AS measure_value,
  COUNT(*) AS frequency
FROM clean_weight_logs
GROUP BY bucket
ORDER BY bucket;
```
SQLPad can then show a histogram with the configure visualization icon next to **Run**. Set it to 'Bar - Verticle', 'Bar Label' as 'bin_floor', and 'Bar Value' as 'count'. 