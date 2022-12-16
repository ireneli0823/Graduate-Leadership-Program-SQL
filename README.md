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

I think the store which located in California is the most efficient store. We can use the following sql query to get a table like this:

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

![alt text](https://github.com/ireneli0823/Graduate-Leadership-Program-SQL/blob/main/SQL_Challenge_Q4%23.png?raw=true)

From the 'revenue/impression' column we can see that, stores located in California can earn an average of $10 for every exposure, while stores in New York State can only earn $3 per exposure, and stores in Texas can only earn $1 per exposure.

Furthermore, if users can click after browsing, each click will bring different revenues to different stores. From the 'revenue/click' column, it can be seen that the average click revenue of store located in California is the highest, which can reach $759 per click. However, this value is only $215 in New York State, and only $41 in Texas.

Therefore, I think that when we conduct marketing activities, if we have more impressions to more users in California and attract them to click, we can get more revenue than users in New York State and Texas. That's why I think California stores are the most efficient.
​

* Question #5 (Challenge)
 Generate a query to rank in order the top 10 revenue producing states
 ​​​
>  SELECT RIGHT(store_location,2) as states, sum(revenue) as total_revenue
>  FROM store_revenue
>  GROUP BY states
>  ORDER BY total_revenue DESC
>  LIMIT 10
