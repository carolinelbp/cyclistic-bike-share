# 🚲 Unlocking Member Growth: Cyclistic Bike Share User Analysis 🚲

# Introduction
### 📈 Business Task

Identify how casual riders and annual members use fictional bike share service Cyclistic differently. Recommend marketing strategies to convert casual riders into riders with annual memberships.
<br><br>

### 👩‍💼 Target Audience

The key stakeholders are:
- The Cyclistic Executive team
- The Director of Marketing
- The Marketing Analytics team
<br><br>

### 🚀 Why It Matters

Memberships are crucial for Cyclistic’s long-term revenue and user loyalty. By understanding the behaviors and habits of casual riders, we can tailor strategies to encourage more conversions.

---

# Data Overview
### 🪄 Dataset

12 months of 'Cyclistic' trip data (anonymised data from a Chicago ride share service for 2024).
<br><br>

### 📊 Platforms Used

1. Initial data clean = Excel
2. Process and analyze = SQL
3. Visualize and present findings = Excel & Tableau. 
<br><br>

### 🔎 Key Variables Used

- ride_id
- rideable_type (classic bike / electric bike / electric scooter)
- started_at, ended_at (date and time)
- start_station_name, end_station_name
- start_lat, start_lng, end_lat, end_lng (the exact coordinates)
- member_casual (the type of user)
- ride_length
- text_day_of_week
<br><br>

### 🧼 Data Cleaning

- Added calculated columns. 
- Fixed cell format issues. 
- Removed invalid records. 
- Moved records into the correct month's workbook. 
- Checked for duplicate entries. 
<br><br>

---

# Exploratory Analysis

Before honing in on member vs. casual rider comparisons, I explored general usage trends: 
- The busiest month for rides is September, but July has the longest total ride time.
- Sundays have the longest rides, while the most rides overall are taken at 6pm.
- Riders use electric bikes slightly more often, but spend longer on classic bike rides. 
- Only 3.3% of rides start and end at the same station — round trips are rare.

---

# In-Depth Analysis & Modeling

Once I better understood the data, I focused on the most impactful differences between casual users and members. The insights below shaped my targeted marketing recommendations. 

## 1. Timing Patterns – When Do Users Ride?

Members take around 61% of all rides, suggesting a strong base of frequent users who enjoy a low per-ride cost. But casual riders show distinct timing behaviors compared to members, which presents targeted marketing opportunities. 

### Key Findings

**Seasonality**: Casual riders peak heavily in summer, while members stay more consistent throughout the year. 

![Casual-Rides-Percentage-Per-Month](images/casual-rides-percentage-per-month.png)

**Marketing Action*: Offer a three-month summer pass or free membership trial to capture casual rider interest at its peak. 


**Day of Week**: Casual rider usage surges on weekends; members are more consistent across the week.

![Average-Rides-Per-Day-of-Week-Per-User](images/average-rides-per-day-of-week-per-user.png)

**Marketing Action*: Create a weekend-only membership tier for casual riders. 


**Time of Day**: Members ride during peak commuting hours; casual riders increase throughout the day before dropping off after 6pm.

![Ride-Count-Per-Time-Of-Day-Per-User](images/ride-count-per-time-of-day-per-user.png)

**Marketing Action*: Shift marketing language and imagery toward how membership can save money on long rides and evening outings, not just commutes. 


**Example Query**

```sql 

-- Calculates the % of all monthly rides taken by casual users

SELECT
	DATE_TRUNC('month', t.started_at) AS ride_month,
	COUNT(CASE WHEN m.member_casual = 'casual' THEN m.ride_id END) AS casual_rides,
	COUNT(t.ride_id) AS total_rides,
	(COUNT(CASE WHEN m.member_casual = 'casual' THEN m.ride_id END) * 100.0) / COUNT(m.ride_id) AS casual_ride_percentage
FROM ride_time AS t
INNER JOIN ride_method AS m
	ON t.ride_id = m.ride_id
GROUP BY ride_month;

```

<br><br>

## 2. Ride Duration (How Long Do They Ride For?)

