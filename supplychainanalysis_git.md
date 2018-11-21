Supply Chain Transparency Analysis
================

### Data Setup and Import

``` r
# Setting the correct working directory
setwd('C:\\Users\\Denis Bozic\\Desktop\\Data Practice\\Supply Chain Transparency')
```

We will start by importing the required .csv file containg data on the
60 companies as well as their supply chain transparency scores.

``` r
# Assigning the .csv data into a variable
initial_data <- read.csv('supplychaindata.csv')
```

### Structure Exploration

``` r
# Exploring the columns and the first few rows:
head(initial_data)
```

    ##       ï..Company.name  Size
    ## 1             16Seven     0
    ## 2 Abercrombie & Fitch 28500
    ## 3        Acne Studios   500
    ## 4          Aiby Craft    10
    ## 5          All Saints  3200
    ## 6 Alternative Apparel   180
    ##                                                             Size.Source
    ## 1                                                                   N/A
    ## 2                                                         Fortune, 2016
    ## 3                                              LinkedIn, 2016 (approx.)
    ## 4       LinkedIn, 2016 (approx.), upper limit taken from 1-10 employees
    ## 5                                              LinkedIn, 2016 (approx.)
    ## 6 LinkedIn (2016), Interview with Greg Alterman for Shop-Eat-Surf(2013)
    ##   Annual.Revenue
    ## 1              0
    ## 2     3519000000
    ## 3      138031250
    ## 4              0
    ## 5      330000000
    ## 6      100000000
    ##                                                                    Revenue.Source
    ## 1                                                                             N/A
    ## 2                                                          Fortune Profile (2016)
    ## 3 Business Off Fashion, Acne Studio Finds Its Groove (Oct 4, 2015), approximation
    ## 4                                                                             N/A
    ## 5                                   Bloomberg, Apr 22, 2016; cites "more thanâ\200¦"
    ## 6                                                            Shop-Eat-Surf (2013)
    ##   Nationality Suppliers.published. CoC.published. Audits.published.
    ## 1          UK                  Yes             No                No
    ## 2         USA                   No            Yes                No
    ## 3      Sweden                   No            Yes                No
    ## 4       Spain                   No             No                No
    ## 5          UK                   No             No                No
    ## 6         USA                   No             No                No
    ##   Full.cost.breakdown.published. Purchasing.practices.documents.published.
    ## 1                             No                                        No
    ## 2                             No                                       Yes
    ## 3                             No                                       Yes
    ## 4                             No                                       Yes
    ## 5                             No                                       Yes
    ## 6                             No                                        No
    ##      Business.Orientation Score   Rank
    ## 1 Sustainability-oriented     1    Low
    ## 2                 Neutral     2 Medium
    ## 3                 Neutral     2 Medium
    ## 4 Sustainability-oriented     1    Low
    ## 5                 Neutral     1    Low
    ## 6 Sustainability-oriented     0    Low

``` r
# Exploring the structure of the dataframe
str(initial_data)
```

    ## 'data.frame':    60 obs. of  14 variables:
    ##  $ ï..Company.name                          : Factor w/ 60 levels "16Seven","Abercrombie & Fitch",..: 1 2 3 4 5 6 7 8 9 10 ...
    ##  $ Size                                     : int  0 28500 500 10 3200 180 7500 1900 50 1500 ...
    ##  $ Size.Source                              : Factor w/ 48 levels "Annual Report, 2016, cited as average number of montly employees",..: 43 18 34 37 34 25 36 24 38 35 ...
    ##  $ Annual.Revenue                           : num  0.00 3.52e+09 1.38e+08 0.00 3.30e+08 ...
    ##  $ Revenue.Source                           : Factor w/ 39 levels "Academic paper, though the figures are 2011",..: 24 16 5 24 3 35 34 2 24 24 ...
    ##  $ Nationality                              : Factor w/ 15 levels "Australia","Belgium",..: 13 15 12 11 13 15 15 13 13 15 ...
    ##  $ Suppliers.published.                     : Factor w/ 2 levels "No","Yes": 2 1 1 1 1 1 1 1 1 1 ...
    ##  $ CoC.published.                           : Factor w/ 2 levels "No","Yes": 1 2 2 1 1 1 1 2 2 2 ...
    ##  $ Audits.published.                        : Factor w/ 2 levels "No","Yes": 1 1 1 1 1 1 1 1 1 1 ...
    ##  $ Full.cost.breakdown.published.           : Factor w/ 2 levels "No","Yes": 1 1 1 1 1 1 1 1 1 1 ...
    ##  $ Purchasing.practices.documents.published.: Factor w/ 2 levels "No","Yes": 1 2 2 2 2 1 2 2 2 2 ...
    ##  $ Business.Orientation                     : Factor w/ 3 levels "Neutral","Sustainability-oriented",..: 2 1 1 2 1 2 3 3 2 1 ...
    ##  $ Score                                    : int  1 2 2 1 1 0 1 2 2 2 ...
    ##  $ Rank                                     : Factor w/ 3 levels "High","Low","Medium": 2 3 3 2 2 2 2 3 3 3 ...

There are specific rows that we won’t need for this analysis,
specifically the Size.Source and and Revenue.Source, since these were
descriptions that I used for augmenting my thesis literature. For
purposes of this analysis, however, they won’t be that useful. Let’s
drop those rows now before proceeding to other parts of the analysis.

``` r
# Dropping the two columns by assigning NULL 
initial_data$Size.Source <- NULL
initial_data$Revenue.Source <- NULL
```

Let’s check again for the presence of these columns:

``` r
# Checking again for adjusted data
head(initial_data)
```

    ##       ï..Company.name  Size Annual.Revenue Nationality
    ## 1             16Seven     0              0          UK
    ## 2 Abercrombie & Fitch 28500     3519000000         USA
    ## 3        Acne Studios   500      138031250      Sweden
    ## 4          Aiby Craft    10              0       Spain
    ## 5          All Saints  3200      330000000          UK
    ## 6 Alternative Apparel   180      100000000         USA
    ##   Suppliers.published. CoC.published. Audits.published.
    ## 1                  Yes             No                No
    ## 2                   No            Yes                No
    ## 3                   No            Yes                No
    ## 4                   No             No                No
    ## 5                   No             No                No
    ## 6                   No             No                No
    ##   Full.cost.breakdown.published. Purchasing.practices.documents.published.
    ## 1                             No                                        No
    ## 2                             No                                       Yes
    ## 3                             No                                       Yes
    ## 4                             No                                       Yes
    ## 5                             No                                       Yes
    ## 6                             No                                        No
    ##      Business.Orientation Score   Rank
    ## 1 Sustainability-oriented     1    Low
    ## 2                 Neutral     2 Medium
    ## 3                 Neutral     2 Medium
    ## 4 Sustainability-oriented     1    Low
    ## 5                 Neutral     1    Low
    ## 6 Sustainability-oriented     0    Low

There are several factor variables, so let’s explore levels of these:

``` r
#Levels of Nationality 
levels(initial_data$Nationality)
```

    ##  [1] "Australia"   "Belgium"     "Europe"      "France"      "Germany"    
    ##  [6] "Ireland"     "Italy"       "Japan"       "Netherlands" "New Zealand"
    ## [11] "Spain"       "Sweden"      "UK"          "UK/Japan"    "USA"

``` r
#Levels of Business Orientation
levels(initial_data$Business.Orientation)
```

    ## [1] "Neutral"                 "Sustainability-oriented"
    ## [3] "Trend-oriented"

``` r
#Levels of Rank
levels(initial_data$Rank)
```

    ## [1] "High"   "Low"    "Medium"

From structure exploration, we can notice two things:

  - The size and revenue columns have 0 values, which represent missing
    data. These will have to be imputated.
  - The Nationality table is broken down into specific countries, along
    with some unclear categories (UK/Japan).We will have to fuse these
    into three major categories for easier analysis: USA, Europe, and
    Oceania.
  - We should also convert all the ‘Yes’ to 1s and all the ‘No’ to 0s so
    that we can approximate these variables as binomial distributions
    with Bernoulli trials.

### Data Cleanup

**Checking for incomplete information**

``` r
# Let's find if there are any missing values
initial_data[!complete.cases(initial_data),]
```

    ##    ï..Company.name Size Annual.Revenue Nationality Suppliers.published.
    ## 52   Scotch & Soda   NA              0 Netherlands                   No
    ##    CoC.published. Audits.published. Full.cost.breakdown.published.
    ## 52            Yes                No                             No
    ##    Purchasing.practices.documents.published. Business.Orientation Score
    ## 52                                       Yes       Trend-oriented     2
    ##      Rank
    ## 52 Medium

We have two missing value for Scotch\&Soda: NA for Size and 0 for Annual
Revenue. Both of these will have to be imputated. However, there are
also other rows with 0s in these columns, so we need to check for those
as well.

``` r
# Let's check for all places where we also have 0s
initial_data[initial_data$Size == 0 | initial_data$Annual.Revenue==0,]
```

    ##            ï..Company.name  Size Annual.Revenue Nationality
    ## 1                  16Seven     0              0          UK
    ## 4               Aiby Craft    10              0       Spain
    ## 9                Braintree    50              0          UK
    ## 10         Brooks Brothers  1500              0         USA
    ## 21           Good Society      0              0     Germany
    ## 25           Henrica Langh     0              0      Europe
    ## 27               Honest By     0              0     Belgium
    ## 30                  Kowtow     0              0 New Zealand
    ## 38                Mayamiko    10              0          UK
    ## 39      Ministry of Supply    50              0         USA
    ## 41               NewYorker 16000              0     Germany
    ## 44 Organic by John Patrick     0              0         USA
    ## 52           Scotch & Soda    NA              0 Netherlands
    ## 53         Shift to Nature     0              0   Australia
    ## 54               Sveekery      0              0     Germany
    ## 59                    Zady    50              0         USA
    ##    Suppliers.published. CoC.published. Audits.published.
    ## 1                   Yes             No                No
    ## 4                    No             No                No
    ## 9                    No            Yes                No
    ## 10                   No            Yes                No
    ## 21                   No             No                No
    ## 25                   No             No                No
    ## 27                  Yes             No                No
    ## 30                   No             No                No
    ## 38                  Yes            Yes                No
    ## 39                   No             No                No
    ## 41                   No             No                No
    ## 44                   No             No                No
    ## 52                   No            Yes                No
    ## 53                  Yes             No                No
    ## 54                   No             No                No
    ## 59                  Yes             No                No
    ##    Full.cost.breakdown.published.
    ## 1                              No
    ## 4                              No
    ## 9                              No
    ## 10                             No
    ## 21                             No
    ## 25                             No
    ## 27                            Yes
    ## 30                             No
    ## 38                             No
    ## 39                             No
    ## 41                             No
    ## 44                             No
    ## 52                             No
    ## 53                             No
    ## 54                             No
    ## 59                             No
    ##    Purchasing.practices.documents.published.    Business.Orientation Score
    ## 1                                         No Sustainability-oriented     1
    ## 4                                        Yes Sustainability-oriented     1
    ## 9                                        Yes Sustainability-oriented     2
    ## 10                                       Yes                 Neutral     2
    ## 21                                        No Sustainability-oriented     0
    ## 25                                        No Sustainability-oriented     0
    ## 27                                        No Sustainability-oriented     2
    ## 30                                        No Sustainability-oriented     0
    ## 38                                        No Sustainability-oriented     2
    ## 39                                       Yes                 Neutral     1
    ## 41                                       Yes          Trend-oriented     1
    ## 44                                        No Sustainability-oriented     0
    ## 52                                       Yes          Trend-oriented     2
    ## 53                                       Yes Sustainability-oriented     2
    ## 54                                        No Sustainability-oriented     0
    ## 59                                        No Sustainability-oriented     1
    ##      Rank
    ## 1     Low
    ## 4     Low
    ## 9  Medium
    ## 10 Medium
    ## 21    Low
    ## 25    Low
    ## 27 Medium
    ## 30    Low
    ## 38 Medium
    ## 39    Low
    ## 41    Low
    ## 44    Low
    ## 52 Medium
    ## 53 Medium
    ## 54    Low
    ## 59    Low

There’s a notable number of data points without information on either
size or annual revenue. While we are operating with only 60 data points
and median-based imputation is certainly not ideal, business
orientation-based replacement should provide us with a reasonable proxy
for these values. Business orientation, based on my research, generally
provides an informative split between companies on size and revenue.

So, let’s understand the median for each category across size and
revenue. We will start with size.

**Median:
Size**

``` r
# Extract the overall median of the dataframa with non-zero values in Size.
# We remember to include also the NA value for Scotch & Soda.

med_size <- median(initial_data[initial_data$Size != 0 & !is.na(initial_data$Size), "Size"])
med_size
```

    ## [1] 10000

The overall median size is 10,000 but we need to split these across the
three business orientation categories.

``` r
# Median size for sustainability-oriented businesses
med_size_sus <- median(initial_data[initial_data$Size != 0 & !is.na(initial_data$Size) & 
                      initial_data$Business.Orientation == 'Sustainability-oriented' , "Size"])
med_size_sus
```

    ## [1] 75

``` r
# Median size for neutral businesses
med_size_neu <- median(initial_data[initial_data$Size != 0 & !is.na(initial_data$Size) & 
                      initial_data$Business.Orientation == 'Neutral' , "Size"])
med_size_neu
```

    ## [1] 10000

``` r
# Median size for trend--oriented businesses
med_size_tre <- median(initial_data[initial_data$Size != 0 & !is.na(initial_data$Size) & 
                      initial_data$Business.Orientation == 'Trend-oriented' , "Size"])
med_size_tre
```

    ## [1] 16000

The overall median size is 10,000 employees, but these numbers differ
notably for each business orientation category. Sustainability-oriented
ones have a median of 75 people, neutral of 10,000 people (like the
overall median), while the trend-oriented ones have a median of 16,000
people.

Let’s do the same for revenue across these three categories.

**Median:
Revenue**

``` r
# Extract the overall median of the dataframa with non-zero values in Annual Revenue.

med_revenue <- median(initial_data[initial_data$Annual.Revenue != 0 & 
                                     !is.na(initial_data$Annual.Revenue), "Annual.Revenue"])
med_revenue
```

    ## [1] 2205500000

The overall annual revenue is $2,205,500,000. Now, let’s look across
different categories.

``` r
# Median annual revenue for sustainability-oriented businesses
med_revenue_sus <- median(initial_data[initial_data$Annual.Revenue != 0 & 
                      !is.na(initial_data$Annual.Revenue) & 
                      initial_data$Business.Orientation == 'Sustainability-oriented' , "Annual.Revenue"])
med_revenue_sus
```

    ## [1] 4.7e+07

``` r
# Median annual revenue for neutral businesses
med_revenue_neu <- median(initial_data[initial_data$Annual.Revenue != 0 & 
                      !is.na(initial_data$Annual.Revenue) & 
                      initial_data$Business.Orientation == 'Neutral' , "Annual.Revenue"])
med_revenue_neu
```

    ## [1] 3709500000

``` r
# Median annual revenue for trend-oriented businesses
med_revenue_tre <- median(initial_data[initial_data$Annual.Revenue != 0 & 
                      !is.na(initial_data$Annual.Revenue) & 
                      initial_data$Business.Orientation == 'Trend-oriented' , "Annual.Revenue"])
med_revenue_tre
```

    ## [1] 2260500000

Now we will imputate all the required missing values with their
respective medians.

**Imputation: Size**

``` r
# Imputating size for sustainability brands
initial_data[initial_data$Business.Orientation == "Sustainability-oriented" & 
               initial_data$Size == 0,"Size"] <- med_size_sus
initial_data[initial_data$Business.Orientation == 'Sustainability-oriented',]
```

    ##            ï..Company.name Size Annual.Revenue Nationality
    ## 1                  16Seven   75              0          UK
    ## 4               Aiby Craft   10              0       Spain
    ## 6      Alternative Apparel  180      100000000         USA
    ## 9                Braintree   50              0          UK
    ## 16           Eileen Fisher 1100      429000000         USA
    ## 17                Everlane  100       50000000         USA
    ## 21           Good Society    75              0     Germany
    ## 25           Henrica Langh   75              0      Europe
    ## 27               Honest By   75              0     Belgium
    ## 30                  Kowtow   75              0 New Zealand
    ## 31            Krochet Kids  150        1567769         USA
    ## 38                Mayamiko   10              0          UK
    ## 43             Nudie Jeans   50       44000000      Sweden
    ## 44 Organic by John Patrick   75              0         USA
    ## 45               Patagonia 2000      570000000         USA
    ## 46              PeopleTree   50        3125000    UK/Japan
    ## 49             Reformation  200       25000000         USA
    ## 53         Shift to Nature   75              0   Australia
    ## 54               Sveekery    75              0     Germany
    ## 59                    Zady   50              0         USA
    ##    Suppliers.published. CoC.published. Audits.published.
    ## 1                   Yes             No                No
    ## 4                    No             No                No
    ## 6                    No             No                No
    ## 9                    No            Yes                No
    ## 16                   No            Yes                No
    ## 17                  Yes             No                No
    ## 21                   No             No                No
    ## 25                   No             No                No
    ## 27                  Yes             No                No
    ## 30                   No             No                No
    ## 31                  Yes             No                No
    ## 38                  Yes            Yes                No
    ## 43                  Yes            Yes               Yes
    ## 44                   No             No                No
    ## 45                  Yes            Yes               Yes
    ## 46                  Yes            Yes                No
    ## 49                   No             No                No
    ## 53                  Yes             No                No
    ## 54                   No             No                No
    ## 59                  Yes             No                No
    ##    Full.cost.breakdown.published.
    ## 1                              No
    ## 4                              No
    ## 6                              No
    ## 9                              No
    ## 16                             No
    ## 17                            Yes
    ## 21                             No
    ## 25                             No
    ## 27                            Yes
    ## 30                             No
    ## 31                             No
    ## 38                             No
    ## 43                             No
    ## 44                             No
    ## 45                             No
    ## 46                             No
    ## 49                             No
    ## 53                             No
    ## 54                             No
    ## 59                             No
    ##    Purchasing.practices.documents.published.    Business.Orientation Score
    ## 1                                         No Sustainability-oriented     1
    ## 4                                        Yes Sustainability-oriented     1
    ## 6                                         No Sustainability-oriented     0
    ## 9                                        Yes Sustainability-oriented     2
    ## 16                                       Yes Sustainability-oriented     2
    ## 17                                        No Sustainability-oriented     2
    ## 21                                        No Sustainability-oriented     0
    ## 25                                        No Sustainability-oriented     0
    ## 27                                        No Sustainability-oriented     2
    ## 30                                        No Sustainability-oriented     0
    ## 31                                        No Sustainability-oriented     1
    ## 38                                        No Sustainability-oriented     2
    ## 43                                       Yes Sustainability-oriented     4
    ## 44                                        No Sustainability-oriented     0
    ## 45                                       Yes Sustainability-oriented     4
    ## 46                                       Yes Sustainability-oriented     3
    ## 49                                        No Sustainability-oriented     0
    ## 53                                       Yes Sustainability-oriented     2
    ## 54                                        No Sustainability-oriented     0
    ## 59                                        No Sustainability-oriented     1
    ##      Rank
    ## 1     Low
    ## 4     Low
    ## 6     Low
    ## 9  Medium
    ## 16 Medium
    ## 17 Medium
    ## 21    Low
    ## 25    Low
    ## 27 Medium
    ## 30    Low
    ## 31    Low
    ## 38 Medium
    ## 43   High
    ## 44    Low
    ## 45   High
    ## 46 Medium
    ## 49    Low
    ## 53 Medium
    ## 54    Low
    ## 59    Low

``` r
# Imputating size for neutral brands
initial_data[initial_data$Business.Orientation == "Neutral" & 
               initial_data$Size == 0,"Size"] <- med_size_neu
initial_data[initial_data$Business.Orientation == 'Neutral',]
```

    ##            ï..Company.name   Size Annual.Revenue Nationality
    ## 2      Abercrombie & Fitch  28500     3519000000         USA
    ## 3             Acne Studios    500      138031250      Sweden
    ## 5               All Saints   3200      330000000          UK
    ## 10         Brooks Brothers   1500              0         USA
    ## 11                Burberry   9698     3900000000          UK
    ## 12 Calvin Klein (thru PVH)  34200     8020000000         USA
    ## 13                  Chanel  10000     5200000000      France
    ## 15                   Dior  122736    41600000000      France
    ## 22                   Gucci  10000     4300000000       Italy
    ## 26                  Hermes  12244     5370000000      France
    ## 28               Hugo Boss  13764     3089900000     Germany
    ## 29                  J Crew  15300     2510000000         USA
    ## 32                L.L.Bean   5000     1610000000         USA
    ## 33                  Lanvin    330      184100000      France
    ## 34                  Levi's  12500     4490000000         USA
    ## 36        Marni (thru OTB)  10000     1590000000       Italy
    ## 37                 MaxMara   5000     1430000000       Italy
    ## 39      Ministry of Supply     50              0         USA
    ## 42    NorthFace (thru VFC)  64000    12400000000         USA
    ## 48            Ralph Lauren  26000     7400000000         USA
    ##    Suppliers.published. CoC.published. Audits.published.
    ## 2                    No            Yes                No
    ## 3                    No            Yes                No
    ## 5                    No             No                No
    ## 10                   No            Yes                No
    ## 11                   No            Yes                No
    ## 12                   No            Yes               Yes
    ## 13                   No             No                No
    ## 15                   No             No                No
    ## 22                   No            Yes                No
    ## 26                   No             No                No
    ## 28                   No            Yes                No
    ## 29                   No            Yes                No
    ## 32                   No            Yes                No
    ## 33                   No             No                No
    ## 34                  Yes            Yes               Yes
    ## 36                   No             No                No
    ## 37                   No             No                No
    ## 39                   No             No                No
    ## 42                  Yes            Yes                No
    ## 48                   No            Yes                No
    ##    Full.cost.breakdown.published.
    ## 2                              No
    ## 3                              No
    ## 5                              No
    ## 10                             No
    ## 11                             No
    ## 12                             No
    ## 13                             No
    ## 15                             No
    ## 22                             No
    ## 26                             No
    ## 28                             No
    ## 29                             No
    ## 32                             No
    ## 33                             No
    ## 34                             No
    ## 36                             No
    ## 37                             No
    ## 39                             No
    ## 42                             No
    ## 48                             No
    ##    Purchasing.practices.documents.published. Business.Orientation Score
    ## 2                                        Yes              Neutral     2
    ## 3                                        Yes              Neutral     2
    ## 5                                        Yes              Neutral     1
    ## 10                                       Yes              Neutral     2
    ## 11                                       Yes              Neutral     2
    ## 12                                       Yes              Neutral     3
    ## 13                                       Yes              Neutral     1
    ## 15                                        No              Neutral     0
    ## 22                                       Yes              Neutral     2
    ## 26                                       Yes              Neutral     1
    ## 28                                       Yes              Neutral     2
    ## 29                                       Yes              Neutral     2
    ## 32                                       Yes              Neutral     2
    ## 33                                        No              Neutral     0
    ## 34                                       Yes              Neutral     4
    ## 36                                        No              Neutral     0
    ## 37                                        No              Neutral     0
    ## 39                                       Yes              Neutral     1
    ## 42                                       Yes              Neutral     3
    ## 48                                       Yes              Neutral     2
    ##      Rank
    ## 2  Medium
    ## 3  Medium
    ## 5     Low
    ## 10 Medium
    ## 11 Medium
    ## 12 Medium
    ## 13    Low
    ## 15    Low
    ## 22 Medium
    ## 26    Low
    ## 28 Medium
    ## 29 Medium
    ## 32 Medium
    ## 33    Low
    ## 34   High
    ## 36    Low
    ## 37    Low
    ## 39    Low
    ## 42 Medium
    ## 48 Medium

``` r
# Imputating size for trend-oriented brands
initial_data[initial_data$Business.Orientation == "Trend-oriented" & 
               is.na(initial_data$Size),"Size"] <- med_size_tre
initial_data[initial_data$Business.Orientation == 'Trend-oriented',]
```

    ##                 ï..Company.name   Size Annual.Revenue Nationality
    ## 7              American Apparel   7500      609000000         USA
    ## 8                          ASOS   1900     1209620000          UK
    ## 14              Charlotte Russe  10000      856000000         USA
    ## 18                   Forever 21  30000     4400000000         USA
    ## 19            French Connection   1999      203608000          UK
    ## 20                          Gap 141000    15797000000         USA
    ## 23                        Guess  13500     2200000000         USA
    ## 24                          H&M 104637    21730000000      Sweden
    ## 35                        Mango  14400     2211000000       Spain
    ## 40                     New Look  18530     1878156000          UK
    ## 41                    NewYorker  16000              0     Germany
    ## 47                      Primark  58000     6250000000     Ireland
    ## 50                 River Island  10000     1166508000          UK
    ## 51                        rue21  10000     1100000000         USA
    ## 52                Scotch & Soda  16000              0 Netherlands
    ## 55 Topshop (thru Arcadia Group)  45000     2587500000          UK
    ## 56 Uniqlo (thru Fast Retailing)  41646    14440000000       Japan
    ## 57    United Colors of Benetton   9500     2310000000       Italy
    ## 58             Urban Outfitters  24000     3400000000         USA
    ## 60          Zara (thru Inditex) 152854    23050000000       Spain
    ##    Suppliers.published. CoC.published. Audits.published.
    ## 7                    No             No                No
    ## 8                    No            Yes                No
    ## 14                   No             No                No
    ## 18                   No             No                No
    ## 19                   No             No                No
    ## 20                  Yes            Yes                No
    ## 23                   No             No                No
    ## 24                  Yes            Yes                No
    ## 35                   No            Yes                No
    ## 40                   No            Yes                No
    ## 41                   No             No                No
    ## 47                   No            Yes                No
    ## 50                   No             No                No
    ## 51                   No             No                No
    ## 52                   No            Yes                No
    ## 55                   No            Yes                No
    ## 56                   No            Yes                No
    ## 57                   No            Yes                No
    ## 58                   No             No                No
    ## 60                   No            Yes                No
    ##    Full.cost.breakdown.published.
    ## 7                              No
    ## 8                              No
    ## 14                             No
    ## 18                             No
    ## 19                             No
    ## 20                             No
    ## 23                             No
    ## 24                             No
    ## 35                             No
    ## 40                             No
    ## 41                             No
    ## 47                             No
    ## 50                             No
    ## 51                             No
    ## 52                             No
    ## 55                             No
    ## 56                             No
    ## 57                             No
    ## 58                             No
    ## 60                             No
    ##    Purchasing.practices.documents.published. Business.Orientation Score
    ## 7                                        Yes       Trend-oriented     1
    ## 8                                        Yes       Trend-oriented     2
    ## 14                                       Yes       Trend-oriented     1
    ## 18                                       Yes       Trend-oriented     1
    ## 19                                       Yes       Trend-oriented     1
    ## 20                                       Yes       Trend-oriented     3
    ## 23                                       Yes       Trend-oriented     1
    ## 24                                       Yes       Trend-oriented     3
    ## 35                                       Yes       Trend-oriented     2
    ## 40                                       Yes       Trend-oriented     2
    ## 41                                       Yes       Trend-oriented     1
    ## 47                                       Yes       Trend-oriented     2
    ## 50                                       Yes       Trend-oriented     1
    ## 51                                       Yes       Trend-oriented     1
    ## 52                                       Yes       Trend-oriented     2
    ## 55                                       Yes       Trend-oriented     2
    ## 56                                       Yes       Trend-oriented     2
    ## 57                                       Yes       Trend-oriented     2
    ## 58                                       Yes       Trend-oriented     1
    ## 60                                       Yes       Trend-oriented     2
    ##      Rank
    ## 7     Low
    ## 8  Medium
    ## 14    Low
    ## 18    Low
    ## 19    Low
    ## 20 Medium
    ## 23    Low
    ## 24 Medium
    ## 35 Medium
    ## 40 Medium
    ## 41    Low
    ## 47 Medium
    ## 50    Low
    ## 51    Low
    ## 52 Medium
    ## 55 Medium
    ## 56 Medium
    ## 57 Medium
    ## 58    Low
    ## 60 Medium

``` r
# Let's check how the dataset looks now.
head(initial_data)
```

    ##       ï..Company.name  Size Annual.Revenue Nationality
    ## 1             16Seven    75              0          UK
    ## 2 Abercrombie & Fitch 28500     3519000000         USA
    ## 3        Acne Studios   500      138031250      Sweden
    ## 4          Aiby Craft    10              0       Spain
    ## 5          All Saints  3200      330000000          UK
    ## 6 Alternative Apparel   180      100000000         USA
    ##   Suppliers.published. CoC.published. Audits.published.
    ## 1                  Yes             No                No
    ## 2                   No            Yes                No
    ## 3                   No            Yes                No
    ## 4                   No             No                No
    ## 5                   No             No                No
    ## 6                   No             No                No
    ##   Full.cost.breakdown.published. Purchasing.practices.documents.published.
    ## 1                             No                                        No
    ## 2                             No                                       Yes
    ## 3                             No                                       Yes
    ## 4                             No                                       Yes
    ## 5                             No                                       Yes
    ## 6                             No                                        No
    ##      Business.Orientation Score   Rank
    ## 1 Sustainability-oriented     1    Low
    ## 2                 Neutral     2 Medium
    ## 3                 Neutral     2 Medium
    ## 4 Sustainability-oriented     1    Low
    ## 5                 Neutral     1    Low
    ## 6 Sustainability-oriented     0    Low

**Imputation: Annual Revenue**

``` r
# Imputating revenue for sustainability brands
initial_data[initial_data$Business.Orientation == "Sustainability-oriented" & 
               initial_data$Annual.Revenue == 0,"Annual.Revenue"] <- med_revenue_sus
initial_data[initial_data$Business.Orientation == 'Sustainability-oriented',]
```

    ##            ï..Company.name Size Annual.Revenue Nationality
    ## 1                  16Seven   75       47000000          UK
    ## 4               Aiby Craft   10       47000000       Spain
    ## 6      Alternative Apparel  180      100000000         USA
    ## 9                Braintree   50       47000000          UK
    ## 16           Eileen Fisher 1100      429000000         USA
    ## 17                Everlane  100       50000000         USA
    ## 21           Good Society    75       47000000     Germany
    ## 25           Henrica Langh   75       47000000      Europe
    ## 27               Honest By   75       47000000     Belgium
    ## 30                  Kowtow   75       47000000 New Zealand
    ## 31            Krochet Kids  150        1567769         USA
    ## 38                Mayamiko   10       47000000          UK
    ## 43             Nudie Jeans   50       44000000      Sweden
    ## 44 Organic by John Patrick   75       47000000         USA
    ## 45               Patagonia 2000      570000000         USA
    ## 46              PeopleTree   50        3125000    UK/Japan
    ## 49             Reformation  200       25000000         USA
    ## 53         Shift to Nature   75       47000000   Australia
    ## 54               Sveekery    75       47000000     Germany
    ## 59                    Zady   50       47000000         USA
    ##    Suppliers.published. CoC.published. Audits.published.
    ## 1                   Yes             No                No
    ## 4                    No             No                No
    ## 6                    No             No                No
    ## 9                    No            Yes                No
    ## 16                   No            Yes                No
    ## 17                  Yes             No                No
    ## 21                   No             No                No
    ## 25                   No             No                No
    ## 27                  Yes             No                No
    ## 30                   No             No                No
    ## 31                  Yes             No                No
    ## 38                  Yes            Yes                No
    ## 43                  Yes            Yes               Yes
    ## 44                   No             No                No
    ## 45                  Yes            Yes               Yes
    ## 46                  Yes            Yes                No
    ## 49                   No             No                No
    ## 53                  Yes             No                No
    ## 54                   No             No                No
    ## 59                  Yes             No                No
    ##    Full.cost.breakdown.published.
    ## 1                              No
    ## 4                              No
    ## 6                              No
    ## 9                              No
    ## 16                             No
    ## 17                            Yes
    ## 21                             No
    ## 25                             No
    ## 27                            Yes
    ## 30                             No
    ## 31                             No
    ## 38                             No
    ## 43                             No
    ## 44                             No
    ## 45                             No
    ## 46                             No
    ## 49                             No
    ## 53                             No
    ## 54                             No
    ## 59                             No
    ##    Purchasing.practices.documents.published.    Business.Orientation Score
    ## 1                                         No Sustainability-oriented     1
    ## 4                                        Yes Sustainability-oriented     1
    ## 6                                         No Sustainability-oriented     0
    ## 9                                        Yes Sustainability-oriented     2
    ## 16                                       Yes Sustainability-oriented     2
    ## 17                                        No Sustainability-oriented     2
    ## 21                                        No Sustainability-oriented     0
    ## 25                                        No Sustainability-oriented     0
    ## 27                                        No Sustainability-oriented     2
    ## 30                                        No Sustainability-oriented     0
    ## 31                                        No Sustainability-oriented     1
    ## 38                                        No Sustainability-oriented     2
    ## 43                                       Yes Sustainability-oriented     4
    ## 44                                        No Sustainability-oriented     0
    ## 45                                       Yes Sustainability-oriented     4
    ## 46                                       Yes Sustainability-oriented     3
    ## 49                                        No Sustainability-oriented     0
    ## 53                                       Yes Sustainability-oriented     2
    ## 54                                        No Sustainability-oriented     0
    ## 59                                        No Sustainability-oriented     1
    ##      Rank
    ## 1     Low
    ## 4     Low
    ## 6     Low
    ## 9  Medium
    ## 16 Medium
    ## 17 Medium
    ## 21    Low
    ## 25    Low
    ## 27 Medium
    ## 30    Low
    ## 31    Low
    ## 38 Medium
    ## 43   High
    ## 44    Low
    ## 45   High
    ## 46 Medium
    ## 49    Low
    ## 53 Medium
    ## 54    Low
    ## 59    Low

``` r
# Imputating revenue for neutral brands
initial_data[initial_data$Business.Orientation == "Neutral" & 
               initial_data$Annual.Revenue == 0,"Annual.Revenue"] <- med_revenue_neu
initial_data[initial_data$Business.Orientation == 'Neutral',]
```

    ##            ï..Company.name   Size Annual.Revenue Nationality
    ## 2      Abercrombie & Fitch  28500     3519000000         USA
    ## 3             Acne Studios    500      138031250      Sweden
    ## 5               All Saints   3200      330000000          UK
    ## 10         Brooks Brothers   1500     3709500000         USA
    ## 11                Burberry   9698     3900000000          UK
    ## 12 Calvin Klein (thru PVH)  34200     8020000000         USA
    ## 13                  Chanel  10000     5200000000      France
    ## 15                   Dior  122736    41600000000      France
    ## 22                   Gucci  10000     4300000000       Italy
    ## 26                  Hermes  12244     5370000000      France
    ## 28               Hugo Boss  13764     3089900000     Germany
    ## 29                  J Crew  15300     2510000000         USA
    ## 32                L.L.Bean   5000     1610000000         USA
    ## 33                  Lanvin    330      184100000      France
    ## 34                  Levi's  12500     4490000000         USA
    ## 36        Marni (thru OTB)  10000     1590000000       Italy
    ## 37                 MaxMara   5000     1430000000       Italy
    ## 39      Ministry of Supply     50     3709500000         USA
    ## 42    NorthFace (thru VFC)  64000    12400000000         USA
    ## 48            Ralph Lauren  26000     7400000000         USA
    ##    Suppliers.published. CoC.published. Audits.published.
    ## 2                    No            Yes                No
    ## 3                    No            Yes                No
    ## 5                    No             No                No
    ## 10                   No            Yes                No
    ## 11                   No            Yes                No
    ## 12                   No            Yes               Yes
    ## 13                   No             No                No
    ## 15                   No             No                No
    ## 22                   No            Yes                No
    ## 26                   No             No                No
    ## 28                   No            Yes                No
    ## 29                   No            Yes                No
    ## 32                   No            Yes                No
    ## 33                   No             No                No
    ## 34                  Yes            Yes               Yes
    ## 36                   No             No                No
    ## 37                   No             No                No
    ## 39                   No             No                No
    ## 42                  Yes            Yes                No
    ## 48                   No            Yes                No
    ##    Full.cost.breakdown.published.
    ## 2                              No
    ## 3                              No
    ## 5                              No
    ## 10                             No
    ## 11                             No
    ## 12                             No
    ## 13                             No
    ## 15                             No
    ## 22                             No
    ## 26                             No
    ## 28                             No
    ## 29                             No
    ## 32                             No
    ## 33                             No
    ## 34                             No
    ## 36                             No
    ## 37                             No
    ## 39                             No
    ## 42                             No
    ## 48                             No
    ##    Purchasing.practices.documents.published. Business.Orientation Score
    ## 2                                        Yes              Neutral     2
    ## 3                                        Yes              Neutral     2
    ## 5                                        Yes              Neutral     1
    ## 10                                       Yes              Neutral     2
    ## 11                                       Yes              Neutral     2
    ## 12                                       Yes              Neutral     3
    ## 13                                       Yes              Neutral     1
    ## 15                                        No              Neutral     0
    ## 22                                       Yes              Neutral     2
    ## 26                                       Yes              Neutral     1
    ## 28                                       Yes              Neutral     2
    ## 29                                       Yes              Neutral     2
    ## 32                                       Yes              Neutral     2
    ## 33                                        No              Neutral     0
    ## 34                                       Yes              Neutral     4
    ## 36                                        No              Neutral     0
    ## 37                                        No              Neutral     0
    ## 39                                       Yes              Neutral     1
    ## 42                                       Yes              Neutral     3
    ## 48                                       Yes              Neutral     2
    ##      Rank
    ## 2  Medium
    ## 3  Medium
    ## 5     Low
    ## 10 Medium
    ## 11 Medium
    ## 12 Medium
    ## 13    Low
    ## 15    Low
    ## 22 Medium
    ## 26    Low
    ## 28 Medium
    ## 29 Medium
    ## 32 Medium
    ## 33    Low
    ## 34   High
    ## 36    Low
    ## 37    Low
    ## 39    Low
    ## 42 Medium
    ## 48 Medium

``` r
# Imputating revenue for trend-oriented brands
initial_data[initial_data$Business.Orientation == "Trend-oriented" & 
               initial_data$Annual.Revenue == 0,"Annual.Revenue"] <- med_revenue_tre
initial_data[initial_data$Business.Orientation == 'Trend-oriented',]
```

    ##                 ï..Company.name   Size Annual.Revenue Nationality
    ## 7              American Apparel   7500      609000000         USA
    ## 8                          ASOS   1900     1209620000          UK
    ## 14              Charlotte Russe  10000      856000000         USA
    ## 18                   Forever 21  30000     4400000000         USA
    ## 19            French Connection   1999      203608000          UK
    ## 20                          Gap 141000    15797000000         USA
    ## 23                        Guess  13500     2200000000         USA
    ## 24                          H&M 104637    21730000000      Sweden
    ## 35                        Mango  14400     2211000000       Spain
    ## 40                     New Look  18530     1878156000          UK
    ## 41                    NewYorker  16000     2260500000     Germany
    ## 47                      Primark  58000     6250000000     Ireland
    ## 50                 River Island  10000     1166508000          UK
    ## 51                        rue21  10000     1100000000         USA
    ## 52                Scotch & Soda  16000     2260500000 Netherlands
    ## 55 Topshop (thru Arcadia Group)  45000     2587500000          UK
    ## 56 Uniqlo (thru Fast Retailing)  41646    14440000000       Japan
    ## 57    United Colors of Benetton   9500     2310000000       Italy
    ## 58             Urban Outfitters  24000     3400000000         USA
    ## 60          Zara (thru Inditex) 152854    23050000000       Spain
    ##    Suppliers.published. CoC.published. Audits.published.
    ## 7                    No             No                No
    ## 8                    No            Yes                No
    ## 14                   No             No                No
    ## 18                   No             No                No
    ## 19                   No             No                No
    ## 20                  Yes            Yes                No
    ## 23                   No             No                No
    ## 24                  Yes            Yes                No
    ## 35                   No            Yes                No
    ## 40                   No            Yes                No
    ## 41                   No             No                No
    ## 47                   No            Yes                No
    ## 50                   No             No                No
    ## 51                   No             No                No
    ## 52                   No            Yes                No
    ## 55                   No            Yes                No
    ## 56                   No            Yes                No
    ## 57                   No            Yes                No
    ## 58                   No             No                No
    ## 60                   No            Yes                No
    ##    Full.cost.breakdown.published.
    ## 7                              No
    ## 8                              No
    ## 14                             No
    ## 18                             No
    ## 19                             No
    ## 20                             No
    ## 23                             No
    ## 24                             No
    ## 35                             No
    ## 40                             No
    ## 41                             No
    ## 47                             No
    ## 50                             No
    ## 51                             No
    ## 52                             No
    ## 55                             No
    ## 56                             No
    ## 57                             No
    ## 58                             No
    ## 60                             No
    ##    Purchasing.practices.documents.published. Business.Orientation Score
    ## 7                                        Yes       Trend-oriented     1
    ## 8                                        Yes       Trend-oriented     2
    ## 14                                       Yes       Trend-oriented     1
    ## 18                                       Yes       Trend-oriented     1
    ## 19                                       Yes       Trend-oriented     1
    ## 20                                       Yes       Trend-oriented     3
    ## 23                                       Yes       Trend-oriented     1
    ## 24                                       Yes       Trend-oriented     3
    ## 35                                       Yes       Trend-oriented     2
    ## 40                                       Yes       Trend-oriented     2
    ## 41                                       Yes       Trend-oriented     1
    ## 47                                       Yes       Trend-oriented     2
    ## 50                                       Yes       Trend-oriented     1
    ## 51                                       Yes       Trend-oriented     1
    ## 52                                       Yes       Trend-oriented     2
    ## 55                                       Yes       Trend-oriented     2
    ## 56                                       Yes       Trend-oriented     2
    ## 57                                       Yes       Trend-oriented     2
    ## 58                                       Yes       Trend-oriented     1
    ## 60                                       Yes       Trend-oriented     2
    ##      Rank
    ## 7     Low
    ## 8  Medium
    ## 14    Low
    ## 18    Low
    ## 19    Low
    ## 20 Medium
    ## 23    Low
    ## 24 Medium
    ## 35 Medium
    ## 40 Medium
    ## 41    Low
    ## 47 Medium
    ## 50    Low
    ## 51    Low
    ## 52 Medium
    ## 55 Medium
    ## 56 Medium
    ## 57 Medium
    ## 58    Low
    ## 60 Medium

``` r
# Let's check how the dataset looks now.
head(initial_data)
```

    ##       ï..Company.name  Size Annual.Revenue Nationality
    ## 1             16Seven    75       47000000          UK
    ## 2 Abercrombie & Fitch 28500     3519000000         USA
    ## 3        Acne Studios   500      138031250      Sweden
    ## 4          Aiby Craft    10       47000000       Spain
    ## 5          All Saints  3200      330000000          UK
    ## 6 Alternative Apparel   180      100000000         USA
    ##   Suppliers.published. CoC.published. Audits.published.
    ## 1                  Yes             No                No
    ## 2                   No            Yes                No
    ## 3                   No            Yes                No
    ## 4                   No             No                No
    ## 5                   No             No                No
    ## 6                   No             No                No
    ##   Full.cost.breakdown.published. Purchasing.practices.documents.published.
    ## 1                             No                                        No
    ## 2                             No                                       Yes
    ## 3                             No                                       Yes
    ## 4                             No                                       Yes
    ## 5                             No                                       Yes
    ## 6                             No                                        No
    ##      Business.Orientation Score   Rank
    ## 1 Sustainability-oriented     1    Low
    ## 2                 Neutral     2 Medium
    ## 3                 Neutral     2 Medium
    ## 4 Sustainability-oriented     1    Low
    ## 5                 Neutral     1    Low
    ## 6 Sustainability-oriented     0    Low

**Transforming into binomial-like variable**

Now, let’s convert all the “Yes” to 1, and all the “No” to 0s for easier
analysis. To make things easier, we will first need to change all
factors to characters. Let’s define a function for that:

``` r
# Defining the unfactorize function

unfactorize <- function(data){
  for(i in which(sapply(data, class) == "factor")){
    data[[i]] = as.character(data[[i]])
  }
  return(data)
}
```

``` r
# Unfactorizing the data and storing in a new variable

unfactorized_data <- unfactorize(initial_data)
```

``` r
# Transforming into binomial distribution
unfactorized_data[unfactorized_data == "Yes"] <- 1
unfactorized_data[unfactorized_data == "No"] <- 0
```

We need to be aware these are character variables now.

**Clustering nationality
variable**

``` r
# We will first rename all instances of Australia and New Zealand to Oceania
unfactorized_data[unfactorized_data$Nationality == 'Australia' |
                    unfactorized_data$Nationality == 'New Zealand', "Nationality"] <- "Oceania"

# Then we take a reverse approach, and convert all non-US and non-Oceania entires to Europe
unfactorized_data[unfactorized_data$Nationality != 'USA' &
                    unfactorized_data$Nationality != 'Oceania', "Nationality"] <- "Europe"
```

**Transforming back variables to their desired states**

*Transforming numeric-based characters to
numeric*

``` r
#Let's also transform all the numeric-based character data into actual numeric data
unfactorized_data[,c(5,6,7,8,9,11)] <- as.numeric(unlist(unfactorized_data[,c(5,6,7,8,9,11)]))
```

*Transforming character variables back to factors*

``` r
# We will first define a reverse function of unfactorized

factorize <- function(data){
  for(i in which(sapply(data, class) == "character")){
    data[[i]] = as.factor(data[[i]])
  }
  return(data)
}
```

``` r
# Putting everything into a final data frame through factorize function

data <- factorize(unfactorized_data)
```

We are now finally ready to proceed with visualizing and analyzing data.

### Data Exploration

In this section, I perform some of the basic data exploration by
visualizing relationships between variables.

**Size vs. Revenue across Business Orientation, Nationality, and
Transparency Rank**

``` r
#Loading the ggplot2 library
library(ggplot2)
```

``` r
#Scatter against Business Orientation - original
scatter1 <- ggplot(data, aes(x=Size, y=Annual.Revenue)) + 
            geom_point(aes(colour=Business.Orientation)) +
            ggtitle("Size and Annual Revenue versus Business Orientation") +
            theme_minimal() +
            theme(plot.title=element_text( hjust=0.5, vjust=0.8, face='bold'))
scatter1
```

![](supplychainanalysis_git_files/figure-gfm/unnamed-chunk-36-1.png)<!-- -->

We can certainly notice two clear observations:

  - If we imagine a regression line passing through, the
    heteroescadastic-like behavior of data points shows that a
    non-linear relationship between these two variables is more likely.

  - Even without any statistical transformation, neutral—and more
    notably the trend-oriented companies are the ones with greater
    number of employees and greater revenue. Albeit not surprising, it
    is somewhat of a good validation of the quality of collected data.

Let’s try transforming the revenue data to a logarithmic scale and see
what we get.

``` r
#Scatter against Business Orientation

scatter2 <- ggplot(data, aes(x=Size, y=log(Annual.Revenue))) + 
            ggtitle("Size and log(Annual Revenue) versus Business Orientation") +
            geom_point(aes(colour=Business.Orientation)) +
            theme_minimal() +
            theme(plot.title=element_text( hjust=0.5, vjust=0.8, face='bold'))
scatter2
```

![](supplychainanalysis_git_files/figure-gfm/unnamed-chunk-37-1.png)<!-- -->

Now, we can observe a clearer relationship between the size of the
company and the logarithmic scale of annual revenue. The relationship is
not linear, and resembles a logarithmic rise, but it shows clearly that,
as the size and revenue increase for a company, the business orientation
changes from sustainability-oriented to neutral and to trend-oriented,
denoting that companies focused on sustainability tend to be much
smaller and likely less scalable.

We can also compare this relationship by looking at size and revenue
across continents.

``` r
#Scatter against Nationality

scatter3 <- ggplot(data, aes(x=Size, y=log(Annual.Revenue))) + 
            geom_point(aes(colour=Nationality)) +
            ggtitle("Size and log(Annual Revenue) versus Nationality") +
            scale_color_manual(values=c("#E69F00", "#ff00ff", "#006600")) + 
            theme_minimal() +
            theme(plot.title=element_text( hjust=0.5, vjust=0.8, face='bold'))
scatter3
```

![](supplychainanalysis_git_files/figure-gfm/unnamed-chunk-38-1.png)<!-- -->

Since companies from Oceania are not largely present in the dataset, it
is most useful to compare US and European companies across this dataset.
What is most notable is that European companies tend to dominate the
space of the graph with large size and revenues (trend-oriented
companies as seen in previous graph), while US companies tend to
dominate the space with lower size and revenue (sustainability-oriented
companies).

Finally, we can also use this scatter plot to visualize the relationship
across the transparency scores: low, medium, high.

``` r
#Scatter against Rank

scatter4 <- ggplot(data, aes(x=Size, y=log(Annual.Revenue))) + 
            geom_point(aes(colour=Rank)) +
            ggtitle("Size and log(Annual Revenue) versus Transparency Rank") +
            scale_color_manual(values=c("#009900", "#ff0000", "#ff9900")) +
            theme_minimal() +
            theme(plot.title=element_text( hjust=0.5, vjust=0.8, face='bold'))
scatter4
```

![](supplychainanalysis_git_files/figure-gfm/unnamed-chunk-39-1.png)<!-- -->

My hypothesis was that companies with a large number of employees and
annual revenue would score lowest on the supply chain transparency
ranking criteria, but as can be seen from the scatter plot, those
companies tend to be of medium transparency.

Instead, low-transparency companies exist in the space with lower size
and lower revenue. Since the criteria for ranking was based on external
transparency, this could be because larger companies tend to face more
public pressure to publish certain information, such as list of
suppliers or their code of conduct.

**Supply chain transparency ranking across business orientation and
nationality**

If we switch gears now, we can look at how supply chain transparency
rank compares across different critera. This was partially visible from
the previous scatter plots, but we can also visualize the relationship
through simple bar charts. Let’s take a look at the transparency rank
scores across business orientation.

``` r
# Loading forcats package

library(forcats)

# Countplot against business orientation

count1 <- ggplot(data, aes(x=fct_infreq(Rank))) + 
          geom_bar(aes(fill=Business.Orientation), width=0.3) +
          ggtitle("Transparency Rank versus Business Orientation") +
          xlab(" Transparency Rank") +
          ylab("Number of companies") +
          theme_minimal() +
          theme(plot.title=element_text( hjust=0.5, vjust=0.8, face='bold'))
count1
```

![](supplychainanalysis_git_files/figure-gfm/unnamed-chunk-40-1.png)<!-- -->

The most interesting category is that of sustainability-oriented
businesses: although they are the ones that constitute the majority of
the high-ranking companies in terms of transparency, they are also the
ones that take up a large percentage of companies with low transparency.
This was an interesting finding for my graduate research because it
showed that, despite many efforts of sustainability-oriented businesses,
many of them score low on external transparency. My hypothesis, which I
later investigated and confirmed through legal research, was that they
simply do not face enough public pressure from external stakeholders.

I also wanted to see if there were any interesting differences across
different continents when it came to supply chain transparency.

``` r
# Countplot against nationality

count2 <- ggplot(data, aes(x=fct_infreq(Rank))) + 
          geom_bar(aes(fill=Nationality), width=0.3) +
          scale_fill_manual(values=c("Europe" = "#E69F00", "Oceania"= "#ff00ff", "USA"= "#006600")) +
          ggtitle("Transparency Rank versus Nationality") +
          xlab("Transparency Rank") +
          ylab("Number of companies") +
          theme_minimal() +
          theme(plot.title=element_text( hjust=0.5, vjust=0.8, face='bold'))
count2
```

![](supplychainanalysis_git_files/figure-gfm/unnamed-chunk-41-1.png)<!-- -->

This bar graph shows that there doesn’t seem to be any notable
difference across the continents in terms of rank distribution. We can
see that one of the earlier observations is confirmed: according to my
ranking methodology, a larger percentage of high-ranking companies
corresponds to US-based firms. This, of course, can only be extrapolated
through a notably small sample in this dataset as there are very few
high-ranking companies.

**Publishing criteria across the three transparency ranks**

It would also be interesting to see how the three levels of transparency
rank across the five publishing criteria. Before doing so, let’s see
which companies belong to each of the three categories.

``` r
#High
data[data$Rank=="High",]
```

    ##    ï..Company.name  Size Annual.Revenue Nationality Suppliers.published.
    ## 34          Levi's 12500       4.49e+09         USA                    1
    ## 43     Nudie Jeans    50       4.40e+07      Europe                    1
    ## 45       Patagonia  2000       5.70e+08         USA                    1
    ##    CoC.published. Audits.published. Full.cost.breakdown.published.
    ## 34              1                 1                              0
    ## 43              1                 1                              0
    ## 45              1                 1                              0
    ##    Purchasing.practices.documents.published.    Business.Orientation Score
    ## 34                                         1                 Neutral     4
    ## 43                                         1 Sustainability-oriented     4
    ## 45                                         1 Sustainability-oriented     4
    ##    Rank
    ## 34 High
    ## 43 High
    ## 45 High

``` r
#Medium
data[data$Rank=="Medium",]
```

    ##                 ï..Company.name   Size Annual.Revenue Nationality
    ## 2           Abercrombie & Fitch  28500     3519000000         USA
    ## 3                  Acne Studios    500      138031250      Europe
    ## 8                          ASOS   1900     1209620000      Europe
    ## 9                     Braintree     50       47000000      Europe
    ## 10              Brooks Brothers   1500     3709500000         USA
    ## 11                     Burberry   9698     3900000000      Europe
    ## 12      Calvin Klein (thru PVH)  34200     8020000000         USA
    ## 16                Eileen Fisher   1100      429000000         USA
    ## 17                     Everlane    100       50000000         USA
    ## 20                          Gap 141000    15797000000         USA
    ## 22                        Gucci  10000     4300000000      Europe
    ## 24                          H&M 104637    21730000000      Europe
    ## 27                    Honest By     75       47000000      Europe
    ## 28                    Hugo Boss  13764     3089900000      Europe
    ## 29                       J Crew  15300     2510000000         USA
    ## 32                     L.L.Bean   5000     1610000000         USA
    ## 35                        Mango  14400     2211000000      Europe
    ## 38                     Mayamiko     10       47000000      Europe
    ## 40                     New Look  18530     1878156000      Europe
    ## 42         NorthFace (thru VFC)  64000    12400000000         USA
    ## 46                   PeopleTree     50        3125000      Europe
    ## 47                      Primark  58000     6250000000      Europe
    ## 48                 Ralph Lauren  26000     7400000000         USA
    ## 52                Scotch & Soda  16000     2260500000      Europe
    ## 53              Shift to Nature     75       47000000     Oceania
    ## 55 Topshop (thru Arcadia Group)  45000     2587500000      Europe
    ## 56 Uniqlo (thru Fast Retailing)  41646    14440000000      Europe
    ## 57    United Colors of Benetton   9500     2310000000      Europe
    ## 60          Zara (thru Inditex) 152854    23050000000      Europe
    ##    Suppliers.published. CoC.published. Audits.published.
    ## 2                     0              1                 0
    ## 3                     0              1                 0
    ## 8                     0              1                 0
    ## 9                     0              1                 0
    ## 10                    0              1                 0
    ## 11                    0              1                 0
    ## 12                    0              1                 1
    ## 16                    0              1                 0
    ## 17                    1              0                 0
    ## 20                    1              1                 0
    ## 22                    0              1                 0
    ## 24                    1              1                 0
    ## 27                    1              0                 0
    ## 28                    0              1                 0
    ## 29                    0              1                 0
    ## 32                    0              1                 0
    ## 35                    0              1                 0
    ## 38                    1              1                 0
    ## 40                    0              1                 0
    ## 42                    1              1                 0
    ## 46                    1              1                 0
    ## 47                    0              1                 0
    ## 48                    0              1                 0
    ## 52                    0              1                 0
    ## 53                    1              0                 0
    ## 55                    0              1                 0
    ## 56                    0              1                 0
    ## 57                    0              1                 0
    ## 60                    0              1                 0
    ##    Full.cost.breakdown.published.
    ## 2                               0
    ## 3                               0
    ## 8                               0
    ## 9                               0
    ## 10                              0
    ## 11                              0
    ## 12                              0
    ## 16                              0
    ## 17                              1
    ## 20                              0
    ## 22                              0
    ## 24                              0
    ## 27                              1
    ## 28                              0
    ## 29                              0
    ## 32                              0
    ## 35                              0
    ## 38                              0
    ## 40                              0
    ## 42                              0
    ## 46                              0
    ## 47                              0
    ## 48                              0
    ## 52                              0
    ## 53                              0
    ## 55                              0
    ## 56                              0
    ## 57                              0
    ## 60                              0
    ##    Purchasing.practices.documents.published.    Business.Orientation Score
    ## 2                                          1                 Neutral     2
    ## 3                                          1                 Neutral     2
    ## 8                                          1          Trend-oriented     2
    ## 9                                          1 Sustainability-oriented     2
    ## 10                                         1                 Neutral     2
    ## 11                                         1                 Neutral     2
    ## 12                                         1                 Neutral     3
    ## 16                                         1 Sustainability-oriented     2
    ## 17                                         0 Sustainability-oriented     2
    ## 20                                         1          Trend-oriented     3
    ## 22                                         1                 Neutral     2
    ## 24                                         1          Trend-oriented     3
    ## 27                                         0 Sustainability-oriented     2
    ## 28                                         1                 Neutral     2
    ## 29                                         1                 Neutral     2
    ## 32                                         1                 Neutral     2
    ## 35                                         1          Trend-oriented     2
    ## 38                                         0 Sustainability-oriented     2
    ## 40                                         1          Trend-oriented     2
    ## 42                                         1                 Neutral     3
    ## 46                                         1 Sustainability-oriented     3
    ## 47                                         1          Trend-oriented     2
    ## 48                                         1                 Neutral     2
    ## 52                                         1          Trend-oriented     2
    ## 53                                         1 Sustainability-oriented     2
    ## 55                                         1          Trend-oriented     2
    ## 56                                         1          Trend-oriented     2
    ## 57                                         1          Trend-oriented     2
    ## 60                                         1          Trend-oriented     2
    ##      Rank
    ## 2  Medium
    ## 3  Medium
    ## 8  Medium
    ## 9  Medium
    ## 10 Medium
    ## 11 Medium
    ## 12 Medium
    ## 16 Medium
    ## 17 Medium
    ## 20 Medium
    ## 22 Medium
    ## 24 Medium
    ## 27 Medium
    ## 28 Medium
    ## 29 Medium
    ## 32 Medium
    ## 35 Medium
    ## 38 Medium
    ## 40 Medium
    ## 42 Medium
    ## 46 Medium
    ## 47 Medium
    ## 48 Medium
    ## 52 Medium
    ## 53 Medium
    ## 55 Medium
    ## 56 Medium
    ## 57 Medium
    ## 60 Medium

``` r
#Low
data[data$Rank=="Low",]
```

    ##            ï..Company.name   Size Annual.Revenue Nationality
    ## 1                  16Seven     75       47000000      Europe
    ## 4               Aiby Craft     10       47000000      Europe
    ## 5               All Saints   3200      330000000      Europe
    ## 6      Alternative Apparel    180      100000000         USA
    ## 7         American Apparel   7500      609000000         USA
    ## 13                  Chanel  10000     5200000000      Europe
    ## 14         Charlotte Russe  10000      856000000         USA
    ## 15                   Dior  122736    41600000000      Europe
    ## 18              Forever 21  30000     4400000000         USA
    ## 19       French Connection   1999      203608000      Europe
    ## 21           Good Society      75       47000000      Europe
    ## 23                   Guess  13500     2200000000         USA
    ## 25           Henrica Langh     75       47000000      Europe
    ## 26                  Hermes  12244     5370000000      Europe
    ## 30                  Kowtow     75       47000000     Oceania
    ## 31            Krochet Kids    150        1567769         USA
    ## 33                  Lanvin    330      184100000      Europe
    ## 36        Marni (thru OTB)  10000     1590000000      Europe
    ## 37                 MaxMara   5000     1430000000      Europe
    ## 39      Ministry of Supply     50     3709500000         USA
    ## 41               NewYorker  16000     2260500000      Europe
    ## 44 Organic by John Patrick     75       47000000         USA
    ## 49             Reformation    200       25000000         USA
    ## 50            River Island  10000     1166508000      Europe
    ## 51                   rue21  10000     1100000000         USA
    ## 54               Sveekery      75       47000000      Europe
    ## 58        Urban Outfitters  24000     3400000000         USA
    ## 59                    Zady     50       47000000         USA
    ##    Suppliers.published. CoC.published. Audits.published.
    ## 1                     1              0                 0
    ## 4                     0              0                 0
    ## 5                     0              0                 0
    ## 6                     0              0                 0
    ## 7                     0              0                 0
    ## 13                    0              0                 0
    ## 14                    0              0                 0
    ## 15                    0              0                 0
    ## 18                    0              0                 0
    ## 19                    0              0                 0
    ## 21                    0              0                 0
    ## 23                    0              0                 0
    ## 25                    0              0                 0
    ## 26                    0              0                 0
    ## 30                    0              0                 0
    ## 31                    1              0                 0
    ## 33                    0              0                 0
    ## 36                    0              0                 0
    ## 37                    0              0                 0
    ## 39                    0              0                 0
    ## 41                    0              0                 0
    ## 44                    0              0                 0
    ## 49                    0              0                 0
    ## 50                    0              0                 0
    ## 51                    0              0                 0
    ## 54                    0              0                 0
    ## 58                    0              0                 0
    ## 59                    1              0                 0
    ##    Full.cost.breakdown.published.
    ## 1                               0
    ## 4                               0
    ## 5                               0
    ## 6                               0
    ## 7                               0
    ## 13                              0
    ## 14                              0
    ## 15                              0
    ## 18                              0
    ## 19                              0
    ## 21                              0
    ## 23                              0
    ## 25                              0
    ## 26                              0
    ## 30                              0
    ## 31                              0
    ## 33                              0
    ## 36                              0
    ## 37                              0
    ## 39                              0
    ## 41                              0
    ## 44                              0
    ## 49                              0
    ## 50                              0
    ## 51                              0
    ## 54                              0
    ## 58                              0
    ## 59                              0
    ##    Purchasing.practices.documents.published.    Business.Orientation Score
    ## 1                                          0 Sustainability-oriented     1
    ## 4                                          1 Sustainability-oriented     1
    ## 5                                          1                 Neutral     1
    ## 6                                          0 Sustainability-oriented     0
    ## 7                                          1          Trend-oriented     1
    ## 13                                         1                 Neutral     1
    ## 14                                         1          Trend-oriented     1
    ## 15                                         0                 Neutral     0
    ## 18                                         1          Trend-oriented     1
    ## 19                                         1          Trend-oriented     1
    ## 21                                         0 Sustainability-oriented     0
    ## 23                                         1          Trend-oriented     1
    ## 25                                         0 Sustainability-oriented     0
    ## 26                                         1                 Neutral     1
    ## 30                                         0 Sustainability-oriented     0
    ## 31                                         0 Sustainability-oriented     1
    ## 33                                         0                 Neutral     0
    ## 36                                         0                 Neutral     0
    ## 37                                         0                 Neutral     0
    ## 39                                         1                 Neutral     1
    ## 41                                         1          Trend-oriented     1
    ## 44                                         0 Sustainability-oriented     0
    ## 49                                         0 Sustainability-oriented     0
    ## 50                                         1          Trend-oriented     1
    ## 51                                         1          Trend-oriented     1
    ## 54                                         0 Sustainability-oriented     0
    ## 58                                         1          Trend-oriented     1
    ## 59                                         0 Sustainability-oriented     1
    ##    Rank
    ## 1   Low
    ## 4   Low
    ## 5   Low
    ## 6   Low
    ## 7   Low
    ## 13  Low
    ## 14  Low
    ## 15  Low
    ## 18  Low
    ## 19  Low
    ## 21  Low
    ## 23  Low
    ## 25  Low
    ## 26  Low
    ## 30  Low
    ## 31  Low
    ## 33  Low
    ## 36  Low
    ## 37  Low
    ## 39  Low
    ## 41  Low
    ## 44  Low
    ## 49  Low
    ## 50  Low
    ## 51  Low
    ## 54  Low
    ## 58  Low
    ## 59  Low

Before I rename them for processing purposes they are:

  - Suppliers: does the brand publish a list of their suppliers online?

  - Code of Conduct (CoC): does the brand publish a supplier code of
    conduct online?

  - Audits: does the brand publish its supplier audits online?

  - Full-cost breakdown (FCB): does the brand publish a full-cost
    breakdown of its products online?

  - Purchasing practices (PP): does the brand publish a list of
    purchasing practices online?

<!-- end list -->

``` r
#Subsetting the dataframe
criteria <- data[, c(5,6,7,8,9,12)]

#Simplifying the column names
colnames(criteria) <- c("Suppliers", "CoC", "Audits", "FCB", "PP", "Rank")

#Calling the new dataframe
criteria
```

    ##    Suppliers CoC Audits FCB PP   Rank
    ## 1          1   0      0   0  0    Low
    ## 2          0   1      0   0  1 Medium
    ## 3          0   1      0   0  1 Medium
    ## 4          0   0      0   0  1    Low
    ## 5          0   0      0   0  1    Low
    ## 6          0   0      0   0  0    Low
    ## 7          0   0      0   0  1    Low
    ## 8          0   1      0   0  1 Medium
    ## 9          0   1      0   0  1 Medium
    ## 10         0   1      0   0  1 Medium
    ## 11         0   1      0   0  1 Medium
    ## 12         0   1      1   0  1 Medium
    ## 13         0   0      0   0  1    Low
    ## 14         0   0      0   0  1    Low
    ## 15         0   0      0   0  0    Low
    ## 16         0   1      0   0  1 Medium
    ## 17         1   0      0   1  0 Medium
    ## 18         0   0      0   0  1    Low
    ## 19         0   0      0   0  1    Low
    ## 20         1   1      0   0  1 Medium
    ## 21         0   0      0   0  0    Low
    ## 22         0   1      0   0  1 Medium
    ## 23         0   0      0   0  1    Low
    ## 24         1   1      0   0  1 Medium
    ## 25         0   0      0   0  0    Low
    ## 26         0   0      0   0  1    Low
    ## 27         1   0      0   1  0 Medium
    ## 28         0   1      0   0  1 Medium
    ## 29         0   1      0   0  1 Medium
    ## 30         0   0      0   0  0    Low
    ## 31         1   0      0   0  0    Low
    ## 32         0   1      0   0  1 Medium
    ## 33         0   0      0   0  0    Low
    ## 34         1   1      1   0  1   High
    ## 35         0   1      0   0  1 Medium
    ## 36         0   0      0   0  0    Low
    ## 37         0   0      0   0  0    Low
    ## 38         1   1      0   0  0 Medium
    ## 39         0   0      0   0  1    Low
    ## 40         0   1      0   0  1 Medium
    ## 41         0   0      0   0  1    Low
    ## 42         1   1      0   0  1 Medium
    ## 43         1   1      1   0  1   High
    ## 44         0   0      0   0  0    Low
    ## 45         1   1      1   0  1   High
    ## 46         1   1      0   0  1 Medium
    ## 47         0   1      0   0  1 Medium
    ## 48         0   1      0   0  1 Medium
    ## 49         0   0      0   0  0    Low
    ## 50         0   0      0   0  1    Low
    ## 51         0   0      0   0  1    Low
    ## 52         0   1      0   0  1 Medium
    ## 53         1   0      0   0  1 Medium
    ## 54         0   0      0   0  0    Low
    ## 55         0   1      0   0  1 Medium
    ## 56         0   1      0   0  1 Medium
    ## 57         0   1      0   0  1 Medium
    ## 58         0   0      0   0  1    Low
    ## 59         1   0      0   0  0    Low
    ## 60         0   1      0   0  1 Medium

``` r
# Calling the advanced data manipulation packages
library(tidyr)
library(dplyr)
```

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

``` r
# Now we need to count how many times we see "1"s in the columns for publishing criteria

criteria %>%
  select(Suppliers, CoC, Audits, FCB, PP, Rank) %>%
  gather(key, value, Suppliers:PP) %>%
  filter(value == 1) %>%
  ggplot(aes(fct_infreq(key)), ..count..) +
  geom_bar(width=0.3, aes(fill=Rank)) +
  scale_fill_manual(values=c("#009900", "#ff0000", "#ff9900")) +
  ggtitle("Publishing Criteria versus Transparency Rank") +
  xlab("Publishing criteria") +
  ylab("Number of companies") +
  theme_minimal() +
  theme(plot.title=element_text( hjust=0.5, vjust=0.8, face='bold'))
```

![](supplychainanalysis_git_files/figure-gfm/unnamed-chunk-47-1.png)<!-- -->

The chart shows few interesting findings:

  - In general, most brands publish their purchasing practices.
    Conversely, full-cost breakdown is the category with the least
    number of brands as only a few of them publish their full-cost
    breakdown. Those brands, at the time of data collection,
    were:

<!-- end list -->

``` r
data[data$Full.cost.breakdown.published.==1,]
```

    ##    ï..Company.name Size Annual.Revenue Nationality Suppliers.published.
    ## 17        Everlane  100        5.0e+07         USA                    1
    ## 27       Honest By   75        4.7e+07      Europe                    1
    ##    CoC.published. Audits.published. Full.cost.breakdown.published.
    ## 17              0                 0                              1
    ## 27              0                 0                              1
    ##    Purchasing.practices.documents.published.    Business.Orientation Score
    ## 17                                         0 Sustainability-oriented     2
    ## 27                                         0 Sustainability-oriented     2
    ##      Rank
    ## 17 Medium
    ## 27 Medium

  - Most brands do not publish their audits either. Those brands that
    did so at the time of data collection were:

<!-- end list -->

``` r
data[data$Audits.published.==1,]
```

    ##            ï..Company.name  Size Annual.Revenue Nationality
    ## 12 Calvin Klein (thru PVH) 34200       8.02e+09         USA
    ## 34                  Levi's 12500       4.49e+09         USA
    ## 43             Nudie Jeans    50       4.40e+07      Europe
    ## 45               Patagonia  2000       5.70e+08         USA
    ##    Suppliers.published. CoC.published. Audits.published.
    ## 12                    0              1                 1
    ## 34                    1              1                 1
    ## 43                    1              1                 1
    ## 45                    1              1                 1
    ##    Full.cost.breakdown.published.
    ## 12                              0
    ## 34                              0
    ## 43                              0
    ## 45                              0
    ##    Purchasing.practices.documents.published.    Business.Orientation Score
    ## 12                                         1                 Neutral     3
    ## 34                                         1                 Neutral     4
    ## 43                                         1 Sustainability-oriented     4
    ## 45                                         1 Sustainability-oriented     4
    ##      Rank
    ## 12 Medium
    ## 34   High
    ## 43   High
    ## 45   High

  - Low-ranking brands in this dataset only publish their list of
    suppliers and their purchasing practices. Interestingly, none of the
    high-ranking brands have published their full-cost breakdown.

**Statistical Analysis of Transparency Score**

Finally, the last question is: are the three models of business
orientation: sustainability-oriented, neutral, and trend-oriented truly
different in terms of their transparency score? For this analysis, I
wanted to implement ANOVA and detect if the difference between their
mean scores was statistically significant. Given the low number of data
points in this dataset, my hypothesis was that ANOVA would produce
p-value higher than 0.05, thus signaling there is no evidence to
conclude the groups are statistically different.

Before doing so, I thought it would be worth listing companies across
their business orientations.

``` r
#Sustainability-oriented
data[data$Business.Orientation=="Sustainability-oriented",]
```

    ##            ï..Company.name Size Annual.Revenue Nationality
    ## 1                  16Seven   75       47000000      Europe
    ## 4               Aiby Craft   10       47000000      Europe
    ## 6      Alternative Apparel  180      100000000         USA
    ## 9                Braintree   50       47000000      Europe
    ## 16           Eileen Fisher 1100      429000000         USA
    ## 17                Everlane  100       50000000         USA
    ## 21           Good Society    75       47000000      Europe
    ## 25           Henrica Langh   75       47000000      Europe
    ## 27               Honest By   75       47000000      Europe
    ## 30                  Kowtow   75       47000000     Oceania
    ## 31            Krochet Kids  150        1567769         USA
    ## 38                Mayamiko   10       47000000      Europe
    ## 43             Nudie Jeans   50       44000000      Europe
    ## 44 Organic by John Patrick   75       47000000         USA
    ## 45               Patagonia 2000      570000000         USA
    ## 46              PeopleTree   50        3125000      Europe
    ## 49             Reformation  200       25000000         USA
    ## 53         Shift to Nature   75       47000000     Oceania
    ## 54               Sveekery    75       47000000      Europe
    ## 59                    Zady   50       47000000         USA
    ##    Suppliers.published. CoC.published. Audits.published.
    ## 1                     1              0                 0
    ## 4                     0              0                 0
    ## 6                     0              0                 0
    ## 9                     0              1                 0
    ## 16                    0              1                 0
    ## 17                    1              0                 0
    ## 21                    0              0                 0
    ## 25                    0              0                 0
    ## 27                    1              0                 0
    ## 30                    0              0                 0
    ## 31                    1              0                 0
    ## 38                    1              1                 0
    ## 43                    1              1                 1
    ## 44                    0              0                 0
    ## 45                    1              1                 1
    ## 46                    1              1                 0
    ## 49                    0              0                 0
    ## 53                    1              0                 0
    ## 54                    0              0                 0
    ## 59                    1              0                 0
    ##    Full.cost.breakdown.published.
    ## 1                               0
    ## 4                               0
    ## 6                               0
    ## 9                               0
    ## 16                              0
    ## 17                              1
    ## 21                              0
    ## 25                              0
    ## 27                              1
    ## 30                              0
    ## 31                              0
    ## 38                              0
    ## 43                              0
    ## 44                              0
    ## 45                              0
    ## 46                              0
    ## 49                              0
    ## 53                              0
    ## 54                              0
    ## 59                              0
    ##    Purchasing.practices.documents.published.    Business.Orientation Score
    ## 1                                          0 Sustainability-oriented     1
    ## 4                                          1 Sustainability-oriented     1
    ## 6                                          0 Sustainability-oriented     0
    ## 9                                          1 Sustainability-oriented     2
    ## 16                                         1 Sustainability-oriented     2
    ## 17                                         0 Sustainability-oriented     2
    ## 21                                         0 Sustainability-oriented     0
    ## 25                                         0 Sustainability-oriented     0
    ## 27                                         0 Sustainability-oriented     2
    ## 30                                         0 Sustainability-oriented     0
    ## 31                                         0 Sustainability-oriented     1
    ## 38                                         0 Sustainability-oriented     2
    ## 43                                         1 Sustainability-oriented     4
    ## 44                                         0 Sustainability-oriented     0
    ## 45                                         1 Sustainability-oriented     4
    ## 46                                         1 Sustainability-oriented     3
    ## 49                                         0 Sustainability-oriented     0
    ## 53                                         1 Sustainability-oriented     2
    ## 54                                         0 Sustainability-oriented     0
    ## 59                                         0 Sustainability-oriented     1
    ##      Rank
    ## 1     Low
    ## 4     Low
    ## 6     Low
    ## 9  Medium
    ## 16 Medium
    ## 17 Medium
    ## 21    Low
    ## 25    Low
    ## 27 Medium
    ## 30    Low
    ## 31    Low
    ## 38 Medium
    ## 43   High
    ## 44    Low
    ## 45   High
    ## 46 Medium
    ## 49    Low
    ## 53 Medium
    ## 54    Low
    ## 59    Low

``` r
#Neutral
data[data$Business.Orientation=="Neutral",]
```

    ##            ï..Company.name   Size Annual.Revenue Nationality
    ## 2      Abercrombie & Fitch  28500     3519000000         USA
    ## 3             Acne Studios    500      138031250      Europe
    ## 5               All Saints   3200      330000000      Europe
    ## 10         Brooks Brothers   1500     3709500000         USA
    ## 11                Burberry   9698     3900000000      Europe
    ## 12 Calvin Klein (thru PVH)  34200     8020000000         USA
    ## 13                  Chanel  10000     5200000000      Europe
    ## 15                   Dior  122736    41600000000      Europe
    ## 22                   Gucci  10000     4300000000      Europe
    ## 26                  Hermes  12244     5370000000      Europe
    ## 28               Hugo Boss  13764     3089900000      Europe
    ## 29                  J Crew  15300     2510000000         USA
    ## 32                L.L.Bean   5000     1610000000         USA
    ## 33                  Lanvin    330      184100000      Europe
    ## 34                  Levi's  12500     4490000000         USA
    ## 36        Marni (thru OTB)  10000     1590000000      Europe
    ## 37                 MaxMara   5000     1430000000      Europe
    ## 39      Ministry of Supply     50     3709500000         USA
    ## 42    NorthFace (thru VFC)  64000    12400000000         USA
    ## 48            Ralph Lauren  26000     7400000000         USA
    ##    Suppliers.published. CoC.published. Audits.published.
    ## 2                     0              1                 0
    ## 3                     0              1                 0
    ## 5                     0              0                 0
    ## 10                    0              1                 0
    ## 11                    0              1                 0
    ## 12                    0              1                 1
    ## 13                    0              0                 0
    ## 15                    0              0                 0
    ## 22                    0              1                 0
    ## 26                    0              0                 0
    ## 28                    0              1                 0
    ## 29                    0              1                 0
    ## 32                    0              1                 0
    ## 33                    0              0                 0
    ## 34                    1              1                 1
    ## 36                    0              0                 0
    ## 37                    0              0                 0
    ## 39                    0              0                 0
    ## 42                    1              1                 0
    ## 48                    0              1                 0
    ##    Full.cost.breakdown.published.
    ## 2                               0
    ## 3                               0
    ## 5                               0
    ## 10                              0
    ## 11                              0
    ## 12                              0
    ## 13                              0
    ## 15                              0
    ## 22                              0
    ## 26                              0
    ## 28                              0
    ## 29                              0
    ## 32                              0
    ## 33                              0
    ## 34                              0
    ## 36                              0
    ## 37                              0
    ## 39                              0
    ## 42                              0
    ## 48                              0
    ##    Purchasing.practices.documents.published. Business.Orientation Score
    ## 2                                          1              Neutral     2
    ## 3                                          1              Neutral     2
    ## 5                                          1              Neutral     1
    ## 10                                         1              Neutral     2
    ## 11                                         1              Neutral     2
    ## 12                                         1              Neutral     3
    ## 13                                         1              Neutral     1
    ## 15                                         0              Neutral     0
    ## 22                                         1              Neutral     2
    ## 26                                         1              Neutral     1
    ## 28                                         1              Neutral     2
    ## 29                                         1              Neutral     2
    ## 32                                         1              Neutral     2
    ## 33                                         0              Neutral     0
    ## 34                                         1              Neutral     4
    ## 36                                         0              Neutral     0
    ## 37                                         0              Neutral     0
    ## 39                                         1              Neutral     1
    ## 42                                         1              Neutral     3
    ## 48                                         1              Neutral     2
    ##      Rank
    ## 2  Medium
    ## 3  Medium
    ## 5     Low
    ## 10 Medium
    ## 11 Medium
    ## 12 Medium
    ## 13    Low
    ## 15    Low
    ## 22 Medium
    ## 26    Low
    ## 28 Medium
    ## 29 Medium
    ## 32 Medium
    ## 33    Low
    ## 34   High
    ## 36    Low
    ## 37    Low
    ## 39    Low
    ## 42 Medium
    ## 48 Medium

``` r
#Trend-oriented
data[data$Business.Orientation=="Trend-oriented",]
```

    ##                 ï..Company.name   Size Annual.Revenue Nationality
    ## 7              American Apparel   7500      609000000         USA
    ## 8                          ASOS   1900     1209620000      Europe
    ## 14              Charlotte Russe  10000      856000000         USA
    ## 18                   Forever 21  30000     4400000000         USA
    ## 19            French Connection   1999      203608000      Europe
    ## 20                          Gap 141000    15797000000         USA
    ## 23                        Guess  13500     2200000000         USA
    ## 24                          H&M 104637    21730000000      Europe
    ## 35                        Mango  14400     2211000000      Europe
    ## 40                     New Look  18530     1878156000      Europe
    ## 41                    NewYorker  16000     2260500000      Europe
    ## 47                      Primark  58000     6250000000      Europe
    ## 50                 River Island  10000     1166508000      Europe
    ## 51                        rue21  10000     1100000000         USA
    ## 52                Scotch & Soda  16000     2260500000      Europe
    ## 55 Topshop (thru Arcadia Group)  45000     2587500000      Europe
    ## 56 Uniqlo (thru Fast Retailing)  41646    14440000000      Europe
    ## 57    United Colors of Benetton   9500     2310000000      Europe
    ## 58             Urban Outfitters  24000     3400000000         USA
    ## 60          Zara (thru Inditex) 152854    23050000000      Europe
    ##    Suppliers.published. CoC.published. Audits.published.
    ## 7                     0              0                 0
    ## 8                     0              1                 0
    ## 14                    0              0                 0
    ## 18                    0              0                 0
    ## 19                    0              0                 0
    ## 20                    1              1                 0
    ## 23                    0              0                 0
    ## 24                    1              1                 0
    ## 35                    0              1                 0
    ## 40                    0              1                 0
    ## 41                    0              0                 0
    ## 47                    0              1                 0
    ## 50                    0              0                 0
    ## 51                    0              0                 0
    ## 52                    0              1                 0
    ## 55                    0              1                 0
    ## 56                    0              1                 0
    ## 57                    0              1                 0
    ## 58                    0              0                 0
    ## 60                    0              1                 0
    ##    Full.cost.breakdown.published.
    ## 7                               0
    ## 8                               0
    ## 14                              0
    ## 18                              0
    ## 19                              0
    ## 20                              0
    ## 23                              0
    ## 24                              0
    ## 35                              0
    ## 40                              0
    ## 41                              0
    ## 47                              0
    ## 50                              0
    ## 51                              0
    ## 52                              0
    ## 55                              0
    ## 56                              0
    ## 57                              0
    ## 58                              0
    ## 60                              0
    ##    Purchasing.practices.documents.published. Business.Orientation Score
    ## 7                                          1       Trend-oriented     1
    ## 8                                          1       Trend-oriented     2
    ## 14                                         1       Trend-oriented     1
    ## 18                                         1       Trend-oriented     1
    ## 19                                         1       Trend-oriented     1
    ## 20                                         1       Trend-oriented     3
    ## 23                                         1       Trend-oriented     1
    ## 24                                         1       Trend-oriented     3
    ## 35                                         1       Trend-oriented     2
    ## 40                                         1       Trend-oriented     2
    ## 41                                         1       Trend-oriented     1
    ## 47                                         1       Trend-oriented     2
    ## 50                                         1       Trend-oriented     1
    ## 51                                         1       Trend-oriented     1
    ## 52                                         1       Trend-oriented     2
    ## 55                                         1       Trend-oriented     2
    ## 56                                         1       Trend-oriented     2
    ## 57                                         1       Trend-oriented     2
    ## 58                                         1       Trend-oriented     1
    ## 60                                         1       Trend-oriented     2
    ##      Rank
    ## 7     Low
    ## 8  Medium
    ## 14    Low
    ## 18    Low
    ## 19    Low
    ## 20 Medium
    ## 23    Low
    ## 24 Medium
    ## 35 Medium
    ## 40 Medium
    ## 41    Low
    ## 47 Medium
    ## 50    Low
    ## 51    Low
    ## 52 Medium
    ## 55 Medium
    ## 56 Medium
    ## 57 Medium
    ## 58    Low
    ## 60 Medium

Let’s define a new dataframe only with the required columns:

``` r
#Extracting a new dataframe

anova_data <- data[,c(10,11)]
anova_data
```

    ##       Business.Orientation Score
    ## 1  Sustainability-oriented     1
    ## 2                  Neutral     2
    ## 3                  Neutral     2
    ## 4  Sustainability-oriented     1
    ## 5                  Neutral     1
    ## 6  Sustainability-oriented     0
    ## 7           Trend-oriented     1
    ## 8           Trend-oriented     2
    ## 9  Sustainability-oriented     2
    ## 10                 Neutral     2
    ## 11                 Neutral     2
    ## 12                 Neutral     3
    ## 13                 Neutral     1
    ## 14          Trend-oriented     1
    ## 15                 Neutral     0
    ## 16 Sustainability-oriented     2
    ## 17 Sustainability-oriented     2
    ## 18          Trend-oriented     1
    ## 19          Trend-oriented     1
    ## 20          Trend-oriented     3
    ## 21 Sustainability-oriented     0
    ## 22                 Neutral     2
    ## 23          Trend-oriented     1
    ## 24          Trend-oriented     3
    ## 25 Sustainability-oriented     0
    ## 26                 Neutral     1
    ## 27 Sustainability-oriented     2
    ## 28                 Neutral     2
    ## 29                 Neutral     2
    ## 30 Sustainability-oriented     0
    ## 31 Sustainability-oriented     1
    ## 32                 Neutral     2
    ## 33                 Neutral     0
    ## 34                 Neutral     4
    ## 35          Trend-oriented     2
    ## 36                 Neutral     0
    ## 37                 Neutral     0
    ## 38 Sustainability-oriented     2
    ## 39                 Neutral     1
    ## 40          Trend-oriented     2
    ## 41          Trend-oriented     1
    ## 42                 Neutral     3
    ## 43 Sustainability-oriented     4
    ## 44 Sustainability-oriented     0
    ## 45 Sustainability-oriented     4
    ## 46 Sustainability-oriented     3
    ## 47          Trend-oriented     2
    ## 48                 Neutral     2
    ## 49 Sustainability-oriented     0
    ## 50          Trend-oriented     1
    ## 51          Trend-oriented     1
    ## 52          Trend-oriented     2
    ## 53 Sustainability-oriented     2
    ## 54 Sustainability-oriented     0
    ## 55          Trend-oriented     2
    ## 56          Trend-oriented     2
    ## 57          Trend-oriented     2
    ## 58          Trend-oriented     1
    ## 59 Sustainability-oriented     1
    ## 60          Trend-oriented     2

``` r
# Checking order of the Business Orientation levels
levels(anova_data$Business.Orientation)
```

    ## [1] "Neutral"                 "Sustainability-oriented"
    ## [3] "Trend-oriented"

``` r
# Let's reorder them

anova_data$Business.Orientation <- ordered(anova_data$Business.Orientation,
                          levels = c("Sustainability-oriented", "Neutral", "Trend-oriented"))
levels(anova_data$Business.Orientation)
```

    ## [1] "Sustainability-oriented" "Neutral"                
    ## [3] "Trend-oriented"

``` r
# Let's compute summary statistics by Busines Orientation group

group_by(anova_data, Business.Orientation) %>%
  summarise(
    count = n(),
    mean = mean(Score),
    st_dev = sd(Score)
  )
```

    ## # A tibble: 3 x 4
    ##   Business.Orientation    count  mean st_dev
    ##   <ord>                   <int> <dbl>  <dbl>
    ## 1 Sustainability-oriented    20  1.35  1.31 
    ## 2 Neutral                    20  1.6   1.10 
    ## 3 Trend-oriented             20  1.65  0.671

``` r
# Let's visualize these statistics throught box-plots

boxplot1 <- ggplot(anova_data, aes(x=Business.Orientation, y=Score, colour=Business.Orientation)) +
           ggtitle("Score and Business Orientation Boxplots") +
           geom_jitter() +
           geom_boxplot(size=1, alpha=0.5) +
           theme_minimal() +
           theme(plot.title=element_text( hjust=0.5, vjust=0.8, face='bold'))
boxplot1        
```

![](supplychainanalysis_git_files/figure-gfm/unnamed-chunk-57-1.png)<!-- -->

Given the few discrete values of transparency scores, it is clear that
the boxplots also become less informative. For example, for neutral and
trend-oriented companies, the median climbs up to the top of the box,
and trend-oriented boxplot is missing a lower whisker, as the minimum
observed value is 1 and at least 25% of the values are 1. At the same
time, the boxplots show that, given the low volume of datapoints in this
case, we are unlikely to observe a statistically significant difference
in means between these three categories.

We can also visualie this relationship through the mean-standard error
graph.

``` r
# Mean-with-SE plot
library(ggpubr)
```

    ## Loading required package: magrittr

    ## 
    ## Attaching package: 'magrittr'

    ## The following object is masked from 'package:tidyr':
    ## 
    ##     extract

``` r
ggline(anova_data, x = "Business.Orientation", y = "Score", 
       add = c("mean_se", "jitter"), 
       order = c("Sustainability-oriented", "Neutral", "Trend-oriented"),
       ylab = "Score", xlab = "Business.Orientation")
```

![](supplychainanalysis_git_files/figure-gfm/unnamed-chunk-58-1.png)<!-- -->

Interestingly, we can see that the mean does slightly increase in terms
of score as we move from sustainability-oriented to trend-oriented,
indicating improvement in transparency. We can also notice a decrease in
standard error as we move from sustainability-oriented to
trend-oriented. These differences are still unlikely to be significant,
but let’s check ANOVA either way.

``` r
# ANOVA analysis
anova <- aov(Score ~ Business.Orientation, data = anova_data)
# Summary of the analysis
summary(anova)
```

    ##                      Df Sum Sq Mean Sq F value Pr(>F)
    ## Business.Orientation  2   1.03  0.5167   0.461  0.633
    ## Residuals            57  63.90  1.1211

What this shows, essentially, is that we cannot reject the
null-hypothesis, which states that the mean scores of these three groups
are the same. Of course, I’d argue that we would be likely to observe a
different truth in reality with a greater number of data points and a
more granular scoring methodology. Furthermore, since the dataset was
built through a qualitative and manual data collection, it’s possible
that some of the information in the dataset is incomplete, and that the
scores are indeed different.
