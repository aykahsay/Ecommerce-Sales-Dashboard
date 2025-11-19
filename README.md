# 🛒 E-Commerce Sales Dashboard (ETL | SQL Server | Power BI)

> A complete end-to-end **data analytics pipeline** integrating **ETL processes**, **SQL Server data management**, and **Power BI visualization** to analyze e-commerce sales performance.

---

## 📸 Dashboard Preview

![Dashboard](https://github.com/user-attachments/assets/095c3e8f-10c4-4e3c-84bc-a756bbc103f1)

---

## 📊 Project Overview
This project demonstrates how to build a **data-driven business intelligence system** from raw sales data.  

It follows a structured ETL pipeline — **Extract**, **Transform**, and **Load** — using **SQL Server** for data processing and **Power BI** for visual analytics.  

The resulting dashboard provides key insights into:
- 🧾 Year-to-Date (YTD) sales and profit
- 🌍 Regional and product category performance
- 📦 Shipping and delivery efficiency
- 📈 Profitability trends over time

---

## 🎯 Objectives
- Develop an **ETL pipeline** to clean and transform raw e-commerce data.  
- Store and process data efficiently using **SQL Server**.  
- Build an **interactive Power BI dashboard** for visualization.  
- Deliver insights into **sales**, **profit**, and **regional trends**.  

---

## 🧩 Data Sources

### 1. `ecommerce_data`
Main fact table containing transaction-level sales and customer details.

| Column | Description |
|--------|--------------|
| `order_id` | Unique order identifier |
| `order_date` | Date of transaction |
| `category_name` | Product category |
| `customer_id` | Customer unique ID |
| `customer_region` | Region of the customer |
| `customer_state` | State for mapping |
| `sales` | Total sales amount |
| `profit` | Profit earned |
| `quantity` | Quantity sold |
| `days_for_shipment_real` | Actual shipment days |
| `days_for_shipment_scheduled` | Scheduled shipment days |
| `delivery_status` | Shipment status |

---

### 2. `us_state_long_lat_codes`
Dimension table containing U.S. states and their geographic coordinates.

| Column | Description |
|--------|--------------|
| `state` | State name |
| `name` | State code |
| `latitude` | Latitude |
| `longitude` | Longitude |

---

### 3. `Calendar`
A generated date dimension used for **time intelligence functions** (YTD, YOY, MTD).  
Some fields such as `Month`, `Month Number`, and `Year` are **calculated during the Transformation step** using Power Query or DAX.

| Column | Description |
|--------|--------------|
| `Date` | Calendar date |
| `Month` | Month name *(calculated from `Date`)* |
| `Month Number` | Month number *(calculated from `Date`)* |
| `Year` | Year *(calculated from `Date`)* |

**Example (DAX Formula):**
```DAX
Calendar = 
ADDCOLUMNS(
    CALENDAR(DATE(2020,1,1), DATE(2025,12,31)),
    "Year", YEAR([Date]),
    "Month", FORMAT([Date], "MMMM"),
    "Month Number", MONTH([Date])
)
````

---

## ⚙️ ETL Process
<img width="1256" height="752" alt="image" src="https://github.com/user-attachments/assets/28bb8375-4fcb-4dcf-9d82-0d10482f0a7e" />

### 🔹 Extract

* Source files imported into **SQL Server** (`ecommerce_data`, `us_state_long_lat_codes`).
* Data ingested from CSV or Excel using the **Import Wizard** or **BULK INSERT**.

### 🔹 Transform

* Data cleaned, standardized, and enhanced with calculated metrics:

  * `Profit Margin` = `Profit / Sales`
  * `Days Difference` = `days_for_shipment_real - days_for_shipment_scheduled`
  * `YTD Sales` and `YTD Profit`
  * Time attributes (`Month`, `Year`) created dynamically

**Example SQL Query:**

```sql
SELECT 
    category_name,
    SUM(sales) AS YTD_Sales,
    SUM(profit) AS YTD_Profit,
    (SUM(profit)/SUM(sales))*100 AS Profit_Margin
FROM ecommerce_data
WHERE YEAR(order_date) = 2025
GROUP BY category_name;
```

### 🔹 Load

* Transformed data stored back in SQL Server (data warehouse schema).
* Power BI connects via:

  ```
  Home → Get Data → SQL Server → Enter Server Name & Database → Select Tables → Load
  ```

---

## 🔗 Data Model Design

*(Insert your Power BI model relationship diagram below)*

> 📍 **Suggested filename:** `images/data_model.png`
![Model Design](https://github.com/user-attachments/assets/37ca4e13-5252-4249-a56c-4f7d37018c34)



The model follows a **Star Schema**, ensuring efficient querying and DAX calculations.

```
us_state_long_lat_codes (1) ───▶ (∞) ecommerce_data (∞) ◀─── (1) Calendar
```

* **Fact Table:** `ecommerce_data`
* **Dimension Tables:** `us_state_long_lat_codes`, `Calendar`

---

## 📈 Power BI Dashboard Features

### **Key Metrics**

* 💰 **YTD Sales:** $11.53M
* 💹 **YTD Profit:** $1.34M
* 📦 **YTD Quantity:** 107.2K units
* 📊 **Profit Margin:** 11.58%

### **Visual Elements**

* KPI Cards — display major performance indicators
* Bar Charts — Top 5 and Bottom 5 Products by Sales
* Donut Charts — Sales by Region & Category
* Map Visual — Geographic sales by U.S. states
* Tables — YTD vs PYTD (Year-Over-Year comparison)

---

## 💡 Business Insights

* **East Region** leads with 32.22% of total sales.
* **Office Supplies** category records the highest sales ($6.92M).
* Profit Margin increased by **+5.37% YoY**, despite a small drop in total sales volume.
* Minor delays in shipment impact delivery performance metrics.

---

## 🛠️ Tools & Technologies

| Tool            | Purpose                                |
| --------------- | -------------------------------------- |
| **SQL Server**  | Data storage, ETL, and transformations |
| **Power BI**    | Dashboard design and visualization     |
| **T-SQL**       | Data querying and aggregation          |
| **Excel/CSV**   | Source data input                      |
| **Power Query** | Data cleaning and model setup          |

---

## 🚀 How to Use

1. **Clone the repository**

   ```bash
   git clone https://github.com/<your-username>/Ecommerce-Sales-Dashboard.git
   ```

2. **Set up SQL Server**

   * Run the provided SQL scripts to create tables.
   * Import the raw data files into SQL Server.

3. **Open Power BI Desktop**

   * Connect Power BI to your SQL Server database.
   * Load and refresh the visuals.

4. **Explore the dashboard**

   * Interact with filters (Category, Region, Year).
   * Analyze YTD and YOY performance metrics.

---

## 📅 Future Enhancements

* Integrate **SSIS or Azure Data Factory** for automated ETL.
* Add **forecasting models** for predictive sales analytics.
* Include **customer segmentation** and **RFM analysis**.
* Build **real-time dashboards** using DirectQuery.

---
