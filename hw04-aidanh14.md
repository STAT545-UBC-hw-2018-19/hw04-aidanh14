Homework 04: Tidy Data and Joins
================
Aidan Hughes
October 9, 2018

Overview
========

We were tasked to choose one prompt for reshaping data and one for joining. Here are the two that I chose:

1.  *"Make a tibble with one row per year and columns for life expectancy for two or more countries."*

2.  *"Create a second data frame, complementary to Gapminder. Join this with (part of) Gapminder using a dplyr join function and make some observations about the process and result. Explore the different types of joins."*

As always, import packages first.

``` r
suppressPackageStartupMessages(library("gapminder"))
suppressPackageStartupMessages(library("tidyverse"))
```

Reshaping: *"Make a tibble with one row per year and columns for life expectancy for two or more countries."*
=============================================================================================================

Let's make a new tibble from the Gapminder dataframe with a subset of a few random countries. We'll print the data as a table in long (or "tidy") format first.

``` r
lifeExpTidy <- gapminder %>%
  select(year, country, lifeExp) %>%
  filter(country %in% c("Argentina", "Canada", "Germany", "Japan", "Mexico", "Spain"))

# The tibble contains data for all years, but only print data from 1952 and 2002 just to keep the table short
lifeExpTidy %>%
  filter(year %in% c("1952", "2002")) %>%
  knitr::kable()
```

|  year| country   |  lifeExp|
|-----:|:----------|--------:|
|  1952| Argentina |   62.485|
|  2002| Argentina |   74.340|
|  1952| Canada    |   68.750|
|  2002| Canada    |   79.770|
|  1952| Germany   |   67.500|
|  2002| Germany   |   78.670|
|  1952| Japan     |   63.030|
|  2002| Japan     |   82.000|
|  1952| Mexico    |   50.789|
|  2002| Mexico    |   74.902|
|  1952| Spain     |   64.940|
|  2002| Spain     |   79.780|

Now we can use the `spread()` function to reshape our data into wide (or "untidy") format.

``` r
lifeExpUntidy <- lifeExpTidy %>%
  spread(key="country", value="lifeExp")
  
lifeExpUntidy %>%
  knitr::kable()
```

|  year|  Argentina|  Canada|  Germany|   Japan|  Mexico|   Spain|
|-----:|----------:|-------:|--------:|-------:|-------:|-------:|
|  1952|     62.485|  68.750|   67.500|  63.030|  50.789|  64.940|
|  1957|     64.399|  69.960|   69.100|  65.500|  55.190|  66.660|
|  1962|     65.142|  71.300|   70.300|  68.730|  58.299|  69.690|
|  1967|     65.634|  72.130|   70.800|  71.430|  60.110|  71.440|
|  1972|     67.065|  72.880|   71.000|  73.420|  62.361|  73.060|
|  1977|     68.481|  74.210|   72.500|  75.380|  65.032|  74.390|
|  1982|     69.942|  75.760|   73.800|  77.110|  67.405|  76.300|
|  1987|     70.774|  76.860|   74.847|  78.670|  69.498|  76.900|
|  1992|     71.868|  77.950|   76.070|  79.360|  71.455|  77.570|
|  1997|     73.275|  78.610|   77.340|  80.690|  73.670|  78.770|
|  2002|     74.340|  79.770|   78.670|  82.000|  74.902|  79.780|
|  2007|     75.320|  80.653|   79.406|  82.603|  76.195|  80.941|

Using the `spread()` function makes tables more human-friendly for reading.

Now we can compare the life expectancies over time of a couple different countries with a scatterplot.

``` r
lifeExpUntidy %>%
  ggplot(aes(x = year)) +
  geom_point(aes(y = Canada, color = "Canada")) +
  geom_point(aes(y = Mexico, color = "Mexico")) +
  labs(x = "Year", y = "Life Expectancy", color="Countries")
```

![](hw04-aidanh14_files/figure-markdown_github/lifeExp%20scatterplot-1.png)

Task 2: *"Create a second data frame, complementary to Gapminder. Join this with (part of) Gapminder using a dplyr join function and make some observations about the process and result. Explore the different types of joins."*
=================================================================================================================================================================================================================================

For the sake of convenience, let's reuse the smaller tibble of random countries that we made in the last task. We can create a second data frame containing the capital city of each country.
