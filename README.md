# Airways-Analysis

<div align="center">

# ✈️ Airline Route Profitability Analytics

### Turning raw flight operations data into a full profit-and-loss intelligence system

[![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)
[![Pandas](https://img.shields.io/badge/Pandas-Data%20Cleaning-150458?style=for-the-badge&logo=pandas&logoColor=white)](https://pandas.pydata.org/)
[![SQLite](https://img.shields.io/badge/SQLite-Database-003B57?style=for-the-badge&logo=sqlite&logoColor=white)](https://www.sqlite.org/)
[![Power BI](https://img.shields.io/badge/Power%20BI-Dashboard-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)](https://powerbi.microsoft.com/)
[![DAX](https://img.shields.io/badge/DAX-25%2B%20Measures-yellow?style=for-the-badge)](#-dax-measure-library)

<img width="1551" height="779" alt="p3" src="https://github.com/user-attachments/assets/1203a44f-81de-4dc2-a30d-b65dce88d0cb" />

</div>

<br>

## 📖 Overview

Airlines don't lose money on *flights* — they lose money on **routes**. A route can be full every single day and still bleed cash once fuel, crew, maintenance, insurance, and airport fees are factored in.

This project builds an end-to-end analytics pipeline that answers one deceptively hard question:

> **"Which routes actually make us money — and why?"**

Starting from a raw relational flight-operations database (9 interconnected tables covering bookings, tickets, boarding passes, seats, aircraft, and airports), the pipeline cleans, reshapes, and enriches the data in Python, then feeds a fully modeled **star-schema semantic layer** in Power BI with **25+ custom DAX measures** — surfacing revenue, cost structure, load factor, and profit margin down to the individual route.

<br>

## 🎯 Business Questions Answered

| # | Question | How it's answered |
|---|---|---|
| 1 | Which routes are profitable, and which are quietly losing money? | Route-level Profit & Profit Margin measures |
| 2 | What's actually eating the revenue — fuel? crew? overhead? | Full cost waterfall (10+ cost categories) |
| 3 | Are we filling planes efficiently? | Load Factor % vs. Aircraft Capacity |
| 4 | How does profitability shift by season / route category? | Season & Route Category slicers |
| 5 | What does the revenue → profit bridge look like at a glance? | Interactive waterfall chart |

<br>

## 🏗️ Architecture

```
┌──────────────────┐     ┌───────────────────────┐     ┌──────────────────────┐
│   travel.sqlite   │     │   Python / Pandas ETL   │     │   Power BI Model     │
│  9 relational      │ ─▶ │  clean · reshape ·      │ ─▶ │  Star schema · DAX    │
│  source tables     │     │  type-cast · export      │     │  measures · visuals   │
└──────────────────┘     └───────────────────────┘     └──────────────────────┘
      raw OLTP data          Jupyter Notebook                interactive report
```

**Pipeline stages:**

1. **Extract** — connect to the SQLite operations database and pull every core table (`flights`, `airports_data`, `boarding_passes`, `bookings`, `seats`, `ticket_flights`, `tickets`, `aircrafts_data`).
2. **Clean**
   - Handled `\N` null placeholders in `actual_departure` / `actual_arrival` by backfilling from scheduled times
   - Split combined datetime fields into separate **date** and **time** columns for clean Power BI relationships
   - Parsed nested JSON fields (`airport_name`, `city`) into flat, readable text
   - Audited every table for nulls and duplicates before export
3. **Load** — exported each cleaned table to Excel, ready for Power Query ingestion
4. **Model** — built a star schema in Power BI linking flights, bookings, tickets, seats, and a dedicated **cost & profitability fact table** to a shared **Calendar** dimension
5. **Measure** — authored a full DAX measure library to calculate revenue, cost, margin, and load metrics at any level of the model

<br>

## 🗂️ Data Model

<div align="center">
<img src="assets/data-model.png" width="850" alt="Star Schema Data Model">
</div>

A relationship-rich star schema: transactional tables (`flights`, `tickets`, `bookings`, `boarding_passes`, `ticket_flights`, `seats`) surround a central `airline_route_profitability` fact table carrying every cost line item, joined through a shared `calendar` dimension for consistent time intelligence — plus a standalone `Sheet1` measure table keeping all 25+ DAX formulas organized and disconnected from the model.

<br>

## 📐 DAX Measure Library

A sample of the measures powering the report — organized by category:

<table>
<tr><td valign="top" width="33%">

**Revenue & Profit**
- Ticket Revenue
- Ancillary Revenue
- Total Cost
- Profit
- Profit Margin
- Waterfall value

</td><td valign="top" width="33%">

**Cost Breakdown**
- Fuel Cost
- Crew Cost
- Maintenance Cost
- Catering Cost
- Handling Cost
- Insurance Cost
- IT Systems Cost
- Marketing Cost
- Navigation Fees
- Overhead Cost
- Depreciation Cost
- Sales & Distribution Cost

</td><td valign="top" width="33%">

**Operational KPIs**
- Load Factor
- Aircraft Capacity
- Flight Hours
- Passengers
- Demand Level

</td></tr>
</table>

<br>

## 📊 Dashboard Preview

<div align="center">
<img src="assets/dashboard-routes.png" width="850" alt="Route Profitability Table">
<br><i>Route-level profitability — revenue, cost, and margin side by side</i>
<br><br>
<img src="assets/dashboard-filtered.png" width="850" alt="Filtered Dashboard View">
<br><i>Drill-down view filtered by route category and season</i>
</div>

<br>

## 🛠️ Tech Stack

| Layer | Tools |
|---|---|
| **Source Database** | SQLite |
| **ETL & Data Cleaning** | Python, Pandas, SQLAlchemy, Jupyter Notebook |
| **Data Modeling & Visualization** | Power BI, DAX, Power Query |
| **Version Control** | Git & GitHub |

<br>

## 🖥️ Dashboard Pages in Detail

**📊 Executive Overview** — Total Revenue, Profit Margin, Sum of Profit, Flights, Passengers, and Best Route at a glance, backed by a full Revenue-to-Profit waterfall that breaks the margin erosion down cost-by-cost (Crew, Marketing Cost, Maintenance, Depreciation, Overhead, Sales Distribution, Fuel).
<img width="1554" height="791" alt="image" src="https://github.com/user-attachments/assets/b25c0010-c3e8-4b7b-bdb9-dd2ece0982f2" />
<img width="1557" height="793" alt="image" src="https://github.com/user-attachments/assets/bbe0afd7-c114-45d1-915a-dd8f63e5a3e2" />

**⚙️ Operations Analysis** — Average Load Factor and Average Flight Hours, a full flight-status breakdown (Arrived / Scheduled / On-Time / Cancelled / Delayed), delay-time analysis by departure airport, and flight-volume ranking by route — filterable by origin, destination, demand level, season, and aircraft type.
<img width="1560" height="792" alt="image" src="https://github.com/user-attachments/assets/cc6bed65-f7dd-4cfe-b332-b677ff0a8a64" />

**👥 Passenger & Booking** — AVG Ticket Profit, a bookings-vs-tickets trend line, seat-class distribution (Economy / Business / Comfort), demand-level split, and an interactive **decomposition tree** that lets you drill from total Passengers down through Season → Demand Level → Route Category → Aircraft Type → Flight Hours → Route → Load Factor → Ticket Revenue.
<img width="1560" height="793" alt="image" src="https://github.com/user-attachments/assets/e0aaacce-3472-44aa-816d-24af088f7921" />

**✈️ Aircraft Analysis** — fleet utilization, per-aircraft profitability, and maintenance-cost insight to evaluate which aircraft types are pulling their weight.

Every page shares a common filter panel (Season, Demand Level, Route Category, Route, Aircraft Type, Load Factor, Month/Quarter) and supports full cross-filtering and drill-through across the model.
<img width="1556" height="786" alt="image" src="https://github.com/user-attachments/assets/083c064c-050a-4fc7-b77a-88d912401289" />

<br>

## 🚀 Getting Started

```bash
# 1. Clone the repository
git clone https://github.com/<your-username>/airline-route-profitability.git
cd airline-route-profitability

# 2. Install dependencies
pip install pandas sqlalchemy pyodbc openpyxl

# 3. Run the ETL notebook
jupyter notebook notebooks/etl_pipeline.ipynb
```

Then open `dashboard/Airline_Route_Profitability.pbix` in Power BI Desktop to explore the full interactive report.

<br>

## 📁 Project Structure

```
airline-route-profitability/
├── notebooks/
│   └── etl_pipeline.ipynb        # Extraction, cleaning & export logic
├── data/
│   └── travel.sqlite              # Source relational database
├── exports/                       # Cleaned Excel tables (Power BI source)
├── dashboard/
│   └── Airline_Route_Profitability.pbix
├── assets/                        # README images
└── README.md
```

<br>

## ⚠️ Data Disclaimer

This project was built strictly for **portfolio and learning purposes**. The underlying dataset is a **public/sample flight-operations dataset** — it does **not** represent real Qatar Airways data, financials, routes, or operations in any way. The airline branding shown in the dashboard was used purely for **visual design and storytelling purposes**, to simulate how a real airline BI report would look and feel.

<br>

## 💡 Key Takeaways

- Built a resilient ETL pipeline that handles messy real-world data: null placeholders, nested JSON, mixed datetime formats
- Designed a clean star-schema model — the foundation every scalable BI report needs
- Authored a self-contained DAX measure library separating **logic** from **data**, making the model easy to audit and extend
- Delivered a decision-ready dashboard that turns route-level cost complexity into a single, clear profitability story

<br>

---

<div align="center">

Built with ❤️ by **Abdelrahman**

*If this project helped you or you find it interesting, consider giving it a ⭐*

</div>
