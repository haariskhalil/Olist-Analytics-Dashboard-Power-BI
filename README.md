# 📊 Olist E-Commerce Analytics Dashboard

A comprehensive Power BI analysis of the Brazilian E-Commerce dataset (Olist). This project transforms raw relational data into actionable business insights, focusing on revenue trends, delivery performance, and customer behavior.

## 🖼️ Dashboard Preview

<img width="1341" height="753" alt="image" src="https://github.com/user-attachments/assets/bec9a428-f67c-4260-9419-7c506d18ebe4" />


## 🎯 Project Objective
The goal of this project was to analyze 100k+ orders to answer three key business questions:
1.  **Sales Performance:** How is revenue trending over time?
2.  **Product Strategy:** Which categories drive the majority of our revenue (Pareto Principle)?
3.  **Logistics:** How does delivery time vary by region, and does it impact sales?

## 🛠️ Tech Stack
* **Tool:** Microsoft Power BI Desktop
* **Languages:** DAX (Data Analysis Expressions), M (Power Query)
* **Data Modeling:** Star Schema (Fact/Dimension model)

---

## 🔄 ETL & Data Modeling Workflow

### 1. Data Ingestion & Transformation (Power Query)
* **Loaded Tables:** Imported raw CSV files (`orders`, `items`, `products`, `payments`, `customers`, `geolocation`).
* **Star Schema Design:**
    * **Fact Tables:** `Fact_Orders`, `Fact_Items`, `Fact_Reviews`.
    * **Dimension Tables:** `Dim_Customer`, `Dim_Product`, `Dim_Date`, `Dim_Geo`.
* **Key Transformations:**
    * **Translation Merge:** Merged the Portuguese product categories with the English translation table to make the report international-friendly.
    * **Date Type Fix:** Solved a critical "Time vs. Date" granularity mismatch by creating a dedicated date key, enabling accurate relationships between `Fact_Orders` and `Dim_Date`.
    * **Geolocation Fix:** Created a custom `MapLocation` column (`[State] & ", Brazil"`) to resolve Bing Maps ambiguity between US and Brazilian state codes (e.g., "AL" = Alagoas, not Alabama).

### 2. Calculated Columns & DAX Measures
Several custom calculations were engineered to drive the visuals:

* **KPI Measures:**
    * `Total Revenue = SUM(Fact_Items[price])`
    * `Total Orders = COUNTROWS(Fact_Orders)`
    * `Avg Delivery Days = AVERAGE(Fact_Orders[delivery_days])`

* **Pareto Analysis (80/20 Rule):**
    * Created a dynamic `Pareto % Correct` measure using `ALLSELECTED` to ensure the cumulative percentage respects "Top N" filters.
    ```dax
    Pareto % = DIVIDE(
        CALCULATE([Total Revenue], FILTER(ALLSELECTED(Dim_Product), [Total Revenue] >= EARLIER_VALUE)),
        CALCULATE([Total Revenue], ALLSELECTED(Dim_Product))
    )
    ```

* **Time Intelligence (Heatmap):**
    * **`TimeOfDay`:** A calculated column binning orders into *Morning, Afternoon, Evening, Night* logic.
    * **`DaySort`:** A helper column to force standard chronological sorting (Monday=1) rather than alphabetical sorting (Friday=1).

---

## 📈 Visual Insights Explained

### 1. Revenue Trend (Area Chart)
* **Visual:** Total Revenue by Year/Month.
* **Insight:** Validates the company's growth trajectory. The chart is sorted chronologically to reveal seasonal peaks and long-term upward trends.

  <img width="682" height="308" alt="Screenshot 2026-01-28 214929" src="https://github.com/user-attachments/assets/2ac0aac9-fb8f-4d4d-b905-731da8780e3a" />


### 2. Product Pareto Chart (80/20 Analysis)
* **Visual:** Combo Chart (Bar + Line).
* **Insight:** Identifies the "Vital Few." The line shows that a small percentage of categories (like Bed Bath & Table, Health Beauty) drive the vast majority of revenue. This signals where marketing spend should be focused.

    <img width="662" height="310" alt="Screenshot 2026-01-28 214954" src="https://github.com/user-attachments/assets/41b79324-9ead-4741-b158-9d49d4ecf86c" />


### 3. Geographic Sales Map
* **Visual:** Filled Map.
* **Insight:** Visualizes the concentration of sales in the Southeast (Sao Paulo, Rio) vs. the remote North. It highlights market penetration gaps.

    <img width="317" height="297" alt="Screenshot 2026-01-28 215020" src="https://github.com/user-attachments/assets/a5e3dad4-e345-4c52-acc6-229cd1ed0bc2" />


### 4. Delivery Performance (Bar Chart)
* **Visual:** Average Delivery Days by State (Sorted Descending).
* **Insight:** Highlights logistics bottlenecks. States like Roraima (RR) and Amapá (AP) average 25+ days for delivery, which correlates with lower sales volume in those regions.
   
  <img width="516" height="297" alt="Screenshot 2026-01-28 215008" src="https://github.com/user-attachments/assets/314da934-6fef-44a9-8842-b0960283bee2" />

### 5. Peak Shopping Heatmap
* **Visual:** Matrix with Conditional Formatting.
* **Insight:** Maps `Day of Week` vs. `Time of Day`.
* **Use Case:** Operational staffing. If the heatmap glows dark blue on "Monday Afternoons," customer support agents should be doubled during that window.
 
    <img width="317" height="297" alt="Screenshot 2026-01-28 215020" src="https://github.com/user-attachments/assets/09009172-ac2e-4b16-bc9b-7679bf6e8c2c" />

---

## 🚀 How to Run This Project
1.  Install [Microsoft Power BI Desktop](https://powerbi.microsoft.com/desktop/).
2.  Clone this repository.
3.  Open the `.pbix` file.
4.  Explore the interactive filters (Year Slicer, Interactive Charts).

---

**Author:** Haaris Khalil <br>
**Dataset Source:** [Olist Brazilian E-Commerce Dataset](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)
