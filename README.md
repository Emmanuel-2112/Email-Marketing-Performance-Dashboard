# 📧 PROJECT
# Wanderlust Email Marketing Performance Analysis Using Power BI

# 🏖️ Company Overview
Wanderlust Adventures is a travel booking company that uses multi-stage email marketing campaigns to acquire new customers and retain existing ones. The business runs structured email sequences — Welcome, Personalized Offers, Promotional, and Thank You — targeting both new leads and existing customers across a five-year period (2019–2023), with the ultimate goal of converting email engagement into travel bookings.

# Prepared By
Taiwo Emmanuel

# Project Date
May–June 2026

# Abstract
This project delivers an end-to-end analysis of Wanderlust Adventures Email Marketing Campaigns to evaluate campaign performance, customer engagement, revenue generation, and customer journey flow. Using Power BI, raw transactional email data was transformed into an interactive, executive-ready dashboard that highlights key performance indicators, compares campaign effectiveness, tracks engagement trends over time, and maps the customer journey funnel — all to support data-driven marketing decisions.

# Background
Email marketing teams often struggle to measure what is truly working across a multi-campaign funnel. Open rates, click rates, and conversion metrics are collected but rarely synthesized into a single analytical view. This project was designed to consolidate Wanderlust Adventures' five years of email campaign data into one interactive dashboard that enables marketing managers to quickly identify which campaigns are driving revenue, how customers are moving through the engagement funnel, and when engagement is highest or declining.

# Aim of the Project
The project aimed at analyzing Wanderlust Adventures' email marketing campaign data and generating actionable insights that help improve campaign effectiveness, customer engagement, conversion performance, and overall email marketing ROI.

# Challenges Faced During the Project
- Raw data provided across two separate CSV files with different structures
- Date columns contained null values requiring careful flag-based transformation
- No pre-built measures or KPIs — all metrics calculated from scratch using DAX measures
- Funnel/Sankey dataset required multi-step filtering to isolate usable path data
- Needed to translate raw email event records into business-meaningful engagement metrics

# Project Objectives
The objective of this project is to:
- Measure and compare engagement performance (Open Rate, Click Rate, CTOR) across all 4 email campaigns
- Identify which campaigns are driving the most revenue and conversions
- Analyse customer journey flow from initial engagement to booking
- Track engagement trends across the 2019–2023 period
- Deliver insights and recommendations aligned with marketing strategy decision-making

---

# 🀄 DATA DESCRIPTION

# Data Source
Two CSV files from the Wanderlust Adventures email marketing system

# Data Size
- **1filtered_dataset.csv** — 42,099 records | 10 fields | Main campaign engagement data
- **2sankey_data.csv** — 1,374 records | 10 fields | Customer journey funnel/Sankey data

# Key Columns — 1filtered_dataset
- **Raw Fields:** index, name, account_number, email_name, sent_date, open_date, click_date, bounce_date, transaction_date, transaction_amount
- **Derived/Calculated Fields:** Campaign_Short, Opened (flag), Clicked (flag), Bounced (flag), Converted (flag), Revenue_Clean, Year, Month, Month_Name

# Key Columns — 2sankey_data
- **Fields:** Step 1 (Segment), Step 2 (Stage_2), Step 3 (Stage_3), Step 4 (Outcome), Size (Volume), Full_Path
- **Segments:** New Leads, Existing Customers
- **Outcomes:** Qualified/Booked, No Response, Discount paths

# Data Collection
Data represents historical email marketing interactions captured through Wanderlust Adventures' CRM system, including email send events, customer open and click activity, bounce records, and transaction conversions with booking values across five years.

# Data Characteristics
The dataset is:
- Structured, tabular event-level data
- Time-series capable (2019–2023)
- Contains categorical, numerical, date, and boolean-flag fields
- Includes customer-level identifiers enabling individual journey tracking
- Transaction amounts present only for converted records (bookings made)

---

# 👨‍💻 TOOLS USED
- Microsoft Power BI Desktop
- Power Query (ETL & transformation)
- DAX (Measures & Calculations)

---

# 🛡 METHODOLOGY

# 1. Data Cleaning & Transformation
All data cleaning was performed inside Power BI using Power Query:
- Loaded both CSV files directly into Power BI Desktop
- Renamed all columns to clean, readable business names
- Set correct data types for all fields (Date/Time, Decimal, Text, Whole Number)
- Created a **Campaign_Short** column using conditional logic to produce clean chart labels
- Created binary flag columns (Opened, Clicked, Bounced, Converted) from date fields — 1 if the event occurred, 0 if null
- Created a **Revenue_Clean** column replacing null transaction amounts with 0
- Extracted Year, Month, and Month Name from Sent_Date for time-trend analysis
- For the Sankey dataset: filtered to Year totals only, retained Min rows, removed unused columns, renamed stages, and built a **Full_Path** column combining all journey stages into readable labels

# 2. Data Modelling
A flat model was used given the single primary fact table structure:
- **Primary Table:** 1filtered_dataset — contains all email event records, flag columns, and revenue
- **Supporting Table:** 2sankey_data — contains customer journey funnel path data
- All DAX measures were created within the 1filtered_dataset table
- No complex relationships required — all campaign analysis driven by Campaign_Short as the grouping dimension

# 3. DAX Measures

## Total Emails Sent
```DAX
Total Emails Sent = COUNTROWS('1filtered_dataset')
```

## Total Opens
```DAX
Total Opens = SUM('1filtered_dataset'[Opened])
```

## Total Clicks
```DAX
Total Clicks = SUM('1filtered_dataset'[Clicked])
```

## Total Conversions
```DAX
Total Conversions = SUM('1filtered_dataset'[Converted])
```

## Total Revenue
```DAX
Total Revenue = SUM('1filtered_dataset'[Revenue])
```

## Open Rate
```DAX
Open Rate = 
DIVIDE(
    COUNT('1filtered_dataset'[open_date]),
    COUNT('1filtered_dataset'[sent_date]),
    0
)
```

## Click Rate
```DAX
Click Rate = 
DIVIDE(
    COUNT('1filtered_dataset'[click_date]),
    COUNT('1filtered_dataset'[sent_date]),
    0
)
```

## Click-to-Open Rate (CTOR)
```DAX
CTOR = 
DIVIDE(
    COUNT('1filtered_dataset'[click_date]),
    COUNT('1filtered_dataset'[open_date]),
    0
)
```

## Conversion Rate
```DAX
Conversion Rate = 
DIVIDE(
    COUNT('1filtered_dataset'[transaction_date]),
    COUNT('1filtered_dataset'[click_date]),
    0
)
```

## Bounce Rate
```DAXBounce Rate = 
DIVIDE(
    COUNT('1filtered_dataset'[bounce_date]),
    COUNT('1filtered_dataset'[sent_date]),
    0
)
```

## Average Booking Value
```DAX
Avg Booking Value = 
AVERAGE('1filtered_dataset'[Revenue])
```

## Revenue per Email
```DAX
Revenue per Email = DIVIDE([Total Revenue], [Total Emails Sent], 0)
```

---

# ✅️ RECOMMENDED ANALYSIS

# Recommended Analysis Questions
- Which email campaign has the highest open rate and click rate?
- Which campaign is generating the most revenue?
- How are customers flowing through the journey funnel from engagement to booking?
- Are there engagement trends over the 2019–2023 period?

# Definitions & Dashboard Visuals

## 1. Which email campaign has the highest open rate and click rate?
- **Definition:** This analysis evaluates and compares all four email campaigns based on recipient engagement — measuring how many people opened the email, how many clicked through, and the ratio of clicks to opens (CTOR). The goal is to identify which campaign messaging and targeting is resonating most with recipients.
  
- **Key Metrics Used:** Open Rate, Click Rate, CTOR, Total Emails Sent
  
- **Dashboard Visuals That Explain This:**
  - KPI Cards: Open Rate, Click Rate, CTOR (overall summary)
  - Pie Chart: Open Rate per Campaign (share of total engagement)
  - Bar Chart: Click Rate per Campaign (ranked by click performance)
  - Clustered Bar Chart: Engagement Rates per Campaign (Open Rate, Click Rate, CTOR side by side)
  - Campaign Metric Summary Table: All campaigns compared across every engagement metric
    
- **Business Value:** This analysis helps marketing teams understand which email type and messaging style drives the most engagement, enabling smarter subject line, timing, and content decisions for future campaigns.

## 2. Which campaign is generating the most revenue?
- **Definition:** This analysis evaluates revenue contribution and conversion efficiency across all four campaigns to identify which campaigns are not just engaging customers but driving actual bookings. It distinguishes between high-open/low-convert campaigns and those that move subscribers to purchase.
  
- **Key Metrics Used:** Total Revenue, Conversion Rate, Average Order Value, Total Conversions
  
- **Dashboard Visuals That Explain This:**
  - KPI Cards: Total Revenue ($878.19K), Avg Booking Value ($987.84), Conversions (902)
  - Campaign Metric Summary Table: Revenue and Conversion Rate per campaign
    
- **Business Value:** This insight enables the marketing team to prioritize high-revenue campaigns, refine low-converting campaigns, and align email strategy with direct booking revenue outcomes.

## 3. How are customers flowing through the journey funnel?
- **Definition:** This analysis maps the customer journey from initial segment (New Leads vs. Existing Customers) through engagement stages (Provided Interests, Discount, Skipped Stage) to final outcomes (Qualified/Booked, No Response). It reveals where customers are dropping off and which paths lead to conversions.
  
- **Key Metrics Used:** Volume (count of customers per journey path), Full_Path labels
  
- **Dashboard Visuals That Explain This:**
  - Sankey/Funnel Chart: Customer Journey Flow (from Segment → Engagement Stage → Outcome)
    
- **Business Value:** Understanding the funnel reveals which customer segments and engagement pathways are most likely to convert to bookings, and where the greatest drop-off occurs — enabling targeted re-engagement strategies.

## 4. Are there engagement trends over the 2019–2023 period?
- **Definition:** This analysis examines how email engagement — opens and clicks — has changed year by year and month by month across the five-year campaign window. It identifies periods of high activity, seasonal patterns, and any year-over-year decline or growth.
  
- **Key Metrics Used:** Open Rate, Click Rate, Total Emails Sent, Year, Month
  
- **Dashboard Visuals That Explain This:**
  - Line Chart: Engagement Over Time (Open Rate and Click Rate trended by month/year)
    
- **Business Value:** Time-trend analysis helps the marketing team plan campaign timing, anticipate seasonal peaks in travel interest, and assess whether overall email program health is improving or declining.

---

# 🎨 DASHBOARD OVERVIEW

- Dashboard 1
<img width="1375" height="755" alt="Dashboard 1" src="https://github.com/user-attachments/assets/30430f50-28ca-45ce-84a0-f4e3982cafcd" />

# Dashboard 1 :
# Executive Summary (KPI Overview)
- Total Revenue ($878.19K)
- Total Emails Sent (42K)
- Average Booking Value ($987.84)
- Overall Open Rate (82.74%)
- Overall Click Rate (20.65%)
- CTOR - Click To Open Rate (24.96%)
- Total Conversions (902)
- Bounce Rate (0.0%)
- Open Rate per Campaign (Pie Chart)
- Click Rate per Campaign (Bar Chart)
  
# Campaign Performance Analysis
- Engagement Rates per Campaign — Open Rate, Click Rate, CTOR (Clustered Bar Chart)
- Campaign Metric Summary Table (all campaigns × all metrics)
- Year slicer (2019, 2020, 2021, 2022, 2023, Select All)

------
- Dashboard 2
<img width="1368" height="753" alt="Dashboard 2" src="https://github.com/user-attachments/assets/10cac854-d388-4550-9a0b-cff157ce8391" />

# Dashboard 2 : Revenue Analysis (KPI Overview)
- Total Revenue ($878.19K)
- Average Booking Value ($987.84)
- Revenue Per Email ($20.86)
- Total Revenue Distribution By Campaign (Donut Chart)
- Revenue By Year And Campaign (Clustered Column Chart)
- Conversion Per Campaign (Stacked Column Chart)
- Average Booking Value By Campaign (Clustered bar Chart)
- Total Revenue & Revenue Per Emails By Campaign (Line & Clustered Column Chart)

----
- Dashboard 3
<img width="1370" height="753" alt="Dashboard 3" src="https://github.com/user-attachments/assets/d869f4ec-323a-4177-ab9d-d082c533526d" />

# Dashboard 3 : 
# Customer Journey Funnel (KPI Overview)
- Total Qualified (87K)
- Total lost (105K)
- Ratio Total Qualified To Total Lost (0.84)
- Customer Journey Funnel (Annual)
- Funnel Path Details (Table)
  
# Customer Engagement Over Time
- Open Rate Vs Click Rate Over Time (2019 - 2023)
- Monthly Send Volume By Campaign (Line Chart)

---

# 🔎 KEY INSIGHTS
- **Email 2 – Personalized Offers** delivered the highest open rate (87.7%), highest click rate (28.3%), highest CTOR (32.3%), and the most revenue ($531,259.50) — making it the single best-performing campaign across every metric
- **Email 1 – Welcome** had the second-highest open rate (85.0%) and a strong click rate (23.5%), suggesting the Welcome sequence effectively activates new subscribers
- **Email 3 – Promotional** had the lowest click rate (8.7%) and lowest CTOR (11.3%) despite a competitive open rate (77.3%), indicating the promotional offer content is not compelling enough to drive clicks after opens
- **Email 4 – Thank You** had the lowest volume (6,270 emails) yet achieved a 24.2% CTOR — indicating highly engaged recipients, though its revenue contribution ($45,676.50) was the lowest overall
- Total revenue of **$878.19K** was generated from just **889 conversions** (out of 42,099 emails sent), pointing to a high average booking value of **$987.84** per conversion
- Bounce rate across all campaigns is **0.0%**, reflecting a clean and well-maintained email list

# 📄 Recommendations
- Marketing investment and messaging resources should be prioritized toward the Personalized Offers campaign format (Email 2), as it consistently outperformed all other campaigns in opens, clicks, CTOR, and revenue — suggesting that personalization and tailored content is the strongest driver of booking conversions.
- The Promotional campaign (Email 3) requires a content and offer redesign. Its open rate of 77.3% indicates recipients are interested enough to open but the click rate of 8.7% reveals the offer or call-to-action inside is not compelling — A/B testing of discount structures, imagery, and CTA button copy is recommended.
- The Welcome campaign (Email 1) should be extended or supplemented with a follow-up sequence, as it achieves the second-best open rate and effectively activates new subscribers — a second touch within the Welcome journey could capture more early conversions before subscribers reach the promotional stage.
- The Thank You campaign (Email 4) should be tested with an upsell or referral component. Its high CTOR (24.2%) signals post-booking customers are engaged and likely to respond to loyalty rewards, referral incentives, or next-trip offers — this represents an underutilised revenue opportunity.
- Funnel analysis should be used to build targeted re-engagement sequences specifically for the "No Response" customer segments identified in the journey flow, as these represent the largest drop-off pool and are addressable with win-back messaging.
- Given the zero bounce rate across all 42K emails, the team should maintain current list hygiene standards and continue regular suppression list updates to preserve deliverability as send volumes grow.

# ⚪️ Conclusion
This project demonstrates an end-to-end analytical workflow — from raw CSV email event data to executive-level marketing insights. By combining Power Query data transformation, DAX-driven metric calculations, and clear business storytelling across campaign performance, revenue analysis, and customer journey visuals, the dashboard provides Wanderlust Adventures' marketing leadership with the actionable intelligence needed to improve campaign ROI, optimise the customer journey funnel, and drive sustainable booking revenue growth.

---

- Detailed Report And Documentation  
*(Links to be added after upload)*

