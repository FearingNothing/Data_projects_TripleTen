Project 2 SQL sample code.

This preoject was performed on TripleTen's integrated dashboard, so no finished project is available, however I have provide my code for the project to showcase my skills.

Print the company_name field. Find the number of taxi rides for each taxi company for November 15-16, 2017, name the resulting field trips_amount and print it, too. Sort the results by the trips_amount field in descending order.

	SELECT 
		cabs.company_name,
		COUNT(trips.trip_id) AS trips_amount
	FROM 
		cabs
	INNER JOIN 
		trips ON cabs.cab_id = trips.cab_id
	WHERE 
		trips.start_ts >= '2017-11-15 00:00:00' 
		AND trips.start_ts < '2017-11-17 00:00:00'
	GROUP BY 
		cabs.company_name
	ORDER BY 
		trips_amount DESC;

Find the number of rides for every taxi companies whose name contains the words "Yellow" or "Blue" for November 1-7, 2017. Name the resulting variable trips_amount. Group the results by the company_name field.

	SELECT
		cabs.company_name as company_name,
		COUNT(trips.trip_id) AS trips_amount
	FROM 
		cabs
	INNER JOIN 
		trips 
	ON 
		trips.cab_id = cabs.cab_id
	WHERE 
		CAST(trips.start_ts AS date) BETWEEN '2017-11-01' AND '2017-11-07'
		AND cabs.company_name LIKE '%%Yellow%%'
	GROUP BY company_name
	UNION ALL
	SELECT
		cabs.company_name as company_name,
		COUNT(trips.trip_id) AS trips_amount
	FROM 
		cabs
	INNER JOIN 
		trips 
	ON 
		trips.cab_id = cabs.cab_id
	WHERE 
		CAST(trips.start_ts AS date) BETWEEN '2017-11-01' AND '2017-11-07'
		AND cabs.company_name LIKE '%%Blue%%'
	GROUP BY company_name;

Retrieve from the trips table all the rides that started in the Loop (pickup_location_id: 50) on a Saturday and ended at O'Hare (dropoff_location_id: 63). Get the weather conditions for each ride. Use the method you applied in the previous task. Also, retrieve the duration of each ride. Ignore rides for which data on weather conditions is not available.

The table columns should be in the following order:

start_ts
weather_conditions
duration_seconds
Sort by trip_id.


	SELECT
		start_ts,
		T.weather_conditions,
		duration_seconds
	FROM 
		trips
	INNER JOIN (
    SELECT
        ts,
        CASE
            WHEN description LIKE '%rain%' OR description LIKE '%storm%' THEN 'Bad'
            ELSE 'Good'
        END AS weather_conditions
    FROM 
        weather_records          
		) T ON T.ts = trips.start_ts
	WHERE 
		pickup_location_id = 50 AND dropoff_location_id = 63 AND EXTRACT (DOW from trips.start_ts) = 6
	ORDER BY trip_id