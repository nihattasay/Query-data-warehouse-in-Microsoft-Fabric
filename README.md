# Querying a Data Warehouse in Microsoft Fabric

This lab demonstrates how to query a data warehouse in **Microsoft Fabric** using T-SQL. You'll use sample data to run analytical queries, validate consistency, and create views for filtered reporting.

---

## 🕒 Duration

**Estimated Time:** 30 minutes  
**Prerequisite:** Microsoft Fabric Trial License

---

## 🚀 Objectives

- Create a workspace and a sample data warehouse
- Query the data warehouse using T-SQL
- Validate data consistency
- Create a reusable view
- Clean up resources

---

## 🏗 Step-by-Step Guide

### 1. Create a Workspace

1. Go to [Microsoft Fabric](https://app.fabric.microsoft.com/home?experience=fabric)
2. Sign in with your Fabric credentials
3. Click **Workspaces → + New Workspace**
4. Enable a Fabric capacity (Trial, Premium, or Fabric)
5. Your workspace will be created and will initially be empty.

---

### 2. Create a Sample Data Warehouse

1. From the sidebar, select **Create → Sample Warehouse**
2. Name it something like `sample-dw`
3. Wait for provisioning and sample data to be populated

---

### 3. Query the Data Warehouse

- Go to `sample-dw`, click **New SQL query**

#### a. Total Trips and Revenue by Month
```sql
SELECT 
    D.MonthName, 
    COUNT(*) AS TotalTrips, 
    SUM(T.TotalAmount) AS TotalRevenue 
FROM dbo.Trip AS T
JOIN dbo.[Date] AS D ON T.DateID = D.DateID
GROUP BY D.MonthName;

b. Average Duration & Distance by Day

SELECT 
    D.DayName, 
    AVG(T.TripDurationSeconds) AS AvgDuration, 
    AVG(T.TripDistanceMiles) AS AvgDistance 
FROM dbo.Trip AS T
JOIN dbo.[Date] AS D ON T.DateID = D.DateID
GROUP BY D.DayName;

c. Top 10 Pickup & Dropoff Cities

-- Pickup
SELECT TOP 10 
    G.City, COUNT(*) AS TotalTrips 
FROM dbo.Trip AS T
JOIN dbo.Geography AS G ON T.PickupGeographyID = G.GeographyID
GROUP BY G.City
ORDER BY TotalTrips DESC;

-- Dropoff
SELECT TOP 10 
    G.City, COUNT(*) AS TotalTrips 
FROM dbo.Trip AS T
JOIN dbo.Geography AS G ON T.DropoffGeographyID = G.GeographyID
GROUP BY G.City
ORDER BY TotalTrips DESC;

4. Verify Data Consistency
a. Trips Longer than 24 Hours

SELECT COUNT(*) FROM dbo.Trip WHERE TripDurationSeconds > 86400;

b. Trips with Negative Duration

SELECT COUNT(*) FROM dbo.Trip WHERE TripDurationSeconds < 0;

c. Remove Invalid Records

DELETE FROM dbo.Trip WHERE TripDurationSeconds < 0;

5. Save as View

    Run this query:

SELECT 
    D.DayName, 
    AVG(T.TripDurationSeconds) AS AvgDuration, 
    AVG(T.TripDistanceMiles) AS AvgDistance 
FROM dbo.Trip AS T
JOIN dbo.[Date] AS D ON T.DateID = D.DateID
WHERE D.Month = 1
GROUP BY D.DayName;

    Highlight the SELECT statement

    Click Save as View

    Name it: vw_JanTrip

🧹 Clean Up

If you're done:

    Go to your Workspace

    Click Workspace Settings

    Scroll down and choose Remove this workspace

    Confirm Delete
