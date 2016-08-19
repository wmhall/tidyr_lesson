`tidyr` lesson
================

What is tidy data?
------------------

A large chunk of your time working with data in `r` is going to be spent cleaning and preparing data. *Tidy* data is a standardized way of organizing data; it was proposed by Hadley Wickham and is designed to make you more effective at exploring and analyzing your data.

Data is *tidy* if it is in a table that follows these rules:

-   Each column is a variable
-   Each row is an observation\*

In practical terms this largely translates into getting each variable into it's own column. `tidyr` helps you do this, but more broadly, `tidyr` gives you a set of functions to manipulate data frames.

### Why tidy data?

-   structuring data frames in a consistent way will make you faster at extracting the information you are interested in.
-   `dplry` and `ggplot2` and other packages in the tidyverse are designed to work with tidy data.

-   becoming proficient at manipulating data frames is one of the most important r skills you can develop.

Example of tidy data
--------------------

``` r
#table1 is tidy
table1
```

    ## Source: local data frame [6 x 4]
    ## 
    ##       country  year  cases population
    ##         (chr) (int)  (int)      (int)
    ## 1 Afghanistan  1999    745   19987071
    ## 2 Afghanistan  2000   2666   20595360
    ## 3      Brazil  1999  37737  172006362
    ## 4      Brazil  2000  80488  174504898
    ## 5       China  1999 212258 1272915272
    ## 6       China  2000 213766 1280428583

Examples of non-tidy data
-------------------------

Non-tidy data typically has one of two problems but it can have both.

-   Variables are spread across multiple columns.
-   Observations are scattered across multiple rows.

### Variables spread across multiple columns

``` r
table4a 
```

    ## Source: local data frame [3 x 3]
    ## 
    ##       country   1999   2000
    ##         (chr)  (int)  (int)
    ## 1 Afghanistan    745   2666
    ## 2      Brazil  37737  80488
    ## 3       China 212258 213766

### Observations scattered across multiple rows

``` r
table2
```

    ## Source: local data frame [12 x 4]
    ## 
    ##        country  year       type      count
    ##          (chr) (int)      (chr)      (int)
    ## 1  Afghanistan  1999      cases        745
    ## 2  Afghanistan  1999 population   19987071
    ## 3  Afghanistan  2000      cases       2666
    ## 4  Afghanistan  2000 population   20595360
    ## 5       Brazil  1999      cases      37737
    ## 6       Brazil  1999 population  172006362
    ## 7       Brazil  2000      cases      80488
    ## 8       Brazil  2000 population  174504898
    ## 9        China  1999      cases     212258
    ## 10       China  1999 population 1272915272
    ## 11       China  2000      cases     213766
    ## 12       China  2000 population 1280428583

Gathering and spreading
-----------------------

`gather()` and `spread()` are functions designed to help you tidy your data by addressing these two common problems.

### `gather()`

`gather()` lets you gather variables that are spread across multiple columns (i.e. make long/tall data frames).

``` r
table4a
```

    ## Source: local data frame [3 x 3]
    ## 
    ##       country   1999   2000
    ##         (chr)  (int)  (int)
    ## 1 Afghanistan    745   2666
    ## 2      Brazil  37737  80488
    ## 3       China 212258 213766

Conceptually, we want to gather up the two variables `1999` and `2000` into a single variable and create a new variable, `year`, to track which year that case comes from (i.e. 1999 vs 2000).

``` r
#key is a new variable that tracks which year a value comes from

#value is a new variable that will contain all the data that we gather into a siginle column (i.e. the cases)

#verbose version
gather(data = table4a, key = year, value = cases, `1999`, `2000`)
```

    ## Source: local data frame [6 x 3]
    ## 
    ##       country  year  cases
    ##         (chr) (chr)  (int)
    ## 1 Afghanistan  1999    745
    ## 2      Brazil  1999  37737
    ## 3       China  1999 212258
    ## 4 Afghanistan  2000   2666
    ## 5      Brazil  2000  80488
    ## 6       China  2000 213766

``` r
#piped version; note the use of -country to specify columns to gather
table4a %>% 
  gather(year, cases, -country)
```

    ## Source: local data frame [6 x 3]
    ## 
    ##       country  year  cases
    ##         (chr) (chr)  (int)
    ## 1 Afghanistan  1999    745
    ## 2      Brazil  1999  37737
    ## 3       China  1999 212258
    ## 4 Afghanistan  2000   2666
    ## 5      Brazil  2000  80488
    ## 6       China  2000 213766

### `spread()`

`spread()` is the inverse of gather; it lets you spread observations that are scattered across multiple rows (i.e. make wide data frames).

``` r
table2
```

    ## Source: local data frame [12 x 4]
    ## 
    ##        country  year       type      count
    ##          (chr) (int)      (chr)      (int)
    ## 1  Afghanistan  1999      cases        745
    ## 2  Afghanistan  1999 population   19987071
    ## 3  Afghanistan  2000      cases       2666
    ## 4  Afghanistan  2000 population   20595360
    ## 5       Brazil  1999      cases      37737
    ## 6       Brazil  1999 population  172006362
    ## 7       Brazil  2000      cases      80488
    ## 8       Brazil  2000 population  174504898
    ## 9        China  1999      cases     212258
    ## 10       China  1999 population 1272915272
    ## 11       China  2000      cases     213766
    ## 12       China  2000 population 1280428583

Conceptually, we want to create two new columns in our data frame that represent the variables `cases` and `population`, and we want to fill those variables with the appropriate values from the `count` variable.

``` r
#key = column that contains the variable names that we want to create
#value = column that contain that values to go into those variables 

#verbose version
spread(data = table2, key = type, value = count)
```

    ## Source: local data frame [6 x 4]
    ## 
    ##       country  year  cases population
    ##         (chr) (int)  (int)      (int)
    ## 1 Afghanistan  1999    745   19987071
    ## 2 Afghanistan  2000   2666   20595360
    ## 3      Brazil  1999  37737  172006362
    ## 4      Brazil  2000  80488  174504898
    ## 5       China  1999 212258 1272915272
    ## 6       China  2000 213766 1280428583

``` r
#piped version
table2 %>% 
  spread(type, count)
```

    ## Source: local data frame [6 x 4]
    ## 
    ##       country  year  cases population
    ##         (chr) (int)  (int)      (int)
    ## 1 Afghanistan  1999    745   19987071
    ## 2 Afghanistan  2000   2666   20595360
    ## 3      Brazil  1999  37737  172006362
    ## 4      Brazil  2000  80488  174504898
    ## 5       China  1999 212258 1272915272
    ## 6       China  2000 213766 1280428583

### Challenge problem 1

1.  In `table4b`, year is spread across two columns.
    1.  Do you need `gather()` or `spread()` to tidy this data?
    2.  Create a tidy data frame from `table4b`.
    3.  Once the data is tidy, find out the overall mean for tb cases.
    4.  Using the tidy and non-tidy data frames find the mean tb cases for each year. How do the two approaches (non-tidy vs. tidy) compare?

Seperating and uniting
----------------------

### `seperate()`

`seperate()` splits a column into multiple columns.

``` r
table3
```

    ## Source: local data frame [6 x 3]
    ## 
    ##       country  year              rate
    ##         (chr) (int)             (chr)
    ## 1 Afghanistan  1999      745/19987071
    ## 2 Afghanistan  2000     2666/20595360
    ## 3      Brazil  1999   37737/172006362
    ## 4      Brazil  2000   80488/174504898
    ## 5       China  1999 212258/1272915272
    ## 6       China  2000 213766/1280428583

``` r
#verbose version
separate(data = table3, col = rate, 
         into = c("cases", "population"), sep = "/")
```

    ## Source: local data frame [6 x 4]
    ## 
    ##       country  year  cases population
    ##         (chr) (int)  (chr)      (chr)
    ## 1 Afghanistan  1999    745   19987071
    ## 2 Afghanistan  2000   2666   20595360
    ## 3      Brazil  1999  37737  172006362
    ## 4      Brazil  2000  80488  174504898
    ## 5       China  1999 212258 1272915272
    ## 6       China  2000 213766 1280428583

``` r
#use for sepertate
table3 %>% 
  separate(rate, c("cases", "population"))
```

    ## Source: local data frame [6 x 4]
    ## 
    ##       country  year  cases population
    ##         (chr) (int)  (chr)      (chr)
    ## 1 Afghanistan  1999    745   19987071
    ## 2 Afghanistan  2000   2666   20595360
    ## 3      Brazil  1999  37737  172006362
    ## 4      Brazil  2000  80488  174504898
    ## 5       China  1999 212258 1272915272
    ## 6       China  2000 213766 1280428583

``` r
#convert = T makes a guess at an appropriate column type
table3 %>% 
  separate(rate, c("cases", "population"), convert = T)
```

    ## Source: local data frame [6 x 4]
    ## 
    ##       country  year  cases population
    ##         (chr) (int)  (int)      (int)
    ## 1 Afghanistan  1999    745   19987071
    ## 2 Afghanistan  2000   2666   20595360
    ## 3      Brazil  1999  37737  172006362
    ## 4      Brazil  2000  80488  174504898
    ## 5       China  1999 212258 1272915272
    ## 6       China  2000 213766 1280428583

### `unite()`

`unite()` is the inverse of `seperate()`; it lets you combine two columns.

``` r
#unite 
table5
```

    ## Source: local data frame [6 x 4]
    ## 
    ##       country century  year              rate
    ##         (chr)   (chr) (chr)             (chr)
    ## 1 Afghanistan      19    99      745/19987071
    ## 2 Afghanistan      20    00     2666/20595360
    ## 3      Brazil      19    99   37737/172006362
    ## 4      Brazil      20    00   80488/174504898
    ## 5       China      19    99 212258/1272915272
    ## 6       China      20    00 213766/1280428583

``` r
#verbose; note that it drops the original columns and uses _ to combine
unite(data = table5, col = date, century, year)
```

    ## Source: local data frame [6 x 3]
    ## 
    ##       country  date              rate
    ##         (chr) (chr)             (chr)
    ## 1 Afghanistan 19_99      745/19987071
    ## 2 Afghanistan 20_00     2666/20595360
    ## 3      Brazil 19_99   37737/172006362
    ## 4      Brazil 20_00   80488/174504898
    ## 5       China 19_99 212258/1272915272
    ## 6       China 20_00 213766/1280428583

``` r
unite(data = table5, col = date, century, year, sep = "", remove = F)
```

    ## Source: local data frame [6 x 5]
    ## 
    ##       country  date century  year              rate
    ##         (chr) (chr)   (chr) (chr)             (chr)
    ## 1 Afghanistan  1999      19    99      745/19987071
    ## 2 Afghanistan  2000      20    00     2666/20595360
    ## 3      Brazil  1999      19    99   37737/172006362
    ## 4      Brazil  2000      20    00   80488/174504898
    ## 5       China  1999      19    99 212258/1272915272
    ## 6       China  2000      20    00 213766/1280428583

``` r
#pipe version

table5 %>% 
  unite(col = date, century, year, sep = "")
```

    ## Source: local data frame [6 x 3]
    ## 
    ##       country  date              rate
    ##         (chr) (chr)             (chr)
    ## 1 Afghanistan  1999      745/19987071
    ## 2 Afghanistan  2000     2666/20595360
    ## 3      Brazil  1999   37737/172006362
    ## 4      Brazil  2000   80488/174504898
    ## 5       China  1999 212258/1272915272
    ## 6       China  2000 213766/1280428583

### Challenge problem 2

1.  Take a look at `table3` and `table5`. Using either `separate()` or `unite()` make `table3` identical to `table5`. Check your work using the `identical()` function. Hint you might need to look at the help pages for either `unite()` or `separate()`, taking a close look at the sep argument.

Other useful tidyr functions
----------------------------

`drop_na()`
-----------

`drop_na()` lets you drop rows with missing values

``` r
smiths
```

    ## Source: local data frame [2 x 5]
    ## 
    ##      subject  time   age weight height
    ##        (chr) (dbl) (dbl)  (dbl)  (dbl)
    ## 1 John Smith     1    33     90   1.87
    ## 2 Mary Smith     1    NA     NA   1.54

``` r
smiths %>% drop_na
```

    ## Source: local data frame [1 x 5]
    ## 
    ##      subject  time   age weight height
    ##        (chr) (dbl) (dbl)  (dbl)  (dbl)
    ## 1 John Smith     1    33     90   1.87

`complete()`
------------

`complete()` takes a data frame and makes implicitly missing values explicitly missing.

``` r
population
```

    ## Source: local data frame [4,060 x 3]
    ## 
    ##        country  year population
    ##          (chr) (int)      (int)
    ## 1  Afghanistan  1995   17586073
    ## 2  Afghanistan  1996   18415307
    ## 3  Afghanistan  1997   19021226
    ## 4  Afghanistan  1998   19496836
    ## 5  Afghanistan  1999   19987071
    ## 6  Afghanistan  2000   20595360
    ## 7  Afghanistan  2001   21347782
    ## 8  Afghanistan  2002   22202806
    ## 9  Afghanistan  2003   23116142
    ## 10 Afghanistan  2004   24018682
    ## ..         ...   ...        ...

``` r
population %>% 
  filter(country == "South Sudan")
```

    ## Source: local data frame [3 x 3]
    ## 
    ##       country  year population
    ##         (chr) (int)      (int)
    ## 1 South Sudan  2011   10381110
    ## 2 South Sudan  2012   10837527
    ## 3 South Sudan  2013   11296173

``` r
population_exp_missing <- 
complete(population, country, year)

population_exp_missing %>% 
  filter(country == "South Sudan")
```

    ## Source: local data frame [19 x 3]
    ## 
    ##        country  year population
    ##          (chr) (int)      (int)
    ## 1  South Sudan  1995         NA
    ## 2  South Sudan  1996         NA
    ## 3  South Sudan  1997         NA
    ## 4  South Sudan  1998         NA
    ## 5  South Sudan  1999         NA
    ## 6  South Sudan  2000         NA
    ## 7  South Sudan  2001         NA
    ## 8  South Sudan  2002         NA
    ## 9  South Sudan  2003         NA
    ## 10 South Sudan  2004         NA
    ## 11 South Sudan  2005         NA
    ## 12 South Sudan  2006         NA
    ## 13 South Sudan  2007         NA
    ## 14 South Sudan  2008         NA
    ## 15 South Sudan  2009         NA
    ## 16 South Sudan  2010         NA
    ## 17 South Sudan  2011   10381110
    ## 18 South Sudan  2012   10837527
    ## 19 South Sudan  2013   11296173

### Challenge problem 3

1.  `drop_na()` removes rows that have missing values on any column, but it can be used in a more focused way (i.e. you can drop rows that have missing data specific columns). Using the `who` dataset (from `tidyr`) make a version of that dataset that has no missing values on the columns `new_sp_m014` and `new_sp_m1524`.

Other resources and what to learn next
--------------------------------------

-   Hadley Wickham's excellent R for Data Science book has a number of great chapters on tidy data. I used his book to help put together this lesson. The whole book is available for free [here](http://r4ds.had.co.nz).

-   Hadley Wickham's journal article on tidy data, [here](http://vita.had.co.nz/papers/tidy-data.pdf).

-   Check out the join functions that are part of `dplyr`. These help you combine data frames.

-   The `nest()` function helps you easily create data frames that have nested lists (i.e. what Jenny talked about last week); these are very powerful when used with `purrr`.

Solutions to challenge problems
-------------------------------

### Challenge problem 1

``` r
#abc)

table4b %>% gather(year, cases, -country) %>% 
  summarise(mean_cases = mean(cases))
```

    ## Source: local data frame [1 x 1]
    ## 
    ##   mean_cases
    ##        (dbl)
    ## 1  490072924

``` r
#d)
#tidy
table4b %>% gather(year, cases, -country) %>% 
  group_by(year) %>% 
  summarise(mean_cases = mean(cases))
```

    ## Source: local data frame [2 x 2]
    ## 
    ##    year mean_cases
    ##   (chr)      (dbl)
    ## 1  1999  488302902
    ## 2  2000  491842947

``` r
#non-tidy
mean(table4b$`1999`)
```

    ## [1] 488302902

``` r
mean(table4b$`2000`)
```

    ## [1] 491842947

``` r
table4b %>% summarise(mean_cases_1999 = 
                        mean(`1999`), 
                      mean_cases_2000 = 
                        mean(`2000`))
```

    ## Source: local data frame [1 x 2]
    ## 
    ##   mean_cases_1999 mean_cases_2000
    ##             (dbl)           (dbl)
    ## 1       488302902       491842947

### Challenge problem 2

``` r
table3
```

    ## Source: local data frame [6 x 3]
    ## 
    ##       country  year              rate
    ##         (chr) (int)             (chr)
    ## 1 Afghanistan  1999      745/19987071
    ## 2 Afghanistan  2000     2666/20595360
    ## 3      Brazil  1999   37737/172006362
    ## 4      Brazil  2000   80488/174504898
    ## 5       China  1999 212258/1272915272
    ## 6       China  2000 213766/1280428583

``` r
table5
```

    ## Source: local data frame [6 x 4]
    ## 
    ##       country century  year              rate
    ##         (chr)   (chr) (chr)             (chr)
    ## 1 Afghanistan      19    99      745/19987071
    ## 2 Afghanistan      20    00     2666/20595360
    ## 3      Brazil      19    99   37737/172006362
    ## 4      Brazil      20    00   80488/174504898
    ## 5       China      19    99 212258/1272915272
    ## 6       China      20    00 213766/1280428583

``` r
new_table3 <- 
table3 %>% 
  separate(year, into = c("century", "year"), sep = 2)

identical(new_table3, table5)
```

    ## [1] TRUE

### Challenge problem 3

``` r
who %>% drop_na(new_sp_m014, new_sp_m1524) 
```

    ## Source: local data frame [3,166 x 60]
    ## 
    ##        country  iso2  iso3  year new_sp_m014 new_sp_m1524 new_sp_m2534
    ##          (chr) (chr) (chr) (int)       (int)        (int)        (int)
    ## 1  Afghanistan    AF   AFG  1997           0           10            6
    ## 2  Afghanistan    AF   AFG  1998          30          129          128
    ## 3  Afghanistan    AF   AFG  1999           8           55           55
    ## 4  Afghanistan    AF   AFG  2000          52          228          183
    ## 5  Afghanistan    AF   AFG  2001         129          379          349
    ## 6  Afghanistan    AF   AFG  2002          90          476          481
    ## 7  Afghanistan    AF   AFG  2003         127          511          436
    ## 8  Afghanistan    AF   AFG  2004         139          537          568
    ## 9  Afghanistan    AF   AFG  2005         151          606          560
    ## 10 Afghanistan    AF   AFG  2006         193          837          791
    ## ..         ...   ...   ...   ...         ...          ...          ...
    ## Variables not shown: new_sp_m3544 (int), new_sp_m4554 (int), new_sp_m5564
    ##   (int), new_sp_m65 (int), new_sp_f014 (int), new_sp_f1524 (int),
    ##   new_sp_f2534 (int), new_sp_f3544 (int), new_sp_f4554 (int), new_sp_f5564
    ##   (int), new_sp_f65 (int), new_sn_m014 (int), new_sn_m1524 (int),
    ##   new_sn_m2534 (int), new_sn_m3544 (int), new_sn_m4554 (int), new_sn_m5564
    ##   (int), new_sn_m65 (int), new_sn_f014 (int), new_sn_f1524 (int),
    ##   new_sn_f2534 (int), new_sn_f3544 (int), new_sn_f4554 (int), new_sn_f5564
    ##   (int), new_sn_f65 (int), new_ep_m014 (int), new_ep_m1524 (int),
    ##   new_ep_m2534 (int), new_ep_m3544 (int), new_ep_m4554 (int), new_ep_m5564
    ##   (int), new_ep_m65 (int), new_ep_f014 (int), new_ep_f1524 (int),
    ##   new_ep_f2534 (int), new_ep_f3544 (int), new_ep_f4554 (int), new_ep_f5564
    ##   (int), new_ep_f65 (int), newrel_m014 (int), newrel_m1524 (int),
    ##   newrel_m2534 (int), newrel_m3544 (int), newrel_m4554 (int), newrel_m5564
    ##   (int), newrel_m65 (int), newrel_f014 (int), newrel_f1524 (int),
    ##   newrel_f2534 (int), newrel_f3544 (int), newrel_f4554 (int), newrel_f5564
    ##   (int), newrel_f65 (int)
