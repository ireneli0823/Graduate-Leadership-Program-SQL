### Answers for the SQL challenge are as follows
*  Question #1
 Generate a query to get the sum of the clicks of the marketing data
​

>  SELECT SUM(clicks) as sum_of_clicks
>  FROM marketing_data
​

*  Question #2
 Generate a query to gather the sum of revenue by store_location from the store_revenue table
​

>  SELECT store_location, SUM(revenue) as sum_of_revenue
>  FROM store_revenue
>  GROUP BY store_location
​

*  Question #3
 Merge these two datasets so we can see impressions, clicks, and revenue together by date
and geo.
 Please ensure all records from each table are accounted for.
​
>  SELECT *
>  FROM marketing_data a
>  FULL JOIN store_revenue b
>  ON a.date = b.date AND a.geo = RIGHT(b.store_location, 2)
​

* Question #4
 In your opinion, what is the most efficient store and why?
​
I think the store which located in California in the most efficient store. We can use the following sql query to get a table like this:

>  SELECT b.store_location, a.impressions, a.clicks, b.revenue, (b.revenue/a.impressions), (b.revenue/a.clicks)
>  FROM
>  (SELECT geo, sum(impressions) as impressions, sum(clicks) as clicks
>  FROM marketing_data
>  GROUP BY geo) a
>  RIGHT JOIN
>  (SELECT store_location, sum(revenue) as revenue
>  FROM store_revenue
>  GROUP BY store_location) b
>  ON a.geo = RIGHT(b.store_location,2)
>  ORDER BY (b.revenue/a.impressions) DESC, (b.revenue/a.clicks) DESC
​

* Question #5 (Challenge)
 Generate a query to rank in order the top 10 revenue producing states
 ​​​
>  SELECT RIGHT(store_location,2) as states, sum(revenue) as total_revenue
>  FROM store_revenue
>  GROUP BY states
>  ORDER BY total_revenue DESC
>  LIMIT 10
