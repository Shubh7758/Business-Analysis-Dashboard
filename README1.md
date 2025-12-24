# Business Analysis Dashboard - Power BI

A comprehensive business intelligence solution for analyzing sales, customer behavior, shipping performance, and employee productivity.

## 📊 Dashboard Overview

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

## 🚀 Getting Started

### Prerequisites
- Power BI Desktop (latest version)
- Windows 10/11
- Basic understanding of DAX and Power BI

### Installation

1. **Clone this repository**
```bash
git clone https://github.com/yourusername/business-analysis-dashboard.git
cd business-analysis-dashboard
```

2. **Open the Power BI file**
```
Open "Business Analysis Dashboard.pbix" in Power BI Desktop
```

3. **Refresh the data**
- Click "Refresh" in the Home ribbon
- Verify all tables load successfully

### Data Sources

This dashboard connects to the following data sources:
- Customer database (CSV/Excel/SQL)
- Order management system
- Product catalog
- Shipping tracker
- HR employee database

**Note:** Update data source connections in Power BI Desktop under Home → Transform Data → Data Source Settings.

## 🔧 Customization Guide

### Adding New Measures

1. **Navigate to Modeling tab**
2. **Select appropriate table**
3. **Click "New Measure"**
4. **Write DAX formula**

Example - Profit Margin:
```dax
Profit Margin = 
DIVIDE(
    [Total Revenue] - [Total Cost],
    [Total Revenue],
    0
)
```

### Creating New Relationships

Use the Model view to:
1. Drag from one table's column to another
2. Configure cardinality (One-to-Many, Many-to-One)
3. Set cross-filter direction (Single or Both)

### Adding Calculated Columns

Example - Order Priority:
```dax
Order Priority = 
SWITCH(
    TRUE(),
    Orders[TotalPrice] > 2000, "High",
    Orders[TotalPrice] > 1000, "Medium",
    "Low"
)
```

## 📐 Best Practices

### Data Modeling
- ✅ Use star schema design (facts and dimensions)
- ✅ Create date tables for time intelligence
- ✅ Minimize bidirectional relationships
- ✅ Use integer keys for relationships
- ✅ Hide technical columns from report view

### DAX Optimization
- ✅ Use variables to store intermediate calculations
- ✅ Prefer CALCULATE over FILTER when possible
- ✅ Use SELECTEDVALUE for single-value contexts
- ✅ Avoid calculated columns when measures suffice
- ✅ Test measure performance with DAX Studio

### Report Design
- ✅ Follow consistent color schemes
- ✅ Use appropriate chart types for data
- ✅ Include KPI cards for key metrics
- ✅ Add slicers for interactive filtering
- ✅ Optimize for mobile viewing

## 🔍 Advanced Analytics

### Time Intelligence
Enable time-based analysis by creating a proper date table:

```dax
Calendar = 
ADDCOLUMNS(
    CALENDAR(DATE(2020,1,1), DATE(2025,12,31)),
    "Year", YEAR([Date]),
    "Month", FORMAT([Date], "MMM"),
    "Quarter", "Q" & FORMAT([Date], "Q"),
    "MonthNum", MONTH([Date])
)
```

### Year-over-Year Growth
```dax
YoY Revenue Growth = 
VAR CurrentRevenue = [Total Revune]
VAR PreviousYearRevenue = 
    CALCULATE(
        [Total Revune],
        SAMEPERIODLASTYEAR('Calendar'[Date])
    )
RETURN
    DIVIDE(
        CurrentRevenue - PreviousYearRevenue,
        PreviousYearRevenue,
        0
    )
```

### Customer Segmentation
```dax
Customer Segment = 
VAR CustomerRevenue = [Revenue by Customer]
RETURN
    SWITCH(
        TRUE(),
        CustomerRevenue > 5000, "VIP",
        CustomerRevenue > 2000, "High Value",
        CustomerRevenue > 500, "Regular",
        "New"
    )
```

## 🐛 Troubleshooting

### Common Issues

**Issue:** Relationships not working
- **Solution:** Check cardinality and filter direction; ensure key columns have matching data types

**Issue:** Measures returning blank
- **Solution:** Verify filter context; use CALCULATE to modify context; check for BLANK() handling

**Issue:** Slow report performance
- **Solution:** Reduce visual count per page; optimize DAX measures; use aggregations; enable query reduction

**Issue:** Date filtering not working
- **Solution:** Mark date table with "Mark as Date Table"; ensure continuous date range; check relationships

## 📚 Resources

### Power BI Documentation
- [Power BI Desktop Documentation](https://docs.microsoft.com/power-bi/desktop-what-is-desktop)
- [DAX Reference](https://docs.microsoft.com/dax/)
- [Power BI Best Practices](https://docs.microsoft.com/power-bi/guidance/)

### Learning Resources
- [SQLBI - DAX Guide](https://dax.guide/)
- [Power BI Community](https://community.powerbi.com/)
- [Guy in a Cube YouTube Channel](https://www.youtube.com/c/GuyinaCube)

### Tools
- [DAX Studio](https://daxstudio.org/) - DAX query and performance tool
- [Tabular Editor](https://tabulareditor.com/) - Advanced model editing
- [Power BI Helper](https://powerbihelper.com/) - Documentation generator

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

### Contribution Guidelines
- Follow DAX formatting standards
- Document all new measures and calculations
- Test thoroughly before submitting
- Update documentation for any schema changes

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 📞 Support

For questions or issues:
- Open an issue on GitHub
- Contact: [your.email@company.com](mailto:your.email@company.com)
- Documentation: [Wiki](https://github.com/yourusername/business-analysis-dashboard/wiki)

## 🙏 Acknowledgments

- Power BI community for best practices
- SQLBI for DAX patterns
- Contributors and testers

---

**Version:** 1.0.0  
**Last Updated:** December 2025  
**Maintained by:** Your Name/Team
