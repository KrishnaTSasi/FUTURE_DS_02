# FUTURE_DS_02

# 📊 Task: Social Media Campaign Performance Tracker

## 🔍 **Overview**

This task focuses on analyzing **Facebook Ads campaign performance data** to evaluate how well marketing efforts are performing. Work with **real-world style datasets** (CSV export from Facebook Ads Manager or Kaggle) and calculated important **digital marketing KPIs** such as:

* **CTR (Click-Through Rate):** How many people clicked on the ad after seeing it.
* **CPC (Cost per Click):** How much it costs for each click.
* **CPA (Cost per Acquisition):** Cost to gain one conversion (sign-up/purchase).
* **ROI (Return on Investment):** Profitability of the campaign.

By cleaning, analyzing and visualizing this data, can identify:

* Which **ads, age groups, or genders** respond best
* Whether the campaign was **profitable or not**
* What strategies should be **improved in the next campaign**

## 🎯 **Objective**

The goal is to simulate how marketing teams use **data analytics + dashboards** to measure ad effectiveness and optimize future campaigns.

## 🧰 **Tools Used**

* **Python (Pandas, Plotly)** → Data cleaning, KPI calculations, visualizations.


## 🚀 **Steps Performed**

1. **Data Preparation**

   * Load dataset (`CSV from Kaggle`).
   * Handle missing values in conversions.
   * Format dates.

2. **KPI Calculation**

   * CTR, CPC, CPA, Revenue, ROI.

3. **Exploratory Data Analysis (EDA)**

   * Which **age group/gender** performed best?
   * How did **spend impact revenue**?
   * ROI trends over time.

4. **Visualization**

   * Interactive **Plotly charts** (CTR by Age/Gender, ROI by Age, Spend vs Revenue, ROI Trend).

# 📝 Code Explanation

---

## 1. **Install & Import Libraries**

```python
!pip install pandas plotly openpyxl
```

* **pandas** → for reading, cleaning, and processing CSV/Excel data.
* **plotly** → for making **interactive dashboards/visualizations**.
* **openpyxl** → needed to export Excel files.

```python
import pandas as pd
import plotly.express as px
```

* Imports `pandas` and `plotly.express` into Python so we can use them.

---

## 2. **Load Dataset**

```python
df = pd.read_csv("facebook_ads_raw.csv")
df.head()
```

* Read the **raw Facebook Ads dataset** into a DataFrame (`df`).
* `.head()` shows the first 5 rows, just to verify it loaded correctly.

---

## 3. **Data Cleaning & KPI Calculation**

```python
df['approved_conversion'] = df['approved_conversion'].fillna(0)
df['total_conversion'] = df['total_conversion'].fillna(0)
```

* Some rows had **missing conversions**, so we fill them with `0`.

---

Now we calculate marketing **Key Performance Indicators (KPIs):**

```python
df['CTR (%)'] = (df['clicks'] / df['impressions']) * 100
```

* **CTR (Click Through Rate)** = `clicks ÷ impressions × 100`.
* Tells us **how effective ads are at generating clicks**.

```python
df['CPC'] = df['spent'] / df['clicks'].replace(0, 1)
```

* **CPC (Cost per Click)** = `Spend ÷ Clicks`.
* `.replace(0,1)` avoids division by zero errors.

```python
df['CPA'] = df['spent'] / df['total_conversion'].replace(0, 1)
```

* **CPA (Cost per Acquisition)** = `Spend ÷ Conversions`.
* Tells how much we pay for each conversion.

```python
df['Revenue'] = df['approved_conversion'] * 500
```

* Assumes **each approved conversion brings \$500 revenue**.
* You can change this value depending on your business.

```python
df['ROI (%)'] = ((df['Revenue'] - df['spent']) / df['spent']) * 100
```

* **ROI (Return on Investment)** = `(Revenue - Spend) ÷ Spend × 100`.
* Positive ROI = campaign profitable, Negative ROI = loss.

---

## 4. **Visualizations with Plotly**

These make interactive charts 📊.

```python
fig1 = px.bar(df, x="age", y="CTR (%)", color="gender", barmode="group", title="CTR by Age & Gender")
fig1.show()
```

* Bar chart of **CTR by Age & Gender**.
* Helps see which age groups respond best to ads.

```python
fig2 = px.bar(df, x="age", y="ROI (%)", title="ROI by Age Group")
fig2.show()
```

* ROI comparison across **different age groups**.

```python
fig3 = px.scatter(df, x="spent", y="Revenue", color="age", size="clicks", title="Spend vs Revenue")
fig3.show()
```

* Scatter plot → checks **correlation between ad spend and revenue**.
* Bubble size = clicks.

```python
df['reporting_start'] = pd.to_datetime(df['reporting_start'])
fig4 = px.line(df.groupby("reporting_start").mean().reset_index(),
               x="reporting_start", y="ROI (%)", title="ROI Trend Over Time")
fig4.show()
```

* Converts reporting date to datetime.
* Groups by **date** → plots ROI trend over time.
* Helps track if campaigns improve over time.

---

## 5. **Export Cleaned Data**

```python
df.to_csv("outputs/facebook_ads_cleaned.csv", index=False)
df.to_excel("outputs/facebook_ads_cleaned.xlsx", index=False)
print("✅ Data exported for Looker Studio / Power BI")
```

* Saves cleaned dataset (with KPIs added) into **CSV**.



