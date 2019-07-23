# Purpose

Maintaining a reproducible and consistent style will allow all users understand and debug each other's code with ease. Though postgresql is very flexible in its styling, creating a common style will allow teams to learn quickly what other's queries are doing. 

This document is written as a manual to all on the Business Operations team to have a general source of truth and guidelines but can also be used company wide for anyone wanting to write clean queries.


# Rules
## General stuff

* SQL calls should be either all capital or all lowercase

   **Good**: `select`, `SELECT`

   **BAD**: `Select`, `SeLeCt spongebob_mock` 

* No trailing whitespaces
* No tabs - instead 2 spaces should be used to indent
* Refrain from using "" when naming variables
* When relabeling a variable always use the `as` call
* Commas should precede the next line's column call
* Use snake case when naming variables and CTEs 

   **GOOD**: `select count(o.id) as total_orders`
   
   **BAD**:  `select count(o.id) as allorders`
   
* When opening up a new parenthesis, the initial select call should align with the opening parenthesis
* The closing line should be on its on own line 
* The alias should be on the same line as the closing parenthesis
```
select 
  o.id
  ,exists(
         select
           1
         from
           drivers as d
         where d.id = o.driver_id
         ) as valid_drivers
from
  orders as o
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
   select
     *
   from
     orders
   where
     metro_id = 1
     ```
* When pulling multiple columns, the comma should come prior to the next column pulled 
   ```
   select
     id
    ,metro_id
   from
     orders
    ```
* Aggregate functions should always be relabeled (`count(id) as total_orders`)
* `select` statements go on their own line
* Using the `distinct` and `distinct on (...)` should be on the same line as the select statement
* Creating a select statement within a join goes by the same rules as initial select statements

## Joins

* Inner joins only need to be refered to as the standard `join`
* Always alias tables when joining 
   - Consider and practice relabeling something unique but also indicative of the table being aliased
   - `join time_slots as ts on ts.id = o.time_slot_id`
* Joins stay on one line unless creating a unique table to join - that would follow the same style as a select statement
  ```
  left join (
             select
               id
              ,name
             from
               metros
             ) as m on m.id = o.metro_id
  ```

## Calculations

* Aggregations should stay as one line 
* Calculations should stay as one line with spaces inside parentheses
   `(o.delivered_at >= (ts.starts_at + interval '1 hour'))`

## Case when

* Case when statements should be aligned by the `WHEN`
```
case when 1 then 'one'
     when 2 then 'two'
     when 3 then 'three'
     else 'no number'
end as number_to_text
```

## CTEs

* The innards of a CTE should be indented once (2 spaces) from the relative indention outside of it
* The opening parenthesis should be on its own line parallel to the beginning of the CTE call
* The closing parenthesis should be parallel to the opening parenthesis
* The comma after the closing of the CTE should be on the line as the closing parenthesis
```
with late_orders as 
(
  select
    late_orders
  from 
    my_late_orders_table
 ),
```

## Window functions

* This one is up for debate but the general consencious seems to keep this on one line 
* Remember this is a form of aggregation that will need to be relabled 

`row_number() over (partition by x order by y,z) as order_counts`

## Order by and group by

* Order by and group by should be on each individual line an should be indented to their respective SELECT and FROM statements
* To call order by and group by use numbers relative to the respective row
```
...
from 
  orders
group by 1,2,5 
order by 3 desc
```
* Refrain from using ASC unless completely necessary 

## Time zones

```
   select
     case when timezone_name = 'Eastern Time (US & Canada)' then 'America/New_York'
          when timezone_name = 'Central Time (US & Canada)' then 'America/Chicago'
          when timezone_name = 'Mountain Time (US & Canada)' then 'America/Denver'
          when timezone_name = 'Arizona' then 'America/Phoenix'
          when timezone_name = 'Pacific Time (US & Canada)' then 'America/Los_Angeles'
          when timezone_name = 'Alaska' then 'America/Anchorage'
          when timezone_name = 'Hawaii' then 'Pacific/Honolulu' 
      end as true_zone
    from
      metros
```
* Postgres functionality will allow this syntax:

`created_at at time zone 'UTC' at time zone true_zone`

* The use of too many parenthesis can prove erroneous and should be avoided for debugging's sake


