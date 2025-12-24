# Business Analysis Dashboard - Power BI
[![Power BI](https://img.shields.io/badge/Power%20BI-Desktop-yellow.svg)](https://powerbi.microsoft.com/)
[![Data](https://img.shields.io/badge/Data-CSV-blue.svg)]()

 A Power BI business intelligence solution that tracks sales, customers, orders, shipping, and employee performance. It provides end-to-end visibility into your business operations

## 🎯 Key Capabilities

- 📊 **Real-time Revenue Tracking** - Monitor sales performance with live DAX calculations

- 📈 **Trend Analysis** - Analyze revenue, orders, and customer patterns over time

- 🎯 **KPI Monitoring** - Track critical business metrics with dynamic measures

- 📉 **Performance Benchmarking** - Compare categories, products, and time periods

- 🔍 **Drill-down Analysis** - Navigate from high-level summaries to detailed transactions

- 👥 **Customer Segmentation** - Analyze customers by revenue, location, and behavior

- 📍 **Geographic Analysis** - View customer distribution and regional performance

- 🛒 **Order Volume Tracking** - Monitor transaction counts and patterns

- 📦 **Product Performance** - Identify top-selling items and categories

- 💳 **Average Order Value** - Calculate and track transaction sizes

- 📊 **Category Comparison** - Compare Electronics, Furniture, and Office Supplies performance




## 📊 Dashboard Overview

 ![Dashboard Preview](https://github.com/Shubh7758/Business-Analysis-Dashboard/blob/main/Business%20Performance%20Dashboard.PNG)

This Power BI dashboard provides end-to-end visibility into business operations with focus on:
- **Sales Performance** - Track revenue trends and product performance
- **Customer Analytics** - Monitor customer behavior and repeat purchases
- **Order Management** - Analyze order patterns and fulfillment
- **Shipping Operations** - Track delivery status and carrier performance
- **Employee Productivity** - Measure workforce efficiency

## 🏗️ Data Model Architecture

### Tables & Relationships

```
Customers (1) ──────→ (*) Orders (*) ──────→ (1) Products
     ↓                      ↓
     |                      ↓
     |              (1) Shipping (1) ──────→ (1) People
     |                      ↓
     └──────────────→ DateTable
```

### Table Details

| Table | Columns | Measures | Purpose |
|-------|---------|----------|---------|
| **Customers** | 4 | 2 | Customer master data |
| **Orders** | 7 | 2 | Transaction records |
| **Products** | 4 | 0 | Product catalog |
| **Shipping** | 6 | 0 | Delivery tracking |
| **People** | 4 | 1 | Employee information |

## 📈 Key Metrics & Measures

### Revenue Metrics
```dax
Total Revune = SUM(Orders[TotalPrice])
```
**Description:** Primary revenue metric tracking total sales across all orders
**Usage:** Revenue dashboards, trend analysis, KPI cards

```dax
Revenue by Customer = 
CALCULATE(
    SUM(Orders[TotalPrice]),
    ALLEXCEPT(Customers, Customers[CustomerID])
)
```
**Description:** Customer-level revenue calculation for segmentation analysis
**Usage:** Customer profitability analysis, top customer reports

### Order Metrics
```dax
Total Orders = COUNT(Orders[OrderID])
```
**Description:** Count of all orders in the system
**Usage:** Order volume tracking, capacity planning

### Customer Metrics
```dax
Repeat Customers = 
CALCULATE(
    DISTINCTCOUNT(Customers[CustomerID]),
    FILTER(
        VALUES(Customers[CustomerID]),
        COUNTROWS(RELATEDTABLE(Orders)) > 1
    )
)
```
**Description:** Identifies customers with multiple orders for loyalty analysis
**Usage:** Customer retention metrics, loyalty programs

### Employee Metrics
```dax
OrdersPerEmployee = COUNTROWS(RELATEDTABLE(Orders))
```
**Description:** Measures individual employee order processing volume
**Usage:** Performance reviews, workload balancing

## 🗂️ Data Schema

### Customers Table
| Column | Type | Description |
|--------|------|-------------|
| CustomerID | Integer | Unique customer identifier (Primary Key) |
| CustomerName | Text | Full customer name |
| Email | Text | Customer email address |
| Location | Text | Customer geographic location |

**Relationships:**
- CustomerID → Orders[CustomerID] (One-to-Many)

### Orders Table
| Column | Type | Description |
|--------|------|-------------|
| OrderID | Integer | Unique order identifier (Primary Key) |
| CustomerID | Integer | Foreign key to Customers |
| ProductID | Integer | Foreign key to Products |
| Quantity | Integer | Number of items ordered |
| TotalPrice | Integer | Total order value |
| OrderDate | Text | Order creation date |
| DaysToShip | Integer (Calculated) | Calculated shipping duration |

**Relationships:**
- CustomerID → Customers[CustomerID] (Many-to-One)
- ProductID → Products[ProductID] (Many-to-One)
- OrderID → Shipping[OrderID] (Many-to-One)

### Products Table
| Column | Type | Description |
|--------|------|-------------|
| ProductID | Integer | Unique product identifier (Primary Key) |
| ProductName | Text | Product display name |
| Category | Text | Product category (Electronics, Furniture, Office Supplies) |
| Price | Integer | Unit price |

**Relationships:**
- ProductID → Orders[ProductID] (One-to-Many)

### Shipping Table
| Column | Type | Description |
|--------|------|-------------|
| ShippingID | Integer | Unique shipping record (Primary Key) |
| OrderID | Integer | Foreign key to Orders |
| Carrier | Text | Shipping carrier name |
| Status | Text | Delivery status (Delivered, Shipped, In Transit, Pending) |
| ShippingDate | DateTime | Shipment date |
| oOnTime | Text (Calculated) | On-time delivery flag |

**Relationships:**
- OrderID → Orders[OrderID] (One-to-Many)
- OrderID → People[OrderID] (One-to-One, Bidirectional)
- ShippingDate → DateTable[Date] (Many-to-One)

### People Table
| Column | Type | Description |
|--------|------|-------------|
| EmployeeID | Integer | Unique employee identifier (Primary Key) |
| EmployeeName | Text | Employee full name |
| Role | Text | Job role/position |
| OrderID | Integer | Foreign key to Shipping |

**Relationships:**
- OrderID → Shipping[OrderID] (One-to-One, Bidirectional)

## 📊 Business Insights (Current Data)

### Revenue Analysis
- **Total Revenue:** $40,460
- **Top Category:** Electronics ($24,330 - 60% of revenue)
- **Average Order Value:** $1,156

### Category Performance
| Category | Revenue | Orders | Avg Order Value |
|----------|---------|--------|-----------------|
| Electronics | $24,330 | 23 | $1,058 |
| Office Supplies | $8,700 | 4 | $2,175 |
| Furniture | $7,430 | 8 | $929 |

### Shipping Performance
| Status | Orders | Revenue |
|--------|--------|---------|
| Delivered | 15 (43%) | $19,030 |
| Shipped | 13 (37%) | $9,790 |
| In Transit | 4 (11%) | $7,810 |
| Pending | 3 (9%) | $3,830 |

**Key Insight:** 80% of orders are either delivered or shipped, indicating strong fulfillment performance.

## 🎯 Use Cases

### 1. Sales Performance Dashboard
Track revenue trends by category, identify top-performing products, and monitor order volumes.

**Key Visualizations:**
- Revenue trend over time
- Category breakdown (pie/donut chart)
- Top 10 products by revenue
- Order volume by date

### 2. Customer Analytics
Analyze customer behavior, identify repeat customers, and segment by location.

**Key Visualizations:**
- Repeat customer percentage
- Customer location map
- Revenue by customer
- Customer lifetime value

### 3. Operational Efficiency
Monitor shipping performance, track delivery status, and measure employee productivity.

**Key Visualizations:**
- Shipping status breakdown
- On-time delivery rate
- Orders per employee
- Days to ship distribution

### 4. Product Analysis
Evaluate product performance, category trends, and pricing effectiveness.

**Key Visualizations:**
- Category performance matrix
- Product revenue ranking
- Price vs quantity sold
- Category mix over time

### Data Sources

This dashboard connects to the following data sources:
- Customer database (CSV/Excel/SQL)
- Order management system
- Product catalog
- Shipping tracker
- HR employee database


## 📞 Support

- Contact: [shubhamshevare5@gmail.com]
- LinkedIn: https://www.linkedin.com/in/shubham-shevare-b04a621a0
## 📊 Sample Visuals

 ![Dashboard Preview](https://github.com/Shubh7758/Business-Analysis-Dashboard/blob/main/customer%20Analysis%20photo.PNG)
  ![Dashboard Preview](https://github.com/Shubh7758/Business-Analysis-Dashboard/blob/main/Product%20Analysis%20Photo.PNG)
  ![Dashboard Preview](https://github.com/Shubh7758/Business-Analysis-Dashboard/blob/main/Shipping%20Analysis%20Photo.PNG)
  ![Dashboard Preview](https://github.com/Shubh7758/Business-Analysis-Dashboard/blob/main/Employee%20Analysis%20Photo.PNG)
