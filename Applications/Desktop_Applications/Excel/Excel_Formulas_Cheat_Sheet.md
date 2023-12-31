---
title: Excel Formulas Cheat Sheet
description: A comprehensive note in my Obsidian vault containing a collection of essential formulas and their usage examples for Microsoft Excel – a widely-used spreadsheet program for data organization, analysis, and visualization. This note may include information on various formula categories (arithmetic, logical, text, lookup, etc.), syntax, and tips to help users work more efficiently and effectively with Excel formulas.
dateCreated: 2022-09-09T04:44:01.149Z
published: true
editor: markdown
tags:
  - Excel
  - Spreadsheets
  - Formulas
dateModified: 
---

# Excel Formulas Cheat Sheet

## Database Functions

```
 DAVERAGE This function will return the
average of selected database entries
 DCOUNT This function will count the cells
that contain numbers in a database
 DCOUNTA This function will count the
nonblank cells in a database
 DGET This function will extract from a
database, a single record that matches the
specified criteria
 DMAX This function will return the
maximum value from selected database
entries
 DMIN This function will return the minimum
value from selected database entries
 DSTDEV This function will estimate the
standard deviation based on a sample of
selected database entries
 DPRODUCT This function will multiply the
values in a particular field of records that
match the criteria in a database
 DSTDEVP This function will calculate the
standard deviation based on the entire
population of selected database entries
 DSUM This function will add the numbers in
the field column of records in the database
that match the criteria
 DVAR This function will estimate the
variance based on a sample from selected
database entries
 DVARP This function will calculate the
variance based on the entire population of
selected database entries
```
## Date and Time Functions

```
 DATE This function will return the serial
number of a particular date
 DATEVALUE This function will convert a date
in the form of text to a serial number
 DAY This function will convert a serial
number to a day of the month
 DAYS360 This function will calculate the
number of days between two dates based
on a 360-day year
 EDATE This function will return the serial
number of the date that is the indicated
number of months before or after the start
date
 EOMONTH This function will return the
serial number of the last day of the month
before or after a specified number of
months
 HOUR This function will convert a serial
number to an hour
 MINUTE This function will convert a serial
number to a minute
 MONTH This function will convert a serial
number to a month
 NETWORKDAYS This function will return the
number of whole workdays between two
dates
 NOW This function will return the serial
number of the current date and time
 SECOND This function will convert a serial
number to a second
 TIME This function will return the serial
number of a particular time
 TIMEVALUE This function will convert a time
in the form of text to a serial number
 TODAY This function will return the serial
number of today's date
 WEEKDAY This function will Convert a serial
number to a day of the week
```

 **WEEKNUM** This function will convert a serial
number to a number representing where the
week falls numerically with a year
 **WORKDAY** This function will return the serial
number of the date before or after a
specified number of workdays
 **YEAR** This function will convert a serial
number to a year
 **YEARFRAC** This function will return the year
fraction representing the number of whole
days between start_date and end_date

## Engineering Functions

 **BESSELI** This function will return the
modified Bessel function In(x)
 **BESSELJ** This function will return the Bessel
function Jn(x)
 **BESSELK** This function will return the
modified Bessel function Kn(x)
 **BESSELY** This function will return the Bessel
function Yn(x)
 **BIN2DEC** This function will convert a binary
number to decimal
 **BIN2HEX** This function will converts a binary
number to hexadecimal
 **BIN2OCT** This function will convert a binary
number to octal
 **COMPLEX** This function will convert real and
imaginary coefficients into a complex
number
 **CONVERT** This function will convert a
number from one measurement system to
another
 **DEC2BIN** This function will convert a decimal
number to binary

```
 DEC2HEX This function will convert a
decimal number to hexadecimal
 DEC2OCT This function will convert a
decimal number to octal
 DELTA This function will Test whether two
values are equal
 ERF This function will return the error
function
 ERFC This function will return the
complementary error function
 GESTEP This function will test whether a
number is greater than a threshold value
 HEX2BIN This function will convert a
hexadecimal number to binary
 HEX2DEC This function will convert a
hexadecimal number to decimal
 HEX2OCT This function will convert a
hexadecimal number to octal
 IMABS This function will return the absolute
value (modulus) of a complex number
 IMAGINARY This function will return the
imaginary coefficient of a complex number
 IMARGUMENT This function will return the
argument theta, an angle expressed in
radians
 IMCONJUGATE This function will return the
complex conjugate of a complex number
 IMCOS This function will return the cosine of
a complex number
 IMDIV This function will return the quotient
of two complex numbers
 IMEXP This function will return the
exponential of a complex number
 IMLN This function will return the natural
logarithm of a complex number
```

 **IMLOG10** This function will return the base-
10 logarithm of a complex number
 **IMLOG2** This function will return the base- 2
logarithm of a complex number
 **IMPOWER** This function will return a
complex number raised to an integer power
 **IMPRODUCT** This function will return the
product of from 2 to 29 complex numbers
 **IMREAL** This function will return the real
coefficient of a complex number
 **IMSIN** This function will return the sine of a
complex number
 **IMSQRT** This function will return the square
root of a complex number
 **IMSUB** This function will return the
difference between two complex numbers
 **MSUM** This function will return the sum of
complex numbers
 **OCT2BIN** This function will convert an octal
number to binary

###  OCT2DEC This function will convert an octal

### number to decimal

###  OCT2HEX This function will convert an octal

### number to hexadecimal

## Financial Functions

 **ACCRINT** This function will return the
accrued interest for a security that pays
periodic interest
 **ACCRINTM** This function will return the
accrued interest for a security that pays
interest at maturity

```
 AMORDEGRC This function will return the
depreciation for each accounting period by
using a depreciation coefficient
 AMORLINC This function will return the
depreciation for each accounting period
 COUPDAYBS This function will return the
number of days from the beginning of the
coupon period to the settlement date
 COUPDAYS This function will return the
number of days in the coupon period that
contains the settlement date
 COUPDAYSNC This function will return the
number of days from the settlement date to
the next coupon date
 COUPNCD This function will return the next
coupon date after the settlement date
 COUPNUM This function will return the
number of coupons payable between the
settlement date and maturity date
 COUPPCD This function will return the
previous coupon date before the settlement
date
 CUMIPMT This function will return the
cumulative interest paid between two
periods
 CUMPRINC This function will return the
cumulative principal paid on a loan between
two periods
 DB This function will return the depreciation
of an asset for a specified period by using
the fixed-declining balance method
 DDB This function will return the
depreciation of an asset for a specified
period by using the double-declining
```

balance method or some other method that
you specify
 **DISC** This function will return the discount
rate for a security
 **DOLLARDE** This function will convert a dollar
price, expressed as a fraction, into a dollar
price, expressed as a decimal number
 **DOLLARFR** This function will convert a dollar
price, expressed as a decimal number, into a
dollar price, expressed as a fraction
 **DURATION** This function will return the
annual duration of a security with periodic
interest payments
 **EFFECT** This function will return the effective
annual interest rate
 **FV** This function will return the future value
of an investment
 **FVSCHEDULE** This function will return the
future value of an initial principal after
applying a series of compound interest rates
 **INTRATE** This function will return the
interest rate for a fully invested security
 **IPMT** This function will return the interest
payment for an investment for a given
period
 **IRR** This function will return the internal rate
of return for a series of cash flows
 **ISPMT** This function will calculate the
interest paid during a specific period of an
investment
 **MDURATION** This function will return the
Macauley modified duration for a security
with an assumed par value of $

```
 MIRR This function will return the internal
rate of return where positive and negative
cash flows are financed at different rates
 NOMINAL This function will return the
annual nominal interest rate
 NPER This function will return the number of
periods for an investment
 NPV This function will return the net present
value of an investment based on a series of
periodic cash flows and a discount rate
 ODDFPRICE This function will return the
price per $100 face value of a security with
an odd first period
 ODDFYIELD This function will return the
yield of a security with an odd first period
 ODDLPRICE This function will return the
price per $100 face value of a security with
an odd last period
 ODDLYIELD This function will return the
yield of a security with an odd last period
 PMT This function will return the periodic
payment for an annuity
 PPMT This function will return the payment
on the principal for an investment for a
given period
 PRICE This function will return the price per
$100 face value of a security that pays
periodic interest
 PRICEDISC This function will return the price
per $100 face value of a discounted security
 PRICEMAT This function will return the price
per $100 face value of a security that pays
interest at maturity
 PV This function will return the present value
of an investment
```

 **RATE** This function will return the interest
rate per period of an annuity
 **RECEIVED** This function will return the
amount received at maturity for a fully
invested security
 **SLN** This function will return the straight-line
depreciation of an asset for one period
 **SYD** This function will return the sum-of-
years' digits depreciation of an asset for a
specified period
 **TBILLEQ** This function will return the bond-
equivalent yield for a Treasury bill
 **TBILLPRICE** This function will return the
price per $100 face value for a Treasury bill
 **TBILLYIELD** This function will return the yield
for a Treasury bill
 **VDB** This function will return the
depreciation of an asset for a specified or
partial period by using a declining balance
method
 **XIRR** This function will return the internal
rate of return for a schedule of cash flows
that is not necessarily periodic
 **XNPV** This function will return the net
present value for a schedule of cash flows
that is not necessarily periodic
 **YIELD** This function will Return the yield on
a security that pays periodic interest
 **YIELDDISC** This function will return the
annual yield for a discounted security; for
example, a Treasury bill

###  YIELDMAT This function will return the

```
annual yield of a security that pays interest
```
### at maturity

## Information Functions

```
 CELL This function will return information
about the formatting, location, or contents
of a cell
 ERROR.TYPE This function will return a
number corresponding to an error type
 INFO This function will return information
about the current operating environment
 ISBLANK This function will return TRUE if the
value is blank
 ISERR This function will return TRUE if the
value is any error value except #N/A
 ISERROR This function will return TRUE if the
value is any error value
 ISEVEN This function will return TRUE if the
number is even
 ISLOGICAL This function will return TRUE if
the value is a logical value
 ISNA This function will return TRUE if the
value is the #N/A error value
 ISNON T This function will return TRUE if the
value is not text
 ISNUMBER This function will return TRUE if
the value is a number
 ISODD This function will return TRUE if the
number is odd
 ISREF This function will return TRUE if the
value is a reference
 ISTEXT This function will return TRUE if the
value is text
 N This function will return a value converted
to a number
 NA This function will return the error value
#N/A
```

###  TYPE This function will return a number

### indicating the data type of a value

## Logical Functions

 **AND** This function will return TRUE if all of
its arguments are TRUE
 **FALSE** This function will return the logical
value FALSE
 **IF** This function will specify a logical test to
perform
 **NOT** This function will reverse the logic of its
argument
 **OR** This function will return TRUE if any
argument is TRUE

###  TRUE This function will return the logical

### value TRUE

## Lookup and Reference

## Functions

 **ADDRESS** This function will return a
reference as text to a single cell in a
worksheet
 **AREAS** This function will return the number
of areas in a reference
 **CHOOSE** This function will choose a value
from a list of values
 **COLUMN** This function will return the
column number of a reference
 **COLUMNS** This function will return the
number of columns in a reference
 **GETPIVOTDATA** This function will return
data stored in a PivotTable

```
 HLOOKUP This function will look in the top
row of an array and returns the value of the
indicated cell
 HYPERLINK This function will create a
shortcut or jump that opens a document
stored on a network server, an intranet, or
the Internet
 INDEX This function will use an index to
choose a value from a reference or array
 INDIRECT This function will return a
reference indicated by a text value
 LOOKUP This function will look up values in
a vector or array
 MATCH This function will look up values in a
reference or array
 OFFSET This function will return a reference
offset from a given reference
 ROW This function will return the row
number of a reference
 ROWS This function will return the number
of rows in a reference
 RTD This function will retrieve real-time data
from a program that supports COM
automation
 TRANSPOSE This function will return the
transpose of an array
```
###  VLOOKUP This function will look in the first

```
column of an array and moves across the
```
### row to return the value of a cell

## Math and Trigonometry

## Functions

```
 ABS This function will return the absolute
value of a number
```

 **ACOS** This function will return the arccosine
of a number
 **ACOSH** This function will return the inverse
hyperbolic cosine of a number
 **ASIN** This function will return the arcsine of
a number
 **ASINH** This function will return the inverse
hyperbolic sine of a number
 **ATAN** This function will return the
arctangent of a number
 **ATAN2** This function will return the
arctangent from x- and y-coordinates
 **ATANH** This function will return the inverse
hyperbolic tangent of a number
 **CEILING** This function will round a number
to the nearest integer or to the nearest
multiple of significance
 **COMBIN** This function will return the
number of combinations for a given number
of objects
 **COS** This function will return the cosine of a
number
 **COSH** This function will return the hyperbolic
cosine of a number
 **DEGREES** This function will convert radians
to degrees
 **EVEN** This function will round a number up
to the nearest even integer
 **EXP** This function will return e raised to the
power of a given number
 **FACT** This function will return the factorial of
a number
 **FACTDOUBLE** This function will return the
double factorial of a number

```
 FLOOR This function will round a number
down, toward zero
 GCD This function will return the greatest
common divisor
 INT This function will round a number down
to the nearest integer
 LCM This function will return the least
common multiple
 LN This function will return the natural
logarithm of a number
 LOG This function will return the logarithm
of a number to a specified base
 LOG10 This function will return the base- 10
logarithm of a number
 MDETERM This function will return the
matrix determinant of an array
 MINVERSE This function will return the
matrix inverse of an array
 MMULT This function will return the matrix
product of two arrays
 MOD This function will return the remainder
from division
 MROUND This function will return a number
rounded to the desired multiple
 MULTINOMIAL This function will return the
multinomial of a set of numbers
 ODD This function will round a number up
to the nearest odd integer
 PI This function will return the value of pi
 POWER This function will return the result of
a number raised to a power
 PRODUCT This function will multiply its
arguments
 QUOTIENT This function will return the
integer portion of a division
```

 **RADIANS** This function will convert degrees
to radians
 **RAND** This function will return a random
number between 0 and 1
 **RANDBETWEEN** This function will return a
random number between the numbers you
specify
 **ROMAN** This function will convert an arabic
numeral to roman, as text
 **ROUND** This function will round a number
to a specified number of digits
 **ROUNDDOWN** This function will round a
number down, toward zero
 **ROUNDUP** This function will round a
number up, away from zero
 **SERIESSUM** This function will return the sum
of a power series based on the formula
 **SIGN** This function will return the sign of a
number
 **SIN** This function will return the sine of the
given angle
 **SINH** This function will return the hyperbolic
sine of a number
 **SQRT** This function will return a positive
square root
 **SQRTPI** This function will return the square
root of (number * pi)
 **SUBTOTAL** This function will return a
subtotal in a list or database
 **SUM** This function will add its arguments
 **SUMIF** Adds the cells specified by a given
criteria
 **SUMPRODUCT** This function will return the
sum of the products of corresponding array
components

```
 SUMSQ This function will return the sum of
the squares of the arguments
 SUMX2MY2 Returns the sum of the
difference of squares of corresponding
values in two arrays
 SUMX2PY2 This function will return the sum
of the sum of squares of corresponding
values in two arrays
 SUMXMY2 This function will return the sum
of squares of differences of corresponding
values in two arrays
 TAN This function will return the tangent of
a number
 TANH This function will return the
hyperbolic tangent of a number
```
###  TRUNC This function will truncate a number

### to an integer

## Statistical Functions

```
 AVEDEV This function will return the average
of the absolute deviations of data points
from their mean
 AVERAGE This function will return the
average of its arguments
 AVERAGEA This function will return the
average of its arguments, including numbers,
text, and logical values
 BETADIST This function will return the beta
cumulative distribution function
 BETAINV This function will return the inverse
of the cumulative distribution function for a
specified beta distribution
```

 **BINOMDIST** This function will return the
individual term binomial distribution
probability
 **CHIDIST** This function will return the one-
tailed probability of the chi-squared
distribution
 **CHIINV** This function will return the inverse
of the one-tailed probability of the chi-
squared distribution
 **CHITEST** This function will return the test for
independence
 **CONFIDENCE** This function will return the
confidence interval for a population mean
 **CORREL** This function will return the
correlation coefficient between two data sets
 **COUNT** This function will count how many
numbers are in the list of arguments
 **COUNTA** This function will count how many
values are in the list of arguments
 **COUNTBLANK** This function will count the
number of blank cells within a range
 **COUNTIF** This function will count the
number of nonblank cells within a range that
meet the given criteria
 **COVAR** This function will return covariance,
the average of the products of paired
deviations
 **CRITBINOM** This function will return the
smallest value for which the cumulative
binomial distribution is less than or equal to
a criterion value
 **DEVSQ** This function will return the sum of
squares of deviations
 **EXPONDIST** This function will return the
exponential distribution

```
 FDIST This function will return the F
probability distribution
 FINV This function will return the inverse of
the F probability distribution
 FISHER This function will return the Fisher
transformation
 FISHERINV This function will return the
inverse of the Fisher transformation
 FORECAST This function will return a value
along a linear trend
 FREQUENCY This function will return a
frequency distribution as a vertical array
 FTEST This function will return the result of
an F-test
 GAMMADIST This function will return the
gamma distribution
 GAMMAINV This function will return the
inverse of the gamma cumulative distribution
 GAMMALN This function will return the
natural logarithm of the gamma function, Γ
(x)
 GEOMEAN This function will return the
geometric mean
 GROWTH This function will return values
along an exponential trend
 HARMEAN This function will return the
harmonic mean
 HYPGEOMDIST This function will return the
hypergeometric distribution
 INTERCEPT This function will return the
intercept of the linear regression line
 KURT This function will return the kurtosis of
a data set
 LARGE This function will return the k-th
largest value in a data set
```

 **LINEST** This function will return the
parameters of a linear trend
 **LOGEST** This function will return the
parameters of an exponential trend
 **LOGINV** This function will return the inverse
of the lognormal distribution
 **LOGNORMDIST** This function will return the
cumulative lognormal distribution
 **MAX** This function will return the maximum
value in a list of arguments
 **MAXA** This function will return the
maximum value in a list of arguments,
including numbers, text, and logical values
 **MEDIAN** This function will return the median
of the given numbers
 **MIN** This function will return the minimum
value in a list of arguments
 **MINA** This function will return the smallest
value in a list of arguments, including
numbers, text, and logical values
 **MODE** This function will return the most
common value in a data set
 **NEGBINOMDIST** return the negative
binomial distribution
 **NORMDIST** This function will return the
normal cumulative distribution
 **NORMINV** This function will return the
inverse of the normal cumulative distribution
 **NORMSDIST** This function will return the
standard normal cumulative distribution
 **NORMSINV** This function will return the
inverse of the standard normal cumulative
distribution

```
 PEARSON This function will return the
Pearson product moment correlation
coefficient
 PERCENTILE This function will return the k-
th percentile of values in a range
 PERCENTRANK This function will return the
percentage rank of a value in a data set
 PERMUT This function will return the
number of permutations for a given number
of objects
 POISSON This function will return the
Poisson distribution
 PROB This function will return the
probability that values in a range are
between two limits
 QUARTILE This function will return the
quartile of a data set
 RANK This function will return the rank of a
number in a list of numbers
 RSQ This function will return the square of
the Pearson product moment correlation
coefficient
 SKEW This function will return the skewness
of a distribution
 SLOPE This function will return the slope of
the linear regression line
 SMALL This function will return the k-th
smallest value in a data set
 STANDARDIZE This function will return a
normalized value
 STDEV This function will estimate standard
deviation based on a sample
 STDEVA This function will estimate standard
deviation based on a sample, including
numbers, text, and logical values
```

 **STDEVP** This function will calculate standard
deviation based on the entire population
 **STDEVPA** This function will calculate
standard deviation based on the entire
population, including numbers, text, and
logical values
 **STEYX** This function will return the standard
error of the predicted y-value for each x in
the regression
 **TDIST** This function will return the Student's
t-distribution
 **TINV** This function will return the inverse of
the Student's t-distribution
 **TREND** This function will return values along
a linear trend
 **TRIMMEAN** This function will return the
mean of the interior of a data set
 **TTEST** This function will return the
probability associated with a Student's t-test
 **VAR** This function will estimate variance
based on a sample
 **VARA** This function will estimate variance
based on a sample, including numbers, text,
and logical values
 **VARP** This function will calculate variance
based on the entire population
 **VARPA** This function will calculate variance
based on the entire population, including
numbers, text, and logical values
 **WEIBULL** This function will return the
Weibull distribution
 **ZTEST** This function will return the one-
tailed probability-value of a z-test

## Text Functions

```
 ASC This function will change full-width
(double-byte) English letters or katakana
within a character string to half-width
(single-byte) characters
 BAHTTEXT This function will converts a
number to text, using the ß (baht) currency
format
 CHAR This function will return the character
specified by the code number
 CLEAN This function will remove all
nonprintable characters from text
 CODE This function will return a numeric
code for the first character in a text string
 CONCATENATE This function will join
several text items into one text item
 DOLLAR This function will convert a number
to text, using the $ (dollar) currency format
 EXACT This function will check to see if two
text values are identical
 FIND, FINDB This function will find one text
value within another (case-sensitive)
 FIXED This function will format a number as
text with a fixed number of decimals
 JIS This function will change half-width
(single-byte) English letters or katakana
within a character string to full-width
(double-byte) characters
 LEFT, LEFTB This function will return the
leftmost characters from a text value
 LEN, LENB This function will return the
number of characters in a text string
 LOWER This function will convert text to
lowercase
```

 **MID, MIDB** This function will return a
specific number of characters from a text
string starting at the position you specify
 **PHONETIC** This function will extract the
phonetic (furigana) characters from a text
string
 **PROPER** This function will capitalize the first
letter in each word of a text value
 **REPLACE, REPLACEB** This function will
replace characters within text
 **REPT** This function will repeat text a given
number of times
 **RIGHT, RIGHTB** This function will return the
rightmost characters from a text value
 **SEARCH, SEARCHB** This function will find
one text value within another (not case-
sensitive)
 **SUBSTITUTE** This function will substitute
new text for old text in a text string
 **T** This function will convert its arguments to
text
 **TEXT** This function will format a number and
converts it to text
 **TRIM** This function will remove spaces from
text
 **UPPER** This function will convert text to
uppercase

###  VALUE Converts a text argument to a

### number


