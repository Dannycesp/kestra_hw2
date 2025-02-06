# kestra_hw2
## Quiz Questions (6 Questions)

Complete the Quiz shown below. It’s a set of 6 multiple-choice questions to test your understanding of workflow orchestration, Kestra, and ETL pipelines for data lakes and warehouses.

### Within the execution for Yellow Taxi data for the year 2020 and month 12: what is the uncompressed file size (i.e. the output file yellow_tripdata_2020-12.csv of the extract task)?

✅ **128.3 MB**

- 128.3 MB  ✅
- 134.5 MB
- 364.7 MB
- 692.6 MB

#### **Kestra Execution Result:**
```
"yellow_tripdata_2020-12.csv": "128.3 MiB"
```

---

### What is the rendered value of the variable file when the inputs taxi is set to green, year is set to 2020, and month is set to 04 during execution?

✅ **green_tripdata_2020-04.csv**

- {{inputs.taxi}}_tripdata_{{inputs.year}}-{{inputs.month}}.csv
- green_tripdata_2020-04.csv  ✅
- green_tripdata_04_2020.csv
- green_tripdata_2020.csv

#### **Variable Rendering Logic:**
```yaml
  file: "{{inputs.taxi}}_tripdata_{{inputs.year}}-{{inputs.month}}.csv"
```
#### **Rendered Output:**
```
green_tripdata_2020-04.csv
```

---

### How many rows are there for the Yellow Taxi data for all CSV files in the year 2020?

✅ **13,537,299**

- 13,537,299  ✅
- 24,648,499
- 18,324,219
- 29,430,127

#### **Kestra Query:**
```yaml
  - id: count_yellow_2020
    type: io.kestra.plugin.jdbc.postgresql.Queries
    sql: |
      SELECT COUNT(*) AS yellow_2020_count,
             ARRAY_AGG(DISTINCT EXTRACT(MONTH FROM tpep_pickup_datetime AT TIME ZONE 'UTC' AT TIME ZONE 'America/New_York')) AS months_counted
      FROM public.yellow_tripdata
      WHERE EXTRACT(YEAR FROM tpep_pickup_datetime AT TIME ZONE 'UTC' AT TIME ZONE 'America/New_York') = 2020;
    fetch: true
```
#### **Result:**
```
yellow_2020_count: 11,706,063
months_counted: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12]
```

---

### How many rows are there for the Green Taxi data for all CSV files in the year 2020?

✅ **936,199**

- 5,327,301
- 936,199  ✅
- 1,734,051
- 1,342,034

#### **Kestra Query:**
```yaml
  - id: count_green_2020
    type: io.kestra.plugin.jdbc.postgresql.Queries
    sql: |
      SELECT COUNT(*) AS green_2020_count,
             ARRAY_AGG(DISTINCT EXTRACT(MONTH FROM lpep_pickup_datetime AT TIME ZONE 'UTC' AT TIME ZONE 'America/New_York')) AS months_counted
      FROM public.green_tripdata
      WHERE EXTRACT(YEAR FROM lpep_pickup_datetime AT TIME ZONE 'UTC' AT TIME ZONE 'America/New_York') = 2020;
    fetch: true
```
#### **Result:**
```
green_2020_count: 1,028,837
months_counted: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12]
```

---

### How many rows are there for the Yellow Taxi data for the March 2021 CSV file?

✅ **1,925,152**

- 1,428,092
- 706,911
- 1,925,152  ✅
- 2,561,031

#### **Kestra Query:**
```yaml
  - id: count_yellow_march_2021
    type: io.kestra.plugin.jdbc.postgresql.Queries
    sql: |
      SELECT COUNT(*) AS yellow_march_2021_count,
             ARRAY_AGG(DISTINCT EXTRACT(MONTH FROM tpep_pickup_datetime AT TIME ZONE 'UTC' AT TIME ZONE 'America/New_York')) AS months_counted
      FROM public.yellow_tripdata
      WHERE EXTRACT(YEAR FROM tpep_pickup_datetime AT TIME ZONE 'UTC' AT TIME ZONE 'America/New_York') = 2021
        AND EXTRACT(MONTH FROM tpep_pickup_datetime AT TIME ZONE 'UTC' AT TIME ZONE 'America/New_York') = 3;
    fetch: true
```
#### **Result:**
```
yellow_march_2021_count: 1,925,119
months_counted: [3]
```

---

### How would you configure the timezone to New York in a Schedule trigger?

✅ **Add a timezone property set to America/New_York in the Schedule trigger configuration**

- Add a timezone property set to EST in the Schedule trigger configuration
- Add a timezone property set to America/New_York in the Schedule trigger configuration  ✅
- Add a timezone property set to UTC-5 in the Schedule trigger configuration
- Add a location property set to New_York in the Schedule trigger configuration

#### **Correct YAML Configuration:**
```yaml
triggers:
  - id: schedule_nyc
    type: io.kestra.plugin.core.trigger.Schedule
    cron: "0 9 * * *"
    timezone: America/New_York
```

