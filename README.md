# Forgith Pharmaceutical Executive Dashboard

A 4-page Power BI report analyzing Forgith Pharmaceutical's sales, marketing, and commercial team performance from January 2022 to 2025.

---

## Overview

This dashboard was built to give Forgith Pharmaceutical's executives a single, interactive view of company-wide commercial performance - covering overall sales health, distributor and target performance, sales team productivity, and geographic revenue distribution. It is designed for stakeholders who need to move from a high-level company snapshot down to team- and city-level detail without switching tools.

**Key company-wide metrics:**
- Total Revenue: **$11.12bn**
- Total Quantity Sold: **27M units**
- YoY Growth: **31.44%**
- Total Sales Reps: **13**
- Total Cities Covered: **549**

---

## Tools & Process

The dashboard was built using a three-stage workflow, moving raw data from documentation through cleaning to modelling and analysis:

### 1. Data Dictionary — Microsoft Excel
Before touching Power BI, a full data dictionary was built in Excel to document the structure of the dataset ahead of modelling. It defines each table, its columns, and the business meaning of every field.

![Forgith Data Dictionary](images/00_data_dictionary.png)

The dictionary covers:
- **Dim_Location** - unique city locations with latitude/longitude coordinates
- **Dim_Subchannel** - subchannels and their parent sales channel
- **Dim_Channel** - the two main sales channels (Hospital, Pharmacy)
- **Dim_Product** - product name, product class, and price
- **Dim_Employee** - employee name, manager, and team
- **Sales 2022** and **Sales 2023–2025** - the fact tables recording monthly sales transactions (Sales ID, MonthYear, Distributor, Customer, Location, Subchannel, Product, Quantity)

This dictionary served as the reference point for consistent naming and relationships throughout the build.

### 2. Data Cleaning — Power Query Editor
Raw data was cleaned and shaped in the Power Query Editor before being loaded into the model, including standardizing column formats, handling inconsistent entries, and preparing the separate yearly sales tables (2022, and 2023–2025) for combination and use in the star schema.

### 3. Data Modelling — Power BI
A star schema was built in Power BI, connecting the fact tables (Sales 2022, Sales 2023–2025) to the dimension tables (Location, Subchannel, Channel, Product, Employee) defined in the data dictionary. This structure supports the cross-filtering used throughout the report (e.g., filtering by Channel, City, or Year across multiple visuals).

### 4. DAX Measures
Custom DAX measures were created to power the KPI cards and visuals across all four pages, including:
- Total Revenue, Total Quantity Sold, Total Target, Qty Variance to Target
- Target Achievement %
- YoY Growth %
- Revenue by Distributor, Product Class, Channel, and City

---

## Report Pages

### Page 1 — Executive Overview

![Executive Overview](images/01_executive_overview.png)

The landing page and company-wide summary.
- KPI cards: Total Revenue, Total Quantity Sold, Qty Variance to Target, YoY Growth %
- Yearly Sales Trend (monthly, Jan 2022–2025)
- Total Revenue by Product Class
- Top 10 Selling Products
- Total Revenue by Channel (Pharmacy vs. Hospital)
- Slicers: Channel, Year

**Key insights surfaced:** Forgith grew 31.44% YoY; sales volume finished ~6M units above target; Antibiotics leads by product class; Pharmacy contributes the larger revenue share (52.9%) over Hospital (47.1%); monthly sales show notable fluctuation worth investigating for demand/ordering patterns.

### Page 2 - Sales Performance

![Sales Performance](images/02_sales_performance.png)

A deeper look at revenue vs. target and distributor performance.
- KPI cards: Total Revenue, Total Target, Target Achievement %, YoY Growth %
- Actual Quantity Sold vs. Target Trend (dual-axis line/area)
- Total Revenue by Distributor
- Top 5 Distributors by Sales Volume
- Distributor Performance Table (Revenue, Revenue YTD, Target Achievement %)
- Slicers: City, Year

**Key insights surfaced:** Forgith hit 131.70% of its sales target, exceeding it by 31.70%; the top 5 distributors (led by Gerlach LLC) account for a substantial share of total volume, underlining their importance to overall commercial performance.

### Page 3 — Sales Team Performance

![Sales Team Performance](images/03_sales_team_performance.png)

Performance of Forgith's commercial teams and individual sales reps.
- KPI cards: Total Revenue, Total Sales Reps
- Top 5 Sales Reps by Target Achievement
- Total Revenue vs. Total Target by Team (Alfa, Bravo, Charlie, Delta)
- Manager and Team Performance Table (Total Revenue, Revenue YTD, Target Achievement %, Revenue Growth)
- Slicers: City, Channel, Year

**Key insights surfaced:** Team Delta (led by Britanny Bold) is the clear leader with ~$3.43bn in revenue and 41% target achievement, while the other three teams are tightly clustered around $2.4–2.7bn at ~30%; the company's overall 131.7% achievement is concentrated in one team, indicating a dependency risk; just 13 reps support $11.12bn in revenue, reflecting high productivity per rep.

### Page 4 — Geographic Performance Analysis

![Geographic Performance Analysis](images/04_geographic_analysis.png)

Revenue distribution across markets.
- KPI cards: Total Revenue, Total Number of Cities
- Geographic Revenue Distribution map (bubble map by Channel — Hospital/Pharmacy)
- Top 10 Cities by Revenue
- Top 10 Cities by Target Achievement %
- Slicers: Channel, Year

**Key insights surfaced:** Revenue is heavily concentrated in a handful of German and neighboring cities despite 549 total locations covered; the top 10 cities follow a classic Pareto pattern with a steep drop-off after the leaders; target achievement is more evenly spread than absolute revenue, with smaller cities hitting their targets more consistently while a few large markets drive most of the $11.12bn.

---

## Navigation

All four pages share a consistent left-hand navigation panel with buttons to jump between Executive Overview, Sales Performance, Sales Team Performance, and Geographic Analysis, along with a shared color theme (maroon and navy) and icon panel for quick visual identification of the report.

---

## Data Model Structure (from Data Dictionary)

| Table | Type | Row Level Definition |
|---|---|---|
| Dim_Location | Dimension | One row = one city location (LocationID, City, Latitude, Longitude) |
| Dim_Subchannel | Dimension | One row = one subchannel and its parent channel |
| Dim_Channel | Dimension | One row = one main channel (Hospital or Pharmacy) |
| Dim_Product | Dimension | One row = one product (ID, Name, Class, Price) |
| Dim_Employee | Dimension | One row = one employee (ID, Name, Manager, Team) |
| Sales 2022 | Fact | One row = one monthly sales transaction in 2022 |
| Sales 2023–2025 | Fact | One row = one monthly sales transaction, 2023–2025 |

---

## Recommendations

Based on the patterns surfaced across all four pages, the following actions are recommended:

1. **Reduce dependency risk on Team Delta.** Team Delta drives a disproportionate share of the company's 131.7% target over-achievement (41% vs. ~30% for the other three teams). Management should study Delta's approach — territory allocation, distributor relationships, product mix, and pilot replicating it with Alfa, Bravo, and Charlie to spread performance more evenly and de-risk future target-setting.

2. **Investigate the cause of monthly sales volatility.** The Yearly Sales Trend shows sharp spikes and dips rather than a smooth growth curve. This should be cross-checked against distributor ordering cycles, promotional calendars, and stock-outs to determine whether the volatility is demand-driven or a symptom of inconsistent distributor ordering behavior that could be smoothed with better forecasting.

3. **Formalize and protect top distributor relationships, while reducing reliance on them.** Gerlach LLC and the other top-5 distributors account for the bulk of sales volume. These relationships should be safeguarded with clear service agreements, while a parallel effort develops mid-tier distributors to reduce concentration risk if a top distributor's volume drops.

4. **Develop the long tail of the 549-city footprint.** Revenue is heavily concentrated in the top 10 cities (led by Butzbach), which follow a steep Pareto drop-off. Since smaller cities show more consistent target achievement, there is an opportunity to grow absolute revenue in these markets, they are performing well relative to target but from a smaller base, suggesting room to scale rather than just retain.

5. **Reassess Antibiotics' leading position for portfolio risk.** Antibiotics is the top product class by revenue; while positive for current performance, over-reliance on a single class exposes the business to regulatory, pricing, or competitive shocks specific to that category. A periodic review of product class mix alongside Antiseptics, Mood Stabilizers, and other classes is recommended to guide future investment and marketing spend.

6. **Use the Pharmacy/Hospital channel split to guide channel-specific strategy.** With Pharmacy at 52.9% and Hospital at 47.1%, both channels are commercially significant. Channel-specific promotional and distribution strategies (rather than a one-size-fits-all approach) could help grow the smaller Hospital channel while protecting the Pharmacy lead.

7. **Recognize and study top individual performers.** Sheila Stones and the other top 5 reps by target achievement should be reviewed for best practices (client relationships, territory approach, product focus) that could be shared across the 13-person sales team, especially given how few reps are driving the full $11.12bn in revenue.
