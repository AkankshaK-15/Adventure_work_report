## 📐 Measures Used in the Report

Below are the key DAX measures created for this Power BI report:

### 🔹 Core Metrics

Total Orders = DISTINCTCOUNT('Sales Data'[OrderNumber])

Total Customers = DISTINCTCOUNT('Sales Data'[CustomerKey])

Total Cost = SUMX('Sales Data',
                  'Sales Data'[OrderQuantity] * 
                  RELATED('Product Lookup'[ProductCost])
                 )

Total Revenue = SUMX('Sales Data',
                     'Sales Data'[OrderQuantity] *
                     RELATED('Product Lookup'[ProductPrice])
                   )

Total Profit = [Total Revenue] - [Total Cost]

Total Returns = COUNT('Returns Data'[ReturnQuantity])

### 🔹 Percentage & Ratios

All Orders = CALCULATE([Total Orders], ALL('Sales Data'))

All Returns = CALCULATE([Total Returns], ALL('Returns Data'))

% Of All Orders = DIVIDE([Total Orders],[All Orders])

% Of All Returns = DIVIDE([Total Returns],[All Returns])

Average Retail Price = AVERAGE('Product Lookup'[ProductPrice])

Average Revenue Per Customer = DIVIDE([Total Revenue],[Total Customers])

Quantity Returned = SUM('Returns Data'[ReturnQuantity])

Return Rate = DIVIDE([Quantity Returned],[Quantity Sold], "No Sales")

Overall Average Price = CALCULATE([Average Retail Price], ALL('Product Lookup'))

### 🔹 Time Intelligence

Previous Month Orders = CALCULATE([Total Orders],
                                  DATEADD('Calendar Lookup'[Date],-1,MONTH))

Previous Month Revenue = CALCULATE([Total Revenue],
                                   DATEADD('Calendar Lookup'[Date],-1,MONTH))

Previous Month Profit = CALCULATE([Total Profit],
                                  DATEADD('Calendar Lookup'[Date],-1,MONTH))

Previous Month Returns = CALCULATE([Total Returns],
                                   DATEADD('Calendar Lookup'[Date],-1,MONTH))

10-day Rolling Revenue = CALCULATE([Total Revenue],
                                   DATESINPERIOD('Calendar Lookup'[Date],
                                   MAX('Calendar Lookup'[Date]),-10,DAY))

90-Day Rolling Profit = CALCULATE([Total Profit],
                                  DATESINPERIOD('Calendar Lookup'[Date],
                                  LASTDATE('Calendar Lookup'[Date]),-90,DAY))

### 🔹 Targets & Gaps

Order Target = [Previous Month Orders] * 1.1
Profit Target = [Previous Month Profit] * 1.1
Revenue Target = [Previous Month Revenue] * 1.1

Order Target Gap = [Total Orders] - [Order Target]
Profit Target Gap = [Total Profit] - [Profit Target]
Revenue Target Gap = [Total Revenue] - [Revenue Target]

### 🔹 Scenario / Parameter-based

Adjusted Price = [Average Retail Price] * 
                 (1 + 'Price Adjustment Parameter'[Price Adjustment Parameter Value])

Adjusted Revenue = SUMX('Sales Data',
                        'Sales Data'[OrderQuantity] * [Adjusted Price])

Adjusted Profit = [Adjusted Revenue] - [Total Cost]




