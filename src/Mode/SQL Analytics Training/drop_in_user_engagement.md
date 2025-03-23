# Investigating a Drop in User Engagement

## Causes for Drop in Retention
1. Was the service down?
2. Did a recent change in product affect user experience?
3. Was the issue prevalent on one medium of interaction (mobile vs computer)?
4. Did the number of registered users go down?
5. Was it a holiday season?
6. Is the user experiencing a highly optimized search result, wherein they are getting what they need in minimal clicks?
7. Are people completing the user creation flow?

## Exploratory Analysis
### Pending vs Active Users
```sql
SELECT
  SUM(CASE WHEN state = 'active' THEN 1 ELSE 0 END) AS "active_count"
  , SUM(CASE WHEN state = 'pending' THEN 1 ELSE 0 END) AS "pending_count"
FROM
  tutorial.yammer_users;
```
#### Observation
There are 9381 active and 9685 pending users

### Number of Active Users by Week
```sql
SELECT
  DATE_TRUNC('week', created_at) AS "week_of_creation"
  , COUNT(*)
FROM
  tutorial.yammer_users
WHERE
  1 = 1
  AND state = 'active'
GROUP BY 1
ORDER BY 1;
```
#### Observation
There is a steady growth in the number of active user id. The user growth hits the peak of 266 in the week of 2014-08-25, which is counter intuitive to what we observe in the graph.

### User Login Trend
```sql
SELECT 
  DATE_TRUNC('week', occurred_at) AS "week_occurred_at"
  , SUM(CASE WHEN event_name = 'login' THEN 1 ELSE 0 END) AS "login_count"
FROM
  tutorial.yammer_events
GROUP BY
  1
ORDER BY 
  1;
```
#### Observation
We can clearly see a pattern similar to that of the reference chart.

## Drop in Retention Analysis
### Is there a decline in user signup trend?
```sql
SELECT
  *
  , ROUND((sub.complete_signup_count::DECIMAL/sub.create_user_count::DECIMAL)*100, 2) AS completion_pct
FROM
  (
    SELECT
      DATE_TRUNC('week', occurred_at) AS "day_of_week"
      , SUM(CASE 
          WHEN event_name = 'create_user' THEN 1 
          ELSE 0 END) AS create_user_count
      , SUM(CASE 
          WHEN event_name = 'complete_signup' THEN 1 
          ELSE 0 END) AS complete_signup_count
    FROM
      tutorial.yammer_events
    WHERE
      1 = 1
      AND event_type = 'signup_flow'
    GROUP BY
      day_of_week
    ) sub
ORDER BY
  sub.day_of_week;
```
#### Observation
There is no dip in the weekly signup completion flow. In fact in the week of 2004-08-18, we see peak signup conversions of 55.34%.

### Was the service down/other issues?
This cannot be measured because, we don't have access to the table containing service tickets. We could see if there was an increase in the number of tickets filed during the period of our interest.

### Did a recent change in product affect user experience?
This too cannot be measured because, we don't have access to the table containing information on when a feature was launched / upgraded. We can map this against our timeline of study and check if there is any relationship between the feature launch and reduced user engagement.

### Was the issue prevalent on one medium of interaction (mobile vs computer)?

### Did the number of registered users go down?
```sql
SELECT
  DATE_TRUNC('week', created_at) AS "week_of_creation"
  , COUNT(*)
FROM
  tutorial.yammer_users
WHERE
  1 = 1
  AND state = 'active'
GROUP BY 1
ORDER BY 1;
```
#### Observation
There is a steady growth in the number of active user id. The user growth hits the peak of 266 in the week of 2014-08-25, which is counter intuitive to what we observe in the graph.
### Was it a holiday season?
Not enough data to deduce whether a given day/week fell on a holiday break

### Is the user experiencing a highly optimized search result, wherein they are getting what they need in minimal clicks?
```sql
SELECT 
  DATE_TRUNC('week', occurred_at) AS "week_occurred_at"
  , CASE 
      WHEN device IN ('amazon fire phone','nexus 10','iphone 5','nexus 7','iphone 5s','nexus 5','htc one','iphone 4s','samsung galaxy note','nokia lumia 635','samsung galaxy s4') THEN 'mobile'
      WHEN device IN ('ipad mini','windows surface','samsumg galaxy tablet','kindle fire','ipad air') THEN 'tablet'
      ELSE 'computer' END AS "device_kind"
  , SUM(CASE WHEN event_name = 'search_autocomplete' THEN 1 ELSE 0 END) AS "autosearch_count"
  , SUM(CASE WHEN (event_name = 'search_run') OR (event_name ILIKE 'search_click_result_%') THEN 1 ELSE 0 END) AS "search_run_count"
FROM
  tutorial.yammer_events
GROUP BY
  1, 2
ORDER BY 
  1, 2;
```
![User Engagement Over Time](drop_in_user_engagement.md)
#### Observation
The results from the above query shows that from the first week of August 2014, we see a significant decrease in search run based engagements (both search run and search click results between 1 and 10). The search click based engagements dropped below the number of auto search based engagements, which could signify that in the most recent engagements, users are seeing more relevant auto search recommendations.