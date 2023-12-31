---
title: Excel Pivot Tables
description: Pivot Tables – an essential data analysis tool in Microsoft Excel and other spreadsheet programs for summarizing, aggregating, and analyzing large datasets. This note may include information on creating, formatting, and customizing pivot tables, as well as examples of common use cases and best practices.
published: true
date: 2022-07-25T13:37:12.115Z
tags:
  - Excel
  - Software
  - Spreadsheets
  - Pivot_Tables
  - data_analysis
editor: markdown
dateCreated: 2022-09-09T04:44:01.149Z
---

- PIVOT TABLE


1. Which state had the highest population in 2002?
2. In which year was overall US population the highest?
3. Which states saw a decline in student population rate
    between 2003 and 2004?

```
Looking at a raw data set like the one here,
how would you answer the following?
```
```
What if you don’t even knowwhat
you’re looking for?
```
WHY PIVOT TABLES?


PivotTables allow you to easily organize, filter,

summarize, and analyze raw data

“Analyzing data without a Pivot is like hammering a nail with a noodle”

```
-Albert Einstein*
```
*Quote not confirmed

PIVOT TABLE 101


1 POWERFUL

FAST

2

ACCURATE

3

FLEXIBLE

4

5

BEAUTIFUL

- Uncover insights and answer key questions about your data
- Apply custom styles and conditional formatting rules to bring your Pivots to life
- Create custom views, filters, and calculated fields on the fly
- Automate calculations to minimize human error
- Manipulate table layouts and create dynamic views in seconds

KEY BENEFITS


- Rectangular (variables as columns, observations as rows)
- No extra formatting
- Contains only dimensions & measures
- Clear column headers
- No extra headers, footers, sub-totals or calculated fields

BAD!

- Transposed (variables as rows, observations as columns)
- Unnecessary formatting
- Contains calculated fields
- Confusing column header names
- Extra header rows

GOOD!

DATA STRUCTURE


INSERTING A PIVOT TABLE

(Insert PivotTable) (Insert Recommended PivotTables)

From the “Insert” menu, select PivotTableto create a blank

```
Pivot, or use the Recommended PivotTablesoption to
browse pre-populated starting points
```
```
What data are
you analyzing?
```
```
Where will the
PivotTable live?
```

```
The Field List shows all the
variables in your dataset, and
which ones are currently
included in the Pivot
```
```
If there are fields that you want
to use to filter the whole data
```
set, drag them to theFilters box

```
Variables included in the Rows
field will appear as individual
rowswithin the Pivot
```
```
Variables included in the Columns
field appear as individual columns
within the Pivot
```
```
Numerical variables are almost always
included in the Valuesfield
```
```
(These are the quantitative measures that you
care about: sales, revenue, clicks, etc.)
```
```
Layout options allow you to adjust
the look and feel of the field list
```
THE FIELD LIST


The“Analyze” Tab:

The “Design” Tab:

ANALYZE & DESIGN OPTIONS


Clearoptions allow you to
clear all fields and values
from a table, or just any filters

```
that have been applied Selectoptions (allow you to select
entire sections of the PivotTable
(or the entire table itself)
```
```
Move options allow you to
relocate an existing PivotTable to
a new worksheet or a new location
within the existing one
```
SELECTING, CLEARING & MOVING PIVOTS

```
PRO TIP:
Select Entire PivotTable, then copy
and paste to duplicate an entire Pivot
```

Refreshupdates the PivotTable based

```
on changes made withinthe defined
source data range or table
```
```
Change Data Sourceallows you
refresh the Pivot to reflect changes
outside of the defined source range
or table (i.e. new columns or rows)
```
```
PRO TIP:
Format your source data as a table to dynamically
adjust as new columns or rows are added, or use
a column-only range reference (i.e. $A:$G)
```
REFRESHING & UPDATING PIVOTS


STEP 1: Detect/evaluate coordinates

- State = Arizona
- Measure = Total Population
- Filter= All ages

STEP 2: Apply arithmetic

- Summarize Values By: AVERAGE
- (vs. SUM, COUNT, MAX, MIN, etc.)

STEP 3: Display result

- (586+859+870+1656+892)/5 = 973

```
Excel isolates relevant source data
```
```
NOTE: You can double-clickany
specific value in a Pivot to generate
a new tab showing the exact source
data used to calculate it
```
HOW DO PIVOTS ACTUALLY WORK?


# PIVOT FORMATTING


```
Right-click a column header or any individual value within
a field to change the number format (number, currency,
percentage, date, etc.)
```
```
PRO TIP:
Right click, select PivotTable Options, and select
the “Layout & Format” tab to customize how you
want to display blank or error values
```
NUMBER FORMATTING


```
Select from a range of styles
(right-click to make default), or
customize your own:
```
TABLE STYLES


CompactForm(default): OutlineForm(recommended):

VS.

- Nested fields/dimensions condensed into
    one column, with one filter option
       - Each field/dimension broken out into its own column, with
          separate column headers and filter options
       - Allows you to apply custom filters to each field (i.e. label
          filters on the Product Categoryfield and value filters on
          the Product Sub-Categoryfield)

TABLE LAYOUTS: COMPACT VS. OUTLINE


PRO TIP:

```
Use Outline Form when you are manipulating data
within a Pivot, and switch to Tabular form with repeating
labels (and no grand totals or subtotals) if you want to
create a new raw dataset
```
Tabular Form(non-repeating): Tabular Form(repeating):

TABLE LAYOUTS: TABULAR FORM


```
Conditional Formatting rules
can be applied to PivotTables just
like normal data ranges
```
(Home Conditional Formatting)

Options include:

- Text and Value-based Formats
- Data Bars
- Color Scales
- Icon Sets
- Formula-Based Rules

CONDITIONAL FORMATTING


# SORTING, FILTERING

# & GROUPING


```
More Sort Options: Label Filters: Value Filters:
```
Manual
Selections

SORTING & FILTERING

```
Hit this button (or right-click one of the values)
to drill into Sorting& Filteringoptions
```

Select values that you’d like to group
(in this case fire-related job titles)

```
Right-click and
select Group
```
```
A new field is created (“Job Title2”)
containing the new group (“Group1”)
```
```
Note: Both names can be customized
```
GROUPING DATA


```
PRO TIP:
Slicers and Timelines work just
like regular report filters, but with
user-friendly interfaces
```
InsertSlicersorTimelines

Basically a prettier
version of a filter!

```
A filter designed
specifically for dates
```
SLICERS & TIMELINES


```
(PivotTable Tools Analyze)
```
```
Year = 2011 Year = 2012 Year = 2013
```
Use the “Show Report Filter Pages”

option to create new tabs for each value

that a given filter (i.e. Year) can take

REPORT FILTER PAGES


CALCULATED VALUES & FIELDS


```
Summarize Values By determines how
numbers should be treated when they are rolled
up or aggregated (sum, count, average, max, etc.)
```
```
PRO TIP:
Excel will default to “Count Of” if a data
column contains blanks or non-numerical
values. Typically you will want to change
this field setting to “Sum Of”
```
SUMMARIZE VALUES BY


```
Show Values Asoptions allow you to
apply additional calculations to change
the way values are shown, such as the
Percent of a Total or Subtotal, Running
Value, Rank, etc.
```
```
In this case, we’re showing Order
Quantityvalues as% of Column Total,
rather than whole numbers
```
SHOW VALUES AS


SHOW VALUES AS - EXAMPLES

In this example we’re summarizing the same Revenue field 6 different ways:

```
Value
(no calculation)
```
```
% of Total
Column
```
```
% of Parent
(genre)
```
```
% Difference
(prev. year)
```
```
Running Total
(by year)
```
```
Rank
(Large Small)
```

SHOW VALUES AS - INDEX

TheIndex calculation uses an aggregated weighted average to reveal the impact of one number

within the context of a data set

```
Each Revenue number is converted to an Indexrepresenting
it’s importance within each column, using the following formula:
```
```
(Cell Value * Grand Total) / (Row Total * Column Total)
```
```
Documentaries index very high in France, meaning that a global
increase in Documentary ticket prices would impact the
French film industry significantly more than any other country
```

Calculated Fields allow you to create new measures based on existing, numerical fields:

```
In this case I’ve added a measure called % Students,
equal to Student Population / Total Population
```
```
PRO TIP:
Don’t calculate rate metrics (i.e. CTR, CPC) in your raw data, use calculated fields in your
Pivot. This ensures that they calculate properly no matter how your data is rolled up
```
CALCULATED FIELDS


Calculated fields are alwaysbased on the SUMof other fields (even if they are shown as a count,

max, average, etc.). But what if you want to make a calculation based on the COUNTof a field?

```
Ex)Create a field to calculate the
Likes per Poston each date STEP 1: Create a new “Count”
column (=1) in the source data
```
```
STEP 2: Create a calculated
field defined as Likes/Count
```
CALCULATING USING COUNTS


Calculated Items allow you to create new dimensions or categories based on existing dimensions:

```
In this case I’ve added a new category called “Kids”,
which combines Gand PGmovie ratings
```
```
PRO TIP:
DON’T USE CALCULATED ITEMS UNLESS YOU NEED TO; you’re usually better off simply
grouping fields or adding new category columns within your source data itself
```
CALCULATED ITEMS


If you’ve defined multiple calculated items, theSolve Order can be used to determine which

calculations to prioritize (value is determined by the last formula in the list)

SOLVE ORDER


The List Formulas tool produces a new tab summarizing all calculated fields and items

associated with a given Pivot, along with the current solve order

LIST FORMULAS


# PIVOT CHARTS


APivotChart is simply a chart that is tied to a specific PivotTable; as you adjust filters and

fields in your Pivot, the PivotChart updates dynamically

```
2) Select a chart type 3) The PivotChart will be inserted, and dynamically tied to the pivot
(note:you can filter the view using either the pivot table or the chart itself)
```
```
1) Select your pivot and choose PivotChart from
either the “Insert” tab or the “Analyze” tab
```
PIVOT CHART 101


PIVOT CHART OPTIONS

The“Analyze” Tab:

The“Design” Tab:

The“Format” Tab:


PIVOT CHART LAYOUTS & STYLES

Chart Layouts &Styles

allow you to adjust the look

and feel of a PivotChart,

including adding elements,

changing color palettes, or

applying pre-set templates


Field Buttonsallow you to apply or

adjust filters directly within the chart

```
PRO TIP:
You can format PivotCharts exactly like normal
Excel charts – the only difference is that
PivotCharts are dynamically tied to a PivotTable
```
PIVOT CHART FIELD BUTTONS

```
Select PivotChart Tools Analyze Field
Buttonsto hide them from the chart (or right
click one of them from the chart itself)
```

ASlicer is basically a “prettier” version of a PivotTable filter; it works exactly the same way by

filtering the data you see in your PivotTable and PivotCharts

```
1) Select a PivotTable and choose “Insert
Slicer” from the “PivotTable Tools” tab
```
```
2) Select the field(s)
that you want to filter
```
```
3) The Slicer will be inserted next to your table, allowing you to
filter on specific values (or combinations, using the CTRLkey)
```
ADDING SLICERS


ATimeline works just like a Slicer – it’s just formatted to work specifically with Date & Time fields

```
1) Select your pivot table and choose “Insert
Timeline” from the “PivotTable Tools” tab
```
```
2) Select the date/time
field(s) that you want to filter
```
```
3) The Timeline is inserted, allowing you to filter on specific time frames
(Note:may need to adjust unit of time (month, year, etc.))
```
ADDING TIMELINES


