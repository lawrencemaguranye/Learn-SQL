# RFM SEGMENTATION SALES ANALYSIS

- Recency-Frequency-Monetary (RFM) is an indexing technique that uses past purchase behavior to segment customers.
- An RFM report is a way of segmenting customers using three key metrics:
	a. Recency - how long ago their last purchase was.
 	b. Frequency - how often they purchase.
 	c. Monetary value - how much they spent.

## DATA POINTS USED IN RFM ANALYSIS

- Recency - last order date
- Frequency - count of total orders
- Monetary value - total spend

## INSPECTING DATA

````sql
SELECT * FROM [dbo].[sales_data_sample]
````

## CHECKING UNIQUE VALUES

````sql
SELECT DISTINCT STATUS FROM [dbo].[sales_data_sample] 
SELECT DISTINCT YEAR_ID FROM [dbo].[sales_data_sample]
SELECT DISTINCT PRODUCTLINE FROM [dbo].[sales_data_sample] 
SELECT DISTINCT COUNTRY FROM [dbo].[sales_data_sample]
SELECT DISTINCT DEALSIZE FROM [dbo].[sales_data_sample] 
SELECT DISTINCT TERRITORY FROM [dbo].[sales_data_sample] 

SELECT DISTINCT MONTH_ID FROM [dbo].[sales_data_sample] WHERE YEAR_ID = 2003
SELECT DISTINCT MONTH_ID FROM [dbo].[sales_data_sample] WHERE YEAR_ID = 2004
SELECT DISTINCT MONTH_ID FROM [dbo].[sales_data_sample] WHERE YEAR_ID = 2005
````

## ANALYSIS

## 1. GROUP SALES BY PRODUCTLINE

````sql
SELECT PRODUCTLINE, SUM(SALES) AS REVENUE
FROM [dbo].[sales_data_sample]
GROUP BY PRODUCTLINE
ORDER BY 2 DESC
````

## 2. GROUP SALES BY YEAR

````sql
SELECT YEAR_ID, SUM(SALES) AS REVENUE
FROM [dbo].[sales_data_sample]
GROUP BY YEAR_ID
ORDER BY 2 DESC
````

## 3. GROUP SALES BY DEALSIZE

````sql
SELECT DEALSIZE, SUM(SALES) AS REVENUE
FROM [dbo].[sales_data_sample]
GROUP BY DEALSIZE
ORDER BY 2 DESC
````

## 4. WHAT WAS THE BEST MONTH FOR SALES IN A SPECIFIC YEAR? HOW MUCH WAS EARNED THAT MONTH?

````sql
SELECT MONTH_ID, SUM(SALES) AS REVENUE, COUNT(ORDERNUMBER) AS FREQUENCY
FROM [dbo].[sales_data_sample] 
WHERE YEAR_ID = 2004 --Change year to see the rest--
GROUP BY MONTH_ID
ORDER BY 2 DESC
````

## 5. NOVEMBER SEEMS TO BE THE MONTH WITH MOST SALES, WHAT PRODUCT DO THEY SELL IN NOVEMBER

````sql
SELECT 
	MONTH_ID, 
	PRODUCTLINE, 
	SUM(SALES) AS Revenue,
	COUNT(ORDERNUMBER) AS NumberOfOrders
FROM [dbo].[sales_data_sample]
WHERE YEAR_ID = 2004 AND MONTH_ID = 11 --Change year to see the rest--
GROUP BY MONTH_ID, PRODUCTLINE
ORDER BY 3 DESC
````

## 6. WHO IS OUR BEST CUSTOMER (This could be best answered with RFM)

````sql
DROP TABLE IF EXISTS #rfm

;WITH RFM AS
(
	SELECT 
		CUSTOMERNAME,
		SUM(SALES) AS MonetaryValue,
		AVG(SALES) AS AvgMonetaryValue,
		COUNT(ORDERNUMBER) AS Frequency,
		MAX(ORDERDATE) AS LastOrderDate,
		(SELECT MAX(ORDERDATE) FROM [dbo].[sales_data_sample]) AS MaxOrderDate,
		DATEDIFF (DD, MAX(ORDERDATE), (SELECT MAX(ORDERDATE) FROM [dbo].[sales_data_sample])) AS Recency
	FROM [dbo].[sales_data_sample]
	GROUP BY CUSTOMERNAME
),
RFM_Calc AS(
	SELECT r.*,
		NTILE(4) OVER (ORDER BY Recency DESC) AS RFM_Recency,
		NTILE(4) OVER (ORDER BY Frequency) AS RFM_Frequency,
		NTILE(4) OVER (ORDER BY MonetaryValue) AS RFM_Monetary
	FROM RFM r
)

SELECT 
	c.*, RFM_Recency+RFM_Frequency+RFM_Monetary AS RFM_Cell,
	CAST(RFM_Recency AS VARCHAR) + CAST(RFM_Frequency AS VARCHAR) + CAST(RFM_Monetary AS VARCHAR) AS RFM_Cell_String
	---CREATING A TEMPORARY TABLE---
	INTO #rfm
FROM RFM_Calc c
````

### QUERING DATA FROM THE TEMPORARY TABLE

````sql
SELECT CUSTOMERNAME , rfm_recency, rfm_frequency, rfm_monetary,
	CASE 
		WHEN rfm_cell_string IN (111, 112 , 121, 122, 123, 132, 211, 212, 114, 141) THEN 'Lost Customers'  --lost customers
		WHEN rfm_cell_string IN (133, 134, 143, 244, 334, 343, 344, 144) then 'Slipping away, cannot lose' -- (Big spenders who haven’t purchased lately) slipping away
		WHEN rfm_cell_string IN (311, 411, 331) THEN 'New customers'
		WHEN rfm_cell_string IN (222, 223, 233, 322) THEN 'Potential churners'
		WHEN rfm_cell_string IN (323, 333,321, 422, 332, 432) THEN 'Active' --(Customers who buy often & recently, but at low price points)
		WHEN rfm_cell_string IN (433, 434, 443, 444) THEN 'Loyal'
	END rfm_segment

FROM #rfm
````

## 7. WHAT PRODUCTS ARE MOST OFTEN SOLD TOGETHER

````sql
--SELECT * FROM [dbo].[sales_data_sample] WHERE ORDERNUMBER = 10411
SELECT DISTINCT ORDERNUMBER, STUFF(
	(SELECT ',' + PRODUCTCODE
	FROM [dbo].[sales_data_sample] AS p
	WHERE ORDERNUMBER IN (
		SELECT ORDERNUMBER
		FROM (
			SELECT ORDERNUMBER, COUNT(*) AS rn
			FROM [dbo].[sales_data_sample]
			WHERE STATUS = 'Shipped'
			GROUP BY ORDERNUMBER
		) AS m
		WHERE rn = 3
	)
	AND p.ORDERNUMBER = s.ORDERNUMBER
	FOR XML PATH ('')), 
	1, 1, '') AS ProductCode

FROM [dbo].[sales_data_sample] AS s
ORDER BY 2 DESC
````
