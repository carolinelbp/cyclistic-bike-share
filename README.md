# 🚲 Unlocking Member Growth: Cyclistic Bike Share User Analysis 🚲

# Introduction
### **📈 Business Task**

Identify how casual riders and annual members use fictional bike share service Cyclistic differently. Recommend marketing strategies to convert casual riders into riders with annual memberships.
<br><br>

### **👩‍💼 Target Audience**

The key stakeholders are:
- The Cyclistic Executive team
- The Director of Marketing
- The Marketing Analytics team
<br><br>

### **🚀 Why It Matters**

Memberships are crucial for Cyclistic’s long-term revenue and user loyalty. By understanding the behaviors and habits of casual riders, we can tailor strategies to encourage more conversions.

---

# Data Overview
### **🪄 Dataset**

12 months of Cyclistic trip data (anonymised data from a Chicago ride share service for 2024).
<br><br>

### **📊 Platforms Used**

1. Initial data clean = Excel
2. Process and analyze = SQL
3. Visualize and present findings = Excel & Tableau. 
<br><br>

### **🔎 Key Variables Used**

- ride_id
- rideable_type (classic bike / electric bike / electric scooter)
- started_at, ended_at (date and time)
- start_station_name, end_station_name
- start_lat, start_lng, end_lat, end_lng (the exact coordinates)
- member_casual (the type of user)
- ride_length
- text_day_of_week
<br><br>

### **🧼 Data Cleaning**

- Added calculated columns. 
- Fixed cell format issues. 
- Removed invalid records. 
- Moved records into the correct month's workbook. 
- Checked for duplicate entries. 

---

# Exploratory Analysis

Before honing in on my business task, I needed to better understand my data by asking SQL some general questions. These are what I felt were my most interesting and important findings. 

## Users take the most rides in September... 

```sql 

-- This query counts the number of rides per month

SELECT 
	DATE_TRUNC('month', started_at) AS month,
	COUNT(*) AS number_of_rides
FROM ride_time
GROUP BY month
ORDER BY month; 

```

![Total Rides Taken Per Month](images/total-rides-taken-per-month.png)
<br><br>

## But they spend the most time riding bikes in July. 

```sql 

-- This query calculates the total time spent on rides per month

SELECT
	DATE_TRUNC('month', started_at) AS month,
	SUM(ride_length) AS time_spent_on_rides
FROM ride_time
GROUP BY month;

```

<insert bar graph?>
<br><br>

## Users take the longest rides, on average, on Sundays...

```sql 

-- This query calculates the average ride duration per day of the week

SELECT
	text_day_of_week AS day_of_week,
	AVG(ride_length) AS average_ride_duration
FROM ride_time
GROUP BY text_day_of_week,
	number_day_of_week
ORDER BY number_day_of_week;

```

<insert bar graph?>
<br><br>

## And the most rides are taken at 6pm. 

```sql 

-- This query counts rides taken each hour of the day

SELECT 
	EXTRACT(HOUR FROM started_at) AS hour_of_day,
	COUNT(*) AS ride_count
FROM ride_time
GROUP BY hour_of_day;

```

![Total Rides Taken Per Time of Day](images/total-rides-taken-per-time-of-day.png)
<br><br>

## Which type of bike is preferred? Riders use electric bikes slightly more... 

```sql 

-- This query counts rides taken on each type of bike

SELECT 
	rideable_type,
	COUNT(rideable_type) AS type_count
FROM ride_method
GROUP BY rideable_type
ORDER BY type_count DESC;

```

![Total Rides Taken Per Bike Type](images/total-rides-taken-per-bike-type.png)

Through further investigation in SQL, it seems electric scooters are only used in August and September - presumably as a trial of some kind. 
<br><br>

## However, they spend longer on classic bike rides, on average. 

```sql 

-- This query calculates the average ride length per bike type

SELECT 
    	m.rideable_type,
	AVG(ride_length) AS average_ride_length
FROM ride_method AS m
INNER JOIN ride_time AS t 
	ON m.ride_id = t.ride_id
GROUP BY rideable_type;

```

![Average Ride Length Per Bike Type](images/average-ride-length-per-bike-type.png)
<br><br>

## Only 3.31% of bike rides are round trips. 

```sql 

-- This query counts, and calculates the percentage of, rides that are round trips

SELECT 
    	COUNT(*) AS round_trips,
	ROUND(COUNT(*) *100.0 / (SELECT COUNT(*) FROM ride_location), 2) AS percentage_of_total
FROM ride_location
WHERE start_station_id = end_station_id;

```

![Round Trips Percentage](images/round-trips-percentage.png)
<br><br>
---

# In-Depth Analysis & Modeling

Once I understood my dataset really well, I moved on to analyse the differences between casual users and members. I used the most relevant discoveries to inform and formulate new marketing strategies moving forward. 

## 1. High-Level Usage Comparison
### Members take more rides than casual riders. 

```sql 

-- This query counts, and calculates the percentage of, rides taken by each type of user

SELECT
	member_casual AS type_of_user,
	COUNT(*),
	ROUND(COUNT(*) *100.0 / (SELECT COUNT (*) FROM ride_method), 2) AS percentage_of_total
FROM ride_method
GROUP BY type_of_user;

```

![Rides-Percentage-Per-Type_Of-User](images/rides-percentage-per-type-of-user.png)
<br><br>
