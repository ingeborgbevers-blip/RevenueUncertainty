Below is an updated README following the structure of your existing portfolio template, but tailored to the **Revenue Is Only Part of the Margin Story** case study. It reflects the dashboard figures, business question, exception logic and limitations in the finished PDF.  The structure also remains consistent with your existing GitHub project format. 

````markdown
# Revenue Is Only Part of the Margin Story

![Power BI Dashboard](Revenue%20uncertainty%20dashboard.png)

## Overview

This project explores how apparently healthy revenue can coexist with operational activity that places pressure on profit margin.

Using the public Global Superstore dataset from Kaggle, the analysis looks beneath headline sales and profit to examine the exceptions surrounding individual orders.

These include:

- discounts,
- high shipping costs,
- negative-profit order lines,
- and orders associated with return records.

The project began with a straightforward question about revenue and profitability.

The headline figures appeared positive:

- 12.64M reported revenue
- 1.47M reported profit
- 11.61% overall profit margin

However, the operational detail presented a less settled picture.

The analysis identified that:

- 7.66% of orders were associated with a return record;
- and 78.10% of orders contained at least one defined exception.

This shifted the focus from:

> How much revenue did the business generate?

to:

> Which sales still look commercially worthwhile once operational exceptions are taken into account?

The project demonstrates a practical business intelligence workflow:

Excel source data → Power Query preparation → Power BI data model → exception measures → dashboard visuals → executive case study

It is designed as a reporting and decision-support case study relevant to:

- SME management reporting,
- margin and profitability analysis,
- operational sales reporting,
- ERP and SAP-style exception reporting,
- finance and commercial decision support,
- and Power BI portfolio work.

---

# The Problem

Headline revenue reports do not always show the full commercial result of an order.

A sale may contribute positively to revenue while carrying:

- a substantial discount,
- unusually high shipping costs,
- a negative-profit line,
- a return,
- or several exceptions at the same time.

These conditions are not automatically evidence of poor management.

A discount may protect an important customer relationship.

A high-cost shipment may meet a valuable service commitment.

A low-margin order may support a wider commercial agreement.

The reporting problem begins when the business can see that an exception occurred, but cannot explain:

- why it occurred,
- whether it was authorised,
- what financial value it affected,
- whether it was commercially justified,
- or whether the same issue is repeating.

This creates a familiar management concern:

> The revenue is there, but the profit does not feel like it follows.

The dashboard therefore does not treat every exception as avoidable loss.

Instead, it identifies where margin pressure may exist and where further investigation would be commercially useful.

---

# Business Question

The main business question was:

> Which products, customers, regions and order types appear profitable at headline level, but become low-margin or loss-making once discounts, shipping costs, returns and other operational exceptions are taken into account?

Supporting questions included:

- Which exception types are most strongly associated with negative-profit orders?
- Where are discounts being used deliberately, and where have they become routine?
- Which customers or product groups generate revenue but repeatedly carry weak margins?
- Are high shipping costs caused by service promises, order size, product characteristics or process decisions?
- Which return-affected orders can be traced to particular products, quantities and reasons?
- Are the same exceptions recurring in particular markets, teams or transaction types?

These questions move the discussion from how much was sold to what the business retained from the sale, and why.

---

# The Dataset

The project uses the Global Superstore dataset published on Kaggle:

https://www.kaggle.com/datasets/rohitgrewal/global-superstore-data

The source workbook includes order, return and supporting reference data.

The Orders table contains fields such as:

- Order ID
- Order Date
- Ship Date
- Ship Mode
- Customer
- Segment
- Market
- Region
- Product
- Category
- Sub-Category
- Sales
- Quantity
- Discount
- Profit
- Shipping Cost
- Order Priority

The Returns table identifies orders associated with return records.

This supports broad analysis of return exposure, but it does not always identify the specific order line, product or quantity returned.

That limitation is important to the interpretation of the dashboard.

---

# The Approach

The analysis was developed in stages.

## Stage 1: Headline Commercial Position

The first stage established the overall reported position:

- reported revenue,
- reported profit,
- profit margin,
- return record count,
- return-affected order rate,
- and order exception rate.

This created a clear starting point for comparing headline performance with the operational detail underneath it.

## Stage 2: Define Operational Exceptions

Three line-level exception conditions were created:

### Discount Exception

An order line was flagged where the recorded discount was greater than zero.

### High Shipping-Cost Exception

An order line was flagged where shipping cost exceeded 10% of reported sales.

This is an analytical threshold used for the case study rather than a universal commercial rule.

A real business should define its threshold according to:

- product economics,
- delivery agreements,
- customer terms,
- destination,
- service level,
- and normal freight patterns.

### Negative-Profit Exception

An order line was flagged where reported profit was below zero.

## Stage 3: Move From Lines to Orders

The analysis distinguished between:

- the number of exception lines;
- and the number of distinct orders affected by an exception.

This matters because one order can contain several order lines and several different exception types.

The order exception rate measures the proportion of distinct orders containing at least one of the following:

- a discounted line,
- a high shipping-cost line,
- or a negative-profit line.

An order containing more than one exception is counted once in the overall order exception rate.

It may still appear in more than one exception category when the individual exception measures are displayed.

## Stage 4: Analyse the Pattern

The dashboard compared:

- revenue against sales on return-affected orders;
- exception volumes by market;
- return exposure by region;
- and the occurrence of discounts, shipping-cost exceptions and negative-profit lines.

The purpose was not to declare one market good or bad.

It was to identify where the available data creates a reason to investigate further.

---

# Power BI Measures

The project includes measures for:

- Reported Revenue
- Reported Profit
- Profit Margin %
- Order Count
- Return Record Count
- Returned Orders
- Return-Affected Order Rate %
- Sales on Return-Affected Orders
- Discounted Line Count
- Negative-Profit Line Count
- High-Shipping-Cost Line Count
- Orders with Discounts
- Orders with Negative-Profit Lines
- Orders with High Shipping Cost
- Orders with Any Line Exception
- Order Exception Rate %

Example core measures:

```DAX
Reported Revenue =
SUM ( Orders[Sales] )
````

```DAX
Reported Profit =
SUM ( Orders[Profit] )
```

```DAX
Profit Margin % =
DIVIDE (
    [Reported Profit],
    [Reported Revenue],
    0
)
```

```DAX
Order Count =
DISTINCTCOUNT ( Orders[Order ID] )
```

```DAX
Orders with Discounts =
CALCULATE (
    DISTINCTCOUNT ( Orders[Order ID] ),
    Orders[Discount] > 0
)
```

```DAX
Orders with Negative-Profit Lines =
CALCULATE (
    DISTINCTCOUNT ( Orders[Order ID] ),
    Orders[Profit] < 0
)
```

```DAX
Orders with High Shipping Cost =
CALCULATE (
    DISTINCTCOUNT ( Orders[Order ID] ),
    Orders[Shipping Cost] > Orders[Sales] * 0.10
)
```

```DAX
Orders with Any Line Exception =
CALCULATE (
    DISTINCTCOUNT ( Orders[Order ID] ),
    FILTER (
        Orders,
        Orders[Discount] > 0
            || Orders[Profit] < 0
            || Orders[Shipping Cost] > Orders[Sales] * 0.10
    )
)
```

```DAX
Order Exception Rate % =
DIVIDE (
    [Orders with Any Line Exception],
    [Order Count],
    0
)
```

---

# Dashboard Highlights

## Executive Summary

The dashboard reports:

* 12.64M reported revenue
* 1.47M reported profit
* 11.61% profit margin
* approximately 2K return records
* 7.66% return-affected order rate
* 78.10% order exception rate

Taken alone, the revenue, profit and margin figures suggest a reasonably healthy commercial position.

The exception measures show that a significant proportion of everyday trading activity contains at least one condition that may warrant further explanation.

This does not mean that 78.10% of orders are defective or unprofitable.

It means that 78.10% contain at least one discount, high shipping-cost line or negative-profit line under the definitions used in this case study.

## Revenue and Return Exposure

The scatter chart compares:

* reported revenue,
* revenue on return-affected orders,
* average discount,
* market,
* and customer segment.

The market-level view provides a clear opening picture before moving into more detailed regional analysis.

It also shows why absolute return-affected revenue should not be interpreted without context.

Markets with more revenue and more orders will often carry more return-affected revenue simply because of their scale.

## Revenue on Return-Affected Orders by Region

The regional chart compares reported revenue with revenue associated with orders carrying a return record.

This identifies where return exposure appears in the order base.

However, it does not represent confirmed returned sales value.

The Returns table does not contain enough line-level detail to identify precisely which product, quantity or order line was returned in every case.

The measure should therefore be interpreted as:

> Revenue on orders affected by a return record.

It should not be interpreted as:

> The value of goods returned.

## Order Exceptions by Market

The exception chart compares total order count with:

* orders containing discounts,
* orders containing high shipping-cost lines,
* and orders containing negative-profit lines.

This helps separate the causes behind the overall exception rate.

It also shows that different exception types overlap.

An order can contain a discount and a high shipping-cost line while also reporting negative profit.

The exception categories should therefore not be added together as though they represent separate orders.

---

# Key Findings

## 1. Headline Performance Does Not Explain the Whole Margin Position

The business reports positive revenue, profit and overall margin.

However, a substantial proportion of orders contain operational conditions capable of weakening the value retained from the sale.

Revenue alone is therefore not enough to explain commercial performance.

## 2. Exceptions Are Part of Everyday Trading Activity

The 78.10% order exception rate indicates that discounts, shipping-cost pressure or negative-profit lines are present across much of the order base.

This does not prove widespread avoidable loss.

It does show that exception handling is not a marginal issue affecting only a few unusual transactions.

## 3. Exception Volume Is Influenced by Business Scale

Larger markets generally contain more orders and therefore more exception-affected orders.

Absolute exception counts should be considered alongside:

* order volume,
* revenue,
* profit,
* exception rates,
* and financial value.

Without this context, the largest market can appear to be the worst simply because it processes the most activity.

## 4. Frequency and Financial Impact Are Different Questions

A frequent exception may have little financial impact.

A small number of high-value exceptions may matter considerably more.

The next analytical stage should measure:

* revenue affected,
* profit affected,
* estimated margin loss,
* shipping cost as a proportion of sales,
* and the value of negative-profit orders.

## 5. Return Traceability Is Limited

The dataset identifies return-affected orders but does not reliably show which individual lines were returned.

Where an order contains several lines and more than one return record, the available data cannot determine which products or quantities were involved.

Assigning the return to a specific line would therefore create false precision.

## 6. The Dashboard Identifies Where to Investigate, Not the Full Cause

The available data can reveal patterns and exceptions.

It cannot fully explain:

* why a discount was approved,
* whether shipping cost was commercially justified,
* whether a low-margin order supported a wider contract,
* why an item was returned,
* or whether a manual correction took place elsewhere.

Those questions require additional system data and discussion with the people involved in the process.

---

# Recommended Next Steps

## 1. Establish a Clear Exception Definition

Agree which conditions genuinely require review.

This should include business-specific thresholds for:

* discounts,
* acceptable margin,
* shipping cost,
* returns,
* cancelled lines,
* credit notes,
* rework,
* substitutions,
* and manual corrections.

The purpose is to separate normal commercial activity from genuine exceptions.

## 2. Measure Financial Value, Not Only Frequency

Add measures for:

* revenue associated with each exception,
* profit associated with each exception,
* value of negative-profit orders,
* shipping cost as a percentage of revenue,
* estimated discount impact,
* and combinations of exceptions.

This will distinguish frequent low-value issues from less common but commercially significant cases.

## 3. Improve Return Traceability

A stronger return process should capture:

* order-line identifier,
* product,
* quantity returned,
* return date,
* return reason,
* credit value,
* condition of returned goods,
* and final outcome.

Without this detail, the business can identify return exposure but cannot calculate the exact financial effect reliably.

## 4. Link Exceptions to Operational Causes

Useful reason codes might include:

### Discounts

* contract pricing
* promotion
* volume agreement
* manual override
* complaint resolution
* damaged goods
* discretionary sales decision

### High Shipping Costs

* urgent shipment
* split delivery
* remote destination
* small order value
* service recovery
* customer-specific delivery terms

### Negative Profit

* discount
* cost increase
* freight
* incorrect product cost
* return
* credit
* rework
* pricing error

This moves the analysis from identifying symptoms to understanding causes.

## 5. Analyse Combinations of Exceptions

Priority combinations may include:

* discount plus high shipping cost,
* discount plus negative profit,
* return-affected order plus discount,
* high shipping cost plus negative profit,
* and multiple exception types on the same customer or product.

The greatest margin pressure may be created by several individually reasonable decisions occurring on the same order.

## 6. Move From Market Totals to Decision-Level Detail

Further analysis should consider:

* customer,
* product,
* category,
* order type,
* sales channel,
* salesperson,
* warehouse,
* fulfilment route,
* month,
* and quarter.

The purpose is not to assign blame.

It is to identify which commercial or operational decision is creating the result.

## 7. Review the Highest-Value Cases Manually

Select the highest-value exception-affected orders and review them with:

* finance,
* sales,
* purchasing,
* customer service,
* and operations.

For each order, ask:

* Was the exception expected?
* Was it authorised?
* Was the reason recorded?
* Could it have been prevented?
* Was the commercial benefit worth the cost?
* Is the same pattern likely to repeat?

A small, structured review can provide more useful understanding than simply adding another dashboard page.

---

# Data Limitations

This project is based on a public portfolio dataset and should not be treated as a complete margin audit.

The analysis does not prove:

* avoidable loss,
* poor commercial judgement,
* incorrect accounting,
* failed operational control,
* or that every exception requires corrective action.

The dataset does not include sufficient detail about:

* discount authorisation,
* supplier cost changes,
* customer contracts,
* delivery agreements,
* return reasons,
* returned quantities,
* credit notes,
* refunds,
* cancellations,
* rework,
* substitutions,
* or manual adjustments.

The high-shipping-cost threshold is an analytical assumption used for this case study.

A real implementation should replace it with a business-approved definition based on normal shipping economics.

The return data is available at order level but does not provide reliable line-level traceability.

The dashboard can therefore identify where further investigation may be useful. It cannot determine the complete operational cause from the supplied data alone.

---

# Conclusion

The analysis shows that positive revenue and profit totals can coexist with widespread operational exceptions.

The immediate priority is not to remove every discount, return or high-cost shipment.

It is to distinguish the exceptions that support the business from those that quietly weaken it.

> Revenue tells us that the order happened. The operational detail tells us whether it was worth having.

---

# Repository Contents

The repository can include:

* Power BI report file
* Power BI dashboard screenshots
* executive case study PDF
* dashboard PDF export
* DAX measure notes
* source dataset notes
* README
* and project images for GitHub and Kaggle

Suggested file structure:

```text
revenue-margin-story/
│
├── README.md
├── data/
│   └── source-data-notes.md
├── dashboard/
│   ├── Revenue uncertainty.pbix
│   └── Revenue uncertainty dashboard.pdf
├── case-study/
│   └── Revenue uncertainty.pdf
├── images/
│   ├── executive-summary.png
│   ├── revenue-return-exposure.png
│   └── order-exceptions-by-market.png
└── measures/
    └── dax-measures.md
```

Do not upload source data to GitHub unless the dataset licence permits redistribution.

Where necessary, direct users to the original Kaggle dataset instead.

---

# How to Use

## Requirements

* Power BI Desktop
* Microsoft Excel or another compatible spreadsheet tool
* Global Superstore source workbook from Kaggle

## Steps

1. Download or clone the repository.

```bash
git clone <repository-url>
```

2. Download the source dataset from Kaggle:

[https://www.kaggle.com/datasets/rohitgrewal/global-superstore-data](https://www.kaggle.com/datasets/rohitgrewal/global-superstore-data)

3. Save the workbook in the expected local data location.

4. Open the Power BI file.

5. Update the data-source path if required.

6. Refresh the Orders and Returns queries.

7. Confirm that the Discount field is imported as a decimal number.

A discount of 60% should have an underlying value of `0.60`.

It should not be converted to a whole number before percentage formatting.

8. Review the model relationship between the Orders and Returns tables.

9. Review the dashboard pages and executive case study.

---

# Suggested Business Use

This project demonstrates how a business can move from a standard revenue report to an exception-led margin discussion.

It is relevant where managers need to understand:

* why profit does not appear to follow revenue,
* how discounts affect order economics,
* where shipping costs create pressure,
* which orders contain negative-profit lines,
* where return exposure appears,
* and what additional data is needed to identify the root cause.

The project also demonstrates the value of not stopping when the dashboard looks complete.

The first view shows the result.

The useful work begins when the business asks what caused it.

---

# Tools Used

* Microsoft Power BI Desktop
* Power Query
* DAX
* Microsoft Excel
* Kaggle source data
* GitHub
* PDF case study export

---

# Project Status

Completed as a portfolio case study.

Potential future improvements include:

* measuring the financial value of each exception type,
* adding weighted discount analysis,
* creating customer and product exception profiles,
* analysing combinations of exceptions,
* adding time-series analysis,
* introducing business-approved exception thresholds,
* adding return reasons and line-level traceability,
* reviewing high-value exceptions manually,
* and applying the same approach to real ERP or SAP Business One data where permitted.

```

One correction before publishing: the page-one chart currently says **“Revenue and retun exposure”**. Change that to **“Revenue and return exposure by market and segment.”**
```
