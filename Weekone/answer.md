## Question 1. Understanding docker first run 

Run docker with the `python:3.12.8` image in an interactive mode, use the entrypoint `bash`.

What's the version of `pip` in the image?

- 24.3.1
- 24.2.1
- 23.3.1
- 23.2.1
### Answer: `24.3.1`
### Approach: 
```
docker run -it --entrypoint=bash  python:3.12.8
```
```
pip --version
```


## Question 2. Understanding Docker networking and docker-compose

Given the following `docker-compose.yaml`, what is the `hostname` and `port` that **pgadmin** should use to connect to the postgres database?

```yaml
services:
  db:
    container_name: postgres
    image: postgres:17-alpine
    environment:
      POSTGRES_USER: 'postgres'
      POSTGRES_PASSWORD: 'postgres'
      POSTGRES_DB: 'ny_taxi'
    ports:
      - '5433:5432'
    volumes:
      - vol-pgdata:/var/lib/postgresql/data

  pgadmin:
    container_name: pgadmin
    image: dpage/pgadmin4:latest
    environment:
      PGADMIN_DEFAULT_EMAIL: "pgadmin@pgadmin.com"
      PGADMIN_DEFAULT_PASSWORD: "pgadmin"
    ports:
      - "8080:80"
    volumes:
      - vol-pgadmin_data:/var/lib/pgadmin  

volumes:
  vol-pgdata:
    name: vol-pgdata
  vol-pgadmin_data:
    name: vol-pgadmin_data
```

- postgres:5433
- localhost:5432
- db:5433
- postgres:5432
- db:5432
### Answer: `db:5432

## Question 3. Trip Segmentation Count

During the period of October 1st 2019 (inclusive) and November 1st 2019 (exclusive), how many trips, **respectively**, happened:
1. Up to 1 mile
2. In between 1 (exclusive) and 3 miles (inclusive),
3. In between 3 (exclusive) and 7 miles (inclusive),
4. In between 7 (exclusive) and 10 miles (inclusive),
5. Over 10 miles 


### Answer: `104,802; 198,924; 109,603; 27,678; 35,189`
### Approach:


```
SELECT count(*) as cnt,'1 mile' as nm
	FROM public.green_taxi_data
	where trip_distance <='1'
	and cast (lpep_pickup_datetime as date )>= '2019-10-01'
	and cast (lpep_dropoff_datetime as date ) < '2019-11-01'
union
SELECT count(*) as cnt,'3 mile' as nm
	FROM public.green_taxi_data
	where trip_distance >'1'
	and  trip_distance <='3'
	and cast (lpep_pickup_datetime as date )>= '2019-10-01'
	and cast (lpep_dropoff_datetime as date ) < '2019-11-01'
union
SELECT count(*) as cnt,'7 mile' as nm
	FROM public.green_taxi_data
	where trip_distance >'3'
	and  trip_distance <='7'
	and cast (lpep_pickup_datetime as date )>= '2019-10-01'
	and cast (lpep_dropoff_datetime as date ) < '2019-11-01'
union
SELECT count(*) as cnt,'10 mile' as nm
	FROM public.green_taxi_data
	where trip_distance >'7'
	and  trip_distance <='10'
	and cast (lpep_pickup_datetime as date )>= '2019-10-01'
	and cast (lpep_dropoff_datetime as date ) < '2019-11-01'
union
SELECT count(*) as cnt,'More than 10 mile' as nm
	FROM public.green_taxi_data
	where trip_distance >'10'
	and cast (lpep_pickup_datetime as date )>= '2019-10-01'
	and cast (lpep_dropoff_datetime as date ) < '2019-11-01'
```
## Question 4. Longest trip for each day

Which was the pick up day with the longest trip distance?
Use the pick up time for your calculations.

Tip: For every day, we only care about one single trip with the longest distance
### Answer: `2019-10-31`
### Approach:
```
SELECT CAST(lpep_pickup_datetime AS date) AS pickup_date, MAX(trip_distance) AS max_trip_distance
FROM public.green_taxi_data
GROUP BY CAST(lpep_pickup_datetime AS date)
ORDER BY max_trip_distance DESC
LIMIT 1;
```
## Question 5. Three biggest pickup zones

Which were the top pickup locations with over 13,000 in
`total_amount` (across all trips) for 2019-10-18?

Consider only `lpep_pickup_datetime` when filtering by date.

### Answer: `East Harlem North, East Harlem South, Morningside Heights`
### Approach:
```
SELECT
    x."pickup_loc"
FROM (
    SELECT
         zpu."Zone" AS "pickup_loc",
        SUM(t."total_amount") AS "total_amount"
    FROM 
        public.green_taxi_data t
    JOIN 
        public.taxi_zone_data zpu ON t."PULocationID" = zpu."LocationID"
    JOIN
        public.taxi_zone_data zdo ON t."DOLocationID" = zdo."LocationID"
    WHERE 
        CAST(t."lpep_pickup_datetime" AS date) = '2019-10-18'
    GROUP BY 
        zpu."Borough", zpu."Zone"
    HAVING 
        SUM(t."total_amount") > 13000
    ORDER BY 
        "total_amount" DESC
) x;
```
## Question 6. Largest tip

For the passengers picked up in October 2019 in the zone
named "East Harlem North" which was the drop off zone that had
the largest tip?

Note: it's `tip` , not `trip`

We need the name of the zone, not the ID.

### Answer: `JFK Airport`
### Approach:
```
SELECT 
    zdo."Zone" AS "dropoff_zone",
    MAX(t."tip_amount") AS max_tip_amount
FROM 
    public.green_taxi_data t
JOIN 
    public.taxi_zone_data zpu ON t."PULocationID" = zpu."LocationID"
JOIN 
    public.taxi_zone_data zdo ON t."DOLocationID" = zdo."LocationID"
WHERE 
    CAST(t."lpep_pickup_datetime" AS date) >= '2019-10-01'
    AND CAST(t."lpep_pickup_datetime" AS date) < '2019-11-01'
    AND zpu."Zone" = 'East Harlem North'
GROUP BY 
    zdo."Zone"
ORDER BY 
    max_tip_amount DESC
LIMIT 1;
```
## Question 7. Terraform Workflow

Which of the following sequences, **respectively**, describes the workflow for: 
1. Downloading the provider plugins and setting up backend,
2. Generating proposed changes and auto-executing the plan
3. Remove all resources managed by terraform`
### Answer:- terraform init, terraform apply -auto-approve, terraform destroy
