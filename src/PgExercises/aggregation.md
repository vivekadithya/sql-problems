# Aggregation
## Count the number of facilities
### For our first foray into aggregates, we're going to stick to something simple. We want to know how many facilities exist - simply produce a total count.
```sql
SELECT
    COUNT(DISTINCT name)
FROM
    cd.facilities;
```

## Count the number of expensive facilities
### Produce a count of the number of facilities that have a cost to guests of 10 or more.
```sql
SELECT
    COUNT(DISTINCT name)
FROM
    cd.facilities
WHERE
    guestcost >= 10;
```

## Count the number of recommendations each member makes.
### Produce a count of the number of recommendations each member has made. Order by member ID.
```sql
SELECT
    recommendedby
    , COUNT(1)
FROM
    cd.members
WHERE
	recommendedby IS NOT NULL
GROUP BY
    recommendedby
ORDER BY
    recommendedby;
```

## List the total slots booked per facility
### Produce a list of the total number of slots booked per facility. For now, just produce an output table consisting of facility id and slots, sorted by facility id.
```sql
SELECT
    facid
    , SUM(slots) "Total Slots"
FROM
    cd.bookings
GROUP BY
    facid
ORDER BY
    facid;
```

## List the total slots booked per facility in a given month
### Produce a list of the total number of slots booked per facility in the month of September 2012. Produce an output table consisting of facility id and slots, sorted by the number of slots.
```sql
SELECT
    facid
    , SUM(slots) "Total Slots"
FROM
    cd.bookings
WHERE
    TO_CHAR(starttime::date, 'Month YYYY')='September 2012'
GROUP BY
    facid
ORDER BY 
    "Total Slots";
```
## List the total slots booked per facility per month
### Produce a list of the total number of slots booked per facility per month in the year of 2012. Produce an output table consisting of facility id and slots, sorted by the id and month.
```sql
SELECT
	facid
	, EXTRACT(MONTH FROM starttime::date) "month"
	, SUM(slots)
FROM
	cd.bookings
WHERE
	EXTRACT(YEAR FROM starttime::date)='2012'
GROUP BY
	facid
	, month
ORDER BY
	facid
	, month
```

## Find the count of members who have made at least one booking
### Find the total number of members (including guests) who have made at least one booking.
```sql
SELECT
    COUNT(DISTINCT memid)
FROM
    cd.bookings;
```

## List facilities with more than 1000 slots booked
### Produce a list of facilities with more than 1000 slots booked. Produce an output table consisting of facility id and slots, sorted by facility id.
```sql
SELECT
    facid
    , SUM(slots) "Total Slots"
FROM
    cd.bookings
GROUP BY
    facid
HAVING
    SUM(slots) > 1000
ORDER BY
    facid;
```

## Find the total revenue of each facility
### Produce a list of facilities along with their total revenue. The output table should consist of facility name and revenue, sorted by revenue. Remember that there's a different cost for guests and members!
```sql
SELECT
	facilities.name
	, SUM(
	  	CASE WHEN bookings.memid=0 THEN facilities.guestcost*bookings.slots
	  	ELSE facilities.membercost*bookings.slots END
	  ) "revenue"
FROM
	cd.bookings JOIN cd.facilities
	ON bookings.facid=facilities.facid
GROUP BY
	facilities.facid
ORDER BY
	revenue;
```

## Find facilities with a total revenue less than 1000
### Produce a list of facilities with a total revenue less than 1000. Produce an output table consisting of facility name and revenue, sorted by revenue. Remember that there's a different cost for guests and members!
```sql
WITH rev_table AS (
    SELECT
        facilities.name
        , SUM(
            CASE WHEN bookings.memid=0 THEN bookings.slots * facilities.guestcost
            ELSE bookings.slots * facilities.membercost END
        ) revenue
    FROM
        cd.facilities AS facilities JOIN cd.bookings AS bookings
        ON facilities.facid=bookings.facid
    GROUP BY
        facilities.name
)

SELECT 
    * 
FROM
    rev_table
WHERE
    revenue<1000
ORDER BY
    revenue;
```

## Output the facility id that has the highest number of slots booked
### Output the facility id that has the highest number of slots booked. For bonus points, try a version without a LIMIT clause. This version will probably look messy!
```sql
WITH temp_table AS
    (
        SELECT
            bookings.facid
            , SUM(bookings.slots) AS total_slots
        FROM
            cd.bookings AS bookings
        GROUP BY
            bookings.facid
    )
SELECT
    facid
    , MAX(total_slots)
FROM
    temp_table
GROUP BY
    facid;
```

## List the total slots booked per facility per month, part 2
### Produce a list of the total number of slots booked per facility per month in the year of 2012. In this version, include output rows containing totals for all months per facility, and a total for all months for all facilities. The output table should consist of facility id, month and slots, sorted by the id and month. When calculating the aggregated values for all months and all facids, return null values in the month and facid columns.
```sql
SELECT
	facid
	, EXTRACT(MONTH FROM starttime::date) AS month
	, SUM(slots) AS slots
FROM
	cd.bookings
WHERE
	EXTRACT(YEAR FROM starttime::date)=2012
GROUP BY ROLLUP
	(facid, EXTRACT(MONTH FROM starttime::date))
ORDER BY
	1, 2;
```

## List the total hours booked per named facility
### Produce a list of the total number of hours booked per facility, remembering that a slot lasts half an hour. The output table should consist of the facility id, name, and hours booked, sorted by facility id. Try formatting the hours to two decimal places.
```sql
SELECT
    facilities.facid
    , facilities.name
    , ROUND(SUM(bookings.slots * 0.5), 2) AS "Total Hours"
FROM
    cd.facilities AS facilities JOIN cd.bookings AS bookings
    ON facilities.facid=bookings.facid
GROUP BY
    facilities.facid
    , facilities.name
ORDER BY
    facilities.facid;
```

## List each member's first booking after September 1st 2012
### Produce a list of each member name, id, and their first booking after September 1st 2012. Order by member ID.
```sql
SELECT
    members.surname
    , members.firstname
    , members.memid
    , MIN(bookings.starttime)
FROM
    cd.members AS members JOIN cd.bookings AS bookings
    ON members.memid=bookings.memid
WHERE
    bookings.starttime > '2012-09-01'::date
GROUP BY
    members.surname
    , members.firstname
    , members.memid
ORDER BY
    members.memid;
```

## Produce a list of member names, with each row containing the total member count
### Produce a list of member names, with each row containing the total member count. Order by join date, and include guest members.
```sql
SELECT
    COUNT(*) OVER ()
    , members.firstname
    , members.surname
FROM
    cd.members
ORDER BY
    joindate;
```

## Produce a numbered list of members
### Produce a monotonically increasing numbered list of members (including guests), ordered by their date of joining. Remember that member IDs are not guaranteed to be sequential.
```sql
SELECT
    ROW_NUMBER() OVER (ORDER BY joindate)
    , firstname
    , surname
FROM
    cd.members;
```

## Output the facility id that has the highest number of slots booked, again
### Output the facility id that has the highest number of slots booked. Ensure that in the event of a tie, all tieing results get output.
```sql
WITH ranked_table AS(
    SELECT
        facid
        , SUM(slots) AS total
        , RANK() OVER (ORDER BY SUM(slots) DESC) AS rank
    FROM
        cd.bookings
    GROUP BY
        facid)
SELECT
    facid
    , total
FROM
    ranked_table
WHERE
    rank=1;
```

## Rank members by (rounded) hours used
### Produce a list of members (including guests), along with the number of hours they've booked in facilities, rounded to the nearest ten hours. Rank them by this rounded figure, producing output of first name, surname, rounded hours, rank. Sort by rank, surname, and first name.
```sql
SELECT
    members.firstname
    , members.surname
    , ROUND(SUM(bookings.slots * 0.5), -1) AS total_hours
    , RANK() OVER (ORDER BY ROUND(SUM(bookings.slots * 0.5), -1) DESC) AS rank
FROM
    cd.bookings AS bookings JOIN cd.members AS members
    ON bookings.memid=members.memid
GROUP BY
    members.firstname
    , members.surname
ORDER BY
    rank
    , members.surname
    , members.firstname;
```

## Find the top three revenue generating facilities
### Produce a list of the top three revenue generating facilities (including ties). Output facility name and rank, sorted by rank and facility name.
```sql
WITH revenues AS(
    SELECT
        facilities.name
        , SUM(
            CASE WHEN bookings.memid=0 THEN bookings.slots*facilities.guestcost
            ELSE bookings.slots*facilities.membercost END
        ) AS revenue
    FROM
        cd.facilities AS facilities JOIN cd.bookings AS bookings
        ON facilities.facid=bookings.facid
    GROUP BY
        facilities.name),
    temp AS(
    SELECT
        revenues.name
        , RANK() OVER (ORDER BY revenue DESC) AS rank
    FROM
        revenues)
SELECT
    *
FROM
    temp
WHERE
    rank <=3;
```