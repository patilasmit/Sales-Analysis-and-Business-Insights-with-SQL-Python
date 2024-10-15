create database orders;
use orders;
select * from df_order;

---find the 10 highest revenue genrating products
select product_id , category , sub_category , sum(sale_price) as high_revenue
from df_orders
group by product_id , category , sub_category
order by high_revenue desc
limit 10;

---find the 5 highest selling product in each region
with sales as (
select region, category, product_id, 
sum(sale_price) as high_selling
from df_orders
group by region, category, product_id
)
select * 
from (
select region, category, product_id, high_selling, 
row_number() over(partition by region order by high_selling desc) as rn
from sales) a
where rn <= 5


--find month over month growth comparison for 2022 and 2023 sales eg : jan 2022 vs jan 2023
with year_over_year_growth as (
select year(order_date) as years ,monthname(order_date) as months , sum(sale_price) as total_sales
from df_orders
group by years , months)
select months
,sum(case when years = 2022 then total_sales else 0 end) as sales_2022
,sum(case when years = 2023 then total_sales else 0 end) as sales_2023
from year_over_year_growth
group by months
order by months;

--for each category which month had highest sales 
with high_month_sales as (
select category , monthname(order_date) as months , sum(sale_price) as sales
from df_orders
group by category , months)
select * from (
select * , row_number() over(partition by category order by months desc) as months_rn
from high_month_sales ) as a
where months_rn = 1


--which sub category had highest growth by profit in 2023 compare to 2022
with highest_profit as (
select year(order_date) as years, sub_category, 
sum(sale_price) as profit
from df_orders
group by years, sub_category
),
years_profit as (
select sub_category,
sum(case when years = 2022 then profit else 0 end) as sales_2022,
sum(case when years = 2023 then profit else 0 end) as sales_2023
from highest_profit
group by sub_category
)
select sub_category,
(sales_2023 - sales_2022) as profit_difference
from years_profit
order by profit_difference desc;
