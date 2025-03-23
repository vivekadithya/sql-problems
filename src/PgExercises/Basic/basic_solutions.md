# Basic Solutions

## Retrieve everything from a table
### How can you retrieve all the information from the cd.facilities table?
```sql
SELECT
	*
FROM
	cd.facilities;
```

## Retrieve specific columns from a table
### You want to print out a list of all of the facilities and their cost to members. How would you retrieve a list of only facility names and costs?
```sql
SELECT DISTINCT
	name
	, membercost
FROM
	cd.facilities;
```

## Control which rows are retrieved
### How can you produce a list of facilities that charge a fee to members?
```sql
SELECT
	*
FROM
	cd.facilities
WHERE
	membercost > 0;
```

## Control which rows are retrieved
### How can you produce a list of facilities that charge a fee to members, and that fee is less than 1/50th of the monthly maintenance cost? Return the facid, facility name, member cost, and monthly maintenance of the facilities in question.
```sql
SELECT
	facid
	, name
	, membercost
	, monthlymaintenance
FROM
	cd.facilities
WHERE
	membercost > 0 AND membercost < monthlymaintenance/50;
```
## Basic string searches
### How can you produce a list of all facilities with the word 'Tennis' in their name?
```sql
SELECT
    *
FROM
    cd.facilities
WHERE
    name ILIKE '%Tennis%';
```
## Matching against multiple possible values
### How can you retrieve the details of facilities with ID 1 and 5? Try to do it without using the OR operator.
```sql
SELECT
	*
FROM
	cd.facilities
WHERE
	facid IN (1, 5);
```

## Classify results into buckets
### How can you produce a list of facilities, with each labelled as 'cheap' or 'expensive' depending on if their monthly maintenance cost is more than $100? Return the name and monthly maintenance of the facilities in question.
```sql
SELECT
    name,
    CASE WHEN monthlymaintenance > 100 THEN 'expensive'
        ELSE 'cheap' END "cost"
FROM
    cd.facilities;

```

## Working with dates
### How can you produce a list of members who joined after the start of September 2012? Return the memid, surname, firstname, and joindate of the members in question.
```sql
SELECT
	memid
	, surname
	, firstname
	, joindate
FROM
	cd.members
WHERE
	joindate::date >= '2012-09-01'::date;
```

## Removing duplicates, and ordering results
### How can you produce an ordered list of the first 10 surnames in the members table? The list must not contain duplicates.
```sql
SELECT
	DISTINCT surname
FROM
	cd.members
ORDER BY
	surname
LIMIT
	10;
```

## Combining results from multiple queries
### You, for some reason, want a combined list of all surnames and all facility names. Yes, this is a contrived example :-). Produce that list!
```sql
SELECT
	surname
FROM 
	cd.members
UNION
SELECT
	name
FROM
	cd.facilities;
```

## Simple aggregation
### You'd like to get the signup date of your last member. How can you retrieve this information?
```sql
SELECT
    MAX(joindate)
FROM
    cd.members;
```

## More aggregation
### You'd like to get the first and last name of the last member(s) who signed up - not just the date. How can you do that?
```sql
SELECT
    firstname
    , surname
    , joindate
FROM
    cd.members
WHERE
    joindate=(SELECT MAX(joindate) FROM cd.members);
```