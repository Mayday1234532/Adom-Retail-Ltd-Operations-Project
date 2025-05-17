
# Adom Retail Ltd's Operations Project

Dashboard: 
## https://app.fabric.microsoft.com/links/tOyC7-J6M4?ctid=81024827-92b4-4316-9f2c-e8815aca29db&pbi_source=linkShare

## Problem Statement
Analysis of Adom Retail Ltd's Power BI Dashboards
Overview of the Dashboard System Based on the provided documents, this Power BI dashboard system was created to address Adom Retail Ltd's supply chain challenges by providing comprehensive visibility across four key operational areas: Inventory, Supplier Performance, Procurement, and Sales & Fulfillment.

Problem Statement to Findings
The dashboards effectively address the initial problems identified by Adom Retail Ltd:

Frequent stockouts: The Inventory Dashboard tracks stock levels and identifies products below reorder levels.

Supplier delivery delays: The Supplier Performance Dashboard monitors lead times and on-time delivery rates.

Procurement cost visibility: The Procurement Dashboard analyzes cost trends and purchase order patterns.

Order fulfillment issues: The Sales & Fulfillment Dashboard tracks fulfillment rates and regional sales trends.


## Steps Followed
Data Loading
Import Data from Excel ("Demand_Supply_Data Final.xlsx")
Open Power BI Desktop.

Click "Get Data" → "Excel" → Select the file.

Ensure all relevant sheets are imported:

Products (Product ID, Category, Reorder Level, Safety Stock)

Inventory Transactions (Inflow/Outflow, Date, Warehouse)

Suppliers (Supplier ID, Name, Region, Lead Time)

Purchase Orders (Order Date, Delivery Date, Status, Cost)

Sales Transactions (Order ID, Customer, Revenue, Fulfillment Status)

Check Data Structure
Verify that each table has a primary key (e.g., Product ID, Supplier ID, Order ID).

Ensure date formats are consistent (e.g., DD/MM/YYYY).


2. Data Cleaning & Transformation (Power Query Editor)
A. Handling Missing Values
Remove blank rows:

In Power Query Editor → Right-click column → "Remove Empty".

Fill missing values (if applicable):

For numeric columns (e.g., Quantity Delivered), replace blanks with 0.

For text columns (e.g., Supplier Name), replace blanks with "Unknown".

Correcting Data Types
Ensure:

Dates → Date type

Numeric values (Quantity, Cost) → Decimal or Whole Number

Text fields (Product Name, Supplier) → Text


Removing Duplicates
Identify duplicates (e.g., same Order ID appearing twice).

Go to "Remove Rows" → "Remove Duplicates".

Then, did some dax functions on measures and columns before proceeding with the visualization.



                    DAX Functions-- Inventory
Total Current Stock = SUM(Inventory[Current Stock])
Products Below Reorder = COUNTROWS(FILTER(Inventory, Inventory[Current Stock] < Inventory[Reorder Level]))
Stockout Count = COUNTROWS(FILTER(Inventory, Inventory[Current Stock] = 0))
Average Stock per Warehouse = AVERAGE(Inventory[Current Stock])
Stock Level Trend = 
VAR SelectedPeriod = SELECTEDVALUE(Dates[MonthYear])
RETURN
    CALCULATE(
        SUM(Inventory[Current Stock]),
        FILTER(
            ALL(Dates),
            Dates[MonthYear] <= SelectedPeriod
        )
    )
Quantity Inflow = SUM(Inventory[Inflow Quantity])
Quantity Outflow = SUM(Inventory[Outflow Quantity])
Net Flow = [Quantity Inflow] - [Quantity Outflow]


1. Inventory Dashboard
Key Metrics: Shows 2.4 K- 6.9 K stock levels across product categories (Beauty, Beverages, Fashion, etc.)

Strengths:

Tracks the quantity of inflow/outflow by year and month

Visualizes stock level trends over time

Includes price trends (GH₵900-10,000 range shown)

Improvement Opportunity: Could better highlight products below safety stock levels

![Image](https://github.com/user-attachments/assets/92768050-a600-49c1-b413-f37a7f19ff96)




                  Dax functions -- Suppliers
Avg Lead Time (Days) = AVERAGE(Purchase_Order[Delivery Date] - Purchase_Order[Order Date])
On-Time Delivery% = 
DIVIDE(
    COUNTROWS(FILTER(Purchase_Order, Purchase_Order[Delivery Date] <= Purchase_Order[Expected Delivery Date])),
    COUNTROWS(Purchase_Order),
    0
)
Late Deliveries = 
COUNTROWS(FILTER(Purchase_Order, Purchase_Order[Delivery Date] > Purchase_Order[Expected Delivery Date]))
Quantity Delivered Variance = 
SUM(Purchase_Order[Quantity Delivered]) - SUM(Purchase_Order[Quantity Ordered])
Monthly Lead Time Trend = 
VAR SelectedMonth = SELECTEDVALUE(Dates[Month])
RETURN
    CALCULATE(
        [Avg Lead Time (Days)],
        FILTER(
            ALL(Dates),
            Dates[Month] = SelectedMonth
        )
    )


2. Supplier Performance Dashboard
Key Findings:

Top suppliers identified (Melendez Garcia, Smith and Sons, etc.)

Lead time and quantity delivered trends visualized

Delivery delays tracked by supplier (Elliott and Sons, Lutz and Sons)

Notable Feature: Includes regional breakdown (Ashanti, Eastern, Greater Accra regions)

![Image](https://github.com/user-attachments/assets/56c0b961-5bc8-4cbf-875b-dbaeea5455b6)




                    DAX functions-- Procurement
Total Procurement Cost = SUM(Purchase_Order[Order Cost])
Avg Unit Cost = DIVIDE([Total Procurement Cost], SUM(Purchase_Order[Quantity Ordered]), 0)
Orders Completed = COUNTROWS(FILTER(Purchase_Order, Purchase_Order[Status] = "Completed"))
Orders Pending = COUNTROWS(FILTER(Purchase_Order, Purchase_Order[Status] = "Pending"))
% Orders On Time = 
DIVIDE(
    [Orders Completed],
    COUNTROWS(Purchase_Order),
    0
)
Procurement Cost Trend = 
VAR SelectedQuarter = SELECTEDVALUE(Dates[Quarter])
RETURN
    CALCULATE(
        [Total Procurement Cost],
        FILTER(
            ALL(Dates),
            Dates[Quarter] = SelectedQuarter
        )
    )

3. Procurement Dashboard
Key Metrics:

Average unit cost: GH₵216

Total procurement cost: GH₵11M

Only 0.04% orders on time (critical issue)

Valuable Visuals:

Procurement cost trend (peaked at GH₵1.2M)

Product-wise purchase quantity trends

Order status breakdown (38.76% completed)


![Image](https://github.com/user-attachments/assets/26a9d917-3a40-49ad-aad1-0bfe1070869f)



                  DAX functions -- Sales

4. Sales & Fulfillment Dashboard
Performance Indicators:

Total sales revenue: GH₵402K

Quantity sold: 1,479 units

Very low order fulfillment rate (1%)

Insightful Visuals:

Regional sales trends (Ashanti, Eastern regions)

Quarterly revenue breakdown (up to GH₵200K)

Interconnected Storytelling
The dashboards work together to reveal:

Poor supplier performance (0.04% on-time orders) leads to...

Inventory challenges (stockouts) which impacts...

Sales fulfillment (only 1% orders fulfilled) while...

Procurement costs remain high (GH₵11M total)

![Image](https://github.com/user-attachments/assets/615af558-f4a7-4ba1-b9f7-3d63d98a0e03)


Recommendations
Supplier Management: Address the critical supplier performance issues (particularly with Elliott and Sons, Lutz and Sons)

Inventory Optimization: Focus on replenishment strategies for categories with lowest stock (Beauty & Personal Care at 2.4K)

Process Improvement: Investigate the extremely low order fulfillment percentage (1%)

Cost Control: Analyze the procurement cost trends to identify savings opportunities

The dashboard system successfully transforms fragmented Excel data into actionable insights across the supply chain, though some metrics (like the 0.04% on-time orders) suggest severe operational challenges that require immediate attention.
