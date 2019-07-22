# Purpose

Maintaining a reproducable and consistent style will allow all users understand and debug each other's code with ease. Though postgresql is very flexible in it's styling, creating a common style will allow teams to learn quickly what other's queries are doing. 

This document is written as a manual to all on the Business Operations team to have a general source of truth and guideline but can also be used company wide for anyone wanting write clean queries.


# Rules
## General stuff

* Capitalize SQL keywords (`SELECT` or `WHERE`)
* No trailing whitespaces
* No tabs - instead 2 spaces should be used to indent (example in **SELECT** section)
* When relabeling a variable always use the `as` call
* Use snake case when naming variables and CTEs 

   **GOOD**: `SELECT COUNT(o.id) AS total_orders`
   
   **BAD**: `SELECT COUNT(o.id) AS allorders`
   
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


## `SELECT` statement

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
* Aggregate functions should always be relabled (`COUNT(id) AS total_orders`)
* Using the `distinct` and `distinct on (...)` should be on the same line as the select statement
* `SELECT` statements go on their own line
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
* Calculations should stay as one line with spaces inside parenthesis
`(o.delivered_at >= (ts.starts_at + INTERVAL '1 hour'))`
* 

