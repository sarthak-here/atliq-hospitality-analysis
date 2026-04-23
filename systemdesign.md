# AtliQ Hospitality Analysis - System Design

## What It Does
An exploratory data analysis project on a hotel chain's booking data. Analyzes revenue,
occupancy rates, room type performance, booking platform trends, and seasonal patterns
across AtliQ's properties using Python and Pandas, with visualizations in Jupyter.

---

## Architecture

```
Raw CSV datasets (star schema)
        |
        v
+----------------------------------------------+
|       hotel-chain-analysis.ipynb             |
|                                              |
|  1. Data Loading    (pandas.read_csv)        |
|  2. Data Cleaning                            |
|     - Handle missing values                  |
|     - Fix dtypes (dates, categories)         |
|     - Remove outliers                        |
|  3. Feature Engineering                      |
|     - Occupancy % per property/date          |
|     - RevPAR (Revenue per Available Room)    |
|     - Booking lead time                      |
|     - Weekend vs weekday split               |
|  4. Analysis + Visualizations                |
|     (matplotlib / seaborn)                   |
|  5. Insights + Recommendations               |
+----------------------------------------------+
```

---

## Dataset Structure

| File                         | Description                                     |
|------------------------------|-------------------------------------------------|
| dim_hotels.csv               | property_id, name, category, city               |
| dim_rooms.csv                | room_id, room_class                             |
| dim_date.csv                 | date, week, day_type (weekend/weekday)          |
| fact_bookings.csv            | Individual bookings with revenue and platform   |
| fact_aggregated_bookings.csv | Daily aggregates per property                   |
| new_data_august.csv          | Incremental data (pipeline simulation)          |

---

## Data Flow

```
dim_hotels + dim_rooms + dim_date (dimension tables)
fact_bookings + fact_aggregated_bookings (fact tables)
        |
  pandas.merge() -- star schema joins
        |
  Data Cleaning:
  - Parse check_in_date to datetime
  - Drop cancelled/no-show rows or flag them
  - Cap revenue outliers (> 3 std deviations)
        |
  Feature Engineering:
  - occupancy_pct  = bookings / capacity
  - RevPAR         = total_revenue / available_rooms
  - lead_time      = check_in - booking_date (days)
  - realization_rt = actual_revenue / potential_revenue
        |
  Analysis Questions:
  Q1: Which city has the highest RevPAR?
  Q2: Which booking platform drives most revenue?
  Q3: Occupancy: weekend vs weekday difference?
  Q4: Which room class has the best realization rate?
  Q5: Week-over-week revenue trend?
        |
  bar charts, line plots, heatmaps, box plots
  -> Insights and business recommendations
```

---

## Key Design Decisions

| Decision                   | Reason                                                |
|----------------------------|-------------------------------------------------------|
| Star schema data model     | Mirrors real hotel chain data warehouse design        |
| Separate dim + fact tables | Same pattern used in production BI (Power BI, Tableau)|
| RevPAR as primary KPI      | Industry-standard hotel performance metric            |
| August data as incremental | Simulates real data pipeline update cycles            |

---

## Interview Conclusion

This project mirrors a real hotel analytics data pipeline in miniature. The star schema --
dimension tables joined to fact tables -- is the standard pattern used in data warehouses
and BI tools. The analysis targets the KPIs that hotel revenue managers actually track:
RevPAR, occupancy rate, realization rate, and platform contribution. The incremental
August data simulates a real pipeline where new data arrives periodically without
reprocessing everything. Production upgrade: move from Jupyter to dbt + Airflow feeding
a Metabase dashboard, with automated alerts when occupancy drops below threshold.
