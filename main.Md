# Purpose

Maintaining a reproducible and consistent style will allow all users understand and debug each other's code with ease. Though postgresql is very flexible in its styling, creating a common style will allow teams to learn quickly what other's queries are doing. 

This document is written as a manual to all on the Business Operations team to have a general source of truth and guidelines but can also be used company wide for anyone wanting to write clean queries.


# Rules
## General stuff

* Capitalize SQL keywords (`SELECT` or `WHERE`)
* No trailing whitespaces
* No tabs - instead 2 spaces should be used to indent (example in **SELECT** section)
* Refrain from using "" when naming variables
* When relabeling a variable always use the `AS` call
* Use snake case when naming variables and CTEs 

   **GOOD**: `SELECT COUNT(o.id) AS total_orders`
   
   **BAD**:  `SELECT COUNT(o.id) AS allorders`
   
* When opening up a new parenthesis, the initial call goes two spaces in from where the parenthesis starts
```
SELECT 
     o.id,
     exists(
             SELECT
               1
             FROM
               drivers AS d
             WHERE d.id = o.driver_id
            ) AS valid_drivers
    FROM
      orders AS o
```
      

## Comments

* All comments should have a space after the respective comment sytax 

   **GOOD**: `-- This is a good comment`
   
   **BAD**: `--dont comment like this`
   
* Comments above the _actual query_ should consist of the multi line commenting (`/* This kind of comment */`)
* Comments above _CTEs and select_ statements should consist of the double hyphen (`-- This kind of comment`)
* When hard coding numbers in, include a comment explaining the reason for the number (ie. metro_ids or date ranges)


## Select statement

* If using comments, they need to be above the select statement
   ```
   -- This is pulling all orders for Birmingham
   SELECT
     *
   FROM
     orders
   WHERE
     metro_id = 1
     ```
* When pulling multiple columns, the comma should come prior to the next column pulled 
   ```
   SELECT
     id,
     metro_id
   FROM
     orders
    ```
* Aggregate functions should always be relabeled (`COUNT(id) AS total_orders`)
* `SELECT` statements go on their own line
* Using the `DISTINCT` and `DISTINCT ON (...)` should be on the same line as the select statement
* Creating a select statement within a join goes by the same rules as initial select statements

## Joins

* Explicitly use `INNER JOIN` instead of `JOIN` for others' readability and debugging
* Always alias tables when joining 
   - Consider and practice relabeling something unique but also indicative of the table being aliased
   - `INNER JOIN time_slots AS ts ON ts.id = o.time_slot_id`
* Joins stay on one line unless creating a unique table to join - that would follow the same style as a select statement
  ```
  LEFT JOIN (
             SELECT
               id,
               name
             FROM
               metros
             ) AS m ON m.id = o.metro_id
  ```

## Calculations

* Aggregations should stay as one line 
* Calculations should stay as one line with spaces inside parentheses
   `(o.delivered_at >= (ts.starts_at + INTERVAL '1 hour'))`

## Case when

* Case when statements should be aligned by the `WHEN`
```
CASE WHEN 1 THEN 'one'
     WHEN 2 THEN 'two'
     WHEN 3 THEN 'three'
     ELSE 'no number'
END AS number_to_text
```

## CTEs

* The innards of a CTE should be indented once (2 spaces) from the relative indention outside of it
* The opening parenthesis should be on its own line parallel to the beginning of the CTE call
* The closing parenthesis should be parallel to the opening parenthesis
* The comma after the closing of the CTE should be on the line as the closing parenthesis
```
WITH late_orders AS 
(
  SELECT
    late_orders
  FROM 
    my_late_orders_table
 ),
```

## Window functions

* This one is up for debate but the general consencious seems to keep this on one line 
* Remember this is a form of aggregation that will need to be relabled 

`ROW_NUMBER() OVER (PARTITION BY x ORDER BY y,z) AS order_counts`

## Order by and group by

* Order by and group by should be on each individual line an should be indented to their respective SELECT and FROM statements
* To call order by and group by use numbers relative to the respective row
```
...
FROM 
  orders
GROUP BY 1,2,5 
ORDER BY 3 DESC
```
* Refrain from using ASC unless completely necessary 

## Time zones

```
   SELECT
     CASE WHEN timezone_name = 'Eastern Time (US & Canada)' THEN 'America/New_York'
          WHEN timezone_name = 'Central Time (US & Canada)'THEN 'America/Chicago'
          WHEN timezone_name = 'Mountain Time (US & Canada)' THEN 'America/Denver'
          WHEN timezone_name = 'Arizona' THEN 'America/Phoenix'
          WHEN timezone_name = 'Pacific Time (US & Canada)' THEN 'America/Los_Angeles'
          WHEN timezone_name = 'Alaska' THEN 'America/Anchorage'
          WHEN timezone_name = 'Hawaii' THEN 'Pacific/Honolulu' 
      END AS true_zone
    FROM
      metros
```
* Refrain from using too many parenthesis
* Postgres functionality will allow this syntax:

`created_at AT TIME ZONE 'UTC' AT TIME ZONE true_zone`

* The use of too many parenthesis can prove erroneous and should be avoided for debugging's sake


