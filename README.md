# 🚚 FastShip Logistics - Performance Analysis FY2023

![Excel](https://img.shields.io/badge/Microsoft_Excel-217346?style=for-the-badge&logo=microsoft-excel&logoColor=white)
![Power Pivot](https://img.shields.io/badge/Power_Pivot-185ABD?style=for-the-badge&logo=microsoft&logoColor=white)
![DAX](https://img.shields.io/badge/DAX-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)

**End-to-end logistics analytics project built in Microsoft Excel using Power Pivot and DAX.**

*2,000 shipment records · 7 carriers · 10 warehouses · Multiple business questions*


## 🗂 Project Overview

FastShip Logistics is a mid-sized US shipping company that partners with **7 carrier companies** across **10 warehouse locations** to deliver packages to 50+ cities nationwide.

This project was commissioned by the Operations Manager, **Sarah Chen**, after the executive team raised concerns about rising shipping costs, inconsistent delivery performance, and increasing package losses throughout 2023.

The goal was to build a complete analytical solution from raw data ingestion and Power Pivot modelling, through DAX measure development and PivotTable analysis, to an interactive executive dashboard and a formal Word insights report — that enables data-driven decision-making at the operational and strategic level.


## ❗ Business Problem

Sarah Chen identified **four operational concerns** heading into the analysis:

| # | Concern | Question Being Asked |
|---|---|---|
| 1 | **Rising shipping costs** | Are we overspending on certain routes or carriers? |
| 2 | **Delivery delays** | Which carriers or warehouses are driving failures? |
| 3 | **Lost & returned packages** | Why are shipments being lost or returned? |
| 4 | **Operational efficiency** | Can we optimise carrier partnerships? |


## 📦 Dataset

### Field Descriptions

| Field | Type | Description |
|---|---|---|
| `Shipment_ID` | String | Unique identifier — SH10000 to SH12000 |
| `Origin_Warehouse` | String | Dispatch location (10 warehouses) |
| `Destination` | String | Destination city (50+ US cities) |
| `Carrier` | String | Assigned shipping carrier (7 carriers) |
| `Shipment_Date` | Date | Date the package was dispatched |
| `Delivery_Date` | Date | Date delivered — blank if undelivered |
| `Weight_kg` | Float | Package weight in kilograms |
| `Cost` | Float | Shipping cost in USD |
| `Status` | String | Delivered / Delayed / In Transit / Lost / Returned |
| `Distance_miles` | Integer | Route distance in miles |
| `Transit_Days` | Integer | Number of days in transit |

### Dataset Summary

| Metric | Value |
|---|---|
| Total records | 2,000 shipments |
| Date range | 01 Jan 2023 – 31 Dec 2023 |
| Carriers | UPS, FedEx, DHL, USPS, LaserShip, OnTrac, Amazon Logistics |
| Warehouses | ATL, BOS, CHI, DEN, HOU, LA, MIA, NYC, SEA, SF |
| Destination cities | 50+ across the United States |
| Costed records | 1,959 (41 null cost values excluded from spend totals) |


## 🏗 Data Model Architecture

The Power Pivot Data Model connects two tables via one active and one inactive relationship.

```
┌─────────────────────┐                    ┌──────────────────────────┐
│   Calendar Table    │                    │     Shipments Table      │
│─────────────────────│                    │──────────────────────────│
│ Date          ──────┼──── ACTIVE ───────►│ Shipment_Date            │
│               ──────┼─── INACTIVE ──────►│ Delivery_Date            │
│ Year                │                    │ Shipment_ID              │
│ Month_Num           │                    │ Origin_Warehouse         │
│ Month_Name          │                    │ Destination              │
│ Quarter             │                    │ Carrier                  │
│ Week_Num            │                    │ Weight_kg                │
│ Day_Name            │                    │ Cost                     │
└─────────────────────┘                    │ Status                   │
                                           │ Distance_miles           │
    1 ──────────────────────────────────── │ Transit_Days             │
    (one date → many shipments)          * └──────────────────────────┘
```

## 📊 Analysis & Findings

### 💰 Cost Analysis 

| Question | Answer | Insight |
|---|---|---|
| Total shipping cost FY2023 | **$401,911.57** | Across 1,959 costed shipments |
| Average cost per shipment | **$205.16** | Median is $196.42 — right-skewed distribution |
| Most expensive carrier | **UPS** at $229.71/shipment | 26% above network average |
| Highest total cost warehouse | **Warehouse_LA** at $49,586.73 | Highest volume location (220 shipments) |


#### Carrier Delivery Performance — Full Ranking

| Rank | Carrier | Shipments | Delivered | Delivery Rate | Avg Transit | Lost |
|---|---|---|---|---|---|---|
| 🥇 1 | UPS | 256 | 221 | **86.33%** | 4.20 days | **0** |
| 🥈 2 | OnTrac | 299 | 248 | 82.94% | 4.12 days | 4 |
| 🥉 3 | FedEx | 295 | 244 | 82.71% | 4.30 days | 6 |
| 4 | DHL | 281 | 231 | 82.21% | 4.27 days | 1 |
| 5 | USPS | 292 | 239 | 81.85% | 4.05 days | 12 |
| 5 | LaserShip | 303 | 248 | 81.85% | 4.04 days | 10 |
| 7 | Amazon Logistics | 274 | 217 | **79.20%** | 4.32 days | **12** |

> 💡 **Critical finding:** UPS is the only carrier with zero lost packages across 256 shipments — the network reliability benchmark. Amazon Logistics holds the worst position on every performance metric simultaneously.

---

### 📍 Operational Insights

| Question | Answer | Insight |
|---|---|---|
| Total weight shipped | **60,369.6 kg** | 1 outlier at 5,404 kg inflates the mean |
| Average package weight | **Mean: 30.18 kg / Median: 20.7 kg** | 9.48 kg gap confirms right-skewed distribution |
| Top destination city | **Chicago — 154 shipments** | Followed by Phoenix & Minneapolis (148 each) |
| Highest volume carrier | **LaserShip — 303 shipments** | 15.2% of total network volume |
| Distance vs transit correlation | **r = 0.761** | Strong positive — r² = 0.579 |


#### Distance vs Transit Time — Correlation Analysis

The Pearson correlation coefficient between `Distance_miles` and `Transit_Days` was calculated using Excel's `=CORREL()` function:
```
r  = 0.761   →   Strong positive linear relationship
```

### 🚨 Problem Areas

| Question | Answer | Insight |
|---|---|---|
| Total lost or returned | **77 shipments** | 45 Lost + 32 Returned = ~$15,797 revenue at risk |
| Overall exception rate | **13.80%** | 276 of 2,000 shipments — Delayed, Lost, or Returned |
| Most lost packages — carrier | **Amazon Logistics & USPS tied at 12** | UPS has zero lost packages |
| Highest problem warehouse | **Warehouse_SF — 17.21%** | 2× the rate of the benchmark Warehouse_NYC |


> 💡 **Key observations:**
> - **October** is the strongest operational month — 86.1% delivery rate, lowest exception rate at 11.0%
> - **December** is the most concerning — 77.6% delivery rate and a 19.4% exception rate. Holiday volume is overwhelming carrier capacity
> - **August** records the highest monthly spend at $44,472 — significantly above the $33,493 monthly average
> - **February** has the lowest volume (139 shipments) yet a disproportionately high 15.8% exception rate

---

## 🎯 Strategic Recommendations

### 🔴 REC 01 — Audit Amazon Logistics & USPS Immediately `URGENT`

Both carriers record **12 lost packages each** — the joint-highest in the network. Amazon Logistics compounds this with the worst delivery rate (79.20%) and highest exception rate (17.88%) across 274 shipments.

**Recommended actions:**
- Mandate root-cause reports for all lost shipments within 30 days
- Restrict high-value shipments (> $400) to UPS or DHL exclusively
- Implement real-time tracking SLA requirements for all shipments over 1,000 miles

**Estimated impact:** Recovery of ~$4,924 in lost package value at current average cost

---

### 🟡 REC 02 — Renegotiate UPS Contract `HIGH PRIORITY`

UPS charges **$229.71/shipment** — the highest average in the network — yet delivers only a 4.48 percentage point advantage over OnTrac. The zero lost-package record is a genuine differentiator but does not justify the full cost premium without renegotiation.

**Recommended actions:**
- Use UPS's zero lost-package performance as leverage in volume discount negotiations
- Model a 5% cost reduction — estimated saving of ~$2,842 annually at current volume

---

### 🟡 REC 03 — Operational Review at Warehouse_SF and Warehouse_HOU `HIGH PRIORITY`

SF (17.21%) and HOU (16.98%) operate at more than **double the exception rate** of the network benchmark Warehouse_NYC (8.24%). Both warehouses use the same carrier pool — the variance points to operational process failures, not structural or carrier issues.

**Recommended actions:**
- Audit carrier mix allocation, packaging compliance, and last-mile handoff protocols at both locations
- Benchmark processes against Warehouse_NYC operations
- A 5-point improvement at both locations would eliminate ~72 annual exception shipments

---

### 🟢 REC 04 — Deploy Predictive ETA Model `MEDIUM PRIORITY`

The **r = 0.761** correlation between distance and transit time is statistically robust enough to support a real-time ETA prediction system without any additional tooling.

**Proposed formula:**
```
Estimated Transit Days = 1.3 + (Distance_miles × 0.002)
```

**Recommended actions:**
- Integrate formula into customer-facing order confirmation communications
- Use to set internal SLA thresholds segmented by distance band
- Run a 90-day pilot and measure reduction in SLA dispute volume

---

### 🟢 REC 05 — Reallocate Short-Haul Volume to USPS `MEDIUM PRIORITY`

USPS is the most cost-efficient carrier at **$0.1598/mile** — 17.6% cheaper than DHL ($0.1880/mile) — with a delivery rate comparable to DHL and LaserShip. It is currently the most underutilised carrier in the network.

**Recommended actions:**
- Route all shipments under 500 miles and 15 kg to USPS by default
- Monitor delivery rate impact over a 90-day pilot period before full rollout

**Estimated annual savings:** $5,600 – $8,000

---

## 📸 Dashboard Preview

### Page 1 — Executive Summary


<img width="1682" height="812" alt="Fastship P1" src="https://github.com/user-attachments/assets/6e649abc-1e32-4669-b0ff-a957a4f6c424" />


### Page 2 — Operations Deep Dive
<img width="1730" height="813" alt="FastShip P2" src="https://github.com/user-attachments/assets/342add5a-a175-4cd8-ba17-be99aefa73ca" />


```

## 👤 Author

**Babatunde**
