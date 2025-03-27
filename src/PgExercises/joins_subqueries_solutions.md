# Joins and Subqueries
## Retrieve the start times of members' bookings
### How can you produce a list of the start times for bookings by members named 'David Farrell'?
```sql
SELECT
    bookings.starttime
FROM
    cd.bookings AS bookings JOIN
    cd.members AS members ON
    bookings.memid=members.memid
WHERE
    members.firstname='David'
    AND members.surname='Farrell';
```

## Work out the start times of bookings for tennis courts
### How can you produce a list of the start times for bookings for tennis courts, for the date '2012-09-21'? Return a list of start time and facility name pairings, ordered by the time.
```sql
SELECT
    bookings.starttime "start"
    , facilities.name "name"
FROM
    cd.facilities AS facilities JOIN
    cd.bookings AS bookings ON
    facilities.facid=bookings.facid
    JOIN cd.members AS members ON
    bookings.memid=members.memid
WHERE
    facilities.name ILIKE '%tennis court%'
    AND bookings.starttime::date='2012-09-21'::date
ORDER BY
    bookings.starttime;
```

## Produce a list of all members who have recommended another member
### How can you output a list of all members who have recommended another member? Ensure that there are no duplicates in the list, and that results are ordered by (surname, firstname).
```sql
SELECT DISTINCT
    members.firstname
    , members.surname
FROM
    cd.members AS members JOIN
    cd.members AS recommended
    ON members.memid=recommended.recommendedby
ORDER BY
    members.surname
    , members.firstname;
```

## Produce a list of all members, along with their recommender
### How can you output a list of all members, including the individual who recommended them (if any)? Ensure that results are ordered by (surname, firstname).
```sql
SELECT
    members.firstname
    , members.surname
    , recommender.firstname
    , recommender.surname
FROM
    cd.members AS members LEFT JOIN
    cd.members AS recommender
    ON members.recommendedby=recommender.memid
ORDER BY
    members.surname
    , members.firstname
    , recommender.surname
    , recommender.firstname;
```

## Produce a list of all members who have used a tennis court
### How can you produce a list of all members who have used a tennis court? Include in your output the name of the court, and the name of the member formatted as a single column. Ensure no duplicate data, and order by the member name followed by the facility name.
```sql
SELECT DISTINCT
	members.firstname || ' '|| members.surname "member"
	, facilities.name "facility"
FROM
	cd.members AS members JOIN cd.bookings ON
	members.memid=bookings.memid JOIN
	cd.facilities "facilities" ON
	bookings.facid=facilities.facid
WHERE
	facilities.name ILIKE '%Tennis Court%'
ORDER BY
	1, 2
```

## Produce a list of costly bookings
### How can you produce a list of bookings on the day of 2012-09-14 which will cost the member (or guest) more than $30? Remember that guests have different costs to members (the listed costs are per half-hour 'slot'), and the guest user is always ID 0. Include in your output the name of the facility, the name of the member formatted as a single column, and the cost. Order by descending cost, and do not use any subqueries.
```sql
SELECT
	members.firstname || ' ' || members.surname "member"
	, facilities.name
	, CASE WHEN bookings.memid=0 THEN facilities.guestcost * bookings.slots
		ELSE facilities.membercost * bookings.slots END "cost"
FROM
	cd.members AS members JOIN cd.bookings ON
	members.memid=bookings.memid JOIN
	cd.facilities "facilities" ON
	bookings.facid=facilities.facid
WHERE
	bookings.starttime::date='2012-09-14'::date
	AND CASE WHEN bookings.memid=0 THEN facilities.guestcost * bookings.slots
		ELSE facilities.membercost * bookings.slots END > 30
ORDER BY
	cost DESC;
```

## Produce a list of all members, along with their recommender, using no joins.
### How can you output a list of all members, including the individual who recommended them (if any), without using any joins? Ensure that there are no duplicates in the list, and that each firstname + surname pairing is formatted as a column and ordered.
```sql
SELECT DISTINCT
    members.firstname || ' ' || members.surname "member"
    , (
        SELECT
            recommenders.firstname || ' ' || recommenders.surname "recommender"
        FROM
            cd.members AS recommenders
        WHERE
            members.recommendedby = recommenders.memid
    )
FROM
    cd.members AS members
ORDER BY
    1, 2
```