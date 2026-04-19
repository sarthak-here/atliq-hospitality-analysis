# AtliQ Hotels - Hospitality Data Analysis

A data analysis project on AtliQ Hotels, a fictional Indian hotel chain. I worked through booking and occupancy data across 25 properties in 4 cities to pull out business insights using Python and Pandas.

---

## About the Project

AtliQ Hotels operates across Delhi, Mumbai, Hyderabad, and Bangalore — a mix of Luxury and Business category properties. The dataset covers May to August 2022 and has over 134,000 individual booking records. The goal was to understand what's driving occupancy and revenue, and where the business might be leaving money on the table.

This was a good exercise in real-world messy data — the raw datasets had negative guest counts, revenue outliers, capacity mismatches, and missing ratings. Cleaned all of that before doing any actual analysis.

---

## Dataset

Six CSV files in the `datasets/` folder:

| File | Description |
|------|-------------|
| `dim_hotels.csv` | Property info — name, city, category (Luxury/Business) |
| `dim_rooms.csv` | Room types: Standard, Elite, Premium, Presidential |
| `dim_date.csv` | Date dimension with week number and weekday/weekend flag |
| `fact_bookings.csv` | 134,590 individual booking records |
| `fact_aggregated_bookings.csv` | Daily bookings aggregated by property and room type |
| `new_data_august.csv` | August 2022 data added later in the analysis |

---

## Data Cleaning

Before jumping into analysis, I had to fix a few things:

- **Negative guest counts** — 12 records had 0 or negative values. Dropped them.
- **Revenue outliers** — Used mean + 3×std dev as the threshold (came out to ~₹2.94 lakh). Removed 5 records that crossed it.
- **Null capacity values** — 2 missing values in the aggregated bookings table. Filled with median capacity grouped by property and room type.
- **Overbooking records** — 6 rows where `successful_bookings > capacity`. These don't make sense, so removed them.
- **Missing ratings** — 57.8% of bookings have no rating. Left these as null rather than imputing — too much data is missing to guess at it.

---

## Analysis & Findings

### Occupancy by Room Category
All four room types cluster around 57–59% occupancy. Presidential rooms lead slightly at 59.28%, but the difference is small. Room type isn't really what's driving occupancy differences.

### Occupancy by City
Delhi has the highest occupancy at **61.51%**, followed by Hyderabad (58.12%), Mumbai (57.91%), and Bangalore (56.33%). Delhi consistently outperforms the others.

### Weekday vs. Weekend
This was one of the more interesting findings:
- **Weekend occupancy: 72.34%**
- **Weekday occupancy: 50.88%**

A ~42% gap. The hotels are heavily leisure-driven. Weekday business travel is clearly underutilized and could be a growth area.

### Monthly Revenue (May–July 2022)
| Month | Revenue Realized |
|-------|-----------------|
| May 2022 | ₹40.8 Cr (highest) |
| July 2022 | ₹38.9 Cr |
| June 2022 | ₹37.7 Cr (lowest) |

May is the strongest month. June dips — likely pre-monsoon slowdown.

### Revenue by City
Mumbai generates the most revenue (₹66.8 Cr) despite having lower occupancy than Delhi. Delhi has the highest occupancy but the lowest revenue. That's a pricing gap — Delhi seems to be undercharging relative to demand.

### Revenue by Property
Atliq Exotica is the top-performing property at ₹32 Cr. Atliq Seasons is way at the bottom at ₹6.6 Cr — nearly 5x lower than the best performer. Worth investigating what's different there.

### Booking Platforms
| Platform | Bookings | Share |
|----------|----------|-------|
| Others | 55,066 | 40.9% |
| MakeYourTrip | 26,898 | 20.0% |
| Logtrip | 14,756 | 11.0% |
| Direct Online | 13,379 | 10.0% |
| Tripster | 9,630 | 7.2% |
| Journey | 8,106 | 6.0% |
| Direct Offline | 6,755 | 5.0% |

Direct bookings dominate. That's a good sign — OTAs take a cut of every booking, so this keeps margins higher.

### Guest Ratings by City
- Delhi: 3.78 ⭐
- Hyderabad: 3.66 ⭐
- Mumbai: 3.65 ⭐
- Bangalore: 3.41 ⭐

All cities sit in the 3.4–3.8 range. Not bad, but there's definitely room to improve, especially Bangalore.

---

## Key Takeaways

1. **Weekdays need attention** — 50.88% weekday occupancy vs 72.34% on weekends. Targeted weekday promotions could make a real difference.
2. **Delhi is underpriced** — highest occupancy, lowest revenue. A pricing review is overdue.
3. **Atliq Seasons is underperforming** — 5x revenue gap compared to Atliq Exotica. Something structural seems off.
4. **Direct bookings are strong** — 40.9% share is a competitive advantage. Worth protecting and growing.
5. **Ratings data collection is broken** — 57.8% missing is too high to be organic. Likely a process issue at checkout.

---

## How to Run

```bash
pip install -r requirements.txt
jupyter notebook hotels_analysis.ipynb
```

---

## Tools Used

- **Python 3.x**
- **Pandas** — data loading, cleaning, merging, groupby aggregations
- **NumPy** — outlier thresholds, numerical operations
- **Matplotlib** — charts and visualizations (via Pandas plotting)
- **Jupyter Notebook** — interactive analysis environment
