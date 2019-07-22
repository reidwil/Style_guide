#Purpose

Maintaining a reproducable and consistent style will allow all users understand and debug each other's code with ease. Though postgresql is very flexible in it's styling, creating a common style will allow teams to learn quickly what other's queries are doing. 

This document is written as a manual to all on the Business Operations team to have a general source of truth and guideline but can also be used company wide for anyone wanting write clean queries.

#Rules
##General stuff

* Capitalize SQL keywords (`SELECT` or `WHERE`)
* No trailing whitespaces
* No tabs - instead 2 spaces should be used to indent (example in **SELECT** section)
* When relabeling a variable always use the `as` call
* Use snake case when naming variables and CTEs 
   **GOOD**: `SELECT COUNT(o.id) AS total_orders`
   **BAD**: `SELECT COUNT(o.id) AS allorders`
* Refrain from using single letter variable names for CTEs

##Comments

* All comments should have a space after the respective comment sytax 
   **GOOD**: `-- This is a good comment`
   **BAD**: `--dont comment like this`
* Comments above the _actual query_ should consist of the multi line commenting (`/* This kind of comment */`)
* Comments above _CTEs and select_ statements should consist of the double hyphen (`-- This kind of comment`)
* When hard coding numbers in, include a comment explaining the reason for the number (ie. metro_ids or date ranges)


##`SELECT` statement

* If using comments, they need to be above the select statement
   ```-- This is pulling all orders for Birmingham
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
        id
        ,metro_id
      FROM 
        orders
    ```
*     
