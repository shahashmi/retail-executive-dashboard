# Retail Executive Dashboard — Power BI

An interactive executive dashboard built in Microsoft Power BI on 541,909 online retail
transactions (UCI Online Retail dataset, Dec 2010–Dec 2011).

**Live interactive version:** https://claude.ai/artifact/QR2dNM9aimnhNzEcAgr9FS

## The problem I solved

The original report had 4 separate summary tables with no relationships between them, so
clicking one visual didn't filter any of the others. I rebuilt the data model as a
**star schema** so every visual cross-filters every other one.

## Data model

- **Fact_Sales** (13,103 rows): Month, Country, CustomerID, Revenue, Quantity,
  aggregated from 541,909 raw transactions
- **Dim_Country**: Country → Region
- **Dim_Month**: Month, Label, Year, MonthNo
- One-to-many relationships from each dimension to the fact table, single-direction filtering

## DAX measures

```DAX
Total Revenue = SUM ( Fact_Sales[Revenue] )
Units Sold    = SUM ( Fact_Sales[Quantity] )
Customers     = DISTINCTCOUNT ( Fact_Sales[CustomerID] )

Top10 Revenue =
    VAR CurrentRank = RANKX ( ALL ( Fact_Sales[CustomerID] ), [Total Revenue], , DESC )
    RETURN IF ( CurrentRank <= 10, [Total Revenue], BLANK() )
```

## Dashboard

- 3 KPI cards: Total Revenue, Units Sold, Customers
- **Revenue Trend**: monthly revenue line chart
- **Country Revenue**: Top 10 countries by revenue
- **Customer Revenue**: Top 10 customers, ranked with the RANKX measure
- **Regional Demand**: units sold by region
- Region slicer, with the United Kingdom excluded to make smaller markets visible

## Validation

I checked the rebuilt model against the original report. For example, Netherlands
revenue reconciles exactly at £285,446.

## Files

- `Retail_Executive_Dashboard.pbix`: the Power BI file (open it with Power BI Desktop, which is free)
- `Retail_Dashboard_Model_Data.xlsx`: the star-schema source data
- `dashboard_screenshot.png`: a preview of the dashboard

## Tools

Microsoft Power BI Desktop, Power Query, DAX, Excel
