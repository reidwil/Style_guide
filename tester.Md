```# Shopper Velocity Measure

# Purpose: 

To have a tracking measure for each shopper on a week to week basis both over their life cycle and calendar delivery week. By creating this measure, we will be able to look at historical trends, predict future shopping habits and seasonality, and cohort shoppers by velocity. 


# Definitions: 

* `week` - Shopper's nth week on the platform starting with the first week they delivered an order

* `delivery_week` - The calendar week (Monday - Sunday) in which a shopper did or did not deliver an order

* `weekly_order_count` - The number of orders delivered by a shopper for the calendar week

* `total_orders` - cumulative sum of orders 

* `weekly_avg` - total_orders / week 

* `rolling_7` - rolling weekly average of orders from prior 6 weeks and current week. Initial 6 values per driver_id will be NULL 

* `rolling_3` - rolling weekly average of orders from prior 2 weeks and current week. Initial 2 values per driver_id will be NULL 

* `velocity_7` - `(rolling_7 - weekly_avg) / weekly_avg` - the percent change of current week's rolling_7 average compared to previous week's rolling_7 average

* `velocity_3` - `(rolling_3 - weekly_avg) / weekly_avg` - the percent change of current week's rolling_3 average compared to previous week's rolling_3 average
```
