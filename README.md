# Project Overview
This is a company called WarmeHands that sells consumer goods across multiple categories. The information provided is 2 datasets, one on inventory and one on categories (of products).

1.	Inventory Dataset (6 Tables)
a.	Stock
b.	Orders
c.	Customer
d.	Country
e.	Price
f.	Costs
2.	Categories Dataset
a.	SKU_Id, category

The goal is to conduct an inventory analysis and generate insights.
 
## ERD Diagram
![image](https://github.com/user-attachments/assets/6b2056cb-2c30-43a6-83dd-aefddeb26ffb)

# Tools Used
- Power Query to clean and transform dataset
- DAX Functions to create new calculated columns and tables
- Power BI to analyze data and create visuals and dashboards

# PowerBI Dashboards
https://app.powerbi.com/view?r=eyJrIjoiZGM0ZDRlZWYtNmYxZC00NDViLTk3ZTUtOGEzN2MwYzBiZDU1IiwidCI6ImVhY2U1MTkxLWY0NDItNGExMS05NzMzLWI3YmViNWIwMTg2YSIsImMiOjF9

# Power Query
- Delete blank values
- Merge columns from one table to another 
- Replace text
- Create new calculated columns
- Extract year from date into new column

# DAX Functions
- Create new column  ‘Total_orders’ that shows total quantity of sales for each item in the ‘Stock’ table
  - Total_orders = calculate(sum(Orders[Quantity]),filter(orders, Stock[SKU-ID]=Orders[SKU]))

- Create new column ‘Quantity_2021’ showing the number of items sold in 2021
  - Quantity_2021 = calculate(sum(Orders[Quantity]), filter(orders, Orders[SKU]=Stock[SKU-ID] && Orders[Year]="2021"))

- Create new table ‘Turnover’ with select columns from table ‘Stock’
  - Turnover = SELECTCOLUMNS(Stock,[SKU-ID],[Description],[2021_start_stock],[Quantity_2021],[Costs.COGS])

- Create a new column ‘CP_revenue’ that calculates a cumulative increase over the ‘percent_revenue_2021’ column
  - CP_revenue = CALCULATE(sum([Stock_Percent_revenue_2021]), filter(abc, ABC[Stock_Percent_revenue_2021]>=earlier(ABC[Stock_Percent_revenue_2021])))

- Create a new column ‘ABC_class’ classifying a cumulative revenue percentage as A (High Value), B (Medium Value), or C (Low Value)
  - ABC_class = if(ABC[CP_revenue] <= 70, "A [High Value]", if([CP_revenue] <= 90, "B [Medium Value]", if([CP_revenue] > 90, "C [Low Value]")))

- Create a new column ‘Rank’ to rank based on the descending order of column ‘percent_revenue_2021’
  - Rank = RANKX(abc,ABC[Stock_Percent_revenue_2021])
