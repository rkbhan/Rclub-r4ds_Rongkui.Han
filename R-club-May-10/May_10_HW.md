# May_10_HW
Rongkui Han  
May 10, 2017  
####5.1.1 Prerequisites  

```r
library(nycflights13)
library(tidyverse)
```

```
## Loading tidyverse: ggplot2
## Loading tidyverse: tibble
## Loading tidyverse: tidyr
## Loading tidyverse: readr
## Loading tidyverse: purrr
## Loading tidyverse: dplyr
```

```
## Conflicts with tidy packages ----------------------------------------------
```

```
## filter(): dplyr, stats
## lag():    dplyr, stats
```

####5.1.2  

```r
flights
```

```
## # A tibble: 336,776 × 19
##     year month   day dep_time sched_dep_time dep_delay arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
## 1   2013     1     1      517            515         2      830
## 2   2013     1     1      533            529         4      850
## 3   2013     1     1      542            540         2      923
## 4   2013     1     1      544            545        -1     1004
## 5   2013     1     1      554            600        -6      812
## 6   2013     1     1      554            558        -4      740
## 7   2013     1     1      555            600        -5      913
## 8   2013     1     1      557            600        -3      709
## 9   2013     1     1      557            600        -3      838
## 10  2013     1     1      558            600        -2      753
## # ... with 336,766 more rows, and 12 more variables: sched_arr_time <int>,
## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
## #   minute <dbl>, time_hour <dttm>
```

####5.2 Filter()

```r
filter(flights, month == 1, day == 1)
```

```
## # A tibble: 842 × 19
##     year month   day dep_time sched_dep_time dep_delay arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
## 1   2013     1     1      517            515         2      830
## 2   2013     1     1      533            529         4      850
## 3   2013     1     1      542            540         2      923
## 4   2013     1     1      544            545        -1     1004
## 5   2013     1     1      554            600        -6      812
## 6   2013     1     1      554            558        -4      740
## 7   2013     1     1      555            600        -5      913
## 8   2013     1     1      557            600        -3      709
## 9   2013     1     1      557            600        -3      838
## 10  2013     1     1      558            600        -2      753
## # ... with 832 more rows, and 12 more variables: sched_arr_time <int>,
## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
## #   minute <dbl>, time_hour <dttm>
```

```r
jan1 = filter(flights, month ==1, day ==1)
dim(jan1)
```

```
## [1] 842  19
```

```r
#I'm just testing if this is the same as subsetting with basic R operations. It looks like they are the same. 
jan1.1 = flights[flights$month == 1 & flights$day ==1,] 
dim(jan1.1)
```

```
## [1] 842  19
```

```r
#putting statement in parentheses makes R print the variable. 
(xmas = filter(flights, month == 12, day == 25))
```

```
## # A tibble: 719 × 19
##     year month   day dep_time sched_dep_time dep_delay arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
## 1   2013    12    25      456            500        -4      649
## 2   2013    12    25      524            515         9      805
## 3   2013    12    25      542            540         2      832
## 4   2013    12    25      546            550        -4     1022
## 5   2013    12    25      556            600        -4      730
## 6   2013    12    25      557            600        -3      743
## 7   2013    12    25      557            600        -3      818
## 8   2013    12    25      559            600        -1      855
## 9   2013    12    25      559            600        -1      849
## 10  2013    12    25      600            600         0      850
## # ... with 709 more rows, and 12 more variables: sched_arr_time <int>,
## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
## #   minute <dbl>, time_hour <dttm>
```

####5.2.1 Comparison  

```r
sqrt(2) ^ 2 == 2
```

```
## [1] FALSE
```

```r
1/49 * 49 == 1
```

```
## [1] FALSE
```

```r
near(sqrt(2) ^ 2 , 2)
```

```
## [1] TRUE
```

```r
near(1/49 * 49 , 1)
```

```
## [1] TRUE
```

####5.2.2 Logical operators  

```r
filter(flights, month == 11 | month == 12)
```

```
## # A tibble: 55,403 × 19
##     year month   day dep_time sched_dep_time dep_delay arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
## 1   2013    11     1        5           2359         6      352
## 2   2013    11     1       35           2250       105      123
## 3   2013    11     1      455            500        -5      641
## 4   2013    11     1      539            545        -6      856
## 5   2013    11     1      542            545        -3      831
## 6   2013    11     1      549            600       -11      912
## 7   2013    11     1      550            600       -10      705
## 8   2013    11     1      554            600        -6      659
## 9   2013    11     1      554            600        -6      826
## 10  2013    11     1      554            600        -6      749
## # ... with 55,393 more rows, and 12 more variables: sched_arr_time <int>,
## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
## #   minute <dbl>, time_hour <dttm>
```

####5.2.4 Exercise  
#####1. Find all flights that   
######1.1 Had an arrival delay of two or more hours  

```r
filter(flights, arr_delay >2)
```

```
## # A tibble: 123,096 × 19
##     year month   day dep_time sched_dep_time dep_delay arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
## 1   2013     1     1      517            515         2      830
## 2   2013     1     1      533            529         4      850
## 3   2013     1     1      542            540         2      923
## 4   2013     1     1      554            558        -4      740
## 5   2013     1     1      555            600        -5      913
## 6   2013     1     1      558            600        -2      753
## 7   2013     1     1      558            600        -2      924
## 8   2013     1     1      559            600        -1      941
## 9   2013     1     1      600            600         0      837
## 10  2013     1     1      602            605        -3      821
## # ... with 123,086 more rows, and 12 more variables: sched_arr_time <int>,
## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
## #   minute <dbl>, time_hour <dttm>
```

######1.2 Flew to Houston (IAH or HOU)  

```r
filter(flights, dest == "IAH" | dest =="HOU")
```

```
## # A tibble: 9,313 × 19
##     year month   day dep_time sched_dep_time dep_delay arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
## 1   2013     1     1      517            515         2      830
## 2   2013     1     1      533            529         4      850
## 3   2013     1     1      623            627        -4      933
## 4   2013     1     1      728            732        -4     1041
## 5   2013     1     1      739            739         0     1104
## 6   2013     1     1      908            908         0     1228
## 7   2013     1     1     1028           1026         2     1350
## 8   2013     1     1     1044           1045        -1     1352
## 9   2013     1     1     1114            900       134     1447
## 10  2013     1     1     1205           1200         5     1503
## # ... with 9,303 more rows, and 12 more variables: sched_arr_time <int>,
## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
## #   minute <dbl>, time_hour <dttm>
```

######1.3 Were operated by United, American, or Delta  

```r
levels(as.factor(flights$carrier))
```

```
##  [1] "9E" "AA" "AS" "B6" "DL" "EV" "F9" "FL" "HA" "MQ" "OO" "UA" "US" "VX"
## [15] "WN" "YV"
```

```r
filter(flights, carrier == "UA" | carrier == "AA" | carrier == "DL")
```

```
## # A tibble: 139,504 × 19
##     year month   day dep_time sched_dep_time dep_delay arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
## 1   2013     1     1      517            515         2      830
## 2   2013     1     1      533            529         4      850
## 3   2013     1     1      542            540         2      923
## 4   2013     1     1      554            600        -6      812
## 5   2013     1     1      554            558        -4      740
## 6   2013     1     1      558            600        -2      753
## 7   2013     1     1      558            600        -2      924
## 8   2013     1     1      558            600        -2      923
## 9   2013     1     1      559            600        -1      941
## 10  2013     1     1      559            600        -1      854
## # ... with 139,494 more rows, and 12 more variables: sched_arr_time <int>,
## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
## #   minute <dbl>, time_hour <dttm>
```

######1.4 Departed in summer (July, August, and September)  

```r
filter(flights, month == 7 | month == 8 | month == 9)
```

```
## # A tibble: 86,326 × 19
##     year month   day dep_time sched_dep_time dep_delay arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
## 1   2013     7     1        1           2029       212      236
## 2   2013     7     1        2           2359         3      344
## 3   2013     7     1       29           2245       104      151
## 4   2013     7     1       43           2130       193      322
## 5   2013     7     1       44           2150       174      300
## 6   2013     7     1       46           2051       235      304
## 7   2013     7     1       48           2001       287      308
## 8   2013     7     1       58           2155       183      335
## 9   2013     7     1      100           2146       194      327
## 10  2013     7     1      100           2245       135      337
## # ... with 86,316 more rows, and 12 more variables: sched_arr_time <int>,
## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
## #   minute <dbl>, time_hour <dttm>
```

######1.5 Arrived more than two hours late, but didn’t leave late  

```r
filter(flights, arr_delay >2 & dep_delay <= 0)
```

```
## # A tibble: 34,583 × 19
##     year month   day dep_time sched_dep_time dep_delay arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
## 1   2013     1     1      554            558        -4      740
## 2   2013     1     1      555            600        -5      913
## 3   2013     1     1      558            600        -2      753
## 4   2013     1     1      558            600        -2      924
## 5   2013     1     1      559            600        -1      941
## 6   2013     1     1      600            600         0      837
## 7   2013     1     1      602            605        -3      821
## 8   2013     1     1      622            630        -8     1017
## 9   2013     1     1      624            630        -6      909
## 10  2013     1     1      624            630        -6      840
## # ... with 34,573 more rows, and 12 more variables: sched_arr_time <int>,
## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
## #   minute <dbl>, time_hour <dttm>
```

######1.6 Were delayed by at least an hour, but made up over 30 minutes in flight   

```r
filter(flights, dep_delay >= 1 & dep_delay-arr_delay > 0.5)
```

```
## # A tibble: 83,728 × 19
##     year month   day dep_time sched_dep_time dep_delay arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
## 1   2013     1     1      601            600         1      844
## 2   2013     1     1      623            610        13      920
## 3   2013     1     1      632            608        24      740
## 4   2013     1     1      644            636         8      931
## 5   2013     1     1      646            645         1      910
## 6   2013     1     1      646            645         1     1023
## 7   2013     1     1      701            700         1     1123
## 8   2013     1     1      732            729         3     1041
## 9   2013     1     1      732            645        47     1011
## 10  2013     1     1      743            730        13     1107
## # ... with 83,718 more rows, and 12 more variables: sched_arr_time <int>,
## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
## #   minute <dbl>, time_hour <dttm>
```

######1.7 Departed between midnight and 6am (inclusive)   

```r
filter(flights, dep_time <= 600 & dep_time >= 0 | dep_time == 2400)
```

```
## # A tibble: 9,373 × 19
##     year month   day dep_time sched_dep_time dep_delay arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
## 1   2013     1     1      517            515         2      830
## 2   2013     1     1      533            529         4      850
## 3   2013     1     1      542            540         2      923
## 4   2013     1     1      544            545        -1     1004
## 5   2013     1     1      554            600        -6      812
## 6   2013     1     1      554            558        -4      740
## 7   2013     1     1      555            600        -5      913
## 8   2013     1     1      557            600        -3      709
## 9   2013     1     1      557            600        -3      838
## 10  2013     1     1      558            600        -2      753
## # ... with 9,363 more rows, and 12 more variables: sched_arr_time <int>,
## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
## #   minute <dbl>, time_hour <dttm>
```

#####2.  

```r
flights[between(flights$dep_time, 0, 600),]
```

```
## # A tibble: 17,599 × 19
##     year month   day dep_time sched_dep_time dep_delay arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
## 1   2013     1     1      517            515         2      830
## 2   2013     1     1      533            529         4      850
## 3   2013     1     1      542            540         2      923
## 4   2013     1     1      544            545        -1     1004
## 5   2013     1     1      554            600        -6      812
## 6   2013     1     1      554            558        -4      740
## 7   2013     1     1      555            600        -5      913
## 8   2013     1     1      557            600        -3      709
## 9   2013     1     1      557            600        -3      838
## 10  2013     1     1      558            600        -2      753
## # ... with 17,589 more rows, and 12 more variables: sched_arr_time <int>,
## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
## #   minute <dbl>, time_hour <dttm>
```

```r
#NA's are not removed using this function
```

#####3.

```r
table(is.na(flights$dep_time))
```

```
## 
##  FALSE   TRUE 
## 328521   8255
```

```r
naorno = is.na(flights)
tablena = apply(naorno, 2, function(x) table(x))
tablena
```

```
## $year
## x
##  FALSE 
## 336776 
## 
## $month
## x
##  FALSE 
## 336776 
## 
## $day
## x
##  FALSE 
## 336776 
## 
## $dep_time
## x
##  FALSE   TRUE 
## 328521   8255 
## 
## $sched_dep_time
## x
##  FALSE 
## 336776 
## 
## $dep_delay
## x
##  FALSE   TRUE 
## 328521   8255 
## 
## $arr_time
## x
##  FALSE   TRUE 
## 328063   8713 
## 
## $sched_arr_time
## x
##  FALSE 
## 336776 
## 
## $arr_delay
## x
##  FALSE   TRUE 
## 327346   9430 
## 
## $carrier
## x
##  FALSE 
## 336776 
## 
## $flight
## x
##  FALSE 
## 336776 
## 
## $tailnum
## x
##  FALSE   TRUE 
## 334264   2512 
## 
## $origin
## x
##  FALSE 
## 336776 
## 
## $dest
## x
##  FALSE 
## 336776 
## 
## $air_time
## x
##  FALSE   TRUE 
## 327346   9430 
## 
## $distance
## x
##  FALSE 
## 336776 
## 
## $hour
## x
##  FALSE 
## 336776 
## 
## $minute
## x
##  FALSE 
## 336776 
## 
## $time_hour
## x
##  FALSE 
## 336776
```
8255 flights have missing departure time. 
It looks light arrival time (and the correpsonding departure delay, arrival delay and air time) and tail numbers have missing values.   

#####4. 

```r
NA^0 # not missing, because the 0th power of anything is 1. 
```

```
## [1] 1
```

```r
filter(flights, dep_time == NA | TRUE) # not missing because the "TRUE" option has content
```

```
## # A tibble: 336,776 × 19
##     year month   day dep_time sched_dep_time dep_delay arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
## 1   2013     1     1      517            515         2      830
## 2   2013     1     1      533            529         4      850
## 3   2013     1     1      542            540         2      923
## 4   2013     1     1      544            545        -1     1004
## 5   2013     1     1      554            600        -6      812
## 6   2013     1     1      554            558        -4      740
## 7   2013     1     1      555            600        -5      913
## 8   2013     1     1      557            600        -3      709
## 9   2013     1     1      557            600        -3      838
## 10  2013     1     1      558            600        -2      753
## # ... with 336,766 more rows, and 12 more variables: sched_arr_time <int>,
## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
## #   minute <dbl>, time_hour <dttm>
```

```r
filter(flights, FALSE & NA) # missing because there is no intersection between "unknown" and "false".
```

```
## # A tibble: 0 × 19
## # ... with 19 variables: year <int>, month <int>, day <int>,
## #   dep_time <int>, sched_dep_time <int>, dep_delay <dbl>, arr_time <int>,
## #   sched_arr_time <int>, arr_delay <dbl>, carrier <chr>, flight <int>,
## #   tailnum <chr>, origin <chr>, dest <chr>, air_time <dbl>,
## #   distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>
```

```r
NA * 0 # i don't know why this is missing...
```

```
## [1] NA
```

####5.3 Arrange()  

```r
arrange(flights, year, month, day)
```

```
## # A tibble: 336,776 × 19
##     year month   day dep_time sched_dep_time dep_delay arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
## 1   2013     1     1      517            515         2      830
## 2   2013     1     1      533            529         4      850
## 3   2013     1     1      542            540         2      923
## 4   2013     1     1      544            545        -1     1004
## 5   2013     1     1      554            600        -6      812
## 6   2013     1     1      554            558        -4      740
## 7   2013     1     1      555            600        -5      913
## 8   2013     1     1      557            600        -3      709
## 9   2013     1     1      557            600        -3      838
## 10  2013     1     1      558            600        -2      753
## # ... with 336,766 more rows, and 12 more variables: sched_arr_time <int>,
## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
## #   minute <dbl>, time_hour <dttm>
```

```r
arrange(flights, desc(arr_time))
```

```
## # A tibble: 336,776 × 19
##     year month   day dep_time sched_dep_time dep_delay arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
## 1   2013     1     1     2209           2155        14     2400
## 2   2013     1     5     2116           2130       -14     2400
## 3   2013     1    13     2243           2129        74     2400
## 4   2013     1    16     2138           2107        31     2400
## 5   2013     1    17     2256           2249         7     2400
## 6   2013     1    22     2212           2055        77     2400
## 7   2013     1    22     2249           2125        84     2400
## 8   2013     1    25     2055           1725       210     2400
## 9   2013     1    28     2303           2250        13     2400
## 10  2013     1    30     2155           1915       160     2400
## # ... with 336,766 more rows, and 12 more variables: sched_arr_time <int>,
## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
## #   minute <dbl>, time_hour <dttm>
```

####5.3.1 Exercise  
#####1.   

```r
arrange(flights, !is.na(arr_time))
```

```
## # A tibble: 336,776 × 19
##     year month   day dep_time sched_dep_time dep_delay arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
## 1   2013     1     1     2016           1930        46       NA
## 2   2013     1     1       NA           1630        NA       NA
## 3   2013     1     1       NA           1935        NA       NA
## 4   2013     1     1       NA           1500        NA       NA
## 5   2013     1     1       NA            600        NA       NA
## 6   2013     1     2     2041           2045        -4       NA
## 7   2013     1     2     2145           2129        16       NA
## 8   2013     1     2       NA           1540        NA       NA
## 9   2013     1     2       NA           1620        NA       NA
## 10  2013     1     2       NA           1355        NA       NA
## # ... with 336,766 more rows, and 12 more variables: sched_arr_time <int>,
## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
## #   minute <dbl>, time_hour <dttm>
```

#####2.  

```r
arrange(flights, desc(dep_delay))
```

```
## # A tibble: 336,776 × 19
##     year month   day dep_time sched_dep_time dep_delay arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
## 1   2013     1     9      641            900      1301     1242
## 2   2013     6    15     1432           1935      1137     1607
## 3   2013     1    10     1121           1635      1126     1239
## 4   2013     9    20     1139           1845      1014     1457
## 5   2013     7    22      845           1600      1005     1044
## 6   2013     4    10     1100           1900       960     1342
## 7   2013     3    17     2321            810       911      135
## 8   2013     6    27      959           1900       899     1236
## 9   2013     7    22     2257            759       898      121
## 10  2013    12     5      756           1700       896     1058
## # ... with 336,766 more rows, and 12 more variables: sched_arr_time <int>,
## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
## #   minute <dbl>, time_hour <dttm>
```

```r
arrange(flights, dep_time)
```

```
## # A tibble: 336,776 × 19
##     year month   day dep_time sched_dep_time dep_delay arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
## 1   2013     1    13        1           2249        72      108
## 2   2013     1    31        1           2100       181      124
## 3   2013    11    13        1           2359         2      442
## 4   2013    12    16        1           2359         2      447
## 5   2013    12    20        1           2359         2      430
## 6   2013    12    26        1           2359         2      437
## 7   2013    12    30        1           2359         2      441
## 8   2013     2    11        1           2100       181      111
## 9   2013     2    24        1           2245        76      121
## 10  2013     3     8        1           2355         6      431
## # ... with 336,766 more rows, and 12 more variables: sched_arr_time <int>,
## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
## #   minute <dbl>, time_hour <dttm>
```

#####3.   

```r
arrange(flights, desc(distance/air_time))
```

```
## # A tibble: 336,776 × 19
##     year month   day dep_time sched_dep_time dep_delay arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
## 1   2013     5    25     1709           1700         9     1923
## 2   2013     7     2     1558           1513        45     1745
## 3   2013     5    13     2040           2025        15     2225
## 4   2013     3    23     1914           1910         4     2045
## 5   2013     1    12     1559           1600        -1     1849
## 6   2013    11    17      650            655        -5     1059
## 7   2013     2    21     2355           2358        -3      412
## 8   2013    11    17      759            800        -1     1212
## 9   2013    11    16     2003           1925        38       17
## 10  2013    11    16     2349           2359       -10      402
## # ... with 336,766 more rows, and 12 more variables: sched_arr_time <int>,
## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
## #   minute <dbl>, time_hour <dttm>
```

#####4.   

```r
arrange(flights, desc(distance)) # longest
```

```
## # A tibble: 336,776 × 19
##     year month   day dep_time sched_dep_time dep_delay arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
## 1   2013     1     1      857            900        -3     1516
## 2   2013     1     2      909            900         9     1525
## 3   2013     1     3      914            900        14     1504
## 4   2013     1     4      900            900         0     1516
## 5   2013     1     5      858            900        -2     1519
## 6   2013     1     6     1019            900        79     1558
## 7   2013     1     7     1042            900       102     1620
## 8   2013     1     8      901            900         1     1504
## 9   2013     1     9      641            900      1301     1242
## 10  2013     1    10      859            900        -1     1449
## # ... with 336,766 more rows, and 12 more variables: sched_arr_time <int>,
## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
## #   minute <dbl>, time_hour <dttm>
```

```r
arrange(flights, distance) # shortest
```

```
## # A tibble: 336,776 × 19
##     year month   day dep_time sched_dep_time dep_delay arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
## 1   2013     7    27       NA            106        NA       NA
## 2   2013     1     3     2127           2129        -2     2222
## 3   2013     1     4     1240           1200        40     1333
## 4   2013     1     4     1829           1615       134     1937
## 5   2013     1     4     2128           2129        -1     2218
## 6   2013     1     5     1155           1200        -5     1241
## 7   2013     1     6     2125           2129        -4     2224
## 8   2013     1     7     2124           2129        -5     2212
## 9   2013     1     8     2127           2130        -3     2304
## 10  2013     1     9     2126           2129        -3     2217
## # ... with 336,766 more rows, and 12 more variables: sched_arr_time <int>,
## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
## #   minute <dbl>, time_hour <dttm>
```

####5.4 Select()  

```r
select(flights, year, month, day)
```

```
## # A tibble: 336,776 × 3
##     year month   day
##    <int> <int> <int>
## 1   2013     1     1
## 2   2013     1     1
## 3   2013     1     1
## 4   2013     1     1
## 5   2013     1     1
## 6   2013     1     1
## 7   2013     1     1
## 8   2013     1     1
## 9   2013     1     1
## 10  2013     1     1
## # ... with 336,766 more rows
```

```r
select(flights, year:day)
```

```
## # A tibble: 336,776 × 3
##     year month   day
##    <int> <int> <int>
## 1   2013     1     1
## 2   2013     1     1
## 3   2013     1     1
## 4   2013     1     1
## 5   2013     1     1
## 6   2013     1     1
## 7   2013     1     1
## 8   2013     1     1
## 9   2013     1     1
## 10  2013     1     1
## # ... with 336,766 more rows
```

```r
select(flights, -(year:day))
```

```
## # A tibble: 336,776 × 16
##    dep_time sched_dep_time dep_delay arr_time sched_arr_time arr_delay
##       <int>          <int>     <dbl>    <int>          <int>     <dbl>
## 1       517            515         2      830            819        11
## 2       533            529         4      850            830        20
## 3       542            540         2      923            850        33
## 4       544            545        -1     1004           1022       -18
## 5       554            600        -6      812            837       -25
## 6       554            558        -4      740            728        12
## 7       555            600        -5      913            854        19
## 8       557            600        -3      709            723       -14
## 9       557            600        -3      838            846        -8
## 10      558            600        -2      753            745         8
## # ... with 336,766 more rows, and 10 more variables: carrier <chr>,
## #   flight <int>, tailnum <chr>, origin <chr>, dest <chr>, air_time <dbl>,
## #   distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>
```

```r
select(flights, starts_with("arr"))
```

```
## # A tibble: 336,776 × 2
##    arr_time arr_delay
##       <int>     <dbl>
## 1       830        11
## 2       850        20
## 3       923        33
## 4      1004       -18
## 5       812       -25
## 6       740        12
## 7       913        19
## 8       709       -14
## 9       838        -8
## 10      753         8
## # ... with 336,766 more rows
```

```r
select(flights, ends_with("delay"))
```

```
## # A tibble: 336,776 × 2
##    dep_delay arr_delay
##        <dbl>     <dbl>
## 1          2        11
## 2          4        20
## 3          2        33
## 4         -1       -18
## 5         -6       -25
## 6         -4        12
## 7         -5        19
## 8         -3       -14
## 9         -3        -8
## 10        -2         8
## # ... with 336,766 more rows
```

```r
select(flights, contains("_"))
```

```
## # A tibble: 336,776 × 8
##    dep_time sched_dep_time dep_delay arr_time sched_arr_time arr_delay
##       <int>          <int>     <dbl>    <int>          <int>     <dbl>
## 1       517            515         2      830            819        11
## 2       533            529         4      850            830        20
## 3       542            540         2      923            850        33
## 4       544            545        -1     1004           1022       -18
## 5       554            600        -6      812            837       -25
## 6       554            558        -4      740            728        12
## 7       555            600        -5      913            854        19
## 8       557            600        -3      709            723       -14
## 9       557            600        -3      838            846        -8
## 10      558            600        -2      753            745         8
## # ... with 336,766 more rows, and 2 more variables: air_time <dbl>,
## #   time_hour <dttm>
```

```r
rename(flights, tail_num = tailnum)
```

```
## # A tibble: 336,776 × 19
##     year month   day dep_time sched_dep_time dep_delay arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
## 1   2013     1     1      517            515         2      830
## 2   2013     1     1      533            529         4      850
## 3   2013     1     1      542            540         2      923
## 4   2013     1     1      544            545        -1     1004
## 5   2013     1     1      554            600        -6      812
## 6   2013     1     1      554            558        -4      740
## 7   2013     1     1      555            600        -5      913
## 8   2013     1     1      557            600        -3      709
## 9   2013     1     1      557            600        -3      838
## 10  2013     1     1      558            600        -2      753
## # ... with 336,766 more rows, and 12 more variables: sched_arr_time <int>,
## #   arr_delay <dbl>, carrier <chr>, flight <int>, tail_num <chr>,
## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
## #   minute <dbl>, time_hour <dttm>
```

```r
select(flights, time_hour, air_time, everything())
```

```
## # A tibble: 336,776 × 19
##              time_hour air_time  year month   day dep_time sched_dep_time
##                 <dttm>    <dbl> <int> <int> <int>    <int>          <int>
## 1  2013-01-01 05:00:00      227  2013     1     1      517            515
## 2  2013-01-01 05:00:00      227  2013     1     1      533            529
## 3  2013-01-01 05:00:00      160  2013     1     1      542            540
## 4  2013-01-01 05:00:00      183  2013     1     1      544            545
## 5  2013-01-01 06:00:00      116  2013     1     1      554            600
## 6  2013-01-01 05:00:00      150  2013     1     1      554            558
## 7  2013-01-01 06:00:00      158  2013     1     1      555            600
## 8  2013-01-01 06:00:00       53  2013     1     1      557            600
## 9  2013-01-01 06:00:00      140  2013     1     1      557            600
## 10 2013-01-01 06:00:00      138  2013     1     1      558            600
## # ... with 336,766 more rows, and 12 more variables: dep_delay <dbl>,
## #   arr_time <int>, sched_arr_time <int>, arr_delay <dbl>, carrier <chr>,
## #   flight <int>, tailnum <chr>, origin <chr>, dest <chr>, distance <dbl>,
## #   hour <dbl>, minute <dbl>
```

####5.4.1  
#####1. 

```r
select(flights, dep_time, dep_delay, arr_time, arr_delay)
```

```
## # A tibble: 336,776 × 4
##    dep_time dep_delay arr_time arr_delay
##       <int>     <dbl>    <int>     <dbl>
## 1       517         2      830        11
## 2       533         4      850        20
## 3       542         2      923        33
## 4       544        -1     1004       -18
## 5       554        -6      812       -25
## 6       554        -4      740        12
## 7       555        -5      913        19
## 8       557        -3      709       -14
## 9       557        -3      838        -8
## 10      558        -2      753         8
## # ... with 336,766 more rows
```

```r
select(flights, -(year:day), -sched_dep_time, -sched_arr_time, -(carrier:time_hour))
```

```
## # A tibble: 336,776 × 4
##    dep_time dep_delay arr_time arr_delay
##       <int>     <dbl>    <int>     <dbl>
## 1       517         2      830        11
## 2       533         4      850        20
## 3       542         2      923        33
## 4       544        -1     1004       -18
## 5       554        -6      812       -25
## 6       554        -4      740        12
## 7       555        -5      913        19
## 8       557        -3      709       -14
## 9       557        -3      838        -8
## 10      558        -2      753         8
## # ... with 336,766 more rows
```

```r
#somehow i can't think of anything else... 
```

#####2.  

```r
select(flights, arr_time, arr_time)# it only appears once. 
```

```
## # A tibble: 336,776 × 1
##    arr_time
##       <int>
## 1       830
## 2       850
## 3       923
## 4      1004
## 5       812
## 6       740
## 7       913
## 8       709
## 9       838
## 10      753
## # ... with 336,766 more rows
```

#####3.   

```r
vars <- c("year", "month", "day", "dep_delay", "arr_delay")
#select(flights, vars) this doesn't run because the input vector is in characters, not in factors.
select(flights, one_of(vars)) # one_of() makes the character inputs readable.
```

```
## # A tibble: 336,776 × 5
##     year month   day dep_delay arr_delay
##    <int> <int> <int>     <dbl>     <dbl>
## 1   2013     1     1         2        11
## 2   2013     1     1         4        20
## 3   2013     1     1         2        33
## 4   2013     1     1        -1       -18
## 5   2013     1     1        -6       -25
## 6   2013     1     1        -4        12
## 7   2013     1     1        -5        19
## 8   2013     1     1        -3       -14
## 9   2013     1     1        -3        -8
## 10  2013     1     1        -2         8
## # ... with 336,766 more rows
```

#####4.  

```r
select(flights, contains("TIME"))
```

```
## # A tibble: 336,776 × 6
##    dep_time sched_dep_time arr_time sched_arr_time air_time
##       <int>          <int>    <int>          <int>    <dbl>
## 1       517            515      830            819      227
## 2       533            529      850            830      227
## 3       542            540      923            850      160
## 4       544            545     1004           1022      183
## 5       554            600      812            837      116
## 6       554            558      740            728      150
## 7       555            600      913            854      158
## 8       557            600      709            723       53
## 9       557            600      838            846      140
## 10      558            600      753            745      138
## # ... with 336,766 more rows, and 1 more variables: time_hour <dttm>
```

```r
#select(flights, contains("TIME"), ignore.case = FALSE)
# this command will turn off ignore.case. 
```

it is actually quite surprising to see that this command worked. I guess the default is case neutral. 

####5.5 Mutate()

```r
flights_sml <- select(flights, 
  year:day, 
  ends_with("delay"), 
  distance, 
  air_time
)
mutate(flights_sml,
  gain = arr_delay - dep_delay,
  speed = distance / air_time * 60
)
```

```
## # A tibble: 336,776 × 9
##     year month   day dep_delay arr_delay distance air_time  gain    speed
##    <int> <int> <int>     <dbl>     <dbl>    <dbl>    <dbl> <dbl>    <dbl>
## 1   2013     1     1         2        11     1400      227     9 370.0441
## 2   2013     1     1         4        20     1416      227    16 374.2731
## 3   2013     1     1         2        33     1089      160    31 408.3750
## 4   2013     1     1        -1       -18     1576      183   -17 516.7213
## 5   2013     1     1        -6       -25      762      116   -19 394.1379
## 6   2013     1     1        -4        12      719      150    16 287.6000
## 7   2013     1     1        -5        19     1065      158    24 404.4304
## 8   2013     1     1        -3       -14      229       53   -11 259.2453
## 9   2013     1     1        -3        -8      944      140    -5 404.5714
## 10  2013     1     1        -2         8      733      138    10 318.6957
## # ... with 336,766 more rows
```

```r
mutate(flights_sml,
  gain = arr_delay - dep_delay,
  hours = air_time / 60,
  gain_per_hour = gain / hours
)
```

```
## # A tibble: 336,776 × 10
##     year month   day dep_delay arr_delay distance air_time  gain     hours
##    <int> <int> <int>     <dbl>     <dbl>    <dbl>    <dbl> <dbl>     <dbl>
## 1   2013     1     1         2        11     1400      227     9 3.7833333
## 2   2013     1     1         4        20     1416      227    16 3.7833333
## 3   2013     1     1         2        33     1089      160    31 2.6666667
## 4   2013     1     1        -1       -18     1576      183   -17 3.0500000
## 5   2013     1     1        -6       -25      762      116   -19 1.9333333
## 6   2013     1     1        -4        12      719      150    16 2.5000000
## 7   2013     1     1        -5        19     1065      158    24 2.6333333
## 8   2013     1     1        -3       -14      229       53   -11 0.8833333
## 9   2013     1     1        -3        -8      944      140    -5 2.3333333
## 10  2013     1     1        -2         8      733      138    10 2.3000000
## # ... with 336,766 more rows, and 1 more variables: gain_per_hour <dbl>
```

```r
transmute(flights,
  gain = arr_delay - dep_delay,
  hours = air_time / 60,
  gain_per_hour = gain / hours
)
```

```
## # A tibble: 336,776 × 3
##     gain     hours gain_per_hour
##    <dbl>     <dbl>         <dbl>
## 1      9 3.7833333      2.378855
## 2     16 3.7833333      4.229075
## 3     31 2.6666667     11.625000
## 4    -17 3.0500000     -5.573770
## 5    -19 1.9333333     -9.827586
## 6     16 2.5000000      6.400000
## 7     24 2.6333333      9.113924
## 8    -11 0.8833333    -12.452830
## 9     -5 2.3333333     -2.142857
## 10    10 2.3000000      4.347826
## # ... with 336,766 more rows
```

```r
transmute(flights,
  dep_time,
  hour = dep_time %/% 100,
  minute = dep_time %% 100
)
```

```
## # A tibble: 336,776 × 3
##    dep_time  hour minute
##       <int> <dbl>  <dbl>
## 1       517     5     17
## 2       533     5     33
## 3       542     5     42
## 4       544     5     44
## 5       554     5     54
## 6       554     5     54
## 7       555     5     55
## 8       557     5     57
## 9       557     5     57
## 10      558     5     58
## # ... with 336,766 more rows
```

####Exercise 5.5.2  
#####1.   

```r
mutate(flights, 
       dep_time_in.min = dep_time %/% 100 * 60 + dep_time %% 100, 
       sched_dep_time_in.min = sched_dep_time %/% 100 * 60 + sched_dep_time %% 100)
```

```
## # A tibble: 336,776 × 21
##     year month   day dep_time sched_dep_time dep_delay arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
## 1   2013     1     1      517            515         2      830
## 2   2013     1     1      533            529         4      850
## 3   2013     1     1      542            540         2      923
## 4   2013     1     1      544            545        -1     1004
## 5   2013     1     1      554            600        -6      812
## 6   2013     1     1      554            558        -4      740
## 7   2013     1     1      555            600        -5      913
## 8   2013     1     1      557            600        -3      709
## 9   2013     1     1      557            600        -3      838
## 10  2013     1     1      558            600        -2      753
## # ... with 336,766 more rows, and 14 more variables: sched_arr_time <int>,
## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
## #   minute <dbl>, time_hour <dttm>, dep_time_in.min <dbl>,
## #   sched_dep_time_in.min <dbl>
```

#####2.   

```r
transmute(flights, air_time, new_air_time = arr_time - dep_time)
```

```
## # A tibble: 336,776 × 2
##    air_time new_air_time
##       <dbl>        <int>
## 1       227          313
## 2       227          317
## 3       160          381
## 4       183          460
## 5       116          258
## 6       150          186
## 7       158          358
## 8        53          152
## 9       140          281
## 10      138          195
## # ... with 336,766 more rows
```

```r
#this is not right because the arr_time and dep_time are not in minutes. 
#to translate arr_time and dep_time into minutes:
transmute(flights, air_time, new_air_time = (arr_time %/% 100 * 60 + arr_time %% 100)-(dep_time %/% 100 * 60 + dep_time %% 100))
```

```
## # A tibble: 336,776 × 2
##    air_time new_air_time
##       <dbl>        <dbl>
## 1       227          193
## 2       227          197
## 3       160          221
## 4       183          260
## 5       116          138
## 6       150          106
## 7       158          198
## 8        53           72
## 9       140          161
## 10      138          115
## # ... with 336,766 more rows
```

```r
#well even after I adjusted the units they still are not equal. I feel like the air_time was calculated wrong. 
#to translate the new_air_time into "hour:minute" format:
transmute(flights, air_time, new_air_time = ((arr_time %/% 100 * 60 + arr_time %% 100)-(dep_time %/% 100 * 60 + dep_time %% 100)) %/% 60 * 100 + ((arr_time %/% 100 * 60 + arr_time %% 100)-(dep_time %/% 100 * 60 + dep_time %% 100)) %% 60)
```

```
## # A tibble: 336,776 × 2
##    air_time new_air_time
##       <dbl>        <dbl>
## 1       227          313
## 2       227          317
## 3       160          341
## 4       183          420
## 5       116          218
## 6       150          146
## 7       158          318
## 8        53          112
## 9       140          241
## 10      138          155
## # ... with 336,766 more rows
```

```r
#nope they are still not equal. 
```

#####3.  

```r
transmute(flights, dep_time, sched_dep_time, dep_delay)
```

```
## # A tibble: 336,776 × 3
##    dep_time sched_dep_time dep_delay
##       <int>          <int>     <dbl>
## 1       517            515         2
## 2       533            529         4
## 3       542            540         2
## 4       544            545        -1
## 5       554            600        -6
## 6       554            558        -4
## 7       555            600        -5
## 8       557            600        -3
## 9       557            600        -3
## 10      558            600        -2
## # ... with 336,766 more rows
```

```r
#I think dep_delay = dep_time (when expressed in minutes) - sched_dep_time (in minutes). To prove that:
flt_delay = transmute(flights, dep_time, sched_dep_time, dep_delay, new_dep_delay = (dep_time %/% 100 * 60 + dep_time %% 100) - (sched_dep_time %/% 100 * 60 + sched_dep_time %% 100), TorF = dep_delay == new_dep_delay)
flt_delay
```

```
## # A tibble: 336,776 × 5
##    dep_time sched_dep_time dep_delay new_dep_delay  TorF
##       <int>          <int>     <dbl>         <dbl> <lgl>
## 1       517            515         2             2  TRUE
## 2       533            529         4             4  TRUE
## 3       542            540         2             2  TRUE
## 4       544            545        -1            -1  TRUE
## 5       554            600        -6            -6  TRUE
## 6       554            558        -4            -4  TRUE
## 7       555            600        -5            -5  TRUE
## 8       557            600        -3            -3  TRUE
## 9       557            600        -3            -3  TRUE
## 10      558            600        -2            -2  TRUE
## # ... with 336,766 more rows
```

```r
table(flt_delay$TorF)
```

```
## 
##  FALSE   TRUE 
##   1207 327314
```

```r
# there are a few that are not exactly the same. Upon looking at them we can see that these involve change of day. It would be difficult to fix these. 
```

#####4.  

```r
arrange(flights, desc(min_rank(flights$dep_delay)))
```

```
## # A tibble: 336,776 × 19
##     year month   day dep_time sched_dep_time dep_delay arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
## 1   2013     1     9      641            900      1301     1242
## 2   2013     6    15     1432           1935      1137     1607
## 3   2013     1    10     1121           1635      1126     1239
## 4   2013     9    20     1139           1845      1014     1457
## 5   2013     7    22      845           1600      1005     1044
## 6   2013     4    10     1100           1900       960     1342
## 7   2013     3    17     2321            810       911      135
## 8   2013     6    27      959           1900       899     1236
## 9   2013     7    22     2257            759       898      121
## 10  2013    12     5      756           1700       896     1058
## # ... with 336,766 more rows, and 12 more variables: sched_arr_time <int>,
## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
## #   minute <dbl>, time_hour <dttm>
```

```r
?min_rank()
# the documentation states that ties.method is "min". I couldn't really find what that means...
```

#####5.  

```r
1:3 + 1:10
```

```
## Warning in 1:3 + 1:10: longer object length is not a multiple of shorter
## object length
```

```
##  [1]  2  4  6  5  7  9  8 10 12 11
```

```r
# c(1:3) got wrapped around 3.3 times to match the length of c(1:10)
```

#####6.  

```r
x = .5
y = 2
cos(x)
```

```
## [1] 0.8775826
```

```r
sin(x)
```

```
## [1] 0.4794255
```

```r
tan(x)
```

```
## [1] 0.5463025
```

```r
acos(x)
```

```
## [1] 1.047198
```

```r
asin(x)
```

```
## [1] 0.5235988
```

```r
atan(x)
```

```
## [1] 0.4636476
```

```r
atan2(y, x)
```

```
## [1] 1.325818
```

```r
cospi(x)
```

```
## [1] 0
```

```r
sinpi(x)
```

```
## [1] 1
```

```r
tanpi(x)
```

```
## Warning in tanpi(x): NaNs produced
```

```
## [1] NaN
```

