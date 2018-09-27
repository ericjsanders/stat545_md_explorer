cm008 Exercises
================

``` r
library(tidyverse)
library(gapminder)
```

`mutate()`
----------

Let's get:

-   GDP by multiplying GPD per capita with population, and
-   GDP in billions, named (`gdpBill`), rounded to two decimals.

``` r
gapminder %>%
  mutate(gdp = gdpPercap * pop,
         gdpBill = round(gdp/10^9,2))
```

    ## # A tibble: 1,704 x 8
    ##    country     continent  year lifeExp      pop gdpPercap      gdp gdpBill
    ##    <fct>       <fct>     <int>   <dbl>    <int>     <dbl>    <dbl>   <dbl>
    ##  1 Afghanistan Asia       1952    28.8  8425333      779.  6.57e 9    6.57
    ##  2 Afghanistan Asia       1957    30.3  9240934      821.  7.59e 9    7.59
    ##  3 Afghanistan Asia       1962    32.0 10267083      853.  8.76e 9    8.76
    ##  4 Afghanistan Asia       1967    34.0 11537966      836.  9.65e 9    9.65
    ##  5 Afghanistan Asia       1972    36.1 13079460      740.  9.68e 9    9.68
    ##  6 Afghanistan Asia       1977    38.4 14880372      786.  1.17e10   11.7 
    ##  7 Afghanistan Asia       1982    39.9 12881816      978.  1.26e10   12.6 
    ##  8 Afghanistan Asia       1987    40.8 13867957      852.  1.18e10   11.8 
    ##  9 Afghanistan Asia       1992    41.7 16317921      649.  1.06e10   10.6 
    ## 10 Afghanistan Asia       1997    41.8 22227415      635.  1.41e10   14.1 
    ## # ... with 1,694 more rows

Notice the backwards compatibility! No need for loops!

Try the same thing, but with `transmute` (drops all other variables).

``` r
gapminder %>%
  transmute(gdp = gdpPercap * pop,
         gdpBill = round(gdp/10^9,2))
```

    ## # A tibble: 1,704 x 2
    ##             gdp gdpBill
    ##           <dbl>   <dbl>
    ##  1  6567086330.    6.57
    ##  2  7585448670.    7.59
    ##  3  8758855797.    8.76
    ##  4  9648014150.    9.65
    ##  5  9678553274.    9.68
    ##  6 11697659231.   11.7 
    ##  7 12598563401.   12.6 
    ##  8 11820990309.   11.8 
    ##  9 10595901589.   10.6 
    ## 10 14121995875.   14.1 
    ## # ... with 1,694 more rows

The `if_else` function is useful for changing certain elements in a data frame.

Example: Suppose Canada's 1952 life expectancy was mistakenly entered as 68.8 in the data frame, but is actually 70. Fix it using `if_else` and `mutate`.

``` r
mutate(gapminder,lifeExp = if_else(country=="Canada" & year==1952,70,lifeExp)) %>%
  filter(country=="Canada")
```

    ## # A tibble: 12 x 6
    ##    country continent  year lifeExp      pop gdpPercap
    ##    <fct>   <fct>     <int>   <dbl>    <int>     <dbl>
    ##  1 Canada  Americas   1952    70   14785584    11367.
    ##  2 Canada  Americas   1957    70.0 17010154    12490.
    ##  3 Canada  Americas   1962    71.3 18985849    13462.
    ##  4 Canada  Americas   1967    72.1 20819767    16077.
    ##  5 Canada  Americas   1972    72.9 22284500    18971.
    ##  6 Canada  Americas   1977    74.2 23796400    22091.
    ##  7 Canada  Americas   1982    75.8 25201900    22899.
    ##  8 Canada  Americas   1987    76.9 26549700    26627.
    ##  9 Canada  Americas   1992    78.0 28523502    26343.
    ## 10 Canada  Americas   1997    78.6 30305843    28955.
    ## 11 Canada  Americas   2002    79.8 31902268    33329.
    ## 12 Canada  Americas   2007    80.7 33390141    36319.

Your turn: Make a new column called `cc` that pastes the country name followed by the continent, separated by a comma. (Hint: use the `paste` function with the `sep=", "` argument).

``` r
mutate(gapminder,cc=paste(country,continent,sep=', '))
```

    ## # A tibble: 1,704 x 7
    ##    country     continent  year lifeExp      pop gdpPercap cc              
    ##    <fct>       <fct>     <int>   <dbl>    <int>     <dbl> <chr>           
    ##  1 Afghanistan Asia       1952    28.8  8425333      779. Afghanistan, As…
    ##  2 Afghanistan Asia       1957    30.3  9240934      821. Afghanistan, As…
    ##  3 Afghanistan Asia       1962    32.0 10267083      853. Afghanistan, As…
    ##  4 Afghanistan Asia       1967    34.0 11537966      836. Afghanistan, As…
    ##  5 Afghanistan Asia       1972    36.1 13079460      740. Afghanistan, As…
    ##  6 Afghanistan Asia       1977    38.4 14880372      786. Afghanistan, As…
    ##  7 Afghanistan Asia       1982    39.9 12881816      978. Afghanistan, As…
    ##  8 Afghanistan Asia       1987    40.8 13867957      852. Afghanistan, As…
    ##  9 Afghanistan Asia       1992    41.7 16317921      649. Afghanistan, As…
    ## 10 Afghanistan Asia       1997    41.8 22227415      635. Afghanistan, As…
    ## # ... with 1,694 more rows

These functions we've seen are called **vectorized functions**.

`summarize()` and `group_by()`
------------------------------

Use `summarize()` to compute the mean and median life expectancy using all entries:

``` r
summarize(gapminder,median(lifeExp),mean(lifeExp))
```

    ## # A tibble: 1 x 2
    ##   `median(lifeExp)` `mean(lifeExp)`
    ##               <dbl>           <dbl>
    ## 1              60.7            59.5

Do the same thing, but try:

1.  grouping by country
2.  grouping by continent and country

``` r
summarize(group_by(gapminder,country),median(lifeExp),mean(lifeExp))
```

    ## # A tibble: 142 x 3
    ##    country     `median(lifeExp)` `mean(lifeExp)`
    ##    <fct>                   <dbl>           <dbl>
    ##  1 Afghanistan              39.1            37.5
    ##  2 Albania                  69.7            68.4
    ##  3 Algeria                  59.7            59.0
    ##  4 Angola                   39.7            37.9
    ##  5 Argentina                69.2            69.1
    ##  6 Australia                74.1            74.7
    ##  7 Austria                  72.7            73.1
    ##  8 Bahrain                  67.3            65.6
    ##  9 Bangladesh               48.5            49.8
    ## 10 Belgium                  73.4            73.6
    ## # ... with 132 more rows

``` r
summarize(group_by(gapminder,continent,country),median(lifeExp),mean(lifeExp))
```

    ## # A tibble: 142 x 4
    ## # Groups:   continent [?]
    ##    continent country                  `median(lifeExp)` `mean(lifeExp)`
    ##    <fct>     <fct>                                <dbl>           <dbl>
    ##  1 Africa    Algeria                               59.7            59.0
    ##  2 Africa    Angola                                39.7            37.9
    ##  3 Africa    Benin                                 50.0            48.8
    ##  4 Africa    Botswana                              52.9            54.6
    ##  5 Africa    Burkina Faso                          47.1            44.7
    ##  6 Africa    Burundi                               45.0            44.8
    ##  7 Africa    Cameroon                              49.6            48.1
    ##  8 Africa    Central African Republic              44.1            43.9
    ##  9 Africa    Chad                                  48.4            46.8
    ## 10 Africa    Comoros                               51.9            52.4
    ## # ... with 132 more rows

-   Notice the columns that are kept.
-   Notice the grouping listed above the tibble, especially without a call after grouping.
-   Notice the peeling of groups for each summarize.

Question: What if I wanted to keep the other numeric columns (gdpPercap, pop)? Can I? Would this even make sense?

For each continent: What is the smallest country-wide median GDP per capita?

``` r
group_by(gapminder,continent,country) %>%
  summarize(md = median(gdpPercap)) %>%
  summarize(min(md))
```

    ## # A tibble: 5 x 2
    ##   continent `min(md)`
    ##   <fct>         <dbl>
    ## 1 Africa         455.
    ## 2 Americas      1691.
    ## 3 Asia           378 
    ## 4 Europe        3194.
    ## 5 Oceania      16933.

Note that ggplot2's grouping is different from dplyr's! Try making a spaghetti plot of lifeExp over time for each coutry, by piping in a grouped data frame -- it won't work:

``` r
gapminder %>%
  group_by(country) %>%
  ggplot(aes(year,lifeExp)) +
  geom_line()
```

![](cm008-exercise_files/figure-markdown_github/unnamed-chunk-9-1.png)

Your turn! For each continent, what is the median GDP per capita of countries with high (&gt;60) life expectancy vs countries with low (&lt;=60)? Sort this data frame by median GDP per capita.

``` r
mutate(gapminder,hvsl=ifelse(lifeExp>60,'high','low')) %>%
  group_by(continent,hvsl) %>%
  summarize(md=median(gdpPercap)) %>%
  arrange(md)
```

    ## # A tibble: 9 x 3
    ## # Groups:   continent [5]
    ##   continent hvsl      md
    ##   <fct>     <chr>  <dbl>
    ## 1 Asia      low    1031.
    ## 2 Africa    low    1071.
    ## 3 Europe    low    2384.
    ## 4 Americas  low    3080.
    ## 5 Africa    high   4442.
    ## 6 Asia      high   5250.
    ## 7 Americas  high   6678.
    ## 8 Europe    high  12672.
    ## 9 Oceania   high  17983.

There are special functions to summarize by. Let's see some of them:

-   `n()`: Number of rows in the group.
-   `n_distinct()`

``` r
gapminder %>%
  group_by(continent) %>%
  tally()
```

    ## # A tibble: 5 x 2
    ##   continent     n
    ##   <fct>     <int>
    ## 1 Africa      624
    ## 2 Americas    300
    ## 3 Asia        396
    ## 4 Europe      360
    ## 5 Oceania      24

``` r
count(gapminder,continent)
```

    ## # A tibble: 5 x 2
    ##   continent     n
    ##   <fct>     <int>
    ## 1 Africa      624
    ## 2 Americas    300
    ## 3 Asia        396
    ## 4 Europe      360
    ## 5 Oceania      24

Convenience functions:

-   `tally()` (= `summarize(n = n())`)
-   `count(...)` (= `group_by(...) %>% tally()`)

n\_distinct: How many years of record does each country have?

count

Function types
--------------

Let's stop to identify some theory of function types, and the `dplyr` approach to them.

<table style="width:32%;">
<colgroup>
<col width="9%" />
<col width="8%" />
<col width="6%" />
<col width="6%" />
</colgroup>
<thead>
<tr class="header">
<th>Function type</th>
<th>Explanation</th>
<th>Examples</th>
<th>In <code>dplyr</code></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Vectorized functions</td>
<td>These take a vector, and operate on each component independently to return a vector of the same length. In other words, they work element-wise.</td>
<td><code>cos</code>, <code>sin</code>, <code>log</code>, <code>exp</code>, <code>round</code></td>
<td><code>mutate</code></td>
</tr>
<tr class="even">
<td>Aggregate functions</td>
<td>These take a vector, and return a vector of length 1</td>
<td><code>mean</code>, <code>sd</code>, <code>length</code>, <code>typeof</code></td>
<td><code>summarize</code>, esp with <code>group_by</code>.</td>
</tr>
<tr class="odd">
<td>Window Functions</td>
<td>these take a vector, and return a vector of the same length that depends on the vector as a whole.</td>
<td><code>lag</code>, <code>rank</code>, <code>cumsum</code></td>
<td><code>mutate</code>, esp with <code>group_by</code></td>
</tr>
</tbody>
</table>

For any generic output, we can use dplyr's `do()` function -- but that's a topic for STAT 547.

Grouped `mutate()`
------------------

Calculate the growth in population since the first year on record *for each country*. `first()` is useful.

``` r
gapminder %>%
  group_by(country) %>%
  mutate(growth = pop-first(pop))
```

    ## # A tibble: 1,704 x 7
    ## # Groups:   country [142]
    ##    country     continent  year lifeExp      pop gdpPercap   growth
    ##    <fct>       <fct>     <int>   <dbl>    <int>     <dbl>    <int>
    ##  1 Afghanistan Asia       1952    28.8  8425333      779.        0
    ##  2 Afghanistan Asia       1957    30.3  9240934      821.   815601
    ##  3 Afghanistan Asia       1962    32.0 10267083      853.  1841750
    ##  4 Afghanistan Asia       1967    34.0 11537966      836.  3112633
    ##  5 Afghanistan Asia       1972    36.1 13079460      740.  4654127
    ##  6 Afghanistan Asia       1977    38.4 14880372      786.  6455039
    ##  7 Afghanistan Asia       1982    39.9 12881816      978.  4456483
    ##  8 Afghanistan Asia       1987    40.8 13867957      852.  5442624
    ##  9 Afghanistan Asia       1992    41.7 16317921      649.  7892588
    ## 10 Afghanistan Asia       1997    41.8 22227415      635. 13802082
    ## # ... with 1,694 more rows

``` r
gapminder %>%
  group_by(country) %>%
  mutate(growth = pop-lag(pop))
```

    ## # A tibble: 1,704 x 7
    ## # Groups:   country [142]
    ##    country     continent  year lifeExp      pop gdpPercap   growth
    ##    <fct>       <fct>     <int>   <dbl>    <int>     <dbl>    <int>
    ##  1 Afghanistan Asia       1952    28.8  8425333      779.       NA
    ##  2 Afghanistan Asia       1957    30.3  9240934      821.   815601
    ##  3 Afghanistan Asia       1962    32.0 10267083      853.  1026149
    ##  4 Afghanistan Asia       1967    34.0 11537966      836.  1270883
    ##  5 Afghanistan Asia       1972    36.1 13079460      740.  1541494
    ##  6 Afghanistan Asia       1977    38.4 14880372      786.  1800912
    ##  7 Afghanistan Asia       1982    39.9 12881816      978. -1998556
    ##  8 Afghanistan Asia       1987    40.8 13867957      852.   986141
    ##  9 Afghanistan Asia       1992    41.7 16317921      649.  2449964
    ## 10 Afghanistan Asia       1997    41.8 22227415      635.  5909494
    ## # ... with 1,694 more rows

Notice that `dplyr` has retained the original grouping.

How about growth compared to `1972`?

``` r
gapminder %>%
  group_by(country) %>%
  mutate(growth = pop-pop[year==1972])
```

    ## # A tibble: 1,704 x 7
    ## # Groups:   country [142]
    ##    country     continent  year lifeExp      pop gdpPercap   growth
    ##    <fct>       <fct>     <int>   <dbl>    <int>     <dbl>    <int>
    ##  1 Afghanistan Asia       1952    28.8  8425333      779. -4654127
    ##  2 Afghanistan Asia       1957    30.3  9240934      821. -3838526
    ##  3 Afghanistan Asia       1962    32.0 10267083      853. -2812377
    ##  4 Afghanistan Asia       1967    34.0 11537966      836. -1541494
    ##  5 Afghanistan Asia       1972    36.1 13079460      740.        0
    ##  6 Afghanistan Asia       1977    38.4 14880372      786.  1800912
    ##  7 Afghanistan Asia       1982    39.9 12881816      978.  -197644
    ##  8 Afghanistan Asia       1987    40.8 13867957      852.   788497
    ##  9 Afghanistan Asia       1992    41.7 16317921      649.  3238461
    ## 10 Afghanistan Asia       1997    41.8 22227415      635.  9147955
    ## # ... with 1,694 more rows

Make a new variable `pop_last_time`, as the "lag-1" population -- that is, the population from the previous entry of that country. Use the `lag` function.

Similar: `lead` function.

Notice the NA's.

Putting it all together
-----------------------

Your turn: Use what we learned to answer the following questions.

1.  Determine the country that experienced the sharpest 5-year drop in life expectancy, in each continent.

``` r
group_by(gapminder,continent,country) %>%
  mutate(lExpChange = lifeExp - lag(lifeExp)) %>%
  filter(!is.na(lExpChange)) %>%
  summarize(min=min(lExpChange)) %>%
  summarize(min=min(min)) %>%
  arrange(min)
```

    ## # A tibble: 5 x 2
    ##   continent     min
    ##   <fct>       <dbl>
    ## 1 Africa    -20.4  
    ## 2 Asia       -9.10 
    ## 3 Americas   -1.51 
    ## 4 Europe     -1.46 
    ## 5 Oceania     0.170

1.  Compute the relative gdp (NOT per capita!) of each country compared to Canada (= GDP of a country / GDP of Canada).

``` r
can.gdp = filter(gapminder,country=='Canada') %>%
  mutate(gdp = gdpPercap * pop) %>%
  `[[`('gdp')

my_df = gapminder %>%
  mutate(gdp = gdpPercap * pop) %>%
  mutate(can.gdp = rep(can.gdp,nlevels(country))) %>%
  mutate(rel.gdp = gdp/can.gdp)

my_df %>% filter(country=='Canada')
```

    ## # A tibble: 12 x 9
    ##    country continent  year lifeExp      pop gdpPercap     gdp can.gdp
    ##    <fct>   <fct>     <int>   <dbl>    <int>     <dbl>   <dbl>   <dbl>
    ##  1 Canada  Americas   1952    68.8 14785584    11367. 1.68e11 1.68e11
    ##  2 Canada  Americas   1957    70.0 17010154    12490. 2.12e11 2.12e11
    ##  3 Canada  Americas   1962    71.3 18985849    13462. 2.56e11 2.56e11
    ##  4 Canada  Americas   1967    72.1 20819767    16077. 3.35e11 3.35e11
    ##  5 Canada  Americas   1972    72.9 22284500    18971. 4.23e11 4.23e11
    ##  6 Canada  Americas   1977    74.2 23796400    22091. 5.26e11 5.26e11
    ##  7 Canada  Americas   1982    75.8 25201900    22899. 5.77e11 5.77e11
    ##  8 Canada  Americas   1987    76.9 26549700    26627. 7.07e11 7.07e11
    ##  9 Canada  Americas   1992    78.0 28523502    26343. 7.51e11 7.51e11
    ## 10 Canada  Americas   1997    78.6 30305843    28955. 8.78e11 8.78e11
    ## 11 Canada  Americas   2002    79.8 31902268    33329. 1.06e12 1.06e12
    ## 12 Canada  Americas   2007    80.7 33390141    36319. 1.21e12 1.21e12
    ## # ... with 1 more variable: rel.gdp <dbl>

``` r
summary(my_df$rel.gdp)
```

    ##      Min.   1st Qu.    Median      Mean   3rd Qu.      Max. 
    ##  0.000217  0.011948  0.041706  0.309287  0.197899 13.115017

Sanity check: are Canada's numbers = 1? What is the spread of numbers like (should be small)?

Summary of major one-table functions
------------------------------------

-   `select()`
-   `filter()`
-   `arrange()`
-   `mutate()`
-   `summarize()`

Together with `group_by()` and "twists" of the above.

Practice Exercises
------------------

Practice these concepts in the following exercises. It might help you to first identify the type of function you are applying.

1.  Convert the population to a number in billions.

2.  Compute the change in population from 1962 to 1972 for each country.

3.  Rank the continents by GDP per capita. You should have two columns: one with continent, and another with the ranking (1 through 5). **Hint**: use the `rank()` or `min_rank()` function.
