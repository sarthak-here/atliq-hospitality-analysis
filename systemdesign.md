# AtliQ Hospitality Analysis — System Design

## What It Does
An exploratory data analysis (EDA) project on a hotel chain's booking data. Analyzes revenue, occupancy rates, room type performance, booking platform trends, and seasonal patterns across AtliQ's properties using Python and Pandas, with visualizations in a Jupyter Notebook.

---

## Architecture

```
Raw CSV datasets
      |
      v
+----------------------------------------------+
|         hotel-chain-analysis.ipynb           |
|                                              |
|  1. Data Loading (pandas.read_csv)           |
|  2. Data Cleaning                            |
|     - Handle missing values                  |
|     - Fix data types (dates, categories)     |
|     - Remove outliers                        |
|  3. Feature Engineering                      |
|     - Occupancy % per property/date          |
|     - Revenue per available room (RevPAR)    |
|     - Booking lead time                      |
|     - Weekend vs weekday split               |
|  4. Analysis & Visualization                 |
|     - matplotlib / seaborn charts            |
|  5. Insights & Recommendations               |
+----------------------------------------------+
```

---

## Dataset Structure

| File | Description |
|---|---|
| dim_hotels.csv | Hotel master: property_id, name, category, city |
| dim_rooms.csv | Room master: room_id, room_class |
| dim_date.csv | Date dimension: date, week, day_type (weekend/weekday) |
| fact_bookings.csv | Individual bookings: hotel, room, check_in, revenue, platform |
| fact_aggregated_bookings.csv | Daily aggregates per property |
| new_data_august.csv | Incremental data for August (data pipeline simulation) |

---

## Data Flow

```
dim_hotels + dim_rooms + dim_date (dimension tables)
fact_bookings + fact_aggregated_bookings (fact tables)
        |
        v
  pandas.merge() -- star schema joins
  hotel_id -> property details
  room_id  -> room class
  date     -> week number, day type
        |
        v
  Data Cleaning:
  - Drop bookings with no_show or cancelled status (or flag them)
  - Parse check_in_date to datetime
  - Cap revenue outliers (> 3 std deviations)
        |
        v
  Feature Engineering:
  - occupancy_pct = bookings / capacity
  - RevPAR = total_revenue / available_rooms
  - lead_time = check_in - booking_date (days)
  - realization_rate = actual_revenue / potential_revenue
        |
        v
  Analysis:
  Q1: Which city has highest RevPAR?
  Q2: Which booking platform drives most revenue?
  Q3: How does occupancy differ weekend vs weekday?
  Q4: Which room class has highest realization rate?
  Q5: Revenue trend over time (week-over-week)?
        |
        v
  Visualizations: bar charts, line plots, heatmaps, box plots
  Insights & business recommendations
```

---

## Key Design Decisions

| Decision | Reason |
|---|---|
| Star schema data model | Mirrors real hotel chain data warehouse design |
| Separate dim + fact tables | Enables flexible joins; same pattern used in production BI tools |
| RevPAR as primary KPI | Industry-standard metric for hotel performance |
| August data as incremental load | Simulates real-world data pipeline updates |

---

## Interview Conclusion

This project mirrors a real hotel analytics data pipeline in miniature. The data is structured as a star schema — dimension tables (hotels, rooms, dates) joined to fact tables (bookings) — which is the standard pattern used in data warehouses and BI tools like Power BI and Tableau. The analysis targets the KPIs that hotel revenue managers actually track: RevPAR, occupancy rate, realization rate, and platform contribution. The inclusion of incremental August data simulates a real data pipeline where new data arrives periodically and must be merged into the existing dataset without reprocessing everything. If I were building a production version, I would move from Jupyter notebooks to a dbt + Airflow pipeline feeding a dashboard in Metabase or Superset, with automated alerting when occupancy drops below threshold.
