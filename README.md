# OLA Ride-Booking & Operations Analytics (Python + Power BI)

An end-to-end data analytics project analyzing **71,201 ride bookings** from OLA (July 2024) using **Python (Pandas)** for exploratory data analysis and feature engineering, paired with an interactive **5-page Power BI dashboard** to track booking performance, revenue, cancellations, vehicle utilization, and customer/driver ratings.

---

## Project Overview

Ride-hailing platforms operate on tight margins where high cancellation rates, long pickup wait times, and incomplete rides directly impact realised revenue. This project cleans and explores raw booking logs to answer key operational questions:

- What percentage of bookings succeed vs. fail, and what drives driver and customer cancellations?
- How much revenue is lost due to cancellations and unfulfilled requests?
- How do ride distance, booking value, and turnaround times (`V_TAT` & `C_TAT`) vary across vehicle types and time slots?
- Which payment methods dominate, and how consistent are driver and customer ratings?

---

## Tech Stack

- **Data Cleaning & EDA:** Python, Pandas, Jupyter Notebook
- **Business Intelligence & Visualization:** Microsoft Power BI, DAX
- **Data Format:** Excel (`.xlsx`) raw source $\rightarrow$ Cleaned CSV (`cleaned_bookings.csv`)

---

## Dataset Summary & Feature Engineering

- **Source File:** `Bookings-70000-Rows.xlsx` (`71,201` rows, `20` columns)
- **Date Range:** July 1, 2024 – July 31, 2024
- **Key Preprocessing Steps (`Untitled.ipynb`):**
  - Audited missing values and data types across all 20 columns (`V_TAT`, `C_TAT`, `Payment_Method`, and ratings are populated strictly for the `44,271` successful rides).
  - Extracted `Hour` from timestamp data and engineered a custom categorical `Time_Slot` column using `pd.cut()`:
    - **Midnight** (00:00–04:00) | **Early Morning** (04:00–06:00) | **Morning** (06:00–12:00) | **Afternoon** (12:00–17:00) | **Evening** (17:00–21:00) | **Night** (21:00–24:00)
  - Dropped unused URL column (`Vehicle Images`) and exported the analysis-ready dataset to `cleaned_bookings.csv`.

---

## Key Metrics & Business Insights

### 1. Booking Status Breakdown
| Booking Status | Count | Share (%) |
| :--- | :---: | :---: |
| **Success** | 44,271 | 62.18% |
| **Canceled by Driver** | 12,728 | 17.88% |
| **Canceled by Customer** | 7,214 | 10.13% |
| **Driver Not Found** | 6,988 | 9.81% |
| **Total Bookings** | **71,201** | **100.00%** |

> **Insight:** **37.82%** of all booking attempts fail to convert into completed rides, with driver-initiated cancellations being the single largest bottleneck.

### 2. Revenue & Payment Methods
- **Total Booking Value (All Requests):** ~₹39.03M
- **Realised Revenue (Successful Rides):** **₹24.22M** (`₹24,216,619`)
- **Average Booking Value (Success):** ~₹541 – ₹554 across all vehicle categories (highest for Prime Sedan at ₹554.37).
- **Payment Split:** **Cash (54.59%)** and **UPI (40.60%)** account for over **95%** of all completed transactions, while Credit Cards (3.80%) and Debit Cards (1.01%) make up the remainder.

### 3. Cancellation & Incomplete Ride Drivers
- **Driver Cancellation Reasons:**
  - *Personal & Car related issue:* **34.95%** (4,449 rides)
  - *Customer related issue:* **29.48%**
  - *Customer was coughing/sick:* **19.94%**
  - *More than permitted people in there:* **15.63%**
- **Incomplete Rides (`2,685` rides):** Driven primarily by *Customer Demand* (**41.12%**) and *Vehicle Breakdown* (**40.26%**), followed by *Other Issues* (**18.62%**).

### 4. Fleet Performance, Turnaround Times & Ratings
- **Average Ride Distance (Success):** Two-wheelers and cars average **24.8 – 25.1 km**, whereas **Auto** averages **10.05 km**, highlighting its role in short-distance urban commutes.
- **Turnaround Times:** Average Vehicle Turnaround Time (`V_TAT`) is **171.32s** (~2.9 mins) and Customer Turnaround Time (`C_TAT`) is **84.96s** (~1.4 mins), remaining consistent across all vehicle types.
- **Ratings:** Both Driver Ratings and Customer Ratings average **4.00 / 5.00** (ranging from 3.0 to 5.0) across every vehicle category.

---

## Power BI Dashboard Architecture

The interactive dark-themed Power BI report features a persistent left-hand navigation panel and date range slicer across **5 dedicated views**:

1. **Overall:** Executive KPIs (Total Bookings, Total Booking Value), Booking Status breakdown pie chart, and daily Booking Volume vs. Average Booking Value trend.
2. **Vehicle:** Ride distance, booking volume, and turnaround time comparisons across Auto, Bike, eBike, Mini, Prime Plus, Prime Sedan, and Prime SUV.
3. **Revenue:** Realised revenue vs. lost booking value, payment method distribution, and top-spending customers.
4. **Cancellation:** Overall cancellation rate, driver vs. customer cancellation reason breakdowns, and `Time_Slot` impact analysis.
5. **Rating:** Driver and customer rating distributions segmented by vehicle type.

*(Add your dashboard screenshots below before pushing to GitHub)*
```markdown
![Overall Dashboard](screenshots/overall.png)
![Cancellation Dashboard](screenshots/cancellation.png)
```

---

## Core DAX Measures

```dax
Total Success Value = 
CALCULATE(
    SUM('cleaned_bookings'[Booking_Value]),
    'cleaned_bookings'[Booking_Status] = "Success"
)

Total Distance Travelled = 
CALCULATE(
    SUM('cleaned_bookings'[Ride_Distance]),
    'cleaned_bookings'[Booking_Status] = "Success"
)

Total Canceled = 
CALCULATE(
    COUNTROWS('cleaned_bookings'),
    'cleaned_bookings'[Booking_Status] IN {"Canceled by Customer", "Canceled by Driver"}
)

Cancellation Rate = 
DIVIDE(
    [Total Canceled],
    COUNTROWS('cleaned_bookings'),
    0
)
```

---

## Strategic Recommendations

1. **Reduce Driver Cancellations:** Address the 34.95% *Personal & Car related* cancellations and 40.26% *Vehicle Breakdown* incomplete rides through proactive fleet maintenance incentives and penalty-free shift-end toggles.
2. **Shift Cash Users to UPI:** With 54.59% of rides still paid in cash, introducing small cashback or loyalty incentives for UPI payments can reduce change-related friction and lower cancellation rates.
3. **Optimize Auto vs. Cab Dispatch:** Leverage Auto's shorter average trip distance (10.05 km) to prioritize quick-turnaround urban zones while reserving Bikes, Sedans, and SUVs for longer ~25 km commutes.

---

## Repository Structure

```text
├── data/
│   ├── Bookings-70000-Rows.xlsx       # Raw dataset
│   └── cleaned_bookings.csv           # Cleaned & feature-engineered dataset
├── notebooks/
│   └── ola_booking_eda.ipynb          # Python EDA & preprocessing notebook
|── OLA_Analytics_Dashboard.pbix   # 5-page Power BI report
├── screenshots/                       # Dashboard preview images
└── README.md                          # Project documentation
```

---

## Author

**Pramatha Paul** — Data Analyst  
*Tools: Python (Pandas, NumPy) | SQL (MySQL) | Power BI | DAX*