# R_club_HW6
Rongkui Han  
May 31, 2017  

####7.6 Patterns and models

```r
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

```r
library(dplyr)
ggplot(data = faithful) +
  geom_point(mapping = aes(x = eruptions, y = waiting))
```

![](HW_May_31_files/figure-html/unnamed-chunk-1-1.png)<!-- -->

```r
library(modelr)
mod = lm(log(price) ~ log(carat), data = diamonds)
diamonds2 = diamonds %>% #to remove the coveriance between price and carat
  add_residuals(mod) %>%
  mutate(resid = exp(resid))
ggplot(data = diamonds2) +
  geom_point(mapping = aes(x = carat, y = resid))
```

![](HW_May_31_files/figure-html/unnamed-chunk-1-2.png)<!-- -->

```r
ggplot(data = diamonds2) +
  geom_boxplot(mapping = aes(x = cut, y = resid))
```

![](HW_May_31_files/figure-html/unnamed-chunk-1-3.png)<!-- -->

Now we can see the price increases (although slightly) with better cuts.

####7.7 ggplot2 calls

```r
ggplot(data = faithful, mapping = aes(x = eruptions)) +
  geom_freqpoly(binwidth = 0.25)
```

![](HW_May_31_files/figure-html/unnamed-chunk-2-1.png)<!-- -->

```r
ggplot(faithful, aes(eruptions)) +
  geom_freqpoly(binwidth = 0.25) #this is like a short hand that omitted all x=, mapping= , and data= etc. 
```

![](HW_May_31_files/figure-html/unnamed-chunk-2-2.png)<!-- -->


```r
diamonds %>%
  count(cut, clarity) %>%
  ggplot(aes(clarity, cut, fill = n)) +
    geom_tile()
```

![](HW_May_31_files/figure-html/unnamed-chunk-3-1.png)<!-- -->

###Chapter 10: Tibble  
####10.2 Creating tibbles  

```r
as_tibble(iris)
```

```
## # A tibble: 150 x 5
##    Sepal.Length Sepal.Width Petal.Length Petal.Width Species
##           <dbl>       <dbl>        <dbl>       <dbl>  <fctr>
##  1          5.1         3.5          1.4         0.2  setosa
##  2          4.9         3.0          1.4         0.2  setosa
##  3          4.7         3.2          1.3         0.2  setosa
##  4          4.6         3.1          1.5         0.2  setosa
##  5          5.0         3.6          1.4         0.2  setosa
##  6          5.4         3.9          1.7         0.4  setosa
##  7          4.6         3.4          1.4         0.3  setosa
##  8          5.0         3.4          1.5         0.2  setosa
##  9          4.4         2.9          1.4         0.2  setosa
## 10          4.9         3.1          1.5         0.1  setosa
## # ... with 140 more rows
```

```r
tibble(x = 1:5, y = 1, z = x^2 + y)
```

```
## # A tibble: 5 x 3
##       x     y     z
##   <int> <dbl> <dbl>
## 1     1     1     2
## 2     2     1     5
## 3     3     1    10
## 4     4     1    17
## 5     5     1    26
```

```r
tibble(':)' = "smile", ' ' = "space", '2000' = "number")
```

```
## # A tibble: 1 x 3
##    `:)`   ` ` `2000`
##   <chr> <chr>  <chr>
## 1 smile space number
```

```r
tribble(~x, ~y, ~z, 
        "a",2,3.6,
        "b",1,8.5) #-_- this is kinda confusing. 
```

```
## # A tibble: 2 x 3
##       x     y     z
##   <chr> <dbl> <dbl>
## 1     a     2   3.6
## 2     b     1   8.5
```

####10.3 Tibbles vs. data.frame  
#####10.3.1 Printing 

```r
tibble(
  a = lubridate::now() +runif(1e3) * 86400,
  b = lubridate::today() + runif(1e3) * 30,
  c = 1:1e3,
  d = runif(1e3),
  e = sample(letters, 1e3, replace = TRUE)
)
```

```
## # A tibble: 1,000 x 5
##                      a          b     c         d     e
##                 <dttm>     <date> <int>     <dbl> <chr>
##  1 2017-05-31 13:12:33 2017-06-11     1 0.9382424     n
##  2 2017-05-31 21:25:43 2017-06-27     2 0.2870410     k
##  3 2017-05-31 23:48:40 2017-06-22     3 0.3961865     a
##  4 2017-06-01 04:03:25 2017-06-26     4 0.9347569     r
##  5 2017-05-31 18:02:37 2017-06-08     5 0.3652527     f
##  6 2017-06-01 05:50:03 2017-06-29     6 0.7372463     t
##  7 2017-05-31 15:01:42 2017-06-11     7 0.3198001     n
##  8 2017-05-31 14:37:42 2017-06-11     8 0.3103825     i
##  9 2017-06-01 11:05:03 2017-06-20     9 0.5601266     f
## 10 2017-05-31 22:45:20 2017-06-23    10 0.5662771     w
## # ... with 990 more rows
```

```r
#somehow it didnt print.
library(nycflights13)
flights %>%
  print(n = 10, width = Inf)
```

```
## # A tibble: 336,776 x 19
##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time arr_delay carrier flight tailnum origin  dest air_time distance  hour minute           time_hour
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>     <dbl>   <chr>  <int>   <chr>  <chr> <chr>    <dbl>    <dbl> <dbl>  <dbl>              <dttm>
##  1  2013     1     1      517            515         2      830            819        11      UA   1545  N14228    EWR   IAH      227     1400     5     15 2013-01-01 05:00:00
##  2  2013     1     1      533            529         4      850            830        20      UA   1714  N24211    LGA   IAH      227     1416     5     29 2013-01-01 05:00:00
##  3  2013     1     1      542            540         2      923            850        33      AA   1141  N619AA    JFK   MIA      160     1089     5     40 2013-01-01 05:00:00
##  4  2013     1     1      544            545        -1     1004           1022       -18      B6    725  N804JB    JFK   BQN      183     1576     5     45 2013-01-01 05:00:00
##  5  2013     1     1      554            600        -6      812            837       -25      DL    461  N668DN    LGA   ATL      116      762     6      0 2013-01-01 06:00:00
##  6  2013     1     1      554            558        -4      740            728        12      UA   1696  N39463    EWR   ORD      150      719     5     58 2013-01-01 05:00:00
##  7  2013     1     1      555            600        -5      913            854        19      B6    507  N516JB    EWR   FLL      158     1065     6      0 2013-01-01 06:00:00
##  8  2013     1     1      557            600        -3      709            723       -14      EV   5708  N829AS    LGA   IAD       53      229     6      0 2013-01-01 06:00:00
##  9  2013     1     1      557            600        -3      838            846        -8      B6     79  N593JB    JFK   MCO      140      944     6      0 2013-01-01 06:00:00
## 10  2013     1     1      558            600        -2      753            745         8      AA    301  N3ALAA    LGA   ORD      138      733     6      0 2013-01-01 06:00:00
## # ... with 3.368e+05 more rows
```

```r
#options(tibble.print_max = n, tibble.print_min = m) to change the difalut option of printing 
#options(tibble.width = Inf) to always print all columns. 
nycflights13::flights %>%
  View()
```

```
## Warning: running command ''/usr/bin/otool' -L '/Library/Frameworks/
## R.framework/Resources/modules/R_de.so'' had status 1
```

##### 10.3.2 Subsetting   

```r
df = tibble(x = runif(5), y = runif(5))
df$x
```

```
## [1] 0.24888359 0.55274460 0.78979022 0.09038521 0.88022760
```

```r
df[["x"]] #never used this one before
```

```
## [1] 0.24888359 0.55274460 0.78979022 0.09038521 0.88022760
```

```r
df[[1]]
```

```
## [1] 0.24888359 0.55274460 0.78979022 0.09038521 0.88022760
```

```r
df %>% .$x #you can subset in a pipe
```

```
## [1] 0.24888359 0.55274460 0.78979022 0.09038521 0.88022760
```

```r
df %>% .[["x"]]
```

```
## [1] 0.24888359 0.55274460 0.78979022 0.09038521 0.88022760
```

####10.4 Interacting with older code

```r
tb = tibble(x = 1:5, y = c("a","b","c","d","e"))
class(as.data.frame(tb))
```

```
## [1] "data.frame"
```

####10.5 Exercises  
1. How can you tell if an object is a tibble?  

```r
print(mtcars)
```

```
##                      mpg cyl  disp  hp drat    wt  qsec vs am gear carb
## Mazda RX4           21.0   6 160.0 110 3.90 2.620 16.46  0  1    4    4
## Mazda RX4 Wag       21.0   6 160.0 110 3.90 2.875 17.02  0  1    4    4
## Datsun 710          22.8   4 108.0  93 3.85 2.320 18.61  1  1    4    1
## Hornet 4 Drive      21.4   6 258.0 110 3.08 3.215 19.44  1  0    3    1
## Hornet Sportabout   18.7   8 360.0 175 3.15 3.440 17.02  0  0    3    2
## Valiant             18.1   6 225.0 105 2.76 3.460 20.22  1  0    3    1
## Duster 360          14.3   8 360.0 245 3.21 3.570 15.84  0  0    3    4
## Merc 240D           24.4   4 146.7  62 3.69 3.190 20.00  1  0    4    2
## Merc 230            22.8   4 140.8  95 3.92 3.150 22.90  1  0    4    2
## Merc 280            19.2   6 167.6 123 3.92 3.440 18.30  1  0    4    4
## Merc 280C           17.8   6 167.6 123 3.92 3.440 18.90  1  0    4    4
## Merc 450SE          16.4   8 275.8 180 3.07 4.070 17.40  0  0    3    3
## Merc 450SL          17.3   8 275.8 180 3.07 3.730 17.60  0  0    3    3
## Merc 450SLC         15.2   8 275.8 180 3.07 3.780 18.00  0  0    3    3
## Cadillac Fleetwood  10.4   8 472.0 205 2.93 5.250 17.98  0  0    3    4
## Lincoln Continental 10.4   8 460.0 215 3.00 5.424 17.82  0  0    3    4
## Chrysler Imperial   14.7   8 440.0 230 3.23 5.345 17.42  0  0    3    4
## Fiat 128            32.4   4  78.7  66 4.08 2.200 19.47  1  1    4    1
## Honda Civic         30.4   4  75.7  52 4.93 1.615 18.52  1  1    4    2
## Toyota Corolla      33.9   4  71.1  65 4.22 1.835 19.90  1  1    4    1
## Toyota Corona       21.5   4 120.1  97 3.70 2.465 20.01  1  0    3    1
## Dodge Challenger    15.5   8 318.0 150 2.76 3.520 16.87  0  0    3    2
## AMC Javelin         15.2   8 304.0 150 3.15 3.435 17.30  0  0    3    2
## Camaro Z28          13.3   8 350.0 245 3.73 3.840 15.41  0  0    3    4
## Pontiac Firebird    19.2   8 400.0 175 3.08 3.845 17.05  0  0    3    2
## Fiat X1-9           27.3   4  79.0  66 4.08 1.935 18.90  1  1    4    1
## Porsche 914-2       26.0   4 120.3  91 4.43 2.140 16.70  0  1    5    2
## Lotus Europa        30.4   4  95.1 113 3.77 1.513 16.90  1  1    5    2
## Ford Pantera L      15.8   8 351.0 264 4.22 3.170 14.50  0  1    5    4
## Ferrari Dino        19.7   6 145.0 175 3.62 2.770 15.50  0  1    5    6
## Maserati Bora       15.0   8 301.0 335 3.54 3.570 14.60  0  1    5    8
## Volvo 142E          21.4   4 121.0 109 4.11 2.780 18.60  1  1    4    2
```

```r
print(diamonds)
```

```
## # A tibble: 53,940 x 10
##    carat       cut color clarity depth table price     x     y     z
##    <dbl>     <ord> <ord>   <ord> <dbl> <dbl> <int> <dbl> <dbl> <dbl>
##  1  0.23     Ideal     E     SI2  61.5    55   326  3.95  3.98  2.43
##  2  0.21   Premium     E     SI1  59.8    61   326  3.89  3.84  2.31
##  3  0.23      Good     E     VS1  56.9    65   327  4.05  4.07  2.31
##  4  0.29   Premium     I     VS2  62.4    58   334  4.20  4.23  2.63
##  5  0.31      Good     J     SI2  63.3    58   335  4.34  4.35  2.75
##  6  0.24 Very Good     J    VVS2  62.8    57   336  3.94  3.96  2.48
##  7  0.24 Very Good     I    VVS1  62.3    57   336  3.95  3.98  2.47
##  8  0.26 Very Good     H     SI1  61.9    55   337  4.07  4.11  2.53
##  9  0.22      Fair     E     VS2  65.1    61   337  3.87  3.78  2.49
## 10  0.23 Very Good     H     VS1  59.4    61   338  4.00  4.05  2.39
## # ... with 53,930 more rows
```

```r
class(mtcars)
```

```
## [1] "data.frame"
```

```r
class(diamonds)
```

```
## [1] "tbl_df"     "tbl"        "data.frame"
```

I don't know I was supposed to be able to tell which one was a tibble just by printing them. But when I uwed the class() function it was pretty clear.   

2. Compare and contrast the following operations on a data.frame and equivalent tibble. What is different? Why might the default data frame behavious cause you frustration?  

```r
df = data.frame(abc = 1, xyz = "a")
df$x
```

```
## [1] a
## Levels: a
```

```r
df[,"xyz"]
```

```
## [1] a
## Levels: a
```

```r
df[,c("abc","xyz")]
```

```
##   abc xyz
## 1   1   a
```

```r
df$a
```

```
## [1] 1
```

the df$x command gave me the same output as df[,"xyz"]. I guess the "x"" was good enough to evoke "xyz?"  

3. If you have the name of a vairable stored in an object, e.g. var = "mpg", how can you extrat the reference variable from a tibble?

```r
diamonds %>%
  select(cut)
```

```
## # A tibble: 53,940 x 1
##          cut
##        <ord>
##  1     Ideal
##  2   Premium
##  3      Good
##  4   Premium
##  5      Good
##  6 Very Good
##  7 Very Good
##  8 Very Good
##  9      Fair
## 10 Very Good
## # ... with 53,930 more rows
```

```r
as.data.frame(diamonds)[,"cut"]
```

```
##     [1] Ideal     Premium   Good      Premium   Good      Very Good
##     [7] Very Good Very Good Fair      Very Good Good      Ideal    
##    [13] Premium   Ideal     Premium   Premium   Ideal     Good     
##    [19] Good      Very Good Good      Very Good Very Good Very Good
##    [25] Very Good Very Good Premium   Very Good Very Good Very Good
##    [31] Very Good Very Good Very Good Very Good Very Good Good     
##    [37] Good      Good      Very Good Ideal     Ideal     Ideal    
##    [43] Good      Good      Good      Premium   Very Good Good     
##    [49] Very Good Very Good Very Good Ideal     Ideal     Premium  
##    [55] Premium   Ideal     Premium   Very Good Very Good Good     
##    [61] Ideal     Premium   Ideal     Ideal     Premium   Ideal    
##    [67] Ideal     Very Good Premium   Premium   Very Good Very Good
##    [73] Premium   Premium   Good      Very Good Very Good Very Good
##    [79] Very Good Very Good Very Good Very Good Ideal     Ideal    
##    [85] Good      Premium   Premium   Premium   Premium   Premium  
##    [91] Ideal     Fair      Ideal     Very Good Very Good Good     
##    [97] Good      Fair      Very Good Premium   Very Good Premium  
##   [103] Ideal     Premium   Ideal     Ideal     Premium   Ideal    
##   [109] Ideal     Ideal     Ideal     Ideal     Premium   Very Good
##   [115] Ideal     Ideal     Premium   Ideal     Ideal     Ideal    
##   [121] Ideal     Ideal     Very Good Fair      Fair      Premium  
##   [127] Premium   Very Good Fair      Fair      Ideal     Very Good
##   [133] Ideal     Very Good Very Good Premium   Very Good Premium  
##   [139] Ideal     Ideal     Premium   Premium   Very Good Very Good
##   [145] Ideal     Good      Very Good Very Good Very Good Ideal    
##   [151] Premium   Ideal     Premium   Premium   Very Good Ideal    
##   [157] Ideal     Premium   Premium   Ideal     Premium   Very Good
##   [163] Very Good Ideal     Ideal     Very Good Very Good Ideal    
##   [169] Ideal     Good      Ideal     Premium   Very Good Ideal    
##   [175] Ideal     Good      Very Good Very Good Premium   Ideal    
##   [181] Ideal     Ideal     Ideal     Ideal     Good      Ideal    
##   [187] Very Good Premium   Very Good Good      Good      Ideal    
##   [193] Premium   Premium   Premium   Premium   Premium   Premium  
##   [199] Ideal     Premium   Very Good Premium   Premium   Good     
##   [205] Fair      Premium   Very Good Ideal     Very Good Ideal    
##   [211] Very Good Premium   Ideal     Ideal     Ideal     Premium  
##   [217] Ideal     Ideal     Very Good Very Good Ideal     Good     
##   [223] Premium   Very Good Ideal     Premium   Premium   Fair     
##   [229] Premium   Ideal     Very Good Very Good Very Good Ideal    
##   [235] Ideal     Very Good Very Good Ideal     Good      Good     
##   [241] Ideal     Fair      Premium   Good      Good      Premium  
##   [247] Premium   Very Good Ideal     Ideal     Ideal     Good     
##   [253] Premium   Premium   Premium   Fair      Ideal     Premium  
##   [259] Ideal     Premium   Premium   Very Good Ideal     Premium  
##   [265] Premium   Ideal     Premium   Premium   Premium   Ideal    
##   [271] Very Good Very Good Good      Ideal     Ideal     Very Good
##   [277] Very Good Premium   Ideal     Good      Premium   Premium  
##   [283] Premium   Premium   Premium   Good      Very Good Very Good
##   [289] Premium   Premium   Premium   Ideal     Ideal     Ideal    
##   [295] Ideal     Ideal     Fair      Premium   Fair      Very Good
##   [301] Ideal     Very Good Ideal     Ideal     Very Good Good     
##   [307] Premium   Very Good Ideal     Ideal     Very Good Premium  
##   [313] Ideal     Ideal     Fair      Ideal     Ideal     Premium  
##   [319] Ideal     Premium   Good      Good      Premium   Premium  
##   [325] Premium   Very Good Ideal     Premium   Premium   Very Good
##   [331] Very Good Ideal     Ideal     Good      Premium   Premium  
##   [337] Premium   Premium   Premium   Premium   Very Good Ideal    
##   [343] Very Good Very Good Very Good Very Good Ideal     Very Good
##   [349] Ideal     Ideal     Ideal     Ideal     Fair      Premium  
##   [355] Premium   Ideal     Fair      Premium   Ideal     Fair     
##   [361] Good      Very Good Very Good Ideal     Very Good Very Good
##   [367] Premium   Very Good Very Good Fair      Very Good Ideal    
##   [373] Very Good Very Good Premium   Premium   Fair      Very Good
##   [379] Very Good Ideal     Good      Good      Very Good Very Good
##   [385] Fair      Fair      Very Good Very Good Good      Very Good
##   [391] Premium   Premium   Ideal     Premium   Very Good Ideal    
##   [397] Premium   Premium   Premium   Very Good Good      Ideal    
##   [403] Very Good Good      Ideal     Very Good Very Good Very Good
##   [409] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
##   [415] Ideal     Very Good Very Good Ideal     Ideal     Ideal    
##   [421] Very Good Very Good Ideal     Fair      Premium   Ideal    
##   [427] Premium   Very Good Good      Premium   Ideal     Premium  
##   [433] Very Good Very Good Premium   Ideal     Ideal     Ideal    
##   [439] Ideal     Fair      Fair      Premium   Premium   Fair     
##   [445] Premium   Very Good Ideal     Good      Premium   Ideal    
##   [451] Ideal     Premium   Ideal     Ideal     Premium   Ideal    
##   [457] Premium   Premium   Ideal     Premium   Ideal     Good     
##   [463] Premium   Premium   Premium   Premium   Premium   Ideal    
##   [469] Ideal     Premium   Premium   Premium   Fair      Very Good
##   [475] Ideal     Good      Good      Ideal     Ideal     Premium  
##   [481] Ideal     Premium   Good      Premium   Premium   Premium  
##   [487] Very Good Premium   Premium   Premium   Premium   Premium  
##   [493] Ideal     Ideal     Ideal     Premium   Premium   Premium  
##   [499] Ideal     Good      Ideal     Premium   Premium   Ideal    
##   [505] Good      Ideal     Ideal     Premium   Premium   Ideal    
##   [511] Very Good Good      Premium   Ideal     Fair      Ideal    
##   [517] Premium   Ideal     Good      Ideal     Premium   Premium  
##   [523] Premium   Very Good Premium   Premium   Fair      Premium  
##   [529] Good      Premium   Premium   Very Good Premium   Very Good
##   [535] Premium   Ideal     Very Good Good      Premium   Ideal    
##   [541] Ideal     Ideal     Premium   Premium   Good      Ideal    
##   [547] Ideal     Premium   Premium   Very Good Very Good Ideal    
##   [553] Ideal     Ideal     Ideal     Premium   Very Good Ideal    
##   [559] Ideal     Premium   Ideal     Very Good Very Good Premium  
##   [565] Premium   Ideal     Premium   Good      Very Good Very Good
##   [571] Very Good Ideal     Ideal     Ideal     Ideal     Ideal    
##   [577] Ideal     Very Good Ideal     Ideal     Very Good Fair     
##   [583] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
##   [589] Premium   Very Good Good      Premium   Ideal     Ideal    
##   [595] Very Good Fair      Premium   Premium   Premium   Premium  
##   [601] Premium   Very Good Premium   Premium   Premium   Very Good
##   [607] Ideal     Ideal     Ideal     Premium   Premium   Premium  
##   [613] Ideal     Ideal     Ideal     Ideal     Premium   Premium  
##   [619] Premium   Premium   Very Good Very Good Ideal     Premium  
##   [625] Ideal     Premium   Premium   Premium   Very Good Very Good
##   [631] Ideal     Ideal     Very Good Ideal     Premium   Fair     
##   [637] Ideal     Premium   Premium   Ideal     Premium   Very Good
##   [643] Very Good Very Good Very Good Very Good Ideal     Very Good
##   [649] Ideal     Ideal     Ideal     Very Good Very Good Ideal    
##   [655] Good      Ideal     Very Good Very Good Good      Premium  
##   [661] Ideal     Very Good Ideal     Fair      Good      Ideal    
##   [667] Ideal     Good      Premium   Premium   Premium   Very Good
##   [673] Ideal     Premium   Very Good Ideal     Fair      Good     
##   [679] Very Good Very Good Ideal     Ideal     Very Good Very Good
##   [685] Ideal     Very Good Premium   Ideal     Very Good Ideal    
##   [691] Ideal     Ideal     Very Good Premium   Good      Ideal    
##   [697] Premium   Premium   Premium   Very Good Very Good Premium  
##   [703] Premium   Fair      Fair      Good      Fair      Premium  
##   [709] Premium   Very Good Good      Premium   Fair      Fair     
##   [715] Fair      Ideal     Ideal     Ideal     Ideal     Fair     
##   [721] Ideal     Ideal     Ideal     Good      Good      Good     
##   [727] Good      Very Good Ideal     Good      Ideal     Ideal    
##   [733] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
##   [739] Ideal     Premium   Premium   Premium   Ideal     Premium  
##   [745] Premium   Premium   Ideal     Ideal     Good      Premium  
##   [751] Fair      Premium   Premium   Good      Very Good Ideal    
##   [757] Premium   Premium   Ideal     Very Good Very Good Premium  
##   [763] Premium   Premium   Very Good Premium   Ideal     Ideal    
##   [769] Premium   Good      Fair      Fair      Ideal     Premium  
##   [775] Premium   Ideal     Premium   Fair      Premium   Very Good
##   [781] Very Good Ideal     Premium   Ideal     Premium   Ideal    
##   [787] Ideal     Premium   Fair      Premium   Premium   Very Good
##   [793] Very Good Very Good Premium   Very Good Ideal     Very Good
##   [799] Premium   Premium   Premium   Fair      Premium   Good     
##   [805] Ideal     Premium   Premium   Premium   Premium   Premium  
##   [811] Ideal     Premium   Premium   Premium   Premium   Premium  
##   [817] Premium   Ideal     Premium   Ideal     Ideal     Very Good
##   [823] Premium   Very Good Very Good Ideal     Ideal     Premium  
##   [829] Very Good Ideal     Ideal     Premium   Premium   Ideal    
##   [835] Ideal     Ideal     Ideal     Premium   Ideal     Fair     
##   [841] Ideal     Ideal     Premium   Very Good Premium   Ideal    
##   [847] Ideal     Ideal     Ideal     Premium   Ideal     Premium  
##   [853] Ideal     Ideal     Ideal     Ideal     Premium   Premium  
##   [859] Very Good Good      Premium   Ideal     Ideal     Good     
##   [865] Very Good Ideal     Very Good Ideal     Good      Ideal    
##   [871] Ideal     Very Good Very Good Ideal     Good      Very Good
##   [877] Very Good Ideal     Ideal     Fair      Fair      Premium  
##   [883] Good      Fair      Fair      Premium   Premium   Premium  
##   [889] Premium   Premium   Premium   Ideal     Ideal     Ideal    
##   [895] Premium   Fair      Ideal     Fair      Fair      Fair     
##   [901] Premium   Premium   Very Good Ideal     Ideal     Good     
##   [907] Good      Very Good Very Good Very Good Ideal     Ideal    
##   [913] Very Good Premium   Premium   Premium   Ideal     Ideal    
##   [919] Ideal     Fair      Ideal     Very Good Very Good Ideal    
##   [925] Very Good Premium   Very Good Ideal     Premium   Fair     
##   [931] Premium   Ideal     Good      Fair      Fair      Very Good
##   [937] Premium   Ideal     Fair      Fair      Ideal     Ideal    
##   [943] Very Good Premium   Ideal     Very Good Very Good Very Good
##   [949] Good      Very Good Ideal     Ideal     Ideal     Premium  
##   [955] Premium   Ideal     Ideal     Premium   Good      Ideal    
##   [961] Very Good Ideal     Ideal     Ideal     Ideal     Good     
##   [967] Ideal     Fair      Very Good Very Good Ideal     Ideal    
##   [973] Very Good Ideal     Good      Very Good Premium   Very Good
##   [979] Ideal     Ideal     Very Good Very Good Ideal     Very Good
##   [985] Very Good Very Good Ideal     Ideal     Ideal     Ideal    
##   [991] Premium   Very Good Ideal     Very Good Premium   Ideal    
##   [997] Ideal     Good      Premium   Premium   Ideal     Ideal    
##  [1003] Good      Ideal     Ideal     Ideal     Ideal     Ideal    
##  [1009] Ideal     Good      Very Good Premium   Premium   Ideal    
##  [1015] Premium   Ideal     Premium   Ideal     Ideal     Fair     
##  [1021] Premium   Premium   Ideal     Ideal     Premium   Premium  
##  [1027] Very Good Very Good Very Good Ideal     Very Good Very Good
##  [1033] Very Good Very Good Very Good Ideal     Very Good Good     
##  [1039] Premium   Very Good Ideal     Good      Ideal     Ideal    
##  [1045] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
##  [1051] Ideal     Ideal     Ideal     Premium   Good      Premium  
##  [1057] Premium   Premium   Premium   Very Good Premium   Premium  
##  [1063] Premium   Ideal     Ideal     Ideal     Premium   Ideal    
##  [1069] Premium   Premium   Premium   Very Good Premium   Premium  
##  [1075] Premium   Good      Premium   Premium   Ideal     Premium  
##  [1081] Very Good Ideal     Premium   Premium   Ideal     Ideal    
##  [1087] Ideal     Very Good Premium   Ideal     Ideal     Good     
##  [1093] Good      Good      Good      Premium   Fair      Fair     
##  [1099] Fair      Very Good Premium   Premium   Premium   Very Good
##  [1105] Ideal     Premium   Premium   Premium   Ideal     Very Good
##  [1111] Ideal     Very Good Very Good Very Good Ideal     Very Good
##  [1117] Ideal     Premium   Ideal     Ideal     Very Good Very Good
##  [1123] Ideal     Ideal     Very Good Very Good Very Good Premium  
##  [1129] Good      Ideal     Ideal     Premium   Ideal     Premium  
##  [1135] Ideal     Very Good Very Good Very Good Very Good Ideal    
##  [1141] Fair      Very Good Very Good Very Good Ideal     Ideal    
##  [1147] Very Good Very Good Very Good Very Good Very Good Ideal    
##  [1153] Ideal     Ideal     Ideal     Good      Premium   Ideal    
##  [1159] Very Good Very Good Very Good Ideal     Ideal     Premium  
##  [1165] Good      Premium   Premium   Very Good Ideal     Premium  
##  [1171] Very Good Very Good Very Good Very Good Ideal     Ideal    
##  [1177] Very Good Premium   Ideal     Ideal     Ideal     Ideal    
##  [1183] Very Good Fair      Ideal     Very Good Very Good Ideal    
##  [1189] Premium   Premium   Very Good Very Good Very Good Premium  
##  [1195] Ideal     Good      Good      Ideal     Premium   Fair     
##  [1201] Premium   Very Good Very Good Ideal     Ideal     Ideal    
##  [1207] Premium   Premium   Premium   Very Good Very Good Ideal    
##  [1213] Very Good Very Good Very Good Premium   Ideal     Ideal    
##  [1219] Ideal     Ideal     Premium   Ideal     Premium   Good     
##  [1225] Very Good Very Good Ideal     Fair      Good      Ideal    
##  [1231] Ideal     Very Good Good      Very Good Premium   Fair     
##  [1237] Ideal     Premium   Fair      Ideal     Ideal     Very Good
##  [1243] Ideal     Very Good Very Good Very Good Good      Very Good
##  [1249] Very Good Ideal     Ideal     Ideal     Ideal     Very Good
##  [1255] Ideal     Ideal     Premium   Ideal     Ideal     Ideal    
##  [1261] Ideal     Ideal     Good      Premium   Ideal     Ideal    
##  [1267] Premium   Fair      Premium   Ideal     Fair      Very Good
##  [1273] Ideal     Ideal     Ideal     Fair      Very Good Ideal    
##  [1279] Premium   Ideal     Very Good Ideal     Ideal     Ideal    
##  [1285] Very Good Very Good Very Good Very Good Ideal     Ideal    
##  [1291] Ideal     Ideal     Ideal     Ideal     Good      Very Good
##  [1297] Premium   Good      Very Good Ideal     Premium   Premium  
##  [1303] Ideal     Very Good Ideal     Premium   Very Good Very Good
##  [1309] Very Good Good      Very Good Ideal     Good      Premium  
##  [1315] Ideal     Ideal     Ideal     Ideal     Good      Fair     
##  [1321] Very Good Very Good Premium   Ideal     Fair      Very Good
##  [1327] Very Good Good      Ideal     Ideal     Ideal     Ideal    
##  [1333] Good      Ideal     Very Good Ideal     Premium   Premium  
##  [1339] Premium   Ideal     Ideal     Ideal     Premium   Premium  
##  [1345] Premium   Ideal     Ideal     Ideal     Premium   Ideal    
##  [1351] Very Good Very Good Very Good Very Good Good      Fair     
##  [1357] Ideal     Fair      Good      Fair      Premium   Fair     
##  [1363] Fair      Premium   Fair      Ideal     Ideal     Ideal    
##  [1369] Ideal     Ideal     Ideal     Very Good Very Good Ideal    
##  [1375] Ideal     Ideal     Very Good Very Good Very Good Ideal    
##  [1381] Premium   Premium   Ideal     Very Good Very Good Very Good
##  [1387] Very Good Ideal     Ideal     Ideal     Ideal     Ideal    
##  [1393] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
##  [1399] Ideal     Ideal     Ideal     Very Good Very Good Very Good
##  [1405] Very Good Ideal     Ideal     Ideal     Premium   Ideal    
##  [1411] Very Good Premium   Fair      Premium   Premium   Ideal    
##  [1417] Ideal     Premium   Very Good Good      Premium   Ideal    
##  [1423] Good      Fair      Very Good Ideal     Ideal     Very Good
##  [1429] Premium   Very Good Ideal     Ideal     Ideal     Very Good
##  [1435] Good      Ideal     Ideal     Good      Fair      Ideal    
##  [1441] Very Good Premium   Very Good Ideal     Premium   Ideal    
##  [1447] Premium   Ideal     Ideal     Ideal     Very Good Ideal    
##  [1453] Premium   Ideal     Ideal     Ideal     Ideal     Ideal    
##  [1459] Ideal     Ideal     Premium   Ideal     Good      Good     
##  [1465] Ideal     Premium   Ideal     Good      Ideal     Fair     
##  [1471] Ideal     Very Good Premium   Ideal     Very Good Premium  
##  [1477] Very Good Ideal     Premium   Very Good Ideal     Ideal    
##  [1483] Ideal     Ideal     Ideal     Premium   Very Good Very Good
##  [1489] Ideal     Ideal     Premium   Very Good Very Good Good     
##  [1495] Ideal     Ideal     Very Good Ideal     Fair      Very Good
##  [1501] Very Good Very Good Ideal     Ideal     Ideal     Ideal    
##  [1507] Fair      Very Good Very Good Very Good Premium   Premium  
##  [1513] Premium   Ideal     Ideal     Fair      Very Good Ideal    
##  [1519] Ideal     Ideal     Ideal     Ideal     Ideal     Fair     
##  [1525] Fair      Very Good Very Good Fair      Premium   Ideal    
##  [1531] Premium   Premium   Premium   Premium   Ideal     Good     
##  [1537] Premium   Very Good Ideal     Very Good Premium   Good     
##  [1543] Very Good Ideal     Very Good Very Good Very Good Premium  
##  [1549] Good      Good      Ideal     Premium   Fair      Ideal    
##  [1555] Fair      Premium   Premium   Ideal     Ideal     Ideal    
##  [1561] Ideal     Ideal     Premium   Very Good Ideal     Very Good
##  [1567] Ideal     Ideal     Very Good Very Good Premium   Very Good
##  [1573] Premium   Ideal     Ideal     Premium   Ideal     Ideal    
##  [1579] Ideal     Ideal     Ideal     Ideal     Very Good Very Good
##  [1585] Ideal     Ideal     Premium   Very Good Very Good Very Good
##  [1591] Very Good Ideal     Very Good Ideal     Ideal     Very Good
##  [1597] Very Good Fair      Fair      Fair      Ideal     Ideal    
##  [1603] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
##  [1609] Fair      Very Good Ideal     Ideal     Ideal     Ideal    
##  [1615] Very Good Premium   Very Good Ideal     Ideal     Ideal    
##  [1621] Premium   Ideal     Ideal     Very Good Premium   Good     
##  [1627] Very Good Very Good Ideal     Ideal     Very Good Premium  
##  [1633] Ideal     Good      Ideal     Premium   Ideal     Very Good
##  [1639] Very Good Premium   Ideal     Good      Very Good Good     
##  [1645] Very Good Premium   Ideal     Ideal     Good      Very Good
##  [1651] Premium   Premium   Premium   Premium   Ideal     Very Good
##  [1657] Ideal     Ideal     Good      Good      Premium   Very Good
##  [1663] Good      Premium   Fair      Premium   Good      Premium  
##  [1669] Very Good Premium   Very Good Very Good Ideal     Ideal    
##  [1675] Ideal     Fair      Fair      Premium   Good      Very Good
##  [1681] Ideal     Ideal     Fair      Premium   Good      Very Good
##  [1687] Ideal     Very Good Ideal     Ideal     Ideal     Good     
##  [1693] Very Good Good      Fair      Premium   Ideal     Good     
##  [1699] Ideal     Very Good Ideal     Ideal     Ideal     Ideal    
##  [1705] Very Good Ideal     Premium   Ideal     Ideal     Very Good
##  [1711] Good      Premium   Good      Premium   Ideal     Premium  
##  [1717] Premium   Very Good Good      Good      Ideal     Ideal    
##  [1723] Ideal     Premium   Very Good Premium   Good      Very Good
##  [1729] Ideal     Very Good Premium   Very Good Ideal     Premium  
##  [1735] Good      Very Good Premium   Ideal     Ideal     Ideal    
##  [1741] Ideal     Ideal     Ideal     Ideal     Very Good Fair     
##  [1747] Very Good Premium   Ideal     Ideal     Ideal     Good     
##  [1753] Ideal     Very Good Premium   Ideal     Very Good Ideal    
##  [1759] Very Good Very Good Very Good Very Good Premium   Ideal    
##  [1765] Ideal     Very Good Good      Very Good Fair      Premium  
##  [1771] Premium   Ideal     Very Good Very Good Premium   Ideal    
##  [1777] Good      Very Good Ideal     Premium   Premium   Ideal    
##  [1783] Ideal     Ideal     Very Good Very Good Ideal     Ideal    
##  [1789] Good      Ideal     Good      Very Good Very Good Ideal    
##  [1795] Very Good Fair      Very Good Very Good Very Good Ideal    
##  [1801] Ideal     Ideal     Ideal     Very Good Premium   Premium  
##  [1807] Ideal     Very Good Premium   Premium   Fair      Fair     
##  [1813] Very Good Very Good Ideal     Ideal     Ideal     Very Good
##  [1819] Very Good Ideal     Good      Good      Fair      Good     
##  [1825] Premium   Premium   Very Good Very Good Very Good Very Good
##  [1831] Good      Very Good Very Good Very Good Very Good Ideal    
##  [1837] Good      Fair      Premium   Ideal     Fair      Premium  
##  [1843] Premium   Premium   Very Good Very Good Good      Very Good
##  [1849] Premium   Ideal     Ideal     Ideal     Very Good Ideal    
##  [1855] Ideal     Very Good Ideal     Ideal     Ideal     Ideal    
##  [1861] Ideal     Very Good Good      Ideal     Very Good Ideal    
##  [1867] Very Good Very Good Very Good Good      Very Good Ideal    
##  [1873] Good      Very Good Very Good Ideal     Ideal     Ideal    
##  [1879] Premium   Premium   Good      Very Good Fair      Ideal    
##  [1885] Good      Very Good Very Good Very Good Very Good Ideal    
##  [1891] Ideal     Ideal     Ideal     Very Good Very Good Ideal    
##  [1897] Fair      Premium   Good      Premium   Premium   Ideal    
##  [1903] Good      Fair      Very Good Ideal     Fair      Premium  
##  [1909] Premium   Premium   Ideal     Ideal     Very Good Ideal    
##  [1915] Ideal     Ideal     Very Good Very Good Ideal     Ideal    
##  [1921] Good      Very Good Premium   Premium   Ideal     Premium  
##  [1927] Premium   Premium   Very Good Premium   Very Good Very Good
##  [1933] Premium   Ideal     Ideal     Ideal     Good      Good     
##  [1939] Very Good Premium   Premium   Fair      Fair      Premium  
##  [1945] Ideal     Premium   Very Good Fair      Ideal     Very Good
##  [1951] Premium   Ideal     Good      Good      Very Good Good     
##  [1957] Ideal     Ideal     Good      Very Good Premium   Ideal    
##  [1963] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
##  [1969] Ideal     Ideal     Ideal     Premium   Ideal     Premium  
##  [1975] Premium   Premium   Very Good Very Good Ideal     Ideal    
##  [1981] Very Good Ideal     Ideal     Ideal     Ideal     Very Good
##  [1987] Very Good Premium   Premium   Very Good Very Good Good     
##  [1993] Premium   Ideal     Premium   Ideal     Ideal     Good     
##  [1999] Very Good Ideal     Premium   Ideal     Ideal     Ideal    
##  [2005] Premium   Very Good Very Good Ideal     Ideal     Ideal    
##  [2011] Ideal     Ideal     Premium   Very Good Very Good Fair     
##  [2017] Fair      Ideal     Premium   Ideal     Ideal     Very Good
##  [2023] Premium   Very Good Good      Good      Ideal     Fair     
##  [2029] Premium   Premium   Very Good Ideal     Premium   Good     
##  [2035] Premium   Ideal     Good      Very Good Ideal     Good     
##  [2041] Good      Ideal     Ideal     Premium   Premium   Premium  
##  [2047] Ideal     Very Good Ideal     Very Good Good      Very Good
##  [2053] Premium   Premium   Very Good Premium   Very Good Very Good
##  [2059] Premium   Ideal     Very Good Very Good Premium   Premium  
##  [2065] Premium   Premium   Premium   Ideal     Premium   Premium  
##  [2071] Premium   Good      Very Good Premium   Premium   Ideal    
##  [2077] Premium   Premium   Very Good Premium   Ideal     Good     
##  [2083] Very Good Good      Ideal     Very Good Very Good Premium  
##  [2089] Very Good Very Good Ideal     Very Good Very Good Ideal    
##  [2095] Good      Very Good Ideal     Ideal     Good      Ideal    
##  [2101] Very Good Good      Good      Premium   Good      Good     
##  [2107] Good      Ideal     Fair      Premium   Ideal     Very Good
##  [2113] Fair      Ideal     Very Good Ideal     Ideal     Premium  
##  [2119] Fair      Fair      Ideal     Very Good Ideal     Good     
##  [2125] Premium   Very Good Premium   Ideal     Very Good Ideal    
##  [2131] Ideal     Good      Very Good Ideal     Ideal     Ideal    
##  [2137] Ideal     Ideal     Very Good Ideal     Very Good Ideal    
##  [2143] Good      Good      Ideal     Premium   Very Good Ideal    
##  [2149] Ideal     Good      Very Good Very Good Ideal     Ideal    
##  [2155] Premium   Ideal     Ideal     Very Good Ideal     Good     
##  [2161] Ideal     Ideal     Ideal     Ideal     Very Good Very Good
##  [2167] Ideal     Ideal     Ideal     Premium   Good      Ideal    
##  [2173] Ideal     Ideal     Very Good Premium   Premium   Fair     
##  [2179] Premium   Ideal     Premium   Ideal     Fair      Fair     
##  [2185] Premium   Ideal     Ideal     Premium   Ideal     Ideal    
##  [2191] Very Good Very Good Very Good Very Good Very Good Good     
##  [2197] Good      Very Good Ideal     Ideal     Ideal     Ideal    
##  [2203] Good      Ideal     Premium   Very Good Premium   Premium  
##  [2209] Ideal     Fair      Ideal     Premium   Fair      Very Good
##  [2215] Ideal     Very Good Good      Very Good Very Good Very Good
##  [2221] Premium   Very Good Ideal     Ideal     Ideal     Ideal    
##  [2227] Fair      Fair      Fair      Fair      Premium   Ideal    
##  [2233] Good      Premium   Very Good Ideal     Very Good Very Good
##  [2239] Ideal     Ideal     Good      Ideal     Fair      Ideal    
##  [2245] Good      Good      Ideal     Good      Good      Fair     
##  [2251] Premium   Very Good Very Good Ideal     Premium   Ideal    
##  [2257] Very Good Ideal     Very Good Good      Ideal     Ideal    
##  [2263] Good      Very Good Very Good Very Good Very Good Ideal    
##  [2269] Ideal     Ideal     Ideal     Ideal     Fair      Ideal    
##  [2275] Premium   Ideal     Premium   Ideal     Very Good Premium  
##  [2281] Ideal     Ideal     Very Good Very Good Good      Very Good
##  [2287] Good      Very Good Ideal     Very Good Very Good Ideal    
##  [2293] Fair      Ideal     Premium   Ideal     Premium   Very Good
##  [2299] Good      Good      Very Good Ideal     Good      Good     
##  [2305] Good      Ideal     Ideal     Ideal     Fair      Ideal    
##  [2311] Premium   Very Good Good      Fair      Premium   Ideal    
##  [2317] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
##  [2323] Fair      Very Good Ideal     Ideal     Very Good Ideal    
##  [2329] Ideal     Very Good Very Good Ideal     Ideal     Ideal    
##  [2335] Premium   Ideal     Fair      Ideal     Ideal     Ideal    
##  [2341] Very Good Ideal     Ideal     Ideal     Ideal     Ideal    
##  [2347] Ideal     Premium   Ideal     Ideal     Ideal     Fair     
##  [2353] Fair      Very Good Premium   Ideal     Fair      Premium  
##  [2359] Very Good Ideal     Ideal     Premium   Premium   Premium  
##  [2365] Good      Fair      Fair      Very Good Good      Ideal    
##  [2371] Ideal     Premium   Ideal     Ideal     Ideal     Very Good
##  [2377] Premium   Very Good Very Good Premium   Premium   Very Good
##  [2383] Very Good Very Good Premium   Good      Premium   Premium  
##  [2389] Premium   Ideal     Ideal     Ideal     Ideal     Ideal    
##  [2395] Very Good Ideal     Premium   Very Good Very Good Ideal    
##  [2401] Ideal     Ideal     Very Good Ideal     Very Good Ideal    
##  [2407] Ideal     Ideal     Ideal     Ideal     Good      Good     
##  [2413] Very Good Very Good Very Good Ideal     Ideal     Ideal    
##  [2419] Fair      Fair      Premium   Very Good Premium   Ideal    
##  [2425] Ideal     Premium   Ideal     Very Good Ideal     Ideal    
##  [2431] Ideal     Very Good Premium   Ideal     Ideal     Premium  
##  [2437] Very Good Ideal     Ideal     Ideal     Ideal     Ideal    
##  [2443] Premium   Fair      Premium   Very Good Good      Very Good
##  [2449] Good      Ideal     Very Good Ideal     Ideal     Ideal    
##  [2455] Ideal     Ideal     Premium   Ideal     Premium   Premium  
##  [2461] Fair      Ideal     Ideal     Ideal     Ideal     Ideal    
##  [2467] Ideal     Ideal     Ideal     Very Good Very Good Ideal    
##  [2473] Ideal     Good      Good      Premium   Fair      Premium  
##  [2479] Ideal     Good      Good      Premium   Good      Premium  
##  [2485] Very Good Ideal     Ideal     Ideal     Good      Very Good
##  [2491] Very Good Ideal     Good      Ideal     Very Good Ideal    
##  [2497] Very Good Fair      Premium   Ideal     Ideal     Ideal    
##  [2503] Ideal     Premium   Fair      Ideal     Ideal     Very Good
##  [2509] Ideal     Ideal     Premium   Very Good Ideal     Ideal    
##  [2515] Good      Ideal     Ideal     Ideal     Ideal     Ideal    
##  [2521] Very Good Good      Very Good Very Good Premium   Ideal    
##  [2527] Good      Very Good Premium   Ideal     Ideal     Fair     
##  [2533] Premium   Fair      Fair      Fair      Fair      Fair     
##  [2539] Premium   Premium   Premium   Ideal     Very Good Ideal    
##  [2545] Ideal     Ideal     Good      Very Good Ideal     Ideal    
##  [2551] Very Good Very Good Very Good Very Good Very Good Very Good
##  [2557] Very Good Ideal     Ideal     Ideal     Premium   Ideal    
##  [2563] Ideal     Premium   Good      Premium   Good      Fair     
##  [2569] Very Good Premium   Ideal     Premium   Premium   Ideal    
##  [2575] Ideal     Ideal     Ideal     Ideal     Premium   Ideal    
##  [2581] Premium   Good      Ideal     Fair      Premium   Premium  
##  [2587] Ideal     Very Good Premium   Ideal     Very Good Very Good
##  [2593] Very Good Ideal     Ideal     Premium   Ideal     Very Good
##  [2599] Good      Ideal     Very Good Very Good Very Good Very Good
##  [2605] Ideal     Ideal     Very Good Very Good Very Good Ideal    
##  [2611] Premium   Very Good Ideal     Ideal     Ideal     Premium  
##  [2617] Premium   Very Good Good      Premium   Very Good Ideal    
##  [2623] Very Good Premium   Good      Fair      Ideal     Ideal    
##  [2629] Very Good Ideal     Premium   Very Good Premium   Ideal    
##  [2635] Ideal     Ideal     Ideal     Ideal     Premium   Good     
##  [2641] Premium   Ideal     Premium   Good      Ideal     Very Good
##  [2647] Very Good Ideal     Very Good Good      Ideal     Very Good
##  [2653] Ideal     Ideal     Very Good Ideal     Premium   Premium  
##  [2659] Very Good Very Good Ideal     Very Good Ideal     Ideal    
##  [2665] Ideal     Premium   Premium   Very Good Very Good Very Good
##  [2671] Very Good Ideal     Ideal     Ideal     Good      Premium  
##  [2677] Very Good Ideal     Very Good Very Good Very Good Ideal    
##  [2683] Ideal     Very Good Very Good Very Good Ideal     Ideal    
##  [2689] Good      Ideal     Ideal     Good      Premium   Good     
##  [2695] Good      Very Good Good      Premium   Fair      Fair     
##  [2701] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
##  [2707] Ideal     Very Good Very Good Ideal     Ideal     Fair     
##  [2713] Very Good Ideal     Ideal     Ideal     Ideal     Ideal    
##  [2719] Good      Good      Ideal     Very Good Very Good Very Good
##  [2725] Good      Ideal     Ideal     Ideal     Ideal     Ideal    
##  [2731] Good      Premium   Ideal     Very Good Very Good Ideal    
##  [2737] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
##  [2743] Ideal     Ideal     Ideal     Fair      Very Good Ideal    
##  [2749] Good      Ideal     Ideal     Good      Good      Very Good
##  [2755] Very Good Premium   Good      Fair      Good      Premium  
##  [2761] Premium   Very Good Ideal     Ideal     Ideal     Ideal    
##  [2767] Ideal     Premium   Ideal     Ideal     Very Good Ideal    
##  [2773] Ideal     Ideal     Ideal     Good      Good      Ideal    
##  [2779] Ideal     Ideal     Ideal     Very Good Ideal     Ideal    
##  [2785] Very Good Ideal     Ideal     Ideal     Ideal     Ideal    
##  [2791] Very Good Very Good Fair      Ideal     Good      Very Good
##  [2797] Premium   Very Good Very Good Ideal     Good      Premium  
##  [2803] Very Good Ideal     Premium   Ideal     Premium   Very Good
##  [2809] Ideal     Ideal     Ideal     Premium   Very Good Good     
##  [2815] Ideal     Ideal     Fair      Very Good Good      Fair     
##  [2821] Fair      Ideal     Premium   Premium   Premium   Premium  
##  [2827] Good      Good      Good      Good      Ideal     Good     
##  [2833] Very Good Very Good Ideal     Ideal     Very Good Ideal    
##  [2839] Premium   Good      Very Good Very Good Very Good Premium  
##  [2845] Ideal     Ideal     Ideal     Ideal     Very Good Premium  
##  [2851] Ideal     Ideal     Very Good Ideal     Good      Good     
##  [2857] Very Good Good      Ideal     Ideal     Ideal     Very Good
##  [2863] Ideal     Ideal     Ideal     Ideal     Very Good Premium  
##  [2869] Very Good Very Good Very Good Premium   Very Good Fair     
##  [2875] Premium   Ideal     Ideal     Ideal     Good      Good     
##  [2881] Ideal     Very Good Fair      Ideal     Fair      Very Good
##  [2887] Ideal     Ideal     Very Good Good      Fair      Premium  
##  [2893] Very Good Fair      Good      Good      Very Good Fair     
##  [2899] Premium   Premium   Very Good Ideal     Ideal     Ideal    
##  [2905] Ideal     Good      Good      Ideal     Ideal     Ideal    
##  [2911] Good      Premium   Premium   Fair      Premium   Good     
##  [2917] Very Good Good      Very Good Ideal     Ideal     Ideal    
##  [2923] Good      Premium   Very Good Very Good Fair      Very Good
##  [2929] Very Good Good      Ideal     Ideal     Ideal     Good     
##  [2935] Fair      Premium   Good      Premium   Premium   Premium  
##  [2941] Very Good Very Good Very Good Very Good Ideal     Premium  
##  [2947] Premium   Ideal     Fair      Ideal     Premium   Premium  
##  [2953] Very Good Very Good Very Good Ideal     Premium   Premium  
##  [2959] Ideal     Fair      Very Good Ideal     Premium   Ideal    
##  [2965] Premium   Premium   Good      Very Good Good      Very Good
##  [2971] Ideal     Very Good Very Good Ideal     Ideal     Ideal    
##  [2977] Ideal     Ideal     Ideal     Ideal     Premium   Fair     
##  [2983] Very Good Premium   Ideal     Ideal     Very Good Ideal    
##  [2989] Ideal     Ideal     Good      Fair      Very Good Premium  
##  [2995] Good      Good      Good      Good      Very Good Good     
##  [3001] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
##  [3007] Ideal     Good      Fair      Fair      Very Good Good     
##  [3013] Ideal     Ideal     Ideal     Very Good Very Good Good     
##  [3019] Ideal     Ideal     Ideal     Very Good Ideal     Premium  
##  [3025] Very Good Very Good Ideal     Ideal     Ideal     Very Good
##  [3031] Ideal     Ideal     Very Good Premium   Good      Ideal    
##  [3037] Good      Good      Good      Very Good Good      Very Good
##  [3043] Ideal     Very Good Very Good Very Good Very Good Very Good
##  [3049] Very Good Very Good Ideal     Ideal     Ideal     Ideal    
##  [3055] Premium   Premium   Very Good Very Good Very Good Premium  
##  [3061] Good      Ideal     Ideal     Premium   Ideal     Premium  
##  [3067] Premium   Premium   Premium   Premium   Good      Premium  
##  [3073] Good      Premium   Good      Ideal     Premium   Premium  
##  [3079] Good      Premium   Ideal     Ideal     Premium   Very Good
##  [3085] Very Good Very Good Ideal     Ideal     Good      Ideal    
##  [3091] Premium   Very Good Good      Good      Ideal     Premium  
##  [3097] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
##  [3103] Ideal     Ideal     Very Good Ideal     Premium   Very Good
##  [3109] Very Good Very Good Ideal     Fair      Very Good Ideal    
##  [3115] Good      Fair      Good      Very Good Ideal     Ideal    
##  [3121] Ideal     Ideal     Ideal     Ideal     Premium   Ideal    
##  [3127] Fair      Premium   Ideal     Premium   Premium   Ideal    
##  [3133] Ideal     Ideal     Very Good Ideal     Ideal     Ideal    
##  [3139] Ideal     Ideal     Ideal     Ideal     Ideal     Very Good
##  [3145] Very Good Ideal     Ideal     Ideal     Fair      Good     
##  [3151] Ideal     Fair      Very Good Very Good Premium   Very Good
##  [3157] Very Good Very Good Ideal     Ideal     Very Good Good     
##  [3163] Ideal     Premium   Ideal     Ideal     Ideal     Ideal    
##  [3169] Premium   Very Good Fair      Premium   Very Good Very Good
##  [3175] Premium   Ideal     Ideal     Very Good Good      Very Good
##  [3181] Good      Good      Good      Ideal     Good      Ideal    
##  [3187] Ideal     Ideal     Ideal     Very Good Fair      Premium  
##  [3193] Ideal     Premium   Ideal     Very Good Premium   Good     
##  [3199] Ideal     Very Good Ideal     Ideal     Ideal     Premium  
##  [3205] Ideal     Ideal     Ideal     Premium   Premium   Premium  
##  [3211] Premium   Premium   Ideal     Good      Fair      Premium  
##  [3217] Good      Good      Premium   Very Good Very Good Good     
##  [3223] Very Good Very Good Ideal     Ideal     Ideal     Ideal    
##  [3229] Ideal     Ideal     Premium   Premium   Premium   Very Good
##  [3235] Very Good Ideal     Good      Good      Fair      Ideal    
##  [3241] Very Good Good      Ideal     Ideal     Ideal     Ideal    
##  [3247] Premium   Ideal     Premium   Very Good Premium   Ideal    
##  [3253] Ideal     Good      Ideal     Good      Premium   Very Good
##  [3259] Very Good Very Good Fair      Ideal     Very Good Ideal    
##  [3265] Premium   Ideal     Ideal     Ideal     Premium   Premium  
##  [3271] Good      Very Good Ideal     Premium   Premium   Premium  
##  [3277] Very Good Ideal     Ideal     Ideal     Ideal     Premium  
##  [3283] Ideal     Ideal     Good      Ideal     Premium   Good     
##  [3289] Premium   Ideal     Premium   Very Good Very Good Very Good
##  [3295] Good      Premium   Very Good Ideal     Very Good Very Good
##  [3301] Very Good Very Good Premium   Good      Very Good Ideal    
##  [3307] Premium   Ideal     Very Good Good      Ideal     Very Good
##  [3313] Very Good Ideal     Ideal     Premium   Premium   Ideal    
##  [3319] Ideal     Ideal     Very Good Ideal     Ideal     Very Good
##  [3325] Ideal     Very Good Very Good Fair      Premium   Very Good
##  [3331] Good      Premium   Ideal     Ideal     Premium   Ideal    
##  [3337] Ideal     Premium   Premium   Premium   Premium   Premium  
##  [3343] Good      Very Good Premium   Ideal     Ideal     Very Good
##  [3349] Very Good Premium   Premium   Ideal     Good      Ideal    
##  [3355] Premium   Very Good Ideal     Good      Very Good Ideal    
##  [3361] Very Good Ideal     Very Good Very Good Very Good Very Good
##  [3367] Very Good Very Good Very Good Ideal     Ideal     Ideal    
##  [3373] Ideal     Ideal     Ideal     Ideal     Ideal     Very Good
##  [3379] Good      Premium   Ideal     Premium   Very Good Very Good
##  [3385] Good      Very Good Premium   Premium   Very Good Very Good
##  [3391] Very Good Very Good Very Good Very Good Very Good Ideal    
##  [3397] Ideal     Ideal     Ideal     Very Good Premium   Premium  
##  [3403] Premium   Good      Good      Good      Good      Ideal    
##  [3409] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
##  [3415] Ideal     Premium   Very Good Good      Very Good Premium  
##  [3421] Fair      Very Good Ideal     Ideal     Ideal     Ideal    
##  [3427] Ideal     Fair      Premium   Ideal     Very Good Premium  
##  [3433] Premium   Ideal     Very Good Very Good Premium   Premium  
##  [3439] Premium   Very Good Premium   Fair      Fair      Ideal    
##  [3445] Very Good Premium   Fair      Ideal     Very Good Very Good
##  [3451] Very Good Premium   Ideal     Premium   Very Good Ideal    
##  [3457] Ideal     Premium   Ideal     Good      Very Good Ideal    
##  [3463] Ideal     Premium   Very Good Premium   Ideal     Premium  
##  [3469] Premium   Good      Good      Premium   Very Good Very Good
##  [3475] Premium   Good      Premium   Premium   Very Good Ideal    
##  [3481] Ideal     Ideal     Very Good Good      Premium   Very Good
##  [3487] Premium   Very Good Good      Good      Premium   Ideal    
##  [3493] Good      Good      Good      Ideal     Ideal     Premium  
##  [3499] Good      Premium   Ideal     Premium   Good      Good     
##  [3505] Very Good Ideal     Ideal     Ideal     Premium   Ideal    
##  [3511] Fair      Premium   Ideal     Premium   Very Good Ideal    
##  [3517] Ideal     Ideal     Ideal     Very Good Very Good Ideal    
##  [3523] Premium   Very Good Ideal     Very Good Very Good Premium  
##  [3529] Fair      Good      Premium   Good      Very Good Premium  
##  [3535] Ideal     Ideal     Ideal     Premium   Very Good Ideal    
##  [3541] Good      Premium   Ideal     Ideal     Very Good Ideal    
##  [3547] Ideal     Premium   Very Good Ideal     Very Good Good     
##  [3553] Good      Premium   Good      Very Good Very Good Ideal    
##  [3559] Ideal     Ideal     Premium   Very Good Ideal     Good     
##  [3565] Fair      Very Good Very Good Ideal     Ideal     Ideal    
##  [3571] Good      Good      Very Good Good      Very Good Very Good
##  [3577] Very Good Ideal     Ideal     Very Good Premium   Ideal    
##  [3583] Ideal     Ideal     Ideal     Ideal     Good      Good     
##  [3589] Fair      Good      Premium   Premium   Ideal     Ideal    
##  [3595] Good      Good      Good      Ideal     Good      Premium  
##  [3601] Very Good Ideal     Good      Good      Good      Fair     
##  [3607] Very Good Ideal     Good      Premium   Good      Ideal    
##  [3613] Ideal     Good      Ideal     Very Good Premium   Premium  
##  [3619] Very Good Very Good Premium   Ideal     Good      Ideal    
##  [3625] Ideal     Ideal     Premium   Premium   Ideal     Ideal    
##  [3631] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
##  [3637] Very Good Ideal     Very Good Ideal     Ideal     Good     
##  [3643] Very Good Very Good Very Good Premium   Fair      Premium  
##  [3649] Premium   Very Good Very Good Ideal     Ideal     Ideal    
##  [3655] Ideal     Ideal     Premium   Very Good Ideal     Ideal    
##  [3661] Ideal     Very Good Good      Ideal     Fair      Fair     
##  [3667] Very Good Ideal     Very Good Premium   Premium   Premium  
##  [3673] Very Good Very Good Premium   Very Good Ideal     Good     
##  [3679] Very Good Premium   Ideal     Ideal     Ideal     Ideal    
##  [3685] Ideal     Very Good Very Good Good      Good      Very Good
##  [3691] Very Good Very Good Premium   Good      Premium   Ideal    
##  [3697] Very Good Very Good Very Good Good      Premium   Good     
##  [3703] Ideal     Ideal     Ideal     Premium   Ideal     Premium  
##  [3709] Premium   Premium   Ideal     Ideal     Very Good Very Good
##  [3715] Very Good Ideal     Ideal     Premium   Good      Ideal    
##  [3721] Very Good Good      Ideal     Good      Ideal     Ideal    
##  [3727] Good      Ideal     Good      Very Good Very Good Very Good
##  [3733] Ideal     Very Good Ideal     Ideal     Ideal     Ideal    
##  [3739] Ideal     Good      Good      Premium   Ideal     Ideal    
##  [3745] Premium   Very Good Ideal     Ideal     Premium   Very Good
##  [3751] Ideal     Ideal     Ideal     Good      Very Good Premium  
##  [3757] Ideal     Ideal     Ideal     Ideal     Premium   Ideal    
##  [3763] Premium   Premium   Very Good Very Good Premium   Premium  
##  [3769] Very Good Ideal     Premium   Ideal     Premium   Very Good
##  [3775] Very Good Ideal     Very Good Fair      Premium   Ideal    
##  [3781] Premium   Premium   Good      Ideal     Premium   Ideal    
##  [3787] Very Good Very Good Ideal     Ideal     Very Good Ideal    
##  [3793] Good      Very Good Very Good Very Good Ideal     Ideal    
##  [3799] Good      Good      Ideal     Very Good Ideal     Very Good
##  [3805] Very Good Ideal     Good      Good      Good      Good     
##  [3811] Good      Ideal     Very Good Ideal     Ideal     Ideal    
##  [3817] Ideal     Ideal     Premium   Premium   Ideal     Good     
##  [3823] Premium   Very Good Very Good Ideal     Ideal     Ideal    
##  [3829] Good      Ideal     Premium   Ideal     Very Good Very Good
##  [3835] Good      Premium   Fair      Very Good Good      Fair     
##  [3841] Premium   Fair      Very Good Premium   Premium   Ideal    
##  [3847] Ideal     Very Good Very Good Good      Fair      Very Good
##  [3853] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
##  [3859] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
##  [3865] Premium   Very Good Premium   Premium   Very Good Premium  
##  [3871] Ideal     Very Good Very Good Ideal     Premium   Very Good
##  [3877] Ideal     Fair      Very Good Very Good Premium   Very Good
##  [3883] Good      Good      Premium   Good      Good      Premium  
##  [3889] Premium   Ideal     Ideal     Very Good Very Good Very Good
##  [3895] Very Good Good      Very Good Very Good Premium   Ideal    
##  [3901] Ideal     Good      Premium   Very Good Very Good Ideal    
##  [3907] Very Good Ideal     Very Good Premium   Ideal     Ideal    
##  [3913] Premium   Very Good Ideal     Ideal     Ideal     Premium  
##  [3919] Premium   Premium   Ideal     Very Good Very Good Ideal    
##  [3925] Ideal     Ideal     Good      Very Good Ideal     Premium  
##  [3931] Premium   Premium   Very Good Ideal     Ideal     Very Good
##  [3937] Good      Ideal     Ideal     Fair      Ideal     Premium  
##  [3943] Very Good Ideal     Ideal     Ideal     Ideal     Premium  
##  [3949] Very Good Ideal     Premium   Ideal     Ideal     Ideal    
##  [3955] Ideal     Fair      Ideal     Good      Good      Premium  
##  [3961] Very Good Premium   Very Good Premium   Premium   Premium  
##  [3967] Ideal     Very Good Premium   Very Good Fair      Good     
##  [3973] Ideal     Good      Very Good Very Good Ideal     Very Good
##  [3979] Premium   Premium   Fair      Fair      Good      Premium  
##  [3985] Good      Very Good Premium   Premium   Ideal     Premium  
##  [3991] Ideal     Very Good Ideal     Ideal     Ideal     Ideal    
##  [3997] Ideal     Very Good Ideal     Ideal     Ideal     Very Good
##  [4003] Ideal     Ideal     Very Good Very Good Very Good Very Good
##  [4009] Very Good Good      Very Good Very Good Good      Very Good
##  [4015] Premium   Ideal     Good      Good      Ideal     Ideal    
##  [4021] Ideal     Ideal     Ideal     Good      Ideal     Ideal    
##  [4027] Good      Premium   Ideal     Ideal     Very Good Ideal    
##  [4033] Very Good Premium   Ideal     Very Good Ideal     Good     
##  [4039] Premium   Good      Premium   Ideal     Premium   Ideal    
##  [4045] Ideal     Premium   Ideal     Fair      Ideal     Fair     
##  [4051] Very Good Ideal     Ideal     Ideal     Ideal     Ideal    
##  [4057] Ideal     Ideal     Ideal     Ideal     Good      Very Good
##  [4063] Premium   Good      Ideal     Premium   Good      Ideal    
##  [4069] Ideal     Very Good Ideal     Ideal     Good      Ideal    
##  [4075] Ideal     Very Good Ideal     Good      Ideal     Good     
##  [4081] Very Good Ideal     Ideal     Ideal     Good      Ideal    
##  [4087] Fair      Very Good Very Good Very Good Good      Very Good
##  [4093] Good      Very Good Very Good Very Good Very Good Very Good
##  [4099] Ideal     Ideal     Premium   Very Good Ideal     Good     
##  [4105] Good      Fair      Ideal     Ideal     Ideal     Ideal    
##  [4111] Good      Fair      Good      Fair      Premium   Ideal    
##  [4117] Premium   Very Good Premium   Very Good Good      Good     
##  [4123] Ideal     Ideal     Fair      Very Good Fair      Good     
##  [4129] Premium   Ideal     Very Good Very Good Ideal     Ideal    
##  [4135] Ideal     Very Good Very Good Ideal     Ideal     Ideal    
##  [4141] Premium   Premium   Premium   Ideal     Fair      Premium  
##  [4147] Premium   Good      Very Good Premium   Premium   Fair     
##  [4153] Ideal     Premium   Ideal     Ideal     Ideal     Good     
##  [4159] Very Good Ideal     Very Good Ideal     Premium   Very Good
##  [4165] Very Good Ideal     Ideal     Premium   Ideal     Premium  
##  [4171] Ideal     Premium   Premium   Ideal     Good      Premium  
##  [4177] Premium   Premium   Premium   Very Good Premium   Premium  
##  [4183] Premium   Ideal     Ideal     Ideal     Ideal     Very Good
##  [4189] Ideal     Ideal     Good      Very Good Premium   Ideal    
##  [4195] Ideal     Very Good Very Good Very Good Premium   Very Good
##  [4201] Premium   Premium   Premium   Very Good Premium   Ideal    
##  [4207] Ideal     Fair      Very Good Premium   Ideal     Very Good
##  [4213] Good      Very Good Very Good Premium   Premium   Ideal    
##  [4219] Very Good Very Good Good      Ideal     Good      Good     
##  [4225] Ideal     Ideal     Very Good Ideal     Ideal     Premium  
##  [4231] Very Good Premium   Very Good Good      Good      Ideal    
##  [4237] Premium   Very Good Fair      Very Good Ideal     Ideal    
##  [4243] Ideal     Good      Good      Very Good Premium   Ideal    
##  [4249] Premium   Ideal     Good      Premium   Very Good Premium  
##  [4255] Premium   Premium   Very Good Very Good Very Good Premium  
##  [4261] Good      Very Good Premium   Very Good Very Good Ideal    
##  [4267] Good      Ideal     Premium   Ideal     Ideal     Good     
##  [4273] Premium   Fair      Very Good Premium   Ideal     Good     
##  [4279] Ideal     Good      Premium   Fair      Premium   Premium  
##  [4285] Ideal     Premium   Ideal     Ideal     Premium   Very Good
##  [4291] Ideal     Premium   Ideal     Ideal     Ideal     Premium  
##  [4297] Ideal     Very Good Ideal     Ideal     Very Good Premium  
##  [4303] Premium   Very Good Very Good Very Good Very Good Fair     
##  [4309] Premium   Ideal     Premium   Very Good Very Good Ideal    
##  [4315] Ideal     Ideal     Good      Good      Good      Good     
##  [4321] Very Good Very Good Fair      Ideal     Good      Fair     
##  [4327] Fair      Fair      Premium   Premium   Ideal     Premium  
##  [4333] Premium   Premium   Premium   Ideal     Ideal     Very Good
##  [4339] Premium   Good      Ideal     Very Good Very Good Very Good
##  [4345] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
##  [4351] Premium   Ideal     Premium   Ideal     Very Good Premium  
##  [4357] Very Good Very Good Premium   Premium   Premium   Good     
##  [4363] Premium   Good      Very Good Very Good Very Good Ideal    
##  [4369] Ideal     Ideal     Premium   Very Good Ideal     Very Good
##  [4375] Premium   Good      Ideal     Premium   Ideal     Premium  
##  [4381] Very Good Very Good Ideal     Very Good Ideal     Premium  
##  [4387] Good      Very Good Ideal     Ideal     Very Good Very Good
##  [4393] Ideal     Premium   Good      Very Good Good      Ideal    
##  [4399] Ideal     Very Good Ideal     Ideal     Ideal     Premium  
##  [4405] Ideal     Good      Good      Ideal     Ideal     Ideal    
##  [4411] Good      Very Good Very Good Ideal     Ideal     Good     
##  [4417] Ideal     Good      Premium   Very Good Ideal     Premium  
##  [4423] Very Good Ideal     Fair      Ideal     Very Good Good     
##  [4429] Ideal     Very Good Very Good Very Good Fair      Ideal    
##  [4435] Good      Good      Ideal     Ideal     Very Good Good     
##  [4441] Good      Premium   Fair      Premium   Premium   Very Good
##  [4447] Very Good Ideal     Ideal     Premium   Premium   Ideal    
##  [4453] Premium   Very Good Ideal     Very Good Good      Good     
##  [4459] Premium   Premium   Premium   Premium   Ideal     Very Good
##  [4465] Good      Very Good Very Good Very Good Ideal     Ideal    
##  [4471] Ideal     Ideal     Fair      Very Good Ideal     Premium  
##  [4477] Very Good Very Good Ideal     Ideal     Ideal     Very Good
##  [4483] Ideal     Premium   Premium   Ideal     Good      Very Good
##  [4489] Good      Good      Good      Premium   Ideal     Ideal    
##  [4495] Ideal     Ideal     Fair      Ideal     Premium   Premium  
##  [4501] Premium   Ideal     Premium   Very Good Ideal     Fair     
##  [4507] Very Good Very Good Good      Ideal     Good      Fair     
##  [4513] Fair      Good      Very Good Premium   Fair      Good     
##  [4519] Fair      Premium   Premium   Premium   Good      Ideal    
##  [4525] Ideal     Ideal     Ideal     Ideal     Ideal     Very Good
##  [4531] Good      Premium   Premium   Good      Premium   Very Good
##  [4537] Very Good Fair      Premium   Good      Premium   Good     
##  [4543] Good      Very Good Premium   Premium   Good      Very Good
##  [4549] Ideal     Premium   Ideal     Very Good Very Good Very Good
##  [4555] Very Good Very Good Premium   Good      Ideal     Very Good
##  [4561] Very Good Ideal     Premium   Premium   Premium   Good     
##  [4567] Good      Premium   Ideal     Ideal     Ideal     Ideal    
##  [4573] Ideal     Premium   Ideal     Good      Very Good Very Good
##  [4579] Ideal     Premium   Very Good Premium   Good      Ideal    
##  [4585] Premium   Very Good Very Good Ideal     Very Good Good     
##  [4591] Ideal     Premium   Very Good Premium   Very Good Ideal    
##  [4597] Ideal     Ideal     Good      Ideal     Premium   Fair     
##  [4603] Very Good Good      Ideal     Good      Good      Premium  
##  [4609] Premium   Very Good Premium   Premium   Very Good Ideal    
##  [4615] Ideal     Very Good Good      Premium   Very Good Very Good
##  [4621] Ideal     Premium   Ideal     Premium   Ideal     Very Good
##  [4627] Premium   Good      Very Good Good      Ideal     Very Good
##  [4633] Very Good Premium   Ideal     Very Good Very Good Premium  
##  [4639] Very Good Ideal     Good      Good      Premium   Premium  
##  [4645] Good      Premium   Very Good Good      Good      Good     
##  [4651] Good      Very Good Premium   Premium   Fair      Ideal    
##  [4657] Ideal     Premium   Fair      Premium   Ideal     Premium  
##  [4663] Ideal     Premium   Very Good Ideal     Ideal     Premium  
##  [4669] Fair      Good      Ideal     Ideal     Very Good Premium  
##  [4675] Very Good Good      Premium   Premium   Ideal     Very Good
##  [4681] Ideal     Premium   Ideal     Ideal     Fair      Premium  
##  [4687] Ideal     Premium   Very Good Premium   Premium   Ideal    
##  [4693] Premium   Good      Premium   Fair      Premium   Very Good
##  [4699] Good      Ideal     Ideal     Ideal     Ideal     Very Good
##  [4705] Very Good Ideal     Very Good Good      Ideal     Premium  
##  [4711] Very Good Premium   Good      Ideal     Very Good Very Good
##  [4717] Very Good Ideal     Ideal     Ideal     Ideal     Ideal    
##  [4723] Premium   Ideal     Ideal     Premium   Premium   Very Good
##  [4729] Good      Very Good Ideal     Ideal     Ideal     Ideal    
##  [4735] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
##  [4741] Premium   Ideal     Very Good Very Good Premium   Ideal    
##  [4747] Ideal     Premium   Very Good Ideal     Premium   Premium  
##  [4753] Premium   Ideal     Very Good Ideal     Fair      Premium  
##  [4759] Very Good Very Good Good      Fair      Fair      Good     
##  [4765] Good      Premium   Premium   Good      Ideal     Good     
##  [4771] Premium   Ideal     Very Good Very Good Very Good Ideal    
##  [4777] Premium   Very Good Ideal     Fair      Ideal     Ideal    
##  [4783] Premium   Ideal     Good      Good      Very Good Good     
##  [4789] Fair      Premium   Good      Premium   Premium   Premium  
##  [4795] Good      Fair      Ideal     Ideal     Very Good Ideal    
##  [4801] Fair      Premium   Premium   Good      Very Good Very Good
##  [4807] Good      Fair      Very Good Premium   Premium   Good     
##  [4813] Premium   Ideal     Ideal     Good      Ideal     Premium  
##  [4819] Very Good Very Good Premium   Ideal     Premium   Premium  
##  [4825] Premium   Premium   Premium   Premium   Premium   Very Good
##  [4831] Very Good Ideal     Very Good Very Good Ideal     Ideal    
##  [4837] Very Good Very Good Very Good Very Good Very Good Premium  
##  [4843] Ideal     Very Good Ideal     Ideal     Ideal     Ideal    
##  [4849] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
##  [4855] Ideal     Ideal     Ideal     Ideal     Very Good Premium  
##  [4861] Premium   Premium   Ideal     Premium   Ideal     Very Good
##  [4867] Ideal     Premium   Premium   Good      Premium   Very Good
##  [4873] Very Good Premium   Ideal     Ideal     Premium   Ideal    
##  [4879] Very Good Very Good Ideal     Ideal     Ideal     Good     
##  [4885] Premium   Good      Very Good Very Good Ideal     Good     
##  [4891] Premium   Ideal     Very Good Very Good Very Good Very Good
##  [4897] Very Good Premium   Ideal     Premium   Ideal     Ideal    
##  [4903] Very Good Ideal     Ideal     Good      Premium   Premium  
##  [4909] Very Good Very Good Very Good Good      Very Good Premium  
##  [4915] Ideal     Good      Premium   Very Good Ideal     Fair     
##  [4921] Good      Very Good Premium   Ideal     Good      Very Good
##  [4927] Ideal     Very Good Ideal     Very Good Very Good Good     
##  [4933] Fair      Fair      Good      Premium   Fair      Premium  
##  [4939] Very Good Ideal     Premium   Ideal     Ideal     Ideal    
##  [4945] Ideal     Premium   Ideal     Premium   Good      Good     
##  [4951] Good      Premium   Ideal     Fair      Premium   Good     
##  [4957] Good      Premium   Good      Very Good Good      Good     
##  [4963] Very Good Good      Good      Good      Very Good Very Good
##  [4969] Good      Premium   Fair      Fair      Ideal     Good     
##  [4975] Premium   Ideal     Ideal     Good      Very Good Good     
##  [4981] Ideal     Very Good Ideal     Ideal     Ideal     Very Good
##  [4987] Good      Ideal     Fair      Ideal     Premium   Premium  
##  [4993] Good      Premium   Good      Ideal     Ideal     Good     
##  [4999] Good      Very Good Premium   Very Good Premium   Ideal    
##  [5005] Good      Fair      Fair      Good      Very Good Good     
##  [5011] Very Good Ideal     Premium   Ideal     Fair      Premium  
##  [5017] Very Good Ideal     Very Good Premium   Premium   Very Good
##  [5023] Ideal     Ideal     Ideal     Good      Very Good Ideal    
##  [5029] Premium   Very Good Premium   Ideal     Ideal     Fair     
##  [5035] Premium   Premium   Premium   Premium   Good      Premium  
##  [5041] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
##  [5047] Ideal     Premium   Premium   Premium   Good      Premium  
##  [5053] Premium   Ideal     Premium   Very Good Ideal     Ideal    
##  [5059] Ideal     Premium   Very Good Premium   Premium   Premium  
##  [5065] Premium   Premium   Good      Premium   Premium   Premium  
##  [5071] Ideal     Good      Fair      Premium   Fair      Premium  
##  [5077] Fair      Very Good Premium   Premium   Ideal     Fair     
##  [5083] Very Good Very Good Ideal     Good      Good      Premium  
##  [5089] Very Good Fair      Premium   Good      Good      Ideal    
##  [5095] Very Good Very Good Ideal     Ideal     Very Good Ideal    
##  [5101] Good      Fair      Ideal     Premium   Ideal     Ideal    
##  [5107] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
##  [5113] Ideal     Ideal     Very Good Ideal     Ideal     Ideal    
##  [5119] Premium   Very Good Very Good Ideal     Very Good Very Good
##  [5125] Very Good Ideal     Ideal     Ideal     Ideal     Good     
##  [5131] Fair      Good      Fair      Premium   Premium   Good     
##  [5137] Ideal     Ideal     Premium   Fair      Very Good Premium  
##  [5143] Fair      Very Good Very Good Fair      Fair      Very Good
##  [5149] Ideal     Premium   Premium   Fair      Good      Good     
##  [5155] Premium   Premium   Very Good Premium   Good      Good     
##  [5161] Very Good Very Good Premium   Very Good Very Good Ideal    
##  [5167] Good      Very Good Very Good Very Good Ideal     Ideal    
##  [5173] Good      Ideal     Premium   Premium   Very Good Very Good
##  [5179] Premium   Premium   Ideal     Ideal     Ideal     Premium  
##  [5185] Premium   Good      Ideal     Premium   Ideal     Good     
##  [5191] Ideal     Premium   Very Good Good      Ideal     Ideal    
##  [5197] Ideal     Premium   Good      Good      Very Good Ideal    
##  [5203] Very Good Premium   Good      Premium   Premium   Fair     
##  [5209] Premium   Ideal     Premium   Ideal     Premium   Premium  
##  [5215] Premium   Fair      Premium   Ideal     Premium   Good     
##  [5221] Premium   Premium   Premium   Very Good Ideal     Ideal    
##  [5227] Ideal     Good      Ideal     Good      Ideal     Ideal    
##  [5233] Very Good Very Good Very Good Premium   Ideal     Fair     
##  [5239] Fair      Good      Premium   Premium   Ideal     Ideal    
##  [5245] Ideal     Very Good Very Good Very Good Premium   Premium  
##  [5251] Premium   Premium   Very Good Ideal     Very Good Very Good
##  [5257] Good      Premium   Very Good Ideal     Ideal     Very Good
##  [5263] Ideal     Very Good Ideal     Very Good Good      Good     
##  [5269] Very Good Very Good Good      Very Good Ideal     Ideal    
##  [5275] Ideal     Ideal     Ideal     Very Good Very Good Very Good
##  [5281] Ideal     Ideal     Good      Very Good Good      Ideal    
##  [5287] Very Good Ideal     Very Good Ideal     Ideal     Premium  
##  [5293] Very Good Good      Premium   Good      Premium   Ideal    
##  [5299] Good      Fair      Ideal     Fair      Premium   Very Good
##  [5305] Fair      Premium   Ideal     Very Good Premium   Premium  
##  [5311] Premium   Premium   Ideal     Ideal     Ideal     Premium  
##  [5317] Ideal     Ideal     Premium   Good      Very Good Good     
##  [5323] Very Good Fair      Ideal     Good      Very Good Premium  
##  [5329] Ideal     Premium   Premium   Very Good Very Good Ideal    
##  [5335] Very Good Very Good Premium   Premium   Premium   Ideal    
##  [5341] Ideal     Good      Ideal     Ideal     Ideal     Ideal    
##  [5347] Premium   Premium   Very Good Fair      Good      Premium  
##  [5353] Ideal     Fair      Very Good Very Good Premium   Very Good
##  [5359] Fair      Ideal     Good      Good      Fair      Fair     
##  [5365] Premium   Fair      Very Good Very Good Ideal     Very Good
##  [5371] Very Good Ideal     Premium   Premium   Premium   Premium  
##  [5377] Good      Premium   Premium   Premium   Premium   Premium  
##  [5383] Premium   Very Good Premium   Ideal     Premium   Premium  
##  [5389] Ideal     Very Good Premium   Premium   Premium   Premium  
##  [5395] Ideal     Premium   Ideal     Premium   Premium   Very Good
##  [5401] Ideal     Premium   Premium   Very Good Premium   Premium  
##  [5407] Good      Fair      Premium   Good      Ideal     Ideal    
##  [5413] Premium   Ideal     Ideal     Premium   Premium   Premium  
##  [5419] Premium   Good      Premium   Ideal     Good      Good     
##  [5425] Very Good Premium   Premium   Ideal     Premium   Ideal    
##  [5431] Ideal     Good      Ideal     Very Good Very Good Ideal    
##  [5437] Premium   Very Good Ideal     Ideal     Very Good Very Good
##  [5443] Premium   Very Good Ideal     Very Good Fair      Premium  
##  [5449] Premium   Premium   Premium   Premium   Very Good Ideal    
##  [5455] Premium   Premium   Very Good Ideal     Very Good Very Good
##  [5461] Fair      Fair      Fair      Very Good Good      Good     
##  [5467] Very Good Ideal     Ideal     Fair      Fair      Premium  
##  [5473] Very Good Good      Ideal     Fair      Good      Premium  
##  [5479] Premium   Premium   Very Good Good      Good      Ideal    
##  [5485] Very Good Very Good Premium   Ideal     Premium   Ideal    
##  [5491] Very Good Very Good Ideal     Good      Good      Premium  
##  [5497] Good      Very Good Good      Ideal     Ideal     Ideal    
##  [5503] Premium   Premium   Good      Ideal     Premium   Premium  
##  [5509] Very Good Fair      Premium   Premium   Very Good Good     
##  [5515] Very Good Ideal     Ideal     Good      Good      Very Good
##  [5521] Premium   Ideal     Premium   Very Good Good      Good     
##  [5527] Good      Ideal     Good      Very Good Premium   Premium  
##  [5533] Very Good Premium   Very Good Premium   Ideal     Ideal    
##  [5539] Good      Fair      Very Good Ideal     Ideal     Fair     
##  [5545] Premium   Premium   Ideal     Premium   Premium   Good     
##  [5551] Ideal     Premium   Premium   Very Good Very Good Very Good
##  [5557] Premium   Good      Ideal     Ideal     Premium   Ideal    
##  [5563] Good      Premium   Premium   Good      Good      Ideal    
##  [5569] Good      Premium   Fair      Premium   Premium   Ideal    
##  [5575] Very Good Fair      Fair      Very Good Very Good Ideal    
##  [5581] Premium   Ideal     Ideal     Ideal     Very Good Fair     
##  [5587] Premium   Ideal     Premium   Good      Good      Ideal    
##  [5593] Ideal     Very Good Fair      Fair      Premium   Premium  
##  [5599] Premium   Very Good Premium   Good      Ideal     Ideal    
##  [5605] Good      Fair      Premium   Very Good Ideal     Very Good
##  [5611] Very Good Ideal     Very Good Ideal     Premium   Ideal    
##  [5617] Ideal     Premium   Good      Very Good Fair      Premium  
##  [5623] Very Good Fair      Premium   Premium   Premium   Premium  
##  [5629] Very Good Very Good Very Good Very Good Good      Good     
##  [5635] Premium   Very Good Very Good Ideal     Premium   Premium  
##  [5641] Ideal     Very Good Good      Very Good Very Good Very Good
##  [5647] Very Good Good      Good      Ideal     Ideal     Ideal    
##  [5653] Fair      Good      Premium   Premium   Very Good Very Good
##  [5659] Premium   Good      Ideal     Good      Ideal     Very Good
##  [5665] Premium   Premium   Very Good Ideal     Premium   Very Good
##  [5671] Ideal     Very Good Good      Good      Ideal     Ideal    
##  [5677] Ideal     Ideal     Very Good Very Good Good      Good     
##  [5683] Very Good Premium   Ideal     Fair      Very Good Very Good
##  [5689] Very Good Very Good Very Good Good      Very Good Very Good
##  [5695] Ideal     Ideal     Premium   Good      Fair      Premium  
##  [5701] Very Good Premium   Premium   Premium   Premium   Premium  
##  [5707] Good      Very Good Very Good Very Good Very Good Very Good
##  [5713] Very Good Very Good Very Good Very Good Very Good Very Good
##  [5719] Very Good Very Good Very Good Very Good Very Good Very Good
##  [5725] Very Good Very Good Very Good Very Good Very Good Very Good
##  [5731] Very Good Very Good Ideal     Good      Good      Very Good
##  [5737] Fair      Premium   Good      Fair      Very Good Good     
##  [5743] Ideal     Good      Ideal     Very Good Good      Very Good
##  [5749] Premium   Very Good Good      Good      Good      Premium  
##  [5755] Good      Ideal     Premium   Premium   Premium   Premium  
##  [5761] Premium   Good      Very Good Premium   Good      Very Good
##  [5767] Very Good Good      Premium   Premium   Premium   Ideal    
##  [5773] Ideal     Good      Premium   Fair      Ideal     Very Good
##  [5779] Ideal     Premium   Premium   Very Good Very Good Premium  
##  [5785] Good      Very Good Very Good Good      Premium   Ideal    
##  [5791] Premium   Very Good Ideal     Good      Very Good Very Good
##  [5797] Very Good Ideal     Good      Very Good Premium   Ideal    
##  [5803] Very Good Very Good Ideal     Good      Good      Very Good
##  [5809] Very Good Premium   Very Good Premium   Very Good Premium  
##  [5815] Ideal     Ideal     Ideal     Ideal     Premium   Very Good
##  [5821] Premium   Ideal     Ideal     Ideal     Premium   Premium  
##  [5827] Fair      Good      Good      Premium   Very Good Premium  
##  [5833] Good      Good      Very Good Ideal     Premium   Premium  
##  [5839] Very Good Very Good Good      Premium   Premium   Premium  
##  [5845] Very Good Fair      Good      Very Good Premium   Premium  
##  [5851] Very Good Good      Good      Good      Premium   Very Good
##  [5857] Premium   Fair      Premium   Good      Ideal     Good     
##  [5863] Premium   Ideal     Ideal     Very Good Premium   Ideal    
##  [5869] Good      Fair      Very Good Premium   Ideal     Very Good
##  [5875] Very Good Good      Good      Ideal     Premium   Ideal    
##  [5881] Good      Very Good Ideal     Very Good Very Good Ideal    
##  [5887] Premium   Good      Ideal     Ideal     Fair      Premium  
##  [5893] Good      Ideal     Fair      Ideal     Premium   Ideal    
##  [5899] Premium   Ideal     Good      Premium   Ideal     Premium  
##  [5905] Very Good Ideal     Good      Ideal     Ideal     Ideal    
##  [5911] Very Good Very Good Premium   Ideal     Good      Ideal    
##  [5917] Premium   Ideal     Premium   Very Good Good      Good     
##  [5923] Good      Ideal     Ideal     Premium   Premium   Ideal    
##  [5929] Ideal     Premium   Premium   Ideal     Premium   Premium  
##  [5935] Fair      Ideal     Very Good Very Good Very Good Ideal    
##  [5941] Good      Premium   Ideal     Good      Good      Very Good
##  [5947] Ideal     Very Good Premium   Ideal     Good      Good     
##  [5953] Premium   Ideal     Good      Good      Very Good Very Good
##  [5959] Good      Very Good Very Good Good      Good      Very Good
##  [5965] Good      Ideal     Very Good Good      Premium   Very Good
##  [5971] Very Good Very Good Very Good Good      Premium   Premium  
##  [5977] Premium   Good      Premium   Fair      Premium   Very Good
##  [5983] Very Good Good      Very Good Premium   Ideal     Ideal    
##  [5989] Very Good Very Good Ideal     Good      Ideal     Ideal    
##  [5995] Very Good Premium   Premium   Very Good Ideal     Premium  
##  [6001] Premium   Good      Premium   Premium   Good      Very Good
##  [6007] Good      Very Good Premium   Very Good Ideal     Premium  
##  [6013] Premium   Ideal     Very Good Very Good Premium   Very Good
##  [6019] Very Good Very Good Fair      Very Good Premium   Good     
##  [6025] Good      Fair      Very Good Very Good Good      Fair     
##  [6031] Very Good Very Good Very Good Very Good Ideal     Ideal    
##  [6037] Ideal     Ideal     Good      Good      Very Good Ideal    
##  [6043] Premium   Ideal     Premium   Ideal     Ideal     Ideal    
##  [6049] Ideal     Premium   Ideal     Ideal     Ideal     Premium  
##  [6055] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
##  [6061] Fair      Premium   Very Good Very Good Very Good Very Good
##  [6067] Ideal     Premium   Premium   Very Good Very Good Premium  
##  [6073] Ideal     Premium   Ideal     Ideal     Very Good Ideal    
##  [6079] Premium   Very Good Good      Premium   Premium   Ideal    
##  [6085] Ideal     Good      Fair      Good      Very Good Very Good
##  [6091] Premium   Very Good Very Good Very Good Ideal     Ideal    
##  [6097] Good      Fair      Premium   Premium   Premium   Premium  
##  [6103] Good      Good      Very Good Very Good Good      Good     
##  [6109] Premium   Ideal     Premium   Ideal     Good      Very Good
##  [6115] Ideal     Ideal     Good      Fair      Very Good Premium  
##  [6121] Ideal     Ideal     Very Good Very Good Premium   Fair     
##  [6127] Premium   Premium   Ideal     Premium   Premium   Ideal    
##  [6133] Premium   Ideal     Very Good Very Good Very Good Premium  
##  [6139] Premium   Ideal     Ideal     Premium   Good      Very Good
##  [6145] Very Good Good      Very Good Premium   Premium   Good     
##  [6151] Ideal     Ideal     Ideal     Ideal     Ideal     Good     
##  [6157] Fair      Ideal     Ideal     Very Good Very Good Very Good
##  [6163] Fair      Good      Good      Fair      Ideal     Premium  
##  [6169] Premium   Very Good Fair      Premium   Very Good Good     
##  [6175] Very Good Ideal     Premium   Very Good Good      Good     
##  [6181] Very Good Premium   Premium   Very Good Ideal     Good     
##  [6187] Very Good Ideal     Premium   Very Good Ideal     Ideal    
##  [6193] Very Good Very Good Very Good Premium   Very Good Ideal    
##  [6199] Very Good Very Good Very Good Premium   Good      Premium  
##  [6205] Premium   Very Good Premium   Good      Premium   Ideal    
##  [6211] Ideal     Very Good Ideal     Ideal     Good      Very Good
##  [6217] Very Good Very Good Very Good Ideal     Very Good Good     
##  [6223] Good      Very Good Very Good Very Good Very Good Very Good
##  [6229] Very Good Good      Fair      Good      Premium   Premium  
##  [6235] Very Good Good      Ideal     Ideal     Ideal     Premium  
##  [6241] Premium   Ideal     Premium   Very Good Ideal     Ideal    
##  [6247] Ideal     Ideal     Good      Very Good Ideal     Very Good
##  [6253] Good      Premium   Premium   Very Good Premium   Ideal    
##  [6259] Ideal     Ideal     Ideal     Good      Good      Premium  
##  [6265] Very Good Ideal     Ideal     Good      Very Good Ideal    
##  [6271] Premium   Very Good Premium   Premium   Premium   Very Good
##  [6277] Good      Ideal     Ideal     Good      Good      Very Good
##  [6283] Premium   Ideal     Premium   Very Good Premium   Premium  
##  [6289] Premium   Good      Ideal     Very Good Very Good Very Good
##  [6295] Ideal     Very Good Premium   Ideal     Ideal     Good     
##  [6301] Premium   Premium   Premium   Premium   Premium   Good     
##  [6307] Premium   Very Good Ideal     Ideal     Very Good Good     
##  [6313] Premium   Premium   Premium   Good      Ideal     Premium  
##  [6319] Ideal     Good      Premium   Very Good Good      Very Good
##  [6325] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
##  [6331] Ideal     Ideal     Very Good Premium   Premium   Premium  
##  [6337] Ideal     Premium   Very Good Ideal     Fair      Fair     
##  [6343] Very Good Premium   Premium   Good      Premium   Good     
##  [6349] Premium   Very Good Ideal     Very Good Very Good Ideal    
##  [6355] Ideal     Ideal     Very Good Ideal     Fair      Premium  
##  [6361] Very Good Premium   Ideal     Ideal     Very Good Premium  
##  [6367] Very Good Premium   Premium   Premium   Very Good Premium  
##  [6373] Ideal     Premium   Premium   Very Good Very Good Premium  
##  [6379] Ideal     Premium   Premium   Ideal     Premium   Premium  
##  [6385] Very Good Premium   Good      Ideal     Very Good Premium  
##  [6391] Premium   Premium   Premium   Very Good Very Good Very Good
##  [6397] Premium   Premium   Premium   Premium   Premium   Fair     
##  [6403] Ideal     Premium   Ideal     Premium   Very Good Very Good
##  [6409] Ideal     Ideal     Premium   Premium   Fair      Very Good
##  [6415] Very Good Very Good Ideal     Ideal     Good      Premium  
##  [6421] Ideal     Ideal     Premium   Ideal     Premium   Premium  
##  [6427] Ideal     Ideal     Premium   Fair      Premium   Ideal    
##  [6433] Ideal     Fair      Fair      Premium   Very Good Premium  
##  [6439] Premium   Very Good Premium   Ideal     Very Good Premium  
##  [6445] Ideal     Good      Fair      Premium   Ideal     Very Good
##  [6451] Ideal     Good      Very Good Premium   Good      Premium  
##  [6457] Premium   Premium   Very Good Good      Premium   Ideal    
##  [6463] Ideal     Ideal     Good      Fair      Good      Very Good
##  [6469] Very Good Very Good Very Good Very Good Ideal     Ideal    
##  [6475] Ideal     Good      Good      Premium   Fair      Premium  
##  [6481] Very Good Fair      Very Good Fair      Very Good Ideal    
##  [6487] Ideal     Premium   Very Good Good      Very Good Very Good
##  [6493] Good      Premium   Good      Premium   Very Good Ideal    
##  [6499] Premium   Premium   Premium   Very Good Fair      Premium  
##  [6505] Very Good Very Good Good      Very Good Very Good Premium  
##  [6511] Premium   Premium   Premium   Ideal     Premium   Premium  
##  [6517] Good      Ideal     Good      Very Good Very Good Very Good
##  [6523] Very Good Ideal     Good      Very Good Very Good Very Good
##  [6529] Very Good Good      Very Good Ideal     Ideal     Ideal    
##  [6535] Ideal     Premium   Premium   Ideal     Good      Premium  
##  [6541] Premium   Premium   Very Good Premium   Ideal     Good     
##  [6547] Premium   Good      Fair      Very Good Premium   Good     
##  [6553] Premium   Premium   Premium   Ideal     Very Good Premium  
##  [6559] Premium   Very Good Ideal     Ideal     Premium   Good     
##  [6565] Good      Ideal     Premium   Premium   Premium   Very Good
##  [6571] Ideal     Premium   Premium   Ideal     Ideal     Premium  
##  [6577] Premium   Premium   Fair      Good      Very Good Ideal    
##  [6583] Premium   Ideal     Premium   Very Good Ideal     Ideal    
##  [6589] Ideal     Premium   Premium   Premium   Good      Ideal    
##  [6595] Premium   Very Good Very Good Very Good Very Good Good     
##  [6601] Very Good Very Good Good      Good      Very Good Premium  
##  [6607] Very Good Good      Very Good Very Good Ideal     Ideal    
##  [6613] Ideal     Premium   Premium   Premium   Premium   Premium  
##  [6619] Premium   Premium   Very Good Premium   Very Good Very Good
##  [6625] Ideal     Good      Very Good Good      Ideal     Good     
##  [6631] Good      Ideal     Very Good Premium   Ideal     Premium  
##  [6637] Very Good Very Good Premium   Ideal     Good      Premium  
##  [6643] Premium   Ideal     Premium   Ideal     Ideal     Ideal    
##  [6649] Very Good Premium   Ideal     Premium   Very Good Good     
##  [6655] Very Good Good      Ideal     Premium   Ideal     Ideal    
##  [6661] Premium   Very Good Good      Ideal     Ideal     Premium  
##  [6667] Premium   Premium   Premium   Premium   Very Good Premium  
##  [6673] Premium   Ideal     Very Good Ideal     Very Good Fair     
##  [6679] Ideal     Very Good Ideal     Ideal     Ideal     Ideal    
##  [6685] Good      Ideal     Premium   Very Good Very Good Very Good
##  [6691] Premium   Very Good Premium   Premium   Ideal     Good     
##  [6697] Ideal     Good      Good      Ideal     Good      Very Good
##  [6703] Premium   Very Good Premium   Ideal     Ideal     Premium  
##  [6709] Very Good Ideal     Good      Very Good Very Good Very Good
##  [6715] Very Good Ideal     Ideal     Ideal     Ideal     Ideal    
##  [6721] Very Good Ideal     Ideal     Ideal     Ideal     Ideal    
##  [6727] Ideal     Ideal     Ideal     Premium   Premium   Premium  
##  [6733] Premium   Very Good Very Good Ideal     Very Good Very Good
##  [6739] Very Good Ideal     Fair      Premium   Ideal     Very Good
##  [6745] Good      Very Good Good      Very Good Premium   Ideal    
##  [6751] Good      Fair      Premium   Premium   Ideal     Premium  
##  [6757] Ideal     Very Good Very Good Premium   Ideal     Premium  
##  [6763] Good      Premium   Fair      Very Good Very Good Fair     
##  [6769] Fair      Ideal     Very Good Ideal     Good      Ideal    
##  [6775] Very Good Ideal     Very Good Premium   Good      Very Good
##  [6781] Very Good Good      Ideal     Fair      Premium   Premium  
##  [6787] Premium   Premium   Premium   Ideal     Very Good Ideal    
##  [6793] Ideal     Good      Premium   Fair      Very Good Good     
##  [6799] Good      Very Good Ideal     Good      Premium   Premium  
##  [6805] Premium   Ideal     Premium   Good      Very Good Ideal    
##  [6811] Ideal     Premium   Fair      Very Good Fair      Fair     
##  [6817] Good      Premium   Ideal     Premium   Premium   Good     
##  [6823] Very Good Very Good Ideal     Good      Very Good Premium  
##  [6829] Ideal     Premium   Good      Premium   Very Good Good     
##  [6835] Very Good Good      Very Good Premium   Premium   Very Good
##  [6841] Premium   Fair      Premium   Very Good Ideal     Good     
##  [6847] Premium   Ideal     Good      Very Good Ideal     Very Good
##  [6853] Very Good Ideal     Good      Fair      Very Good Very Good
##  [6859] Ideal     Ideal     Premium   Ideal     Ideal     Ideal    
##  [6865] Fair      Ideal     Premium   Premium   Premium   Ideal    
##  [6871] Premium   Ideal     Very Good Ideal     Very Good Good     
##  [6877] Fair      Premium   Very Good Ideal     Ideal     Good     
##  [6883] Ideal     Ideal     Premium   Very Good Ideal     Premium  
##  [6889] Premium   Premium   Very Good Ideal     Good      Ideal    
##  [6895] Premium   Ideal     Very Good Premium   Ideal     Ideal    
##  [6901] Very Good Very Good Ideal     Ideal     Good      Ideal    
##  [6907] Good      Very Good Very Good Very Good Very Good Very Good
##  [6913] Very Good Very Good Premium   Premium   Premium   Premium  
##  [6919] Premium   Good      Premium   Premium   Premium   Premium  
##  [6925] Very Good Premium   Good      Premium   Ideal     Very Good
##  [6931] Good      Good      Very Good Fair      Ideal     Ideal    
##  [6937] Premium   Premium   Premium   Fair      Good      Premium  
##  [6943] Very Good Very Good Premium   Very Good Very Good Ideal    
##  [6949] Very Good Premium   Premium   Ideal     Very Good Very Good
##  [6955] Premium   Premium   Fair      Premium   Ideal     Fair     
##  [6961] Fair      Ideal     Good      Very Good Good      Good     
##  [6967] Ideal     Premium   Good      Good      Fair      Premium  
##  [6973] Ideal     Premium   Premium   Premium   Very Good Premium  
##  [6979] Very Good Ideal     Very Good Premium   Premium   Premium  
##  [6985] Good      Very Good Premium   Very Good Premium   Premium  
##  [6991] Very Good Very Good Very Good Good      Premium   Premium  
##  [6997] Good      Ideal     Premium   Premium   Premium   Good     
##  [7003] Good      Premium   Premium   Premium   Premium   Good     
##  [7009] Very Good Ideal     Ideal     Ideal     Premium   Premium  
##  [7015] Very Good Good      Good      Good      Very Good Ideal    
##  [7021] Premium   Premium   Fair      Good      Premium   Premium  
##  [7027] Premium   Ideal     Ideal     Good      Premium   Premium  
##  [7033] Ideal     Premium   Very Good Ideal     Ideal     Ideal    
##  [7039] Very Good Very Good Ideal     Very Good Very Good Very Good
##  [7045] Very Good Good      Premium   Ideal     Very Good Fair     
##  [7051] Ideal     Ideal     Premium   Ideal     Ideal     Very Good
##  [7057] Premium   Ideal     Very Good Premium   Very Good Premium  
##  [7063] Very Good Ideal     Very Good Good      Ideal     Premium  
##  [7069] Premium   Very Good Very Good Ideal     Ideal     Very Good
##  [7075] Premium   Very Good Premium   Premium   Premium   Ideal    
##  [7081] Ideal     Ideal     Premium   Ideal     Good      Very Good
##  [7087] Very Good Very Good Good      Good      Premium   Very Good
##  [7093] Premium   Premium   Good      Very Good Fair      Good     
##  [7099] Ideal     Ideal     Premium   Good      Ideal     Ideal    
##  [7105] Very Good Fair      Good      Premium   Very Good Very Good
##  [7111] Premium   Very Good Ideal     Premium   Very Good Very Good
##  [7117] Premium   Ideal     Good      Good      Ideal     Ideal    
##  [7123] Very Good Premium   Premium   Very Good Very Good Ideal    
##  [7129] Good      Very Good Very Good Premium   Ideal     Ideal    
##  [7135] Ideal     Very Good Ideal     Fair      Very Good Premium  
##  [7141] Good      Premium   Premium   Very Good Good      Good     
##  [7147] Ideal     Good      Premium   Ideal     Ideal     Premium  
##  [7153] Very Good Very Good Good      Ideal     Ideal     Good     
##  [7159] Fair      Very Good Ideal     Ideal     Good      Premium  
##  [7165] Premium   Premium   Ideal     Ideal     Very Good Good     
##  [7171] Premium   Fair      Good      Ideal     Good      Premium  
##  [7177] Very Good Good      Very Good Ideal     Very Good Very Good
##  [7183] Very Good Very Good Very Good Ideal     Premium   Very Good
##  [7189] Good      Very Good Very Good Good      Very Good Very Good
##  [7195] Ideal     Premium   Very Good Ideal     Ideal     Good     
##  [7201] Premium   Very Good Good      Premium   Very Good Very Good
##  [7207] Very Good Very Good Ideal     Good      Good      Premium  
##  [7213] Very Good Fair      Premium   Ideal     Premium   Premium  
##  [7219] Premium   Very Good Good      Ideal     Very Good Very Good
##  [7225] Good      Good      Ideal     Premium   Good      Ideal    
##  [7231] Very Good Very Good Premium   Ideal     Fair      Good     
##  [7237] Premium   Premium   Very Good Good      Very Good Ideal    
##  [7243] Premium   Ideal     Ideal     Premium   Premium   Ideal    
##  [7249] Very Good Premium   Premium   Premium   Good      Premium  
##  [7255] Good      Good      Ideal     Good      Premium   Ideal    
##  [7261] Premium   Good      Very Good Ideal     Ideal     Ideal    
##  [7267] Very Good Ideal     Premium   Ideal     Ideal     Very Good
##  [7273] Premium   Very Good Very Good Premium   Good      Ideal    
##  [7279] Premium   Very Good Premium   Premium   Premium   Premium  
##  [7285] Premium   Very Good Very Good Premium   Very Good Premium  
##  [7291] Very Good Very Good Premium   Very Good Ideal     Ideal    
##  [7297] Good      Very Good Very Good Premium   Good      Good     
##  [7303] Premium   Good      Good      Premium   Premium   Very Good
##  [7309] Ideal     Good      Fair      Very Good Premium   Very Good
##  [7315] Premium   Premium   Very Good Good      Ideal     Very Good
##  [7321] Ideal     Very Good Good      Premium   Ideal     Premium  
##  [7327] Premium   Premium   Ideal     Good      Very Good Very Good
##  [7333] Very Good Very Good Good      Good      Very Good Very Good
##  [7339] Very Good Premium   Very Good Good      Good      Premium  
##  [7345] Very Good Ideal     Ideal     Premium   Very Good Ideal    
##  [7351] Good      Fair      Very Good Very Good Premium   Ideal    
##  [7357] Premium   Ideal     Very Good Good      Good      Good     
##  [7363] Good      Good      Very Good Very Good Very Good Ideal    
##  [7369] Fair      Good      Good      Ideal     Ideal     Ideal    
##  [7375] Fair      Premium   Ideal     Premium   Very Good Good     
##  [7381] Premium   Ideal     Ideal     Very Good Very Good Very Good
##  [7387] Very Good Ideal     Very Good Very Good Very Good Ideal    
##  [7393] Ideal     Premium   Premium   Premium   Very Good Ideal    
##  [7399] Ideal     Premium   Ideal     Ideal     Very Good Very Good
##  [7405] Very Good Good      Premium   Premium   Premium   Very Good
##  [7411] Ideal     Very Good Very Good Premium   Ideal     Ideal    
##  [7417] Good      Premium   Fair      Ideal     Premium   Premium  
##  [7423] Very Good Very Good Very Good Very Good Very Good Ideal    
##  [7429] Ideal     Premium   Very Good Ideal     Good      Premium  
##  [7435] Premium   Fair      Premium   Ideal     Ideal     Good     
##  [7441] Premium   Ideal     Very Good Good      Premium   Very Good
##  [7447] Ideal     Good      Very Good Ideal     Premium   Good     
##  [7453] Premium   Good      Very Good Premium   Good      Good     
##  [7459] Premium   Very Good Premium   Premium   Premium   Good     
##  [7465] Premium   Ideal     Premium   Premium   Premium   Very Good
##  [7471] Premium   Premium   Premium   Ideal     Premium   Very Good
##  [7477] Ideal     Premium   Very Good Ideal     Premium   Premium  
##  [7483] Premium   Premium   Ideal     Premium   Good      Premium  
##  [7489] Ideal     Premium   Premium   Good      Ideal     Ideal    
##  [7495] Ideal     Very Good Very Good Premium   Very Good Ideal    
##  [7501] Ideal     Premium   Premium   Premium   Very Good Very Good
##  [7507] Good      Very Good Good      Very Good Ideal     Ideal    
##  [7513] Ideal     Good      Premium   Good      Good      Very Good
##  [7519] Premium   Premium   Premium   Premium   Ideal     Premium  
##  [7525] Premium   Premium   Premium   Premium   Ideal     Premium  
##  [7531] Good      Premium   Very Good Ideal     Ideal     Good     
##  [7537] Very Good Very Good Very Good Very Good Ideal     Good     
##  [7543] Very Good Good      Ideal     Premium   Good      Very Good
##  [7549] Premium   Premium   Very Good Very Good Ideal     Ideal    
##  [7555] Ideal     Very Good Premium   Very Good Good      Premium  
##  [7561] Ideal     Premium   Premium   Premium   Very Good Very Good
##  [7567] Ideal     Fair      Premium   Ideal     Very Good Very Good
##  [7573] Ideal     Good      Good      Very Good Very Good Very Good
##  [7579] Very Good Very Good Premium   Very Good Good      Ideal    
##  [7585] Very Good Premium   Fair      Premium   Good      Good     
##  [7591] Fair      Fair      Ideal     Fair      Premium   Good     
##  [7597] Ideal     Good      Good      Premium   Very Good Good     
##  [7603] Good      Ideal     Premium   Ideal     Good      Very Good
##  [7609] Ideal     Premium   Premium   Premium   Premium   Very Good
##  [7615] Premium   Very Good Good      Good      Very Good Very Good
##  [7621] Good      Ideal     Ideal     Ideal     Very Good Very Good
##  [7627] Very Good Fair      Ideal     Premium   Very Good Fair     
##  [7633] Good      Premium   Good      Ideal     Premium   Very Good
##  [7639] Ideal     Ideal     Very Good Premium   Very Good Good     
##  [7645] Good      Premium   Ideal     Premium   Premium   Very Good
##  [7651] Very Good Very Good Ideal     Good      Very Good Ideal    
##  [7657] Ideal     Premium   Good      Premium   Premium   Very Good
##  [7663] Ideal     Ideal     Premium   Very Good Very Good Ideal    
##  [7669] Premium   Very Good Ideal     Very Good Good      Premium  
##  [7675] Fair      Premium   Very Good Good      Good      Premium  
##  [7681] Ideal     Premium   Premium   Premium   Premium   Premium  
##  [7687] Ideal     Ideal     Premium   Very Good Very Good Very Good
##  [7693] Very Good Ideal     Ideal     Good      Good      Good     
##  [7699] Fair      Fair      Fair      Ideal     Premium   Fair     
##  [7705] Fair      Ideal     Premium   Premium   Good      Ideal    
##  [7711] Ideal     Ideal     Premium   Ideal     Ideal     Ideal    
##  [7717] Ideal     Ideal     Premium   Ideal     Ideal     Premium  
##  [7723] Premium   Premium   Ideal     Premium   Ideal     Ideal    
##  [7729] Premium   Very Good Very Good Very Good Ideal     Ideal    
##  [7735] Ideal     Good      Premium   Fair      Very Good Ideal    
##  [7741] Ideal     Premium   Premium   Premium   Premium   Premium  
##  [7747] Good      Very Good Ideal     Ideal     Ideal     Ideal    
##  [7753] Very Good Ideal     Fair      Fair      Very Good Very Good
##  [7759] Very Good Ideal     Very Good Very Good Very Good Ideal    
##  [7765] Premium   Premium   Good      Good      Good      Premium  
##  [7771] Very Good Very Good Premium   Fair      Ideal     Ideal    
##  [7777] Very Good Premium   Premium   Premium   Ideal     Premium  
##  [7783] Premium   Ideal     Premium   Ideal     Good      Very Good
##  [7789] Very Good Premium   Very Good Good      Very Good Very Good
##  [7795] Very Good Premium   Ideal     Ideal     Good      Ideal    
##  [7801] Ideal     Very Good Premium   Very Good Very Good Premium  
##  [7807] Very Good Very Good Very Good Ideal     Premium   Very Good
##  [7813] Premium   Very Good Ideal     Very Good Good      Very Good
##  [7819] Fair      Very Good Fair      Very Good Premium   Premium  
##  [7825] Very Good Good      Ideal     Very Good Premium   Good     
##  [7831] Very Good Premium   Very Good Very Good Very Good Ideal    
##  [7837] Very Good Premium   Ideal     Good      Ideal     Very Good
##  [7843] Very Good Premium   Very Good Very Good Very Good Very Good
##  [7849] Ideal     Very Good Premium   Premium   Premium   Very Good
##  [7855] Very Good Premium   Premium   Premium   Good      Very Good
##  [7861] Very Good Ideal     Very Good Very Good Good      Fair     
##  [7867] Very Good Premium   Premium   Very Good Good      Good     
##  [7873] Good      Very Good Very Good Good      Premium   Good     
##  [7879] Premium   Ideal     Very Good Fair      Ideal     Very Good
##  [7885] Premium   Very Good Very Good Very Good Fair      Very Good
##  [7891] Very Good Very Good Good      Very Good Good      Premium  
##  [7897] Fair      Fair      Premium   Fair      Fair      Good     
##  [7903] Ideal     Premium   Very Good Ideal     Ideal     Ideal    
##  [7909] Ideal     Premium   Very Good Very Good Very Good Very Good
##  [7915] Very Good Ideal     Ideal     Ideal     Good      Premium  
##  [7921] Very Good Very Good Very Good Ideal     Very Good Premium  
##  [7927] Premium   Ideal     Ideal     Fair      Very Good Premium  
##  [7933] Ideal     Good      Premium   Very Good Premium   Premium  
##  [7939] Very Good Premium   Premium   Premium   Very Good Ideal    
##  [7945] Premium   Ideal     Premium   Ideal     Premium   Very Good
##  [7951] Ideal     Premium   Good      Premium   Fair      Very Good
##  [7957] Premium   Very Good Very Good Very Good Premium   Good     
##  [7963] Premium   Good      Very Good Premium   Premium   Very Good
##  [7969] Ideal     Ideal     Ideal     Very Good Very Good Very Good
##  [7975] Premium   Good      Ideal     Very Good Ideal     Ideal    
##  [7981] Ideal     Ideal     Ideal     Ideal     Good      Very Good
##  [7987] Good      Good      Premium   Premium   Premium   Very Good
##  [7993] Premium   Good      Very Good Ideal     Premium   Premium  
##  [7999] Premium   Premium   Ideal     Premium   Ideal     Premium  
##  [8005] Premium   Premium   Premium   Very Good Very Good Fair     
##  [8011] Premium   Premium   Fair      Premium   Fair      Ideal    
##  [8017] Very Good Premium   Premium   Ideal     Ideal     Premium  
##  [8023] Premium   Premium   Very Good Very Good Very Good Ideal    
##  [8029] Ideal     Ideal     Premium   Premium   Premium   Very Good
##  [8035] Ideal     Premium   Good      Premium   Ideal     Ideal    
##  [8041] Good      Ideal     Very Good Premium   Premium   Ideal    
##  [8047] Ideal     Ideal     Ideal     Ideal     Very Good Very Good
##  [8053] Very Good Very Good Very Good Very Good Very Good Very Good
##  [8059] Very Good Very Good Ideal     Ideal     Ideal     Ideal    
##  [8065] Premium   Premium   Ideal     Good      Good      Ideal    
##  [8071] Ideal     Good      Very Good Ideal     Premium   Very Good
##  [8077] Good      Premium   Good      Premium   Premium   Premium  
##  [8083] Premium   Premium   Premium   Premium   Very Good Good     
##  [8089] Good      Very Good Premium   Premium   Premium   Very Good
##  [8095] Ideal     Premium   Premium   Ideal     Ideal     Good     
##  [8101] Premium   Very Good Good      Ideal     Premium   Premium  
##  [8107] Ideal     Very Good Very Good Very Good Premium   Very Good
##  [8113] Premium   Premium   Ideal     Ideal     Very Good Ideal    
##  [8119] Very Good Premium   Very Good Very Good Premium   Ideal    
##  [8125] Good      Very Good Premium   Premium   Ideal     Premium  
##  [8131] Ideal     Premium   Very Good Very Good Very Good Very Good
##  [8137] Good      Very Good Premium   Good      Premium   Premium  
##  [8143] Premium   Fair      Premium   Ideal     Ideal     Good     
##  [8149] Premium   Premium   Very Good Good      Ideal     Good     
##  [8155] Premium   Very Good Ideal     Ideal     Premium   Good     
##  [8161] Very Good Ideal     Very Good Premium   Very Good Premium  
##  [8167] Ideal     Ideal     Good      Premium   Very Good Very Good
##  [8173] Premium   Ideal     Ideal     Premium   Premium   Very Good
##  [8179] Ideal     Premium   Premium   Premium   Premium   Ideal    
##  [8185] Ideal     Ideal     Fair      Fair      Ideal     Premium  
##  [8191] Ideal     Good      Premium   Premium   Premium   Premium  
##  [8197] Premium   Fair      Premium   Premium   Fair      Ideal    
##  [8203] Fair      Fair      Very Good Very Good Ideal     Ideal    
##  [8209] Good      Very Good Good      Good      Premium   Very Good
##  [8215] Premium   Ideal     Premium   Ideal     Premium   Very Good
##  [8221] Very Good Premium   Very Good Very Good Premium   Ideal    
##  [8227] Ideal     Premium   Good      Very Good Ideal     Ideal    
##  [8233] Premium   Premium   Very Good Very Good Ideal     Ideal    
##  [8239] Premium   Very Good Ideal     Ideal     Premium   Ideal    
##  [8245] Premium   Ideal     Ideal     Very Good Very Good Premium  
##  [8251] Fair      Ideal     Good      Ideal     Ideal     Ideal    
##  [8257] Ideal     Ideal     Premium   Very Good Very Good Ideal    
##  [8263] Premium   Very Good Good      Very Good Good      Premium  
##  [8269] Very Good Very Good Very Good Good      Very Good Very Good
##  [8275] Good      Premium   Fair      Premium   Premium   Premium  
##  [8281] Very Good Good      Ideal     Good      Ideal     Very Good
##  [8287] Ideal     Ideal     Ideal     Very Good Premium   Ideal    
##  [8293] Premium   Very Good Premium   Premium   Ideal     Premium  
##  [8299] Premium   Premium   Very Good Ideal     Good      Premium  
##  [8305] Premium   Premium   Ideal     Premium   Good      Very Good
##  [8311] Good      Premium   Good      Good      Very Good Ideal    
##  [8317] Ideal     Ideal     Very Good Premium   Premium   Premium  
##  [8323] Very Good Ideal     Good      Premium   Premium   Ideal    
##  [8329] Very Good Good      Premium   Premium   Good      Very Good
##  [8335] Ideal     Good      Premium   Premium   Very Good Ideal    
##  [8341] Ideal     Good      Fair      Very Good Good      Very Good
##  [8347] Very Good Very Good Ideal     Very Good Very Good Fair     
##  [8353] Ideal     Premium   Premium   Ideal     Fair      Ideal    
##  [8359] Very Good Premium   Good      Good      Very Good Good     
##  [8365] Premium   Very Good Very Good Very Good Good      Premium  
##  [8371] Very Good Good      Ideal     Ideal     Premium   Very Good
##  [8377] Good      Very Good Ideal     Very Good Ideal     Ideal    
##  [8383] Premium   Ideal     Ideal     Good      Good      Premium  
##  [8389] Very Good Very Good Very Good Very Good Fair      Ideal    
##  [8395] Good      Very Good Good      Premium   Premium   Very Good
##  [8401] Premium   Ideal     Very Good Ideal     Ideal     Premium  
##  [8407] Good      Ideal     Ideal     Ideal     Ideal     Fair     
##  [8413] Ideal     Good      Very Good Very Good Very Good Premium  
##  [8419] Very Good Premium   Very Good Ideal     Ideal     Good     
##  [8425] Good      Ideal     Ideal     Ideal     Good      Premium  
##  [8431] Very Good Good      Premium   Very Good Premium   Ideal    
##  [8437] Ideal     Good      Premium   Good      Premium   Ideal    
##  [8443] Ideal     Premium   Very Good Premium   Ideal     Very Good
##  [8449] Premium   Premium   Very Good Premium   Very Good Very Good
##  [8455] Ideal     Good      Ideal     Very Good Premium   Good     
##  [8461] Very Good Premium   Very Good Very Good Very Good Ideal    
##  [8467] Good      Very Good Very Good Very Good Good      Premium  
##  [8473] Premium   Ideal     Premium   Premium   Premium   Ideal    
##  [8479] Good      Very Good Very Good Good      Ideal     Ideal    
##  [8485] Fair      Very Good Very Good Very Good Premium   Very Good
##  [8491] Very Good Very Good Ideal     Ideal     Good      Premium  
##  [8497] Ideal     Ideal     Very Good Ideal     Ideal     Premium  
##  [8503] Ideal     Very Good Premium   Ideal     Premium   Good     
##  [8509] Fair      Premium   Fair      Premium   Ideal     Very Good
##  [8515] Ideal     Ideal     Good      Ideal     Very Good Very Good
##  [8521] Premium   Premium   Good      Very Good Very Good Premium  
##  [8527] Ideal     Premium   Good      Good      Very Good Ideal    
##  [8533] Ideal     Ideal     Premium   Very Good Very Good Very Good
##  [8539] Ideal     Very Good Premium   Premium   Very Good Good     
##  [8545] Premium   Very Good Very Good Very Good Good      Ideal    
##  [8551] Ideal     Very Good Ideal     Premium   Very Good Very Good
##  [8557] Premium   Ideal     Premium   Premium   Ideal     Premium  
##  [8563] Very Good Very Good Very Good Very Good Very Good Premium  
##  [8569] Premium   Premium   Premium   Good      Very Good Very Good
##  [8575] Fair      Fair      Premium   Very Good Premium   Good     
##  [8581] Very Good Very Good Premium   Good      Premium   Very Good
##  [8587] Very Good Premium   Ideal     Ideal     Fair      Premium  
##  [8593] Very Good Premium   Good      Ideal     Premium   Very Good
##  [8599] Fair      Premium   Good      Good      Very Good Ideal    
##  [8605] Premium   Premium   Very Good Very Good Ideal     Good     
##  [8611] Good      Very Good Premium   Ideal     Very Good Good     
##  [8617] Premium   Premium   Very Good Good      Premium   Premium  
##  [8623] Good      Premium   Very Good Ideal     Premium   Ideal    
##  [8629] Very Good Very Good Ideal     Premium   Very Good Premium  
##  [8635] Very Good Very Good Premium   Good      Ideal     Premium  
##  [8641] Premium   Premium   Ideal     Ideal     Ideal     Premium  
##  [8647] Premium   Premium   Premium   Good      Ideal     Ideal    
##  [8653] Good      Ideal     Ideal     Premium   Very Good Ideal    
##  [8659] Very Good Very Good Ideal     Very Good Very Good Ideal    
##  [8665] Premium   Premium   Very Good Premium   Premium   Premium  
##  [8671] Ideal     Ideal     Fair      Fair      Ideal     Good     
##  [8677] Very Good Good      Premium   Good      Ideal     Ideal    
##  [8683] Good      Ideal     Ideal     Ideal     Ideal     Ideal    
##  [8689] Ideal     Premium   Ideal     Premium   Premium   Premium  
##  [8695] Premium   Ideal     Good      Premium   Good      Good     
##  [8701] Very Good Very Good Very Good Premium   Very Good Very Good
##  [8707] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
##  [8713] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
##  [8719] Ideal     Ideal     Ideal     Fair      Good      Premium  
##  [8725] Good      Very Good Very Good Ideal     Ideal     Good     
##  [8731] Premium   Premium   Premium   Ideal     Premium   Ideal    
##  [8737] Fair      Fair      Premium   Very Good Very Good Good     
##  [8743] Very Good Good      Premium   Premium   Good      Very Good
##  [8749] Very Good Ideal     Ideal     Very Good Good      Very Good
##  [8755] Very Good Very Good Very Good Very Good Very Good Very Good
##  [8761] Ideal     Ideal     Good      Good      Good      Very Good
##  [8767] Ideal     Ideal     Premium   Premium   Very Good Good     
##  [8773] Good      Premium   Premium   Good      Ideal     Ideal    
##  [8779] Good      Very Good Premium   Premium   Premium   Very Good
##  [8785] Premium   Ideal     Ideal     Good      Very Good Premium  
##  [8791] Ideal     Ideal     Very Good Premium   Premium   Very Good
##  [8797] Premium   Very Good Premium   Very Good Fair      Good     
##  [8803] Very Good Premium   Very Good Very Good Good      Premium  
##  [8809] Premium   Ideal     Good      Very Good Good      Very Good
##  [8815] Ideal     Very Good Premium   Good      Premium   Very Good
##  [8821] Premium   Very Good Good      Ideal     Very Good Very Good
##  [8827] Ideal     Ideal     Premium   Ideal     Premium   Premium  
##  [8833] Good      Premium   Premium   Premium   Very Good Premium  
##  [8839] Good      Good      Ideal     Good      Ideal     Very Good
##  [8845] Premium   Ideal     Premium   Good      Fair      Good     
##  [8851] Premium   Fair      Good      Fair      Very Good Fair     
##  [8857] Premium   Premium   Very Good Ideal     Premium   Very Good
##  [8863] Premium   Premium   Ideal     Fair      Premium   Very Good
##  [8869] Premium   Ideal     Ideal     Very Good Premium   Very Good
##  [8875] Ideal     Very Good Ideal     Ideal     Very Good Premium  
##  [8881] Ideal     Good      Very Good Ideal     Ideal     Premium  
##  [8887] Premium   Very Good Very Good Good      Very Good Ideal    
##  [8893] Ideal     Good      Premium   Premium   Ideal     Good     
##  [8899] Premium   Premium   Premium   Premium   Ideal     Good     
##  [8905] Premium   Ideal     Ideal     Ideal     Ideal     Very Good
##  [8911] Good      Good      Premium   Ideal     Premium   Ideal    
##  [8917] Premium   Very Good Fair      Premium   Good      Premium  
##  [8923] Very Good Good      Good      Very Good Ideal     Ideal    
##  [8929] Premium   Premium   Good      Ideal     Premium   Premium  
##  [8935] Premium   Premium   Ideal     Premium   Premium   Premium  
##  [8941] Very Good Premium   Premium   Very Good Very Good Very Good
##  [8947] Very Good Good      Premium   Premium   Premium   Premium  
##  [8953] Good      Ideal     Premium   Very Good Good      Ideal    
##  [8959] Good      Very Good Premium   Premium   Good      Very Good
##  [8965] Premium   Very Good Premium   Very Good Very Good Very Good
##  [8971] Very Good Good      Good      Ideal     Very Good Very Good
##  [8977] Ideal     Good      Premium   Good      Very Good Premium  
##  [8983] Ideal     Ideal     Good      Very Good Premium   Premium  
##  [8989] Ideal     Ideal     Ideal     Premium   Premium   Good     
##  [8995] Premium   Premium   Very Good Ideal     Ideal     Good     
##  [9001] Very Good Good      Ideal     Ideal     Premium   Very Good
##  [9007] Premium   Ideal     Very Good Premium   Premium   Premium  
##  [9013] Premium   Very Good Very Good Very Good Premium   Very Good
##  [9019] Very Good Premium   Premium   Premium   Very Good Ideal    
##  [9025] Ideal     Very Good Very Good Very Good Ideal     Ideal    
##  [9031] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
##  [9037] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
##  [9043] Ideal     Very Good Ideal     Very Good Very Good Good     
##  [9049] Very Good Good      Premium   Premium   Premium   Ideal    
##  [9055] Ideal     Ideal     Ideal     Ideal     Ideal     Very Good
##  [9061] Premium   Fair      Ideal     Good      Fair      Good     
##  [9067] Good      Very Good Ideal     Premium   Premium   Ideal    
##  [9073] Very Good Premium   Ideal     Good      Very Good Very Good
##  [9079] Ideal     Good      Very Good Ideal     Premium   Premium  
##  [9085] Good      Very Good Very Good Very Good Ideal     Ideal    
##  [9091] Ideal     Good      Premium   Premium   Very Good Ideal    
##  [9097] Premium   Fair      Very Good Premium   Premium   Very Good
##  [9103] Very Good Good      Good      Good      Premium   Premium  
##  [9109] Ideal     Ideal     Very Good Ideal     Ideal     Very Good
##  [9115] Very Good Very Good Good      Premium   Very Good Very Good
##  [9121] Fair      Good      Ideal     Good      Very Good Premium  
##  [9127] Premium   Ideal     Premium   Fair      Ideal     Ideal    
##  [9133] Ideal     Premium   Very Good Premium   Ideal     Ideal    
##  [9139] Very Good Very Good Very Good Ideal     Very Good Very Good
##  [9145] Very Good Fair      Ideal     Premium   Good      Premium  
##  [9151] Very Good Very Good Ideal     Ideal     Very Good Good     
##  [9157] Good      Very Good Very Good Very Good Very Good Good     
##  [9163] Ideal     Ideal     Premium   Premium   Premium   Very Good
##  [9169] Good      Very Good Good      Very Good Good      Good     
##  [9175] Very Good Very Good Very Good Premium   Ideal     Premium  
##  [9181] Ideal     Very Good Ideal     Ideal     Very Good Very Good
##  [9187] Very Good Good      Ideal     Ideal     Ideal     Ideal    
##  [9193] Good      Ideal     Ideal     Premium   Premium   Good     
##  [9199] Premium   Ideal     Good      Very Good Ideal     Fair     
##  [9205] Good      Ideal     Fair      Very Good Very Good Good     
##  [9211] Premium   Premium   Ideal     Fair      Fair      Premium  
##  [9217] Very Good Ideal     Ideal     Ideal     Ideal     Ideal    
##  [9223] Premium   Premium   Premium   Premium   Ideal     Good     
##  [9229] Premium   Premium   Premium   Premium   Good      Very Good
##  [9235] Ideal     Good      Very Good Premium   Very Good Good     
##  [9241] Premium   Good      Ideal     Good      Premium   Ideal    
##  [9247] Very Good Premium   Ideal     Premium   Very Good Very Good
##  [9253] Ideal     Good      Premium   Ideal     Premium   Ideal    
##  [9259] Very Good Very Good Ideal     Ideal     Very Good Fair     
##  [9265] Premium   Premium   Very Good Premium   Premium   Premium  
##  [9271] Very Good Ideal     Good      Ideal     Very Good Very Good
##  [9277] Very Good Ideal     Ideal     Very Good Ideal     Fair     
##  [9283] Very Good Ideal     Good      Good      Premium   Ideal    
##  [9289] Ideal     Good      Good      Premium   Very Good Very Good
##  [9295] Fair      Premium   Premium   Premium   Premium   Premium  
##  [9301] Ideal     Good      Good      Very Good Premium   Very Good
##  [9307] Good      Premium   Ideal     Very Good Very Good Very Good
##  [9313] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
##  [9319] Premium   Premium   Premium   Very Good Ideal     Premium  
##  [9325] Premium   Premium   Premium   Very Good Premium   Very Good
##  [9331] Good      Very Good Good      Very Good Very Good Very Good
##  [9337] Ideal     Premium   Premium   Premium   Premium   Very Good
##  [9343] Good      Ideal     Good      Very Good Premium   Very Good
##  [9349] Very Good Very Good Ideal     Ideal     Ideal     Premium  
##  [9355] Ideal     Ideal     Ideal     Ideal     Very Good Good     
##  [9361] Ideal     Very Good Premium   Ideal     Ideal     Good     
##  [9367] Good      Good      Ideal     Very Good Very Good Premium  
##  [9373] Premium   Ideal     Ideal     Premium   Premium   Very Good
##  [9379] Ideal     Ideal     Premium   Good      Ideal     Very Good
##  [9385] Ideal     Good      Good      Good      Ideal     Good     
##  [9391] Premium   Premium   Good      Good      Fair      Ideal    
##  [9397] Premium   Very Good Premium   Premium   Ideal     Premium  
##  [9403] Good      Premium   Very Good Premium   Premium   Ideal    
##  [9409] Ideal     Very Good Very Good Very Good Very Good Good     
##  [9415] Good      Very Good Very Good Premium   Good      Very Good
##  [9421] Good      Very Good Good      Ideal     Very Good Very Good
##  [9427] Premium   Very Good Very Good Ideal     Ideal     Very Good
##  [9433] Ideal     Ideal     Premium   Good      Premium   Good     
##  [9439] Very Good Very Good Premium   Ideal     Ideal     Ideal    
##  [9445] Ideal     Ideal     Ideal     Very Good Good      Very Good
##  [9451] Very Good Ideal     Good      Premium   Ideal     Premium  
##  [9457] Very Good Very Good Good      Premium   Premium   Very Good
##  [9463] Very Good Ideal     Fair      Good      Very Good Premium  
##  [9469] Ideal     Good      Ideal     Ideal     Very Good Premium  
##  [9475] Ideal     Premium   Very Good Premium   Good      Ideal    
##  [9481] Ideal     Ideal     Ideal     Very Good Very Good Premium  
##  [9487] Very Good Very Good Good      Good      Very Good Very Good
##  [9493] Good      Premium   Ideal     Good      Good      Very Good
##  [9499] Ideal     Good      Premium   Good      Good      Ideal    
##  [9505] Ideal     Premium   Fair      Premium   Ideal     Premium  
##  [9511] Premium   Very Good Good      Ideal     Premium   Premium  
##  [9517] Ideal     Ideal     Ideal     Very Good Premium   Good     
##  [9523] Premium   Very Good Very Good Good      Very Good Very Good
##  [9529] Very Good Ideal     Ideal     Ideal     Ideal     Premium  
##  [9535] Very Good Good      Very Good Good      Very Good Ideal    
##  [9541] Premium   Ideal     Premium   Good      Good      Good     
##  [9547] Very Good Premium   Very Good Very Good Premium   Premium  
##  [9553] Ideal     Premium   Very Good Ideal     Premium   Ideal    
##  [9559] Very Good Very Good Ideal     Premium   Ideal     Ideal    
##  [9565] Premium   Very Good Ideal     Ideal     Good      Premium  
##  [9571] Very Good Premium   Very Good Premium   Premium   Premium  
##  [9577] Premium   Fair      Premium   Premium   Premium   Ideal    
##  [9583] Ideal     Very Good Very Good Ideal     Ideal     Premium  
##  [9589] Ideal     Premium   Very Good Very Good Ideal     Ideal    
##  [9595] Ideal     Ideal     Fair      Ideal     Ideal     Premium  
##  [9601] Ideal     Very Good Premium   Good      Good      Very Good
##  [9607] Premium   Premium   Very Good Ideal     Premium   Very Good
##  [9613] Very Good Good      Ideal     Very Good Fair      Premium  
##  [9619] Ideal     Very Good Premium   Good      Very Good Premium  
##  [9625] Very Good Very Good Good      Premium   Premium   Premium  
##  [9631] Very Good Fair      Premium   Ideal     Premium   Ideal    
##  [9637] Premium   Premium   Very Good Good      Premium   Very Good
##  [9643] Premium   Very Good Very Good Ideal     Good      Premium  
##  [9649] Premium   Premium   Premium   Premium   Ideal     Ideal    
##  [9655] Ideal     Premium   Very Good Premium   Premium   Good     
##  [9661] Very Good Ideal     Premium   Very Good Very Good Ideal    
##  [9667] Ideal     Very Good Premium   Ideal     Ideal     Good     
##  [9673] Ideal     Fair      Premium   Premium   Fair      Fair     
##  [9679] Ideal     Very Good Ideal     Ideal     Ideal     Premium  
##  [9685] Premium   Very Good Premium   Very Good Very Good Very Good
##  [9691] Premium   Very Good Ideal     Ideal     Good      Very Good
##  [9697] Premium   Premium   Ideal     Good      Very Good Ideal    
##  [9703] Ideal     Ideal     Ideal     Ideal     Ideal     Very Good
##  [9709] Good      Ideal     Very Good Very Good Very Good Very Good
##  [9715] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
##  [9721] Very Good Ideal     Ideal     Very Good Premium   Premium  
##  [9727] Premium   Premium   Good      Very Good Ideal     Premium  
##  [9733] Very Good Good      Fair      Premium   Very Good Premium  
##  [9739] Premium   Very Good Fair      Ideal     Ideal     Good     
##  [9745] Very Good Premium   Premium   Ideal     Ideal     Premium  
##  [9751] Premium   Very Good Very Good Premium   Premium   Very Good
##  [9757] Premium   Very Good Very Good Very Good Ideal     Premium  
##  [9763] Premium   Premium   Very Good Ideal     Ideal     Ideal    
##  [9769] Ideal     Ideal     Premium   Ideal     Ideal     Ideal    
##  [9775] Premium   Premium   Premium   Very Good Ideal     Premium  
##  [9781] Very Good Very Good Ideal     Very Good Fair      Very Good
##  [9787] Very Good Ideal     Very Good Ideal     Premium   Premium  
##  [9793] Very Good Ideal     Good      Ideal     Premium   Ideal    
##  [9799] Premium   Ideal     Ideal     Very Good Very Good Good     
##  [9805] Very Good Very Good Ideal     Ideal     Very Good Good     
##  [9811] Premium   Premium   Premium   Premium   Very Good Ideal    
##  [9817] Ideal     Good      Premium   Premium   Good      Premium  
##  [9823] Very Good Premium   Very Good Good      Premium   Premium  
##  [9829] Good      Premium   Premium   Ideal     Premium   Premium  
##  [9835] Good      Premium   Premium   Very Good Ideal     Good     
##  [9841] Premium   Ideal     Ideal     Ideal     Premium   Very Good
##  [9847] Ideal     Premium   Ideal     Premium   Premium   Very Good
##  [9853] Ideal     Ideal     Very Good Ideal     Very Good Premium  
##  [9859] Very Good Good      Good      Ideal     Ideal     Very Good
##  [9865] Good      Premium   Very Good Ideal     Ideal     Ideal    
##  [9871] Very Good Ideal     Good      Fair      Good      Very Good
##  [9877] Ideal     Very Good Very Good Very Good Premium   Good     
##  [9883] Premium   Good      Ideal     Very Good Premium   Premium  
##  [9889] Ideal     Ideal     Very Good Very Good Very Good Very Good
##  [9895] Ideal     Ideal     Good      Good      Premium   Premium  
##  [9901] Ideal     Ideal     Premium   Premium   Premium   Premium  
##  [9907] Very Good Fair      Very Good Premium   Premium   Premium  
##  [9913] Very Good Premium   Premium   Very Good Ideal     Premium  
##  [9919] Premium   Very Good Very Good Very Good Premium   Very Good
##  [9925] Ideal     Premium   Good      Ideal     Premium   Very Good
##  [9931] Premium   Premium   Very Good Very Good Premium   Ideal    
##  [9937] Very Good Very Good Premium   Very Good Very Good Ideal    
##  [9943] Ideal     Ideal     Good      Ideal     Ideal     Premium  
##  [9949] Ideal     Fair      Ideal     Ideal     Very Good Very Good
##  [9955] Good      Ideal     Fair      Premium   Premium   Ideal    
##  [9961] Premium   Premium   Good      Premium   Very Good Very Good
##  [9967] Ideal     Premium   Very Good Good      Good      Premium  
##  [9973] Very Good Ideal     Premium   Very Good Very Good Good     
##  [9979] Premium   Very Good Fair      Fair      Fair      Very Good
##  [9985] Ideal     Ideal     Premium   Premium   Very Good Fair     
##  [9991] Premium   Premium   Premium   Premium   Very Good Very Good
##  [9997] Premium   Premium   Premium   Fair      Fair      Very Good
## [10003] Good      Fair      Ideal     Premium   Very Good Very Good
## [10009] Very Good Very Good Ideal     Very Good Premium   Premium  
## [10015] Ideal     Ideal     Ideal     Very Good Ideal     Ideal    
## [10021] Ideal     Ideal     Ideal     Ideal     Premium   Good     
## [10027] Very Good Very Good Good      Ideal     Fair      Good     
## [10033] Ideal     Ideal     Very Good Premium   Ideal     Very Good
## [10039] Good      Premium   Good      Very Good Very Good Ideal    
## [10045] Premium   Ideal     Very Good Premium   Ideal     Very Good
## [10051] Ideal     Ideal     Very Good Very Good Very Good Very Good
## [10057] Very Good Ideal     Ideal     Ideal     Ideal     Good     
## [10063] Premium   Premium   Very Good Premium   Very Good Very Good
## [10069] Premium   Premium   Premium   Good      Premium   Ideal    
## [10075] Good      Very Good Very Good Ideal     Ideal     Ideal    
## [10081] Good      Ideal     Very Good Very Good Very Good Ideal    
## [10087] Ideal     Good      Very Good Good      Good      Very Good
## [10093] Good      Ideal     Premium   Very Good Ideal     Ideal    
## [10099] Ideal     Very Good Very Good Very Good Premium   Very Good
## [10105] Very Good Very Good Premium   Premium   Very Good Fair     
## [10111] Premium   Good      Premium   Very Good Premium   Premium  
## [10117] Premium   Premium   Very Good Premium   Premium   Very Good
## [10123] Very Good Good      Premium   Very Good Very Good Ideal    
## [10129] Premium   Premium   Premium   Very Good Ideal     Premium  
## [10135] Very Good Ideal     Very Good Good      Ideal     Premium  
## [10141] Very Good Very Good Ideal     Ideal     Ideal     Ideal    
## [10147] Premium   Good      Ideal     Very Good Fair      Premium  
## [10153] Ideal     Premium   Premium   Very Good Very Good Good     
## [10159] Good      Very Good Premium   Very Good Ideal     Good     
## [10165] Premium   Fair      Premium   Good      Ideal     Good     
## [10171] Premium   Fair      Very Good Premium   Premium   Fair     
## [10177] Ideal     Ideal     Ideal     Premium   Premium   Premium  
## [10183] Premium   Premium   Good      Ideal     Very Good Premium  
## [10189] Premium   Premium   Very Good Ideal     Very Good Good     
## [10195] Premium   Premium   Premium   Very Good Premium   Premium  
## [10201] Ideal     Very Good Ideal     Ideal     Ideal     Ideal    
## [10207] Premium   Very Good Premium   Premium   Ideal     Good     
## [10213] Premium   Ideal     Premium   Ideal     Premium   Premium  
## [10219] Very Good Ideal     Very Good Premium   Ideal     Good     
## [10225] Premium   Premium   Premium   Ideal     Ideal     Good     
## [10231] Very Good Ideal     Premium   Premium   Ideal     Very Good
## [10237] Very Good Fair      Ideal     Premium   Premium   Premium  
## [10243] Premium   Premium   Very Good Good      Ideal     Good     
## [10249] Good      Very Good Premium   Ideal     Ideal     Ideal    
## [10255] Premium   Premium   Ideal     Fair      Fair      Premium  
## [10261] Premium   Good      Ideal     Premium   Ideal     Premium  
## [10267] Fair      Premium   Ideal     Ideal     Fair      Good     
## [10273] Very Good Ideal     Ideal     Ideal     Very Good Ideal    
## [10279] Very Good Premium   Ideal     Very Good Premium   Premium  
## [10285] Ideal     Premium   Very Good Ideal     Ideal     Premium  
## [10291] Good      Premium   Very Good Fair      Premium   Very Good
## [10297] Good      Very Good Very Good Ideal     Very Good Very Good
## [10303] Ideal     Good      Premium   Premium   Very Good Very Good
## [10309] Ideal     Good      Very Good Ideal     Ideal     Ideal    
## [10315] Premium   Very Good Ideal     Premium   Ideal     Ideal    
## [10321] Ideal     Premium   Very Good Premium   Ideal     Ideal    
## [10327] Ideal     Fair      Very Good Very Good Good      Very Good
## [10333] Good      Very Good Very Good Good      Ideal     Ideal    
## [10339] Very Good Ideal     Premium   Very Good Very Good Very Good
## [10345] Premium   Premium   Ideal     Premium   Ideal     Good     
## [10351] Premium   Good      Ideal     Fair      Premium   Ideal    
## [10357] Premium   Ideal     Ideal     Premium   Ideal     Premium  
## [10363] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [10369] Premium   Ideal     Premium   Premium   Ideal     Premium  
## [10375] Premium   Very Good Premium   Ideal     Good      Ideal    
## [10381] Fair      Good      Very Good Very Good Very Good Ideal    
## [10387] Ideal     Fair      Ideal     Premium   Premium   Premium  
## [10393] Ideal     Premium   Premium   Ideal     Very Good Premium  
## [10399] Premium   Very Good Premium   Very Good Premium   Premium  
## [10405] Good      Premium   Good      Ideal     Premium   Premium  
## [10411] Ideal     Ideal     Premium   Ideal     Very Good Very Good
## [10417] Very Good Ideal     Good      Ideal     Ideal     Good     
## [10423] Ideal     Ideal     Ideal     Good      Ideal     Ideal    
## [10429] Ideal     Very Good Very Good Very Good Premium   Ideal    
## [10435] Very Good Very Good Very Good Premium   Very Good Good     
## [10441] Premium   Ideal     Premium   Premium   Good      Ideal    
## [10447] Very Good Premium   Good      Premium   Premium   Ideal    
## [10453] Premium   Premium   Premium   Ideal     Very Good Premium  
## [10459] Ideal     Premium   Ideal     Very Good Very Good Premium  
## [10465] Fair      Good      Very Good Very Good Very Good Very Good
## [10471] Premium   Fair      Fair      Fair      Ideal     Premium  
## [10477] Very Good Premium   Very Good Premium   Very Good Very Good
## [10483] Ideal     Ideal     Premium   Premium   Good      Very Good
## [10489] Ideal     Fair      Very Good Ideal     Ideal     Premium  
## [10495] Very Good Ideal     Fair      Fair      Ideal     Premium  
## [10501] Ideal     Fair      Ideal     Ideal     Very Good Ideal    
## [10507] Ideal     Ideal     Very Good Ideal     Very Good Very Good
## [10513] Ideal     Premium   Ideal     Ideal     Ideal     Ideal    
## [10519] Ideal     Very Good Ideal     Ideal     Very Good Ideal    
## [10525] Premium   Good      Good      Ideal     Premium   Premium  
## [10531] Premium   Good      Premium   Premium   Premium   Very Good
## [10537] Premium   Good      Premium   Ideal     Ideal     Premium  
## [10543] Ideal     Ideal     Ideal     Premium   Premium   Premium  
## [10549] Ideal     Premium   Ideal     Very Good Ideal     Very Good
## [10555] Premium   Ideal     Premium   Very Good Very Good Very Good
## [10561] Premium   Ideal     Good      Ideal     Premium   Premium  
## [10567] Premium   Premium   Premium   Premium   Good      Premium  
## [10573] Premium   Ideal     Good      Premium   Ideal     Premium  
## [10579] Ideal     Premium   Very Good Very Good Very Good Very Good
## [10585] Ideal     Very Good Good      Ideal     Good      Very Good
## [10591] Very Good Very Good Very Good Good      Ideal     Very Good
## [10597] Good      Good      Ideal     Fair      Ideal     Ideal    
## [10603] Very Good Very Good Very Good Good      Premium   Premium  
## [10609] Very Good Ideal     Very Good Premium   Good      Good     
## [10615] Ideal     Premium   Ideal     Ideal     Ideal     Premium  
## [10621] Fair      Premium   Ideal     Premium   Ideal     Very Good
## [10627] Very Good Ideal     Ideal     Premium   Premium   Ideal    
## [10633] Very Good Premium   Premium   Very Good Ideal     Ideal    
## [10639] Good      Premium   Ideal     Premium   Premium   Very Good
## [10645] Very Good Ideal     Very Good Very Good Good      Premium  
## [10651] Ideal     Premium   Premium   Ideal     Ideal     Ideal    
## [10657] Fair      Good      Ideal     Premium   Very Good Premium  
## [10663] Ideal     Premium   Fair      Premium   Premium   Ideal    
## [10669] Premium   Good      Premium   Ideal     Very Good Very Good
## [10675] Ideal     Ideal     Ideal     Premium   Very Good Very Good
## [10681] Premium   Premium   Good      Ideal     Very Good Very Good
## [10687] Good      Good      Fair      Fair      Premium   Ideal    
## [10693] Good      Fair      Ideal     Premium   Very Good Very Good
## [10699] Ideal     Very Good Very Good Good      Good      Very Good
## [10705] Very Good Very Good Very Good Ideal     Fair      Fair     
## [10711] Premium   Premium   Ideal     Premium   Ideal     Good     
## [10717] Very Good Ideal     Ideal     Ideal     Premium   Ideal    
## [10723] Good      Premium   Ideal     Premium   Good      Premium  
## [10729] Ideal     Premium   Very Good Premium   Premium   Premium  
## [10735] Ideal     Premium   Ideal     Premium   Ideal     Premium  
## [10741] Very Good Very Good Very Good Very Good Good      Premium  
## [10747] Very Good Good      Ideal     Ideal     Very Good Very Good
## [10753] Ideal     Very Good Very Good Good      Premium   Good     
## [10759] Premium   Good      Fair      Ideal     Premium   Premium  
## [10765] Very Good Ideal     Ideal     Ideal     Ideal     Premium  
## [10771] Premium   Very Good Ideal     Premium   Premium   Premium  
## [10777] Very Good Premium   Very Good Very Good Ideal     Ideal    
## [10783] Premium   Ideal     Good      Ideal     Ideal     Fair     
## [10789] Ideal     Fair      Very Good Very Good Good      Very Good
## [10795] Ideal     Premium   Good      Ideal     Premium   Good     
## [10801] Ideal     Premium   Ideal     Premium   Very Good Premium  
## [10807] Good      Premium   Ideal     Very Good Very Good Ideal    
## [10813] Premium   Fair      Premium   Good      Ideal     Ideal    
## [10819] Very Good Very Good Ideal     Ideal     Very Good Ideal    
## [10825] Very Good Very Good Ideal     Good      Ideal     Very Good
## [10831] Ideal     Ideal     Ideal     Ideal     Very Good Premium  
## [10837] Ideal     Ideal     Very Good Premium   Ideal     Premium  
## [10843] Premium   Premium   Premium   Premium   Ideal     Premium  
## [10849] Ideal     Premium   Ideal     Very Good Very Good Ideal    
## [10855] Premium   Premium   Premium   Fair      Premium   Ideal    
## [10861] Ideal     Very Good Very Good Premium   Ideal     Very Good
## [10867] Very Good Premium   Ideal     Ideal     Good      Premium  
## [10873] Very Good Good      Good      Very Good Premium   Premium  
## [10879] Very Good Ideal     Very Good Premium   Ideal     Premium  
## [10885] Premium   Premium   Ideal     Ideal     Ideal     Good     
## [10891] Premium   Ideal     Ideal     Premium   Premium   Premium  
## [10897] Ideal     Premium   Premium   Very Good Premium   Premium  
## [10903] Ideal     Ideal     Very Good Ideal     Good      Fair     
## [10909] Fair      Very Good Premium   Premium   Very Good Very Good
## [10915] Very Good Ideal     Ideal     Ideal     Premium   Ideal    
## [10921] Ideal     Very Good Fair      Premium   Premium   Very Good
## [10927] Ideal     Ideal     Premium   Very Good Very Good Very Good
## [10933] Ideal     Ideal     Very Good Good      Fair      Very Good
## [10939] Very Good Good      Good      Very Good Good      Good     
## [10945] Good      Premium   Premium   Ideal     Ideal     Ideal    
## [10951] Very Good Ideal     Good      Premium   Ideal     Premium  
## [10957] Ideal     Very Good Very Good Very Good Very Good Fair     
## [10963] Premium   Very Good Premium   Very Good Very Good Very Good
## [10969] Very Good Very Good Ideal     Premium   Premium   Premium  
## [10975] Premium   Good      Premium   Good      Ideal     Premium  
## [10981] Ideal     Premium   Ideal     Very Good Good      Good     
## [10987] Very Good Very Good Premium   Ideal     Very Good Premium  
## [10993] Very Good Very Good Very Good Ideal     Good      Premium  
## [10999] Good      Premium   Good      Ideal     Premium   Premium  
## [11005] Fair      Very Good Very Good Premium   Premium   Premium  
## [11011] Premium   Very Good Ideal     Ideal     Very Good Premium  
## [11017] Ideal     Ideal     Good      Good      Fair      Premium  
## [11023] Ideal     Fair      Premium   Ideal     Very Good Ideal    
## [11029] Ideal     Premium   Ideal     Very Good Very Good Ideal    
## [11035] Premium   Good      Premium   Very Good Ideal     Premium  
## [11041] Premium   Ideal     Ideal     Ideal     Ideal     Ideal    
## [11047] Ideal     Ideal     Good      Very Good Very Good Very Good
## [11053] Very Good Very Good Very Good Ideal     Ideal     Ideal    
## [11059] Ideal     Ideal     Ideal     Good      Good      Premium  
## [11065] Good      Premium   Very Good Ideal     Ideal     Ideal    
## [11071] Ideal     Premium   Very Good Very Good Very Good Ideal    
## [11077] Ideal     Very Good Ideal     Ideal     Very Good Very Good
## [11083] Ideal     Fair      Ideal     Fair      Ideal     Ideal    
## [11089] Ideal     Good      Good      Ideal     Ideal     Ideal    
## [11095] Fair      Premium   Premium   Very Good Premium   Ideal    
## [11101] Ideal     Ideal     Good      Very Good Very Good Ideal    
## [11107] Ideal     Premium   Very Good Premium   Very Good Premium  
## [11113] Very Good Fair      Good      Good      Premium   Ideal    
## [11119] Ideal     Good      Ideal     Ideal     Ideal     Very Good
## [11125] Very Good Ideal     Ideal     Very Good Fair      Fair     
## [11131] Premium   Ideal     Ideal     Premium   Premium   Premium  
## [11137] Premium   Premium   Very Good Premium   Good      Premium  
## [11143] Very Good Ideal     Ideal     Ideal     Premium   Premium  
## [11149] Ideal     Premium   Premium   Ideal     Premium   Ideal    
## [11155] Premium   Very Good Very Good Ideal     Premium   Good     
## [11161] Good      Very Good Very Good Premium   Premium   Premium  
## [11167] Very Good Premium   Premium   Premium   Very Good Premium  
## [11173] Ideal     Ideal     Ideal     Very Good Very Good Good     
## [11179] Ideal     Premium   Premium   Premium   Ideal     Ideal    
## [11185] Ideal     Premium   Ideal     Ideal     Premium   Premium  
## [11191] Premium   Premium   Ideal     Ideal     Ideal     Premium  
## [11197] Very Good Premium   Ideal     Ideal     Premium   Premium  
## [11203] Very Good Very Good Ideal     Very Good Premium   Premium  
## [11209] Good      Premium   Premium   Ideal     Ideal     Premium  
## [11215] Very Good Very Good Very Good Premium   Premium   Very Good
## [11221] Good      Ideal     Very Good Very Good Good      Very Good
## [11227] Ideal     Good      Very Good Very Good Very Good Very Good
## [11233] Ideal     Premium   Ideal     Very Good Ideal     Premium  
## [11239] Premium   Good      Ideal     Fair      Premium   Premium  
## [11245] Good      Very Good Good      Ideal     Ideal     Ideal    
## [11251] Premium   Ideal     Ideal     Very Good Very Good Premium  
## [11257] Very Good Ideal     Ideal     Premium   Premium   Premium  
## [11263] Premium   Ideal     Premium   Very Good Ideal     Ideal    
## [11269] Very Good Very Good Ideal     Premium   Fair      Very Good
## [11275] Premium   Ideal     Premium   Ideal     Premium   Premium  
## [11281] Ideal     Premium   Premium   Premium   Very Good Very Good
## [11287] Ideal     Premium   Very Good Good      Very Good Premium  
## [11293] Ideal     Very Good Very Good Very Good Premium   Premium  
## [11299] Premium   Premium   Premium   Premium   Premium   Premium  
## [11305] Very Good Ideal     Ideal     Ideal     Premium   Ideal    
## [11311] Ideal     Premium   Fair      Premium   Ideal     Ideal    
## [11317] Very Good Good      Very Good Premium   Premium   Ideal    
## [11323] Ideal     Ideal     Premium   Ideal     Premium   Ideal    
## [11329] Premium   Good      Very Good Premium   Premium   Ideal    
## [11335] Good      Good      Good      Premium   Very Good Good     
## [11341] Premium   Good      Good      Premium   Good      Premium  
## [11347] Very Good Premium   Ideal     Very Good Premium   Ideal    
## [11353] Very Good Premium   Premium   Good      Very Good Very Good
## [11359] Premium   Very Good Very Good Very Good Very Good Very Good
## [11365] Very Good Good      Very Good Ideal     Ideal     Good     
## [11371] Very Good Ideal     Ideal     Ideal     Premium   Ideal    
## [11377] Very Good Very Good Good      Very Good Good      Ideal    
## [11383] Premium   Premium   Very Good Ideal     Premium   Ideal    
## [11389] Ideal     Very Good Ideal     Ideal     Ideal     Premium  
## [11395] Very Good Ideal     Very Good Ideal     Premium   Ideal    
## [11401] Ideal     Premium   Ideal     Very Good Ideal     Ideal    
## [11407] Very Good Very Good Very Good Ideal     Premium   Premium  
## [11413] Premium   Premium   Ideal     Fair      Ideal     Ideal    
## [11419] Good      Very Good Premium   Ideal     Ideal     Ideal    
## [11425] Good      Good      Very Good Very Good Premium   Premium  
## [11431] Ideal     Premium   Premium   Ideal     Premium   Very Good
## [11437] Good      Good      Ideal     Good      Ideal     Premium  
## [11443] Ideal     Very Good Ideal     Premium   Premium   Premium  
## [11449] Premium   Ideal     Ideal     Premium   Ideal     Premium  
## [11455] Ideal     Ideal     Premium   Premium   Premium   Ideal    
## [11461] Very Good Very Good Very Good Very Good Very Good Good     
## [11467] Premium   Ideal     Premium   Ideal     Very Good Ideal    
## [11473] Ideal     Ideal     Ideal     Good      Very Good Very Good
## [11479] Very Good Ideal     Ideal     Very Good Premium   Premium  
## [11485] Good      Very Good Very Good Ideal     Ideal     Premium  
## [11491] Very Good Very Good Premium   Very Good Good      Ideal    
## [11497] Good      Ideal     Ideal     Very Good Very Good Ideal    
## [11503] Ideal     Ideal     Very Good Very Good Ideal     Ideal    
## [11509] Ideal     Ideal     Good      Good      Ideal     Ideal    
## [11515] Ideal     Ideal     Ideal     Ideal     Ideal     Very Good
## [11521] Very Good Very Good Very Good Premium   Premium   Premium  
## [11527] Premium   Ideal     Premium   Premium   Fair      Ideal    
## [11533] Very Good Ideal     Premium   Premium   Very Good Ideal    
## [11539] Ideal     Fair      Premium   Good      Good      Very Good
## [11545] Very Good Good      Ideal     Very Good Ideal     Premium  
## [11551] Ideal     Ideal     Ideal     Premium   Very Good Very Good
## [11557] Premium   Premium   Very Good Premium   Ideal     Very Good
## [11563] Fair      Very Good Ideal     Ideal     Very Good Very Good
## [11569] Ideal     Very Good Ideal     Premium   Premium   Ideal    
## [11575] Very Good Very Good Premium   Premium   Premium   Very Good
## [11581] Good      Premium   Ideal     Fair      Ideal     Very Good
## [11587] Premium   Fair      Very Good Fair      Premium   Very Good
## [11593] Very Good Ideal     Premium   Premium   Premium   Very Good
## [11599] Ideal     Ideal     Ideal     Premium   Good      Very Good
## [11605] Premium   Very Good Very Good Ideal     Very Good Very Good
## [11611] Very Good Very Good Ideal     Premium   Very Good Ideal    
## [11617] Good      Ideal     Very Good Ideal     Very Good Premium  
## [11623] Ideal     Very Good Very Good Premium   Premium   Good     
## [11629] Premium   Good      Ideal     Very Good Very Good Very Good
## [11635] Premium   Ideal     Ideal     Ideal     Good      Good     
## [11641] Ideal     Very Good Very Good Ideal     Good      Very Good
## [11647] Premium   Good      Good      Good      Very Good Very Good
## [11653] Very Good Good      Ideal     Good      Ideal     Premium  
## [11659] Premium   Ideal     Premium   Premium   Premium   Premium  
## [11665] Premium   Good      Good      Very Good Premium   Ideal    
## [11671] Premium   Very Good Ideal     Premium   Very Good Premium  
## [11677] Very Good Premium   Premium   Premium   Good      Ideal    
## [11683] Ideal     Fair      Ideal     Premium   Very Good Ideal    
## [11689] Ideal     Ideal     Ideal     Very Good Very Good Very Good
## [11695] Very Good Very Good Ideal     Ideal     Good      Very Good
## [11701] Premium   Premium   Premium   Premium   Ideal     Premium  
## [11707] Premium   Ideal     Premium   Premium   Very Good Very Good
## [11713] Ideal     Ideal     Ideal     Premium   Ideal     Good     
## [11719] Premium   Ideal     Ideal     Ideal     Very Good Ideal    
## [11725] Good      Good      Ideal     Ideal     Ideal     Premium  
## [11731] Very Good Good      Ideal     Premium   Very Good Ideal    
## [11737] Premium   Very Good Premium   Very Good Very Good Premium  
## [11743] Very Good Ideal     Fair      Ideal     Premium   Premium  
## [11749] Very Good Ideal     Very Good Premium   Ideal     Premium  
## [11755] Very Good Very Good Premium   Fair      Ideal     Premium  
## [11761] Premium   Premium   Ideal     Ideal     Ideal     Ideal    
## [11767] Very Good Good      Very Good Very Good Good      Very Good
## [11773] Premium   Premium   Premium   Ideal     Premium   Very Good
## [11779] Fair      Ideal     Premium   Very Good Good      Fair     
## [11785] Very Good Very Good Ideal     Premium   Premium   Premium  
## [11791] Premium   Premium   Premium   Very Good Very Good Very Good
## [11797] Good      Premium   Premium   Ideal     Premium   Premium  
## [11803] Ideal     Premium   Fair      Very Good Premium   Premium  
## [11809] Premium   Premium   Premium   Ideal     Ideal     Premium  
## [11815] Premium   Premium   Ideal     Ideal     Good      Ideal    
## [11821] Ideal     Ideal     Good      Premium   Ideal     Good     
## [11827] Ideal     Fair      Premium   Ideal     Good      Premium  
## [11833] Premium   Premium   Premium   Premium   Good      Ideal    
## [11839] Premium   Premium   Ideal     Ideal     Premium   Very Good
## [11845] Premium   Very Good Ideal     Premium   Premium   Premium  
## [11851] Very Good Very Good Ideal     Ideal     Premium   Ideal    
## [11857] Premium   Premium   Ideal     Ideal     Premium   Ideal    
## [11863] Premium   Ideal     Fair      Very Good Very Good Premium  
## [11869] Ideal     Ideal     Premium   Premium   Ideal     Very Good
## [11875] Good      Ideal     Ideal     Good      Very Good Very Good
## [11881] Ideal     Premium   Ideal     Very Good Ideal     Premium  
## [11887] Very Good Good      Very Good Premium   Premium   Very Good
## [11893] Ideal     Very Good Very Good Ideal     Premium   Very Good
## [11899] Very Good Premium   Ideal     Premium   Premium   Premium  
## [11905] Very Good Premium   Very Good Very Good Very Good Ideal    
## [11911] Fair      Premium   Very Good Premium   Premium   Premium  
## [11917] Ideal     Ideal     Ideal     Good      Ideal     Ideal    
## [11923] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [11929] Ideal     Ideal     Ideal     Ideal     Premium   Ideal    
## [11935] Ideal     Premium   Premium   Ideal     Premium   Very Good
## [11941] Very Good Premium   Fair      Good      Good      Good     
## [11947] Very Good Good      Very Good Very Good Ideal     Premium  
## [11953] Premium   Premium   Premium   Premium   Premium   Very Good
## [11959] Very Good Premium   Very Good Premium   Premium   Very Good
## [11965] Premium   Premium   Ideal     Fair      Premium   Premium  
## [11971] Very Good Ideal     Premium   Premium   Very Good Good     
## [11977] Premium   Ideal     Ideal     Ideal     Ideal     Premium  
## [11983] Premium   Very Good Very Good Very Good Ideal     Ideal    
## [11989] Ideal     Fair      Very Good Ideal     Ideal     Good     
## [11995] Ideal     Ideal     Very Good Ideal     Good      Good     
## [12001] Good      Premium   Premium   Premium   Premium   Fair     
## [12007] Good      Very Good Very Good Premium   Ideal     Ideal    
## [12013] Premium   Ideal     Very Good Ideal     Premium   Premium  
## [12019] Premium   Very Good Premium   Premium   Ideal     Ideal    
## [12025] Premium   Premium   Premium   Ideal     Ideal     Ideal    
## [12031] Ideal     Good      Ideal     Premium   Ideal     Premium  
## [12037] Ideal     Very Good Very Good Premium   Very Good Good     
## [12043] Good      Ideal     Good      Premium   Good      Very Good
## [12049] Ideal     Ideal     Good      Ideal     Premium   Premium  
## [12055] Very Good Premium   Very Good Very Good Very Good Very Good
## [12061] Ideal     Ideal     Ideal     Ideal     Premium   Premium  
## [12067] Premium   Premium   Very Good Very Good Ideal     Premium  
## [12073] Ideal     Premium   Premium   Ideal     Ideal     Ideal    
## [12079] Ideal     Ideal     Ideal     Ideal     Very Good Very Good
## [12085] Ideal     Fair      Premium   Premium   Premium   Ideal    
## [12091] Very Good Very Good Ideal     Ideal     Ideal     Premium  
## [12097] Ideal     Ideal     Premium   Premium   Very Good Very Good
## [12103] Ideal     Premium   Premium   Premium   Ideal     Good     
## [12109] Premium   Premium   Premium   Very Good Ideal     Premium  
## [12115] Premium   Premium   Very Good Ideal     Good      Premium  
## [12121] Premium   Premium   Premium   Good      Very Good Premium  
## [12127] Ideal     Premium   Premium   Ideal     Ideal     Fair     
## [12133] Premium   Premium   Ideal     Very Good Ideal     Premium  
## [12139] Very Good Very Good Premium   Ideal     Very Good Very Good
## [12145] Ideal     Ideal     Good      Good      Premium   Premium  
## [12151] Premium   Ideal     Premium   Ideal     Good      Very Good
## [12157] Premium   Ideal     Premium   Ideal     Premium   Ideal    
## [12163] Fair      Very Good Premium   Premium   Ideal     Ideal    
## [12169] Premium   Premium   Very Good Ideal     Ideal     Ideal    
## [12175] Ideal     Very Good Premium   Very Good Premium   Good     
## [12181] Premium   Premium   Ideal     Premium   Premium   Ideal    
## [12187] Fair      Very Good Very Good Premium   Fair      Very Good
## [12193] Ideal     Ideal     Very Good Ideal     Ideal     Premium  
## [12199] Very Good Very Good Premium   Premium   Very Good Premium  
## [12205] Very Good Very Good Very Good Ideal     Ideal     Very Good
## [12211] Premium   Very Good Premium   Premium   Ideal     Very Good
## [12217] Ideal     Very Good Premium   Premium   Premium   Ideal    
## [12223] Ideal     Premium   Premium   Ideal     Good      Premium  
## [12229] Premium   Very Good Very Good Good      Good      Fair     
## [12235] Premium   Very Good Good      Premium   Ideal     Ideal    
## [12241] Ideal     Ideal     Good      Very Good Premium   Premium  
## [12247] Premium   Premium   Very Good Ideal     Ideal     Premium  
## [12253] Ideal     Premium   Premium   Ideal     Ideal     Ideal    
## [12259] Ideal     Premium   Ideal     Ideal     Ideal     Premium  
## [12265] Premium   Good      Good      Fair      Premium   Good     
## [12271] Premium   Very Good Premium   Very Good Very Good Good     
## [12277] Very Good Very Good Good      Good      Fair      Ideal    
## [12283] Premium   Very Good Ideal     Very Good Ideal     Very Good
## [12289] Very Good Premium   Premium   Ideal     Premium   Premium  
## [12295] Very Good Very Good Premium   Ideal     Premium   Premium  
## [12301] Premium   Good      Good      Ideal     Ideal     Ideal    
## [12307] Ideal     Premium   Very Good Ideal     Ideal     Very Good
## [12313] Ideal     Premium   Ideal     Good      Very Good Ideal    
## [12319] Very Good Ideal     Premium   Ideal     Very Good Very Good
## [12325] Very Good Very Good Good      Very Good Ideal     Very Good
## [12331] Very Good Good      Very Good Good      Premium   Premium  
## [12337] Ideal     Premium   Ideal     Premium   Premium   Ideal    
## [12343] Premium   Very Good Fair      Very Good Very Good Very Good
## [12349] Premium   Premium   Premium   Very Good Ideal     Good     
## [12355] Very Good Very Good Very Good Very Good Ideal     Ideal    
## [12361] Very Good Very Good Very Good Very Good Very Good Very Good
## [12367] Very Good Very Good Very Good Ideal     Ideal     Ideal    
## [12373] Ideal     Ideal     Ideal     Ideal     Good      Good     
## [12379] Ideal     Very Good Very Good Ideal     Ideal     Ideal    
## [12385] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [12391] Very Good Premium   Ideal     Ideal     Good      Ideal    
## [12397] Premium   Very Good Ideal     Premium   Premium   Very Good
## [12403] Good      Good      Very Good Ideal     Ideal     Ideal    
## [12409] Ideal     Ideal     Very Good Good      Very Good Very Good
## [12415] Premium   Premium   Ideal     Premium   Premium   Ideal    
## [12421] Ideal     Very Good Ideal     Fair      Ideal     Premium  
## [12427] Very Good Very Good Ideal     Ideal     Premium   Good     
## [12433] Premium   Very Good Ideal     Good      Ideal     Good     
## [12439] Fair      Ideal     Premium   Ideal     Ideal     Premium  
## [12445] Premium   Ideal     Premium   Very Good Ideal     Premium  
## [12451] Very Good Very Good Very Good Ideal     Very Good Very Good
## [12457] Ideal     Premium   Very Good Very Good Premium   Premium  
## [12463] Premium   Premium   Ideal     Ideal     Ideal     Premium  
## [12469] Premium   Very Good Premium   Premium   Premium   Ideal    
## [12475] Ideal     Good      Good      Very Good Ideal     Ideal    
## [12481] Ideal     Good      Ideal     Ideal     Very Good Good     
## [12487] Very Good Ideal     Very Good Premium   Fair      Ideal    
## [12493] Ideal     Very Good Very Good Very Good Very Good Very Good
## [12499] Very Good Ideal     Premium   Very Good Premium   Ideal    
## [12505] Premium   Very Good Premium   Ideal     Ideal     Ideal    
## [12511] Good      Premium   Very Good Ideal     Premium   Premium  
## [12517] Very Good Ideal     Premium   Ideal     Ideal     Ideal    
## [12523] Ideal     Ideal     Ideal     Very Good Ideal     Premium  
## [12529] Premium   Very Good Premium   Very Good Premium   Premium  
## [12535] Ideal     Very Good Ideal     Premium   Premium   Fair     
## [12541] Ideal     Premium   Good      Premium   Ideal     Premium  
## [12547] Very Good Good      Good      Ideal     Ideal     Premium  
## [12553] Premium   Ideal     Ideal     Ideal     Premium   Ideal    
## [12559] Ideal     Good      Premium   Premium   Ideal     Ideal    
## [12565] Ideal     Ideal     Premium   Very Good Premium   Premium  
## [12571] Good      Premium   Very Good Good      Premium   Very Good
## [12577] Very Good Premium   Very Good Premium   Very Good Ideal    
## [12583] Ideal     Ideal     Premium   Premium   Premium   Premium  
## [12589] Premium   Premium   Good      Very Good Ideal     Premium  
## [12595] Premium   Premium   Very Good Premium   Premium   Very Good
## [12601] Very Good Ideal     Ideal     Premium   Premium   Premium  
## [12607] Good      Good      Very Good Very Good Very Good Premium  
## [12613] Very Good Premium   Ideal     Premium   Premium   Ideal    
## [12619] Fair      Premium   Premium   Premium   Premium   Premium  
## [12625] Fair      Very Good Ideal     Premium   Premium   Ideal    
## [12631] Premium   Premium   Premium   Premium   Premium   Premium  
## [12637] Premium   Ideal     Very Good Premium   Very Good Good     
## [12643] Premium   Very Good Premium   Fair      Ideal     Premium  
## [12649] Very Good Ideal     Ideal     Ideal     Ideal     Good     
## [12655] Premium   Ideal     Premium   Premium   Premium   Good     
## [12661] Very Good Ideal     Premium   Ideal     Ideal     Very Good
## [12667] Very Good Ideal     Premium   Ideal     Very Good Premium  
## [12673] Very Good Very Good Very Good Premium   Premium   Premium  
## [12679] Ideal     Ideal     Ideal     Ideal     Ideal     Very Good
## [12685] Good      Good      Premium   Premium   Premium   Premium  
## [12691] Ideal     Very Good Very Good Ideal     Ideal     Ideal    
## [12697] Ideal     Ideal     Fair      Ideal     Good      Ideal    
## [12703] Very Good Ideal     Ideal     Premium   Premium   Ideal    
## [12709] Premium   Ideal     Ideal     Ideal     Ideal     Ideal    
## [12715] Ideal     Ideal     Good      Ideal     Ideal     Very Good
## [12721] Premium   Ideal     Premium   Premium   Premium   Premium  
## [12727] Ideal     Premium   Good      Very Good Ideal     Good     
## [12733] Ideal     Good      Premium   Premium   Ideal     Ideal    
## [12739] Premium   Premium   Premium   Ideal     Good      Ideal    
## [12745] Ideal     Premium   Good      Ideal     Premium   Very Good
## [12751] Very Good Premium   Very Good Ideal     Ideal     Premium  
## [12757] Very Good Very Good Premium   Premium   Premium   Very Good
## [12763] Very Good Ideal     Very Good Ideal     Ideal     Premium  
## [12769] Premium   Very Good Ideal     Premium   Very Good Ideal    
## [12775] Premium   Premium   Ideal     Very Good Very Good Good     
## [12781] Ideal     Premium   Ideal     Very Good Ideal     Good     
## [12787] Fair      Good      Ideal     Ideal     Premium   Premium  
## [12793] Fair      Very Good Ideal     Ideal     Very Good Very Good
## [12799] Premium   Ideal     Ideal     Ideal     Premium   Very Good
## [12805] Very Good Ideal     Very Good Ideal     Very Good Very Good
## [12811] Ideal     Ideal     Ideal     Premium   Good      Premium  
## [12817] Premium   Premium   Premium   Ideal     Ideal     Very Good
## [12823] Premium   Premium   Ideal     Ideal     Ideal     Ideal    
## [12829] Premium   Fair      Very Good Very Good Premium   Premium  
## [12835] Very Good Very Good Ideal     Ideal     Ideal     Very Good
## [12841] Very Good Premium   Ideal     Ideal     Ideal     Premium  
## [12847] Ideal     Good      Good      Ideal     Ideal     Ideal    
## [12853] Ideal     Ideal     Premium   Ideal     Ideal     Very Good
## [12859] Ideal     Ideal     Ideal     Good      Ideal     Very Good
## [12865] Ideal     Ideal     Premium   Premium   Ideal     Very Good
## [12871] Ideal     Premium   Ideal     Premium   Premium   Premium  
## [12877] Ideal     Premium   Premium   Premium   Very Good Very Good
## [12883] Ideal     Premium   Premium   Premium   Very Good Premium  
## [12889] Very Good Ideal     Premium   Very Good Very Good Very Good
## [12895] Ideal     Very Good Ideal     Ideal     Ideal     Ideal    
## [12901] Ideal     Ideal     Premium   Very Good Very Good Very Good
## [12907] Ideal     Ideal     Premium   Very Good Ideal     Fair     
## [12913] Premium   Premium   Premium   Ideal     Ideal     Premium  
## [12919] Premium   Premium   Premium   Good      Ideal     Ideal    
## [12925] Ideal     Good      Ideal     Good      Very Good Premium  
## [12931] Very Good Ideal     Very Good Very Good Very Good Ideal    
## [12937] Good      Premium   Premium   Premium   Very Good Ideal    
## [12943] Ideal     Fair      Fair      Very Good Ideal     Ideal    
## [12949] Good      Very Good Ideal     Ideal     Ideal     Ideal    
## [12955] Premium   Very Good Premium   Premium   Ideal     Ideal    
## [12961] Ideal     Very Good Very Good Very Good Premium   Very Good
## [12967] Good      Premium   Ideal     Ideal     Ideal     Very Good
## [12973] Very Good Very Good Very Good Ideal     Premium   Good     
## [12979] Very Good Ideal     Ideal     Premium   Premium   Ideal    
## [12985] Premium   Premium   Very Good Ideal     Premium   Very Good
## [12991] Ideal     Very Good Premium   Premium   Ideal     Ideal    
## [12997] Premium   Premium   Ideal     Premium   Premium   Premium  
## [13003] Fair      Ideal     Premium   Ideal     Ideal     Very Good
## [13009] Good      Premium   Good      Ideal     Ideal     Good     
## [13015] Very Good Very Good Very Good Very Good Very Good Ideal    
## [13021] Very Good Ideal     Ideal     Good      Very Good Premium  
## [13027] Ideal     Premium   Ideal     Very Good Premium   Good     
## [13033] Premium   Premium   Premium   Very Good Very Good Premium  
## [13039] Premium   Ideal     Ideal     Very Good Premium   Ideal    
## [13045] Premium   Ideal     Very Good Very Good Ideal     Ideal    
## [13051] Premium   Premium   Ideal     Premium   Premium   Premium  
## [13057] Premium   Very Good Very Good Ideal     Ideal     Ideal    
## [13063] Good      Fair      Ideal     Good      Ideal     Ideal    
## [13069] Premium   Premium   Ideal     Ideal     Ideal     Ideal    
## [13075] Ideal     Premium   Very Good Very Good Good      Very Good
## [13081] Premium   Ideal     Good      Premium   Ideal     Ideal    
## [13087] Ideal     Good      Premium   Ideal     Premium   Good     
## [13093] Premium   Premium   Ideal     Premium   Premium   Ideal    
## [13099] Premium   Very Good Very Good Ideal     Very Good Ideal    
## [13105] Premium   Premium   Ideal     Ideal     Ideal     Premium  
## [13111] Very Good Premium   Ideal     Ideal     Premium   Good     
## [13117] Premium   Very Good Fair      Ideal     Fair      Premium  
## [13123] Fair      Premium   Good      Good      Premium   Ideal    
## [13129] Premium   Very Good Very Good Premium   Premium   Premium  
## [13135] Premium   Premium   Very Good Ideal     Ideal     Premium  
## [13141] Ideal     Premium   Ideal     Premium   Very Good Premium  
## [13147] Good      Premium   Ideal     Ideal     Ideal     Premium  
## [13153] Premium   Ideal     Good      Premium   Good      Ideal    
## [13159] Very Good Good      Good      Very Good Very Good Ideal    
## [13165] Ideal     Very Good Ideal     Premium   Premium   Premium  
## [13171] Very Good Fair      Ideal     Premium   Good      Very Good
## [13177] Very Good Good      Premium   Very Good Very Good Ideal    
## [13183] Very Good Very Good Ideal     Very Good Ideal     Very Good
## [13189] Very Good Ideal     Very Good Premium   Ideal     Premium  
## [13195] Premium   Very Good Very Good Very Good Ideal     Premium  
## [13201] Ideal     Very Good Very Good Very Good Ideal     Ideal    
## [13207] Ideal     Very Good Ideal     Ideal     Ideal     Ideal    
## [13213] Premium   Premium   Ideal     Ideal     Good      Premium  
## [13219] Premium   Very Good Ideal     Fair      Fair      Premium  
## [13225] Very Good Very Good Ideal     Premium   Premium   Very Good
## [13231] Premium   Premium   Premium   Premium   Premium   Premium  
## [13237] Premium   Premium   Premium   Premium   Ideal     Ideal    
## [13243] Good      Premium   Ideal     Very Good Premium   Fair     
## [13249] Very Good Very Good Ideal     Ideal     Premium   Premium  
## [13255] Ideal     Very Good Very Good Very Good Good      Premium  
## [13261] Premium   Very Good Good      Premium   Ideal     Good     
## [13267] Premium   Very Good Good      Good      Fair      Fair     
## [13273] Ideal     Premium   Premium   Very Good Ideal     Ideal    
## [13279] Good      Good      Fair      Premium   Premium   Fair     
## [13285] Premium   Premium   Premium   Premium   Premium   Ideal    
## [13291] Ideal     Premium   Very Good Good      Very Good Very Good
## [13297] Ideal     Premium   Ideal     Good      Ideal     Very Good
## [13303] Ideal     Ideal     Ideal     Ideal     Good      Ideal    
## [13309] Good      Very Good Very Good Ideal     Good      Very Good
## [13315] Ideal     Premium   Very Good Good      Very Good Very Good
## [13321] Premium   Premium   Good      Very Good Very Good Ideal    
## [13327] Good      Fair      Ideal     Very Good Premium   Ideal    
## [13333] Ideal     Premium   Very Good Very Good Ideal     Premium  
## [13339] Premium   Ideal     Ideal     Fair      Premium   Good     
## [13345] Very Good Ideal     Premium   Ideal     Premium   Very Good
## [13351] Very Good Very Good Very Good Very Good Very Good Very Good
## [13357] Very Good Very Good Very Good Very Good Very Good Very Good
## [13363] Very Good Very Good Very Good Very Good Very Good Very Good
## [13369] Very Good Very Good Very Good Very Good Very Good Ideal    
## [13375] Ideal     Ideal     Very Good Very Good Ideal     Ideal    
## [13381] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [13387] Fair      Good      Premium   Ideal     Ideal     Ideal    
## [13393] Ideal     Very Good Good      Ideal     Very Good Ideal    
## [13399] Ideal     Very Good Ideal     Very Good Premium   Very Good
## [13405] Premium   Very Good Very Good Very Good Very Good Ideal    
## [13411] Very Good Ideal     Ideal     Ideal     Premium   Premium  
## [13417] Ideal     Ideal     Ideal     Premium   Premium   Very Good
## [13423] Very Good Very Good Good      Very Good Premium   Good     
## [13429] Premium   Very Good Ideal     Ideal     Ideal     Premium  
## [13435] Very Good Ideal     Very Good Ideal     Ideal     Premium  
## [13441] Very Good Premium   Ideal     Premium   Very Good Very Good
## [13447] Ideal     Very Good Very Good Ideal     Premium   Premium  
## [13453] Very Good Very Good Fair      Very Good Premium   Ideal    
## [13459] Good      Very Good Ideal     Ideal     Very Good Ideal    
## [13465] Premium   Very Good Very Good Very Good Ideal     Premium  
## [13471] Premium   Ideal     Very Good Fair      Very Good Premium  
## [13477] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [13483] Premium   Ideal     Ideal     Ideal     Good      Premium  
## [13489] Ideal     Very Good Very Good Good      Fair      Premium  
## [13495] Very Good Ideal     Ideal     Ideal     Ideal     Ideal    
## [13501] Ideal     Premium   Premium   Very Good Ideal     Premium  
## [13507] Premium   Premium   Good      Very Good Premium   Premium  
## [13513] Premium   Premium   Premium   Premium   Very Good Premium  
## [13519] Good      Very Good Good      Good      Good      Good     
## [13525] Ideal     Premium   Premium   Premium   Ideal     Premium  
## [13531] Ideal     Premium   Ideal     Good      Premium   Ideal    
## [13537] Premium   Ideal     Very Good Premium   Ideal     Ideal    
## [13543] Premium   Very Good Very Good Ideal     Premium   Premium  
## [13549] Very Good Premium   Ideal     Very Good Very Good Ideal    
## [13555] Ideal     Fair      Ideal     Premium   Premium   Ideal    
## [13561] Ideal     Very Good Fair      Premium   Ideal     Premium  
## [13567] Good      Ideal     Ideal     Ideal     Ideal     Ideal    
## [13573] Premium   Very Good Very Good Ideal     Premium   Premium  
## [13579] Premium   Ideal     Premium   Very Good Very Good Premium  
## [13585] Premium   Ideal     Premium   Very Good Very Good Ideal    
## [13591] Premium   Good      Very Good Premium   Good      Premium  
## [13597] Very Good Very Good Very Good Ideal     Ideal     Ideal    
## [13603] Very Good Very Good Very Good Good      Premium   Premium  
## [13609] Premium   Premium   Ideal     Very Good Good      Ideal    
## [13615] Very Good Ideal     Premium   Premium   Premium   Very Good
## [13621] Ideal     Premium   Premium   Good      Very Good Good     
## [13627] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [13633] Very Good Premium   Premium   Premium   Premium   Premium  
## [13639] Premium   Very Good Premium   Ideal     Premium   Very Good
## [13645] Very Good Very Good Very Good Ideal     Ideal     Very Good
## [13651] Very Good Ideal     Ideal     Very Good Good      Premium  
## [13657] Premium   Ideal     Ideal     Very Good Good      Premium  
## [13663] Good      Very Good Ideal     Very Good Premium   Very Good
## [13669] Very Good Premium   Very Good Ideal     Ideal     Ideal    
## [13675] Very Good Good      Very Good Good      Very Good Very Good
## [13681] Ideal     Premium   Good      Premium   Good      Good     
## [13687] Ideal     Premium   Ideal     Ideal     Premium   Premium  
## [13693] Premium   Premium   Very Good Very Good Very Good Ideal    
## [13699] Good      Good      Premium   Premium   Good      Very Good
## [13705] Good      Good      Premium   Ideal     Good      Premium  
## [13711] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [13717] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [13723] Ideal     Good      Good      Premium   Ideal     Ideal    
## [13729] Ideal     Premium   Ideal     Premium   Premium   Very Good
## [13735] Very Good Ideal     Ideal     Ideal     Premium   Ideal    
## [13741] Premium   Ideal     Ideal     Ideal     Ideal     Very Good
## [13747] Ideal     Very Good Good      Good      Premium   Ideal    
## [13753] Premium   Premium   Good      Premium   Very Good Fair     
## [13759] Ideal     Ideal     Very Good Ideal     Ideal     Good     
## [13765] Premium   Very Good Premium   Premium   Very Good Ideal    
## [13771] Ideal     Very Good Ideal     Very Good Good      Fair     
## [13777] Premium   Premium   Premium   Premium   Very Good Very Good
## [13783] Premium   Premium   Very Good Premium   Premium   Premium  
## [13789] Very Good Very Good Good      Very Good Very Good Ideal    
## [13795] Premium   Premium   Fair      Ideal     Very Good Premium  
## [13801] Premium   Premium   Ideal     Fair      Ideal     Very Good
## [13807] Ideal     Fair      Ideal     Ideal     Very Good Very Good
## [13813] Good      Very Good Very Good Fair      Premium   Ideal    
## [13819] Good      Ideal     Premium   Premium   Premium   Ideal    
## [13825] Fair      Very Good Ideal     Ideal     Ideal     Very Good
## [13831] Very Good Premium   Very Good Ideal     Very Good Ideal    
## [13837] Very Good Ideal     Premium   Premium   Premium   Ideal    
## [13843] Good      Premium   Premium   Very Good Premium   Good     
## [13849] Fair      Very Good Good      Very Good Good      Premium  
## [13855] Ideal     Premium   Premium   Premium   Premium   Very Good
## [13861] Very Good Very Good Good      Good      Premium   Ideal    
## [13867] Ideal     Good      Premium   Premium   Ideal     Ideal    
## [13873] Premium   Premium   Premium   Premium   Premium   Very Good
## [13879] Ideal     Ideal     Ideal     Very Good Premium   Very Good
## [13885] Ideal     Premium   Ideal     Ideal     Very Good Very Good
## [13891] Premium   Ideal     Ideal     Good      Ideal     Premium  
## [13897] Very Good Premium   Ideal     Premium   Ideal     Ideal    
## [13903] Very Good Premium   Very Good Very Good Ideal     Very Good
## [13909] Very Good Ideal     Premium   Ideal     Very Good Ideal    
## [13915] Ideal     Premium   Premium   Very Good Ideal     Very Good
## [13921] Very Good Ideal     Fair      Very Good Ideal     Premium  
## [13927] Premium   Ideal     Very Good Fair      Good      Good     
## [13933] Very Good Premium   Ideal     Ideal     Very Good Ideal    
## [13939] Very Good Very Good Premium   Premium   Premium   Very Good
## [13945] Fair      Premium   Premium   Very Good Premium   Ideal    
## [13951] Ideal     Ideal     Premium   Premium   Premium   Very Good
## [13957] Premium   Good      Very Good Fair      Premium   Ideal    
## [13963] Ideal     Very Good Ideal     Very Good Premium   Premium  
## [13969] Very Good Ideal     Very Good Very Good Premium   Good     
## [13975] Premium   Ideal     Premium   Ideal     Very Good Ideal    
## [13981] Ideal     Ideal     Ideal     Premium   Premium   Very Good
## [13987] Ideal     Premium   Ideal     Very Good Premium   Fair     
## [13993] Fair      Ideal     Ideal     Very Good Very Good Ideal    
## [13999] Ideal     Premium   Premium   Premium   Premium   Very Good
## [14005] Premium   Premium   Ideal     Premium   Good      Good     
## [14011] Premium   Very Good Very Good Good      Good      Very Good
## [14017] Good      Good      Premium   Premium   Good      Good     
## [14023] Very Good Very Good Ideal     Ideal     Ideal     Ideal    
## [14029] Premium   Ideal     Ideal     Very Good Premium   Premium  
## [14035] Ideal     Ideal     Ideal     Premium   Good      Ideal    
## [14041] Very Good Ideal     Ideal     Ideal     Ideal     Ideal    
## [14047] Ideal     Premium   Ideal     Ideal     Premium   Premium  
## [14053] Premium   Premium   Premium   Very Good Premium   Very Good
## [14059] Premium   Ideal     Ideal     Good      Ideal     Good     
## [14065] Ideal     Ideal     Ideal     Ideal     Very Good Good     
## [14071] Premium   Good      Premium   Very Good Ideal     Premium  
## [14077] Ideal     Very Good Premium   Premium   Ideal     Very Good
## [14083] Very Good Premium   Premium   Ideal     Ideal     Very Good
## [14089] Premium   Ideal     Premium   Premium   Ideal     Very Good
## [14095] Ideal     Premium   Premium   Premium   Premium   Very Good
## [14101] Premium   Ideal     Very Good Premium   Very Good Ideal    
## [14107] Premium   Ideal     Ideal     Premium   Very Good Good     
## [14113] Premium   Fair      Very Good Premium   Premium   Very Good
## [14119] Good      Premium   Very Good Very Good Ideal     Ideal    
## [14125] Ideal     Ideal     Ideal     Premium   Fair      Premium  
## [14131] Very Good Premium   Good      Very Good Very Good Ideal    
## [14137] Very Good Very Good Fair      Ideal     Very Good Very Good
## [14143] Premium   Premium   Premium   Very Good Premium   Premium  
## [14149] Premium   Premium   Premium   Premium   Premium   Premium  
## [14155] Very Good Ideal     Good      Premium   Premium   Premium  
## [14161] Ideal     Premium   Premium   Premium   Premium   Very Good
## [14167] Good      Premium   Ideal     Ideal     Ideal     Premium  
## [14173] Fair      Very Good Premium   Ideal     Premium   Ideal    
## [14179] Premium   Very Good Ideal     Very Good Very Good Ideal    
## [14185] Premium   Ideal     Good      Very Good Very Good Good     
## [14191] Very Good Ideal     Premium   Premium   Ideal     Very Good
## [14197] Very Good Very Good Very Good Very Good Very Good Premium  
## [14203] Premium   Very Good Very Good Very Good Premium   Premium  
## [14209] Ideal     Good      Very Good Premium   Ideal     Ideal    
## [14215] Premium   Ideal     Premium   Ideal     Very Good Ideal    
## [14221] Ideal     Premium   Good      Ideal     Ideal     Premium  
## [14227] Very Good Good      Premium   Ideal     Ideal     Good     
## [14233] Very Good Ideal     Ideal     Ideal     Premium   Ideal    
## [14239] Good      Ideal     Premium   Good      Very Good Very Good
## [14245] Fair      Good      Very Good Very Good Ideal     Premium  
## [14251] Ideal     Premium   Ideal     Premium   Premium   Ideal    
## [14257] Ideal     Premium   Very Good Premium   Premium   Premium  
## [14263] Premium   Very Good Ideal     Very Good Ideal     Premium  
## [14269] Very Good Premium   Very Good Good      Premium   Ideal    
## [14275] Ideal     Premium   Very Good Premium   Premium   Premium  
## [14281] Premium   Very Good Very Good Very Good Premium   Premium  
## [14287] Very Good Very Good Good      Fair      Very Good Good     
## [14293] Premium   Very Good Premium   Good      Premium   Very Good
## [14299] Very Good Ideal     Premium   Ideal     Ideal     Premium  
## [14305] Premium   Very Good Ideal     Ideal     Ideal     Ideal    
## [14311] Ideal     Ideal     Fair      Ideal     Ideal     Very Good
## [14317] Premium   Premium   Premium   Very Good Premium   Fair     
## [14323] Premium   Premium   Premium   Very Good Very Good Ideal    
## [14329] Ideal     Ideal     Ideal     Ideal     Very Good Ideal    
## [14335] Ideal     Premium   Premium   Premium   Very Good Very Good
## [14341] Premium   Ideal     Ideal     Ideal     Ideal     Ideal    
## [14347] Ideal     Very Good Ideal     Premium   Good      Premium  
## [14353] Ideal     Very Good Ideal     Premium   Ideal     Premium  
## [14359] Very Good Ideal     Premium   Very Good Premium   Very Good
## [14365] Good      Ideal     Ideal     Ideal     Ideal     Very Good
## [14371] Ideal     Good      Ideal     Premium   Very Good Ideal    
## [14377] Premium   Ideal     Ideal     Very Good Premium   Premium  
## [14383] Very Good Very Good Ideal     Premium   Ideal     Very Good
## [14389] Good      Good      Very Good Very Good Very Good Very Good
## [14395] Very Good Good      Ideal     Very Good Good      Premium  
## [14401] Good      Good      Premium   Premium   Ideal     Good     
## [14407] Good      Very Good Good      Very Good Ideal     Ideal    
## [14413] Ideal     Very Good Good      Premium   Premium   Premium  
## [14419] Premium   Premium   Premium   Ideal     Premium   Ideal    
## [14425] Good      Premium   Ideal     Very Good Good      Very Good
## [14431] Premium   Premium   Ideal     Ideal     Ideal     Ideal    
## [14437] Good      Good      Very Good Ideal     Good      Ideal    
## [14443] Premium   Premium   Very Good Premium   Ideal     Very Good
## [14449] Very Good Premium   Premium   Ideal     Very Good Ideal    
## [14455] Fair      Good      Good      Very Good Ideal     Very Good
## [14461] Ideal     Premium   Ideal     Ideal     Ideal     Premium  
## [14467] Very Good Very Good Ideal     Ideal     Fair      Ideal    
## [14473] Premium   Ideal     Ideal     Ideal     Very Good Premium  
## [14479] Fair      Premium   Premium   Premium   Premium   Very Good
## [14485] Ideal     Very Good Very Good Premium   Premium   Ideal    
## [14491] Ideal     Fair      Very Good Very Good Ideal     Premium  
## [14497] Ideal     Good      Very Good Ideal     Premium   Ideal    
## [14503] Premium   Very Good Fair      Very Good Good      Premium  
## [14509] Premium   Very Good Very Good Ideal     Ideal     Ideal    
## [14515] Ideal     Good      Premium   Premium   Premium   Premium  
## [14521] Premium   Premium   Good      Premium   Premium   Premium  
## [14527] Premium   Premium   Ideal     Very Good Very Good Very Good
## [14533] Ideal     Ideal     Ideal     Very Good Ideal     Premium  
## [14539] Premium   Very Good Good      Good      Premium   Good     
## [14545] Premium   Ideal     Premium   Ideal     Premium   Ideal    
## [14551] Ideal     Premium   Premium   Ideal     Very Good Ideal    
## [14557] Ideal     Premium   Premium   Premium   Ideal     Ideal    
## [14563] Ideal     Ideal     Fair      Ideal     Ideal     Premium  
## [14569] Ideal     Premium   Premium   Premium   Very Good Premium  
## [14575] Very Good Very Good Very Good Ideal     Very Good Ideal    
## [14581] Premium   Ideal     Premium   Very Good Ideal     Premium  
## [14587] Very Good Ideal     Ideal     Premium   Ideal     Ideal    
## [14593] Premium   Ideal     Premium   Premium   Premium   Premium  
## [14599] Good      Ideal     Premium   Ideal     Premium   Premium  
## [14605] Ideal     Ideal     Premium   Premium   Good      Ideal    
## [14611] Premium   Very Good Ideal     Good      Premium   Ideal    
## [14617] Good      Premium   Premium   Very Good Good      Very Good
## [14623] Ideal     Premium   Ideal     Ideal     Very Good Ideal    
## [14629] Good      Premium   Ideal     Ideal     Premium   Premium  
## [14635] Fair      Ideal     Premium   Very Good Ideal     Ideal    
## [14641] Premium   Very Good Good      Premium   Very Good Fair     
## [14647] Ideal     Ideal     Very Good Ideal     Very Good Good     
## [14653] Fair      Ideal     Premium   Good      Very Good Ideal    
## [14659] Very Good Fair      Very Good Very Good Premium   Very Good
## [14665] Very Good Very Good Premium   Fair      Fair      Good     
## [14671] Very Good Premium   Premium   Good      Premium   Very Good
## [14677] Good      Ideal     Ideal     Ideal     Good      Premium  
## [14683] Very Good Premium   Premium   Premium   Ideal     Premium  
## [14689] Ideal     Good      Premium   Premium   Ideal     Ideal    
## [14695] Very Good Very Good Very Good Good      Premium   Very Good
## [14701] Ideal     Ideal     Ideal     Premium   Ideal     Ideal    
## [14707] Very Good Premium   Ideal     Good      Ideal     Ideal    
## [14713] Premium   Very Good Premium   Ideal     Premium   Ideal    
## [14719] Good      Good      Ideal     Very Good Premium   Very Good
## [14725] Premium   Very Good Ideal     Ideal     Good      Very Good
## [14731] Ideal     Premium   Very Good Very Good Ideal     Ideal    
## [14737] Ideal     Ideal     Ideal     Good      Ideal     Ideal    
## [14743] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [14749] Ideal     Premium   Very Good Ideal     Ideal     Ideal    
## [14755] Very Good Premium   Premium   Ideal     Premium   Ideal    
## [14761] Very Good Premium   Very Good Good      Premium   Very Good
## [14767] Premium   Good      Very Good Premium   Ideal     Ideal    
## [14773] Premium   Premium   Premium   Ideal     Ideal     Premium  
## [14779] Premium   Premium   Very Good Ideal     Premium   Ideal    
## [14785] Premium   Very Good Premium   Premium   Fair      Premium  
## [14791] Good      Premium   Premium   Ideal     Very Good Premium  
## [14797] Ideal     Very Good Good      Very Good Very Good Premium  
## [14803] Very Good Premium   Premium   Ideal     Ideal     Ideal    
## [14809] Premium   Premium   Premium   Very Good Ideal     Very Good
## [14815] Very Good Premium   Premium   Very Good Premium   Premium  
## [14821] Ideal     Ideal     Ideal     Premium   Good      Premium  
## [14827] Ideal     Ideal     Very Good Premium   Very Good Ideal    
## [14833] Premium   Premium   Very Good Ideal     Very Good Good     
## [14839] Premium   Premium   Very Good Ideal     Ideal     Very Good
## [14845] Premium   Ideal     Very Good Good      Premium   Ideal    
## [14851] Premium   Premium   Ideal     Ideal     Premium   Premium  
## [14857] Good      Very Good Ideal     Very Good Premium   Good     
## [14863] Good      Very Good Very Good Very Good Very Good Good     
## [14869] Premium   Premium   Very Good Good      Good      Premium  
## [14875] Very Good Ideal     Ideal     Good      Ideal     Ideal    
## [14881] Premium   Fair      Ideal     Ideal     Ideal     Ideal    
## [14887] Ideal     Very Good Ideal     Ideal     Ideal     Premium  
## [14893] Premium   Very Good Premium   Ideal     Premium   Premium  
## [14899] Very Good Premium   Premium   Very Good Premium   Very Good
## [14905] Good      Premium   Ideal     Premium   Very Good Very Good
## [14911] Premium   Very Good Fair      Fair      Ideal     Good     
## [14917] Ideal     Premium   Premium   Ideal     Fair      Good     
## [14923] Good      Good      Very Good Very Good Fair      Premium  
## [14929] Ideal     Ideal     Premium   Premium   Ideal     Very Good
## [14935] Ideal     Ideal     Ideal     Ideal     Premium   Ideal    
## [14941] Very Good Very Good Very Good Very Good Very Good Very Good
## [14947] Very Good Ideal     Ideal     Good      Very Good Premium  
## [14953] Very Good Very Good Good      Premium   Premium   Ideal    
## [14959] Ideal     Good      Good      Ideal     Premium   Premium  
## [14965] Premium   Premium   Ideal     Very Good Very Good Good     
## [14971] Ideal     Good      Premium   Premium   Very Good Good     
## [14977] Premium   Premium   Premium   Premium   Very Good Very Good
## [14983] Ideal     Very Good Good      Good      Premium   Premium  
## [14989] Premium   Premium   Ideal     Very Good Ideal     Very Good
## [14995] Ideal     Ideal     Premium   Very Good Premium   Fair     
## [15001] Good      Very Good Ideal     Ideal     Ideal     Premium  
## [15007] Ideal     Ideal     Ideal     Premium   Ideal     Good     
## [15013] Good      Very Good Very Good Very Good Premium   Premium  
## [15019] Premium   Premium   Ideal     Premium   Premium   Premium  
## [15025] Premium   Premium   Premium   Ideal     Very Good Premium  
## [15031] Ideal     Premium   Ideal     Very Good Very Good Ideal    
## [15037] Very Good Good      Ideal     Very Good Premium   Ideal    
## [15043] Very Good Good      Very Good Premium   Ideal     Very Good
## [15049] Ideal     Ideal     Ideal     Ideal     Premium   Very Good
## [15055] Very Good Premium   Ideal     Ideal     Good      Premium  
## [15061] Ideal     Good      Premium   Ideal     Very Good Premium  
## [15067] Ideal     Premium   Premium   Good      Premium   Very Good
## [15073] Very Good Very Good Ideal     Good      Ideal     Ideal    
## [15079] Ideal     Fair      Very Good Very Good Very Good Very Good
## [15085] Very Good Ideal     Very Good Very Good Ideal     Ideal    
## [15091] Premium   Good      Premium   Ideal     Good      Premium  
## [15097] Ideal     Premium   Ideal     Ideal     Ideal     Very Good
## [15103] Premium   Ideal     Very Good Very Good Premium   Premium  
## [15109] Very Good Very Good Very Good Ideal     Good      Very Good
## [15115] Very Good Ideal     Premium   Ideal     Ideal     Premium  
## [15121] Very Good Ideal     Ideal     Fair      Premium   Ideal    
## [15127] Very Good Good      Fair      Ideal     Very Good Ideal    
## [15133] Ideal     Premium   Very Good Very Good Premium   Very Good
## [15139] Premium   Fair      Ideal     Premium   Very Good Very Good
## [15145] Ideal     Good      Good      Ideal     Good      Premium  
## [15151] Ideal     Good      Ideal     Premium   Premium   Premium  
## [15157] Premium   Premium   Very Good Very Good Ideal     Premium  
## [15163] Ideal     Ideal     Premium   Premium   Ideal     Very Good
## [15169] Ideal     Premium   Ideal     Ideal     Premium   Very Good
## [15175] Premium   Premium   Premium   Premium   Premium   Good     
## [15181] Premium   Premium   Very Good Good      Very Good Very Good
## [15187] Very Good Ideal     Premium   Premium   Very Good Ideal    
## [15193] Very Good Very Good Very Good Very Good Good      Ideal    
## [15199] Very Good Ideal     Ideal     Premium   Premium   Very Good
## [15205] Premium   Ideal     Premium   Very Good Premium   Ideal    
## [15211] Ideal     Ideal     Premium   Premium   Fair      Premium  
## [15217] Premium   Premium   Good      Ideal     Very Good Ideal    
## [15223] Premium   Good      Ideal     Ideal     Very Good Good     
## [15229] Ideal     Very Good Very Good Premium   Premium   Fair     
## [15235] Fair      Premium   Premium   Premium   Premium   Premium  
## [15241] Ideal     Very Good Premium   Premium   Premium   Premium  
## [15247] Premium   Premium   Ideal     Premium   Good      Ideal    
## [15253] Premium   Very Good Premium   Very Good Very Good Good     
## [15259] Premium   Very Good Very Good Ideal     Ideal     Fair     
## [15265] Premium   Premium   Premium   Premium   Premium   Very Good
## [15271] Very Good Very Good Very Good Premium   Ideal     Ideal    
## [15277] Ideal     Ideal     Premium   Premium   Premium   Ideal    
## [15283] Ideal     Ideal     Ideal     Ideal     Very Good Ideal    
## [15289] Ideal     Ideal     Premium   Very Good Very Good Ideal    
## [15295] Ideal     Very Good Ideal     Premium   Premium   Premium  
## [15301] Premium   Good      Very Good Ideal     Premium   Very Good
## [15307] Premium   Ideal     Premium   Ideal     Premium   Premium  
## [15313] Premium   Fair      Good      Very Good Very Good Very Good
## [15319] Premium   Premium   Premium   Good      Very Good Very Good
## [15325] Ideal     Good      Premium   Very Good Premium   Premium  
## [15331] Premium   Fair      Very Good Ideal     Premium   Ideal    
## [15337] Ideal     Very Good Very Good Very Good Very Good Good     
## [15343] Very Good Very Good Premium   Good      Premium   Premium  
## [15349] Ideal     Good      Very Good Good      Premium   Premium  
## [15355] Very Good Premium   Premium   Premium   Ideal     Good     
## [15361] Ideal     Premium   Very Good Ideal     Good      Very Good
## [15367] Very Good Ideal     Very Good Premium   Premium   Ideal    
## [15373] Very Good Very Good Very Good Ideal     Premium   Ideal    
## [15379] Good      Very Good Ideal     Ideal     Good      Ideal    
## [15385] Very Good Very Good Very Good Very Good Very Good Ideal    
## [15391] Very Good Very Good Very Good Ideal     Very Good Very Good
## [15397] Ideal     Very Good Ideal     Very Good Premium   Very Good
## [15403] Ideal     Premium   Premium   Premium   Premium   Ideal    
## [15409] Premium   Premium   Very Good Premium   Premium   Ideal    
## [15415] Good      Premium   Good      Ideal     Premium   Good     
## [15421] Fair      Premium   Ideal     Good      Premium   Ideal    
## [15427] Ideal     Very Good Premium   Very Good Very Good Good     
## [15433] Good      Ideal     Ideal     Premium   Premium   Premium  
## [15439] Premium   Premium   Good      Good      Very Good Good     
## [15445] Very Good Premium   Ideal     Premium   Ideal     Premium  
## [15451] Ideal     Ideal     Good      Premium   Very Good Very Good
## [15457] Good      Good      Very Good Very Good Ideal     Ideal    
## [15463] Ideal     Ideal     Very Good Good      Very Good Very Good
## [15469] Good      Premium   Premium   Very Good Very Good Very Good
## [15475] Ideal     Premium   Very Good Premium   Ideal     Ideal    
## [15481] Ideal     Ideal     Ideal     Premium   Premium   Very Good
## [15487] Ideal     Very Good Very Good Very Good Premium   Premium  
## [15493] Very Good Very Good Very Good Premium   Ideal     Ideal    
## [15499] Premium   Premium   Good      Premium   Good      Fair     
## [15505] Ideal     Premium   Fair      Very Good Good      Very Good
## [15511] Premium   Premium   Premium   Premium   Premium   Good     
## [15517] Premium   Premium   Premium   Very Good Very Good Premium  
## [15523] Premium   Very Good Very Good Good      Good      Good     
## [15529] Fair      Ideal     Good      Good      Premium   Premium  
## [15535] Very Good Premium   Good      Ideal     Ideal     Ideal    
## [15541] Very Good Premium   Ideal     Premium   Ideal     Ideal    
## [15547] Premium   Very Good Very Good Ideal     Premium   Premium  
## [15553] Premium   Very Good Very Good Premium   Ideal     Good     
## [15559] Premium   Premium   Very Good Very Good Premium   Ideal    
## [15565] Ideal     Ideal     Ideal     Ideal     Premium   Premium  
## [15571] Very Good Good      Good      Premium   Good      Good     
## [15577] Very Good Very Good Ideal     Ideal     Good      Good     
## [15583] Premium   Premium   Premium   Good      Premium   Fair     
## [15589] Ideal     Premium   Ideal     Ideal     Ideal     Ideal    
## [15595] Premium   Premium   Premium   Premium   Very Good Very Good
## [15601] Very Good Ideal     Ideal     Ideal     Ideal     Premium  
## [15607] Premium   Premium   Premium   Good      Ideal     Ideal    
## [15613] Very Good Very Good Very Good Very Good Ideal     Very Good
## [15619] Good      Premium   Good      Very Good Very Good Fair     
## [15625] Very Good Very Good Fair      Premium   Premium   Premium  
## [15631] Premium   Good      Very Good Premium   Fair      Premium  
## [15637] Premium   Ideal     Premium   Premium   Ideal     Premium  
## [15643] Fair      Good      Ideal     Premium   Good      Premium  
## [15649] Very Good Premium   Ideal     Good      Ideal     Premium  
## [15655] Very Good Premium   Premium   Very Good Ideal     Ideal    
## [15661] Good      Premium   Ideal     Premium   Very Good Premium  
## [15667] Good      Premium   Very Good Fair      Very Good Ideal    
## [15673] Ideal     Very Good Very Good Very Good Ideal     Ideal    
## [15679] Ideal     Good      Good      Premium   Very Good Very Good
## [15685] Fair      Ideal     Ideal     Very Good Ideal     Premium  
## [15691] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [15697] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [15703] Good      Very Good Very Good Very Good Very Good Very Good
## [15709] Very Good Very Good Very Good Very Good Very Good Ideal    
## [15715] Premium   Premium   Premium   Premium   Premium   Ideal    
## [15721] Ideal     Ideal     Premium   Ideal     Premium   Ideal    
## [15727] Premium   Very Good Premium   Fair      Ideal     Fair     
## [15733] Ideal     Ideal     Very Good Very Good Ideal     Ideal    
## [15739] Ideal     Ideal     Very Good Premium   Very Good Premium  
## [15745] Premium   Very Good Good      Ideal     Ideal     Premium  
## [15751] Premium   Very Good Very Good Ideal     Ideal     Ideal    
## [15757] Premium   Good      Ideal     Premium   Very Good Very Good
## [15763] Ideal     Very Good Ideal     Ideal     Premium   Ideal    
## [15769] Ideal     Ideal     Good      Premium   Very Good Premium  
## [15775] Ideal     Ideal     Very Good Very Good Premium   Ideal    
## [15781] Very Good Fair      Good      Very Good Ideal     Premium  
## [15787] Ideal     Ideal     Very Good Very Good Very Good Premium  
## [15793] Very Good Premium   Ideal     Premium   Premium   Ideal    
## [15799] Ideal     Ideal     Premium   Very Good Very Good Very Good
## [15805] Ideal     Ideal     Ideal     Ideal     Premium   Very Good
## [15811] Very Good Good      Premium   Ideal     Premium   Fair     
## [15817] Ideal     Good      Premium   Ideal     Premium   Premium  
## [15823] Good      Ideal     Good      Premium   Premium   Ideal    
## [15829] Very Good Very Good Premium   Ideal     Premium   Ideal    
## [15835] Good      Premium   Premium   Premium   Very Good Premium  
## [15841] Very Good Ideal     Premium   Ideal     Ideal     Ideal    
## [15847] Very Good Ideal     Ideal     Fair      Premium   Ideal    
## [15853] Very Good Ideal     Premium   Premium   Ideal     Ideal    
## [15859] Good      Premium   Ideal     Premium   Good      Ideal    
## [15865] Ideal     Fair      Fair      Ideal     Ideal     Ideal    
## [15871] Ideal     Good      Premium   Premium   Very Good Premium  
## [15877] Ideal     Ideal     Ideal     Premium   Good      Very Good
## [15883] Very Good Premium   Premium   Ideal     Ideal     Premium  
## [15889] Fair      Premium   Very Good Premium   Ideal     Very Good
## [15895] Very Good Premium   Premium   Premium   Premium   Very Good
## [15901] Ideal     Very Good Very Good Ideal     Premium   Premium  
## [15907] Premium   Ideal     Good      Premium   Very Good Good     
## [15913] Ideal     Premium   Ideal     Good      Very Good Ideal    
## [15919] Fair      Premium   Fair      Premium   Very Good Good     
## [15925] Premium   Ideal     Ideal     Ideal     Very Good Very Good
## [15931] Good      Premium   Very Good Ideal     Ideal     Ideal    
## [15937] Very Good Very Good Fair      Very Good Fair      Premium  
## [15943] Ideal     Ideal     Premium   Ideal     Premium   Ideal    
## [15949] Premium   Ideal     Ideal     Fair      Very Good Premium  
## [15955] Ideal     Ideal     Very Good Premium   Premium   Premium  
## [15961] Ideal     Very Good Very Good Ideal     Very Good Ideal    
## [15967] Good      Good      Premium   Fair      Ideal     Ideal    
## [15973] Premium   Good      Ideal     Ideal     Very Good Good     
## [15979] Ideal     Premium   Fair      Premium   Fair      Good     
## [15985] Premium   Ideal     Ideal     Very Good Good      Good     
## [15991] Very Good Very Good Very Good Ideal     Ideal     Premium  
## [15997] Premium   Premium   Ideal     Ideal     Premium   Ideal    
## [16003] Premium   Ideal     Ideal     Premium   Ideal     Very Good
## [16009] Ideal     Premium   Premium   Very Good Very Good Ideal    
## [16015] Premium   Good      Very Good Ideal     Ideal     Ideal    
## [16021] Premium   Premium   Premium   Ideal     Premium   Premium  
## [16027] Premium   Premium   Very Good Ideal     Ideal     Premium  
## [16033] Premium   Premium   Premium   Premium   Premium   Very Good
## [16039] Premium   Good      Very Good Premium   Premium   Ideal    
## [16045] Premium   Premium   Very Good Premium   Premium   Premium  
## [16051] Ideal     Good      Premium   Ideal     Ideal     Premium  
## [16057] Very Good Premium   Premium   Ideal     Premium   Good     
## [16063] Very Good Premium   Premium   Ideal     Premium   Premium  
## [16069] Premium   Ideal     Premium   Ideal     Ideal     Ideal    
## [16075] Very Good Ideal     Ideal     Ideal     Very Good Good     
## [16081] Very Good Very Good Ideal     Ideal     Fair      Premium  
## [16087] Ideal     Ideal     Premium   Good      Ideal     Ideal    
## [16093] Premium   Very Good Very Good Premium   Ideal     Very Good
## [16099] Premium   Premium   Good      Very Good Premium   Very Good
## [16105] Ideal     Ideal     Ideal     Ideal     Very Good Ideal    
## [16111] Good      Premium   Very Good Ideal     Premium   Premium  
## [16117] Premium   Very Good Ideal     Premium   Very Good Ideal    
## [16123] Good      Premium   Ideal     Good      Premium   Good     
## [16129] Premium   Very Good Premium   Premium   Very Good Very Good
## [16135] Premium   Ideal     Ideal     Ideal     Good      Ideal    
## [16141] Premium   Ideal     Premium   Premium   Ideal     Premium  
## [16147] Premium   Very Good Ideal     Good      Fair      Ideal    
## [16153] Very Good Ideal     Premium   Ideal     Ideal     Premium  
## [16159] Premium   Premium   Ideal     Very Good Very Good Ideal    
## [16165] Ideal     Ideal     Premium   Good      Premium   Very Good
## [16171] Premium   Very Good Good      Ideal     Ideal     Very Good
## [16177] Ideal     Very Good Very Good Good      Premium   Premium  
## [16183] Ideal     Premium   Premium   Ideal     Ideal     Premium  
## [16189] Ideal     Ideal     Premium   Ideal     Premium   Ideal    
## [16195] Premium   Premium   Ideal     Ideal     Ideal     Good     
## [16201] Ideal     Good      Ideal     Ideal     Ideal     Very Good
## [16207] Ideal     Very Good Premium   Good      Ideal     Ideal    
## [16213] Premium   Very Good Ideal     Ideal     Premium   Very Good
## [16219] Very Good Ideal     Premium   Ideal     Ideal     Ideal    
## [16225] Very Good Very Good Ideal     Premium   Very Good Ideal    
## [16231] Ideal     Premium   Ideal     Premium   Premium   Very Good
## [16237] Premium   Ideal     Premium   Very Good Very Good Premium  
## [16243] Premium   Good      Premium   Good      Premium   Premium  
## [16249] Ideal     Ideal     Premium   Very Good Ideal     Very Good
## [16255] Ideal     Ideal     Fair      Ideal     Premium   Ideal    
## [16261] Good      Premium   Premium   Ideal     Premium   Premium  
## [16267] Premium   Premium   Premium   Premium   Ideal     Good     
## [16273] Good      Ideal     Ideal     Ideal     Premium   Ideal    
## [16279] Good      Ideal     Ideal     Ideal     Premium   Very Good
## [16285] Very Good Ideal     Very Good Very Good Premium   Ideal    
## [16291] Very Good Good      Ideal     Very Good Ideal     Ideal    
## [16297] Premium   Premium   Good      Very Good Very Good Very Good
## [16303] Good      Good      Very Good Ideal     Very Good Very Good
## [16309] Very Good Premium   Ideal     Premium   Very Good Very Good
## [16315] Good      Premium   Ideal     Fair      Very Good Very Good
## [16321] Ideal     Premium   Premium   Ideal     Premium   Premium  
## [16327] Premium   Fair      Premium   Very Good Premium   Premium  
## [16333] Premium   Premium   Ideal     Premium   Very Good Ideal    
## [16339] Premium   Very Good Premium   Premium   Ideal     Ideal    
## [16345] Very Good Very Good Very Good Ideal     Ideal     Very Good
## [16351] Premium   Very Good Premium   Premium   Premium   Premium  
## [16357] Premium   Very Good Good      Premium   Ideal     Premium  
## [16363] Ideal     Premium   Very Good Ideal     Very Good Premium  
## [16369] Very Good Ideal     Premium   Ideal     Very Good Very Good
## [16375] Very Good Ideal     Very Good Very Good Very Good Very Good
## [16381] Ideal     Premium   Very Good Ideal     Ideal     Very Good
## [16387] Premium   Premium   Good      Ideal     Ideal     Ideal    
## [16393] Ideal     Premium   Very Good Premium   Good      Very Good
## [16399] Very Good Very Good Ideal     Premium   Ideal     Ideal    
## [16405] Ideal     Very Good Ideal     Very Good Fair      Good     
## [16411] Premium   Good      Very Good Premium   Premium   Ideal    
## [16417] Ideal     Very Good Ideal     Ideal     Ideal     Very Good
## [16423] Good      Ideal     Ideal     Ideal     Ideal     Ideal    
## [16429] Premium   Ideal     Ideal     Premium   Very Good Ideal    
## [16435] Ideal     Very Good Good      Premium   Very Good Fair     
## [16441] Premium   Ideal     Very Good Very Good Very Good Premium  
## [16447] Premium   Good      Very Good Very Good Ideal     Ideal    
## [16453] Very Good Very Good Premium   Ideal     Ideal     Very Good
## [16459] Premium   Premium   Ideal     Very Good Ideal     Premium  
## [16465] Premium   Premium   Ideal     Very Good Premium   Very Good
## [16471] Ideal     Ideal     Premium   Very Good Premium   Premium  
## [16477] Premium   Ideal     Ideal     Premium   Ideal     Ideal    
## [16483] Very Good Premium   Ideal     Ideal     Premium   Premium  
## [16489] Ideal     Premium   Ideal     Ideal     Premium   Premium  
## [16495] Ideal     Premium   Very Good Very Good Very Good Premium  
## [16501] Very Good Fair      Very Good Very Good Fair      Fair     
## [16507] Ideal     Ideal     Very Good Ideal     Very Good Premium  
## [16513] Very Good Ideal     Premium   Very Good Premium   Premium  
## [16519] Very Good Premium   Very Good Premium   Very Good Ideal    
## [16525] Very Good Ideal     Ideal     Premium   Ideal     Ideal    
## [16531] Good      Premium   Premium   Good      Premium   Premium  
## [16537] Premium   Premium   Very Good Ideal     Ideal     Ideal    
## [16543] Premium   Premium   Premium   Very Good Very Good Ideal    
## [16549] Premium   Ideal     Very Good Good      Very Good Very Good
## [16555] Ideal     Good      Ideal     Premium   Ideal     Ideal    
## [16561] Ideal     Ideal     Ideal     Premium   Very Good Premium  
## [16567] Premium   Ideal     Good      Ideal     Premium   Premium  
## [16573] Premium   Ideal     Very Good Ideal     Ideal     Premium  
## [16579] Ideal     Ideal     Premium   Ideal     Very Good Very Good
## [16585] Premium   Very Good Very Good Good      Premium   Premium  
## [16591] Premium   Very Good Premium   Very Good Very Good Ideal    
## [16597] Ideal     Ideal     Premium   Very Good Very Good Ideal    
## [16603] Fair      Very Good Very Good Ideal     Ideal     Ideal    
## [16609] Premium   Ideal     Ideal     Ideal     Ideal     Ideal    
## [16615] Premium   Premium   Premium   Premium   Premium   Very Good
## [16621] Ideal     Ideal     Premium   Premium   Ideal     Premium  
## [16627] Ideal     Ideal     Ideal     Very Good Good      Fair     
## [16633] Ideal     Very Good Premium   Ideal     Very Good Fair     
## [16639] Premium   Premium   Good      Good      Premium   Premium  
## [16645] Ideal     Ideal     Ideal     Very Good Very Good Premium  
## [16651] Very Good Ideal     Premium   Fair      Ideal     Premium  
## [16657] Ideal     Premium   Ideal     Premium   Ideal     Very Good
## [16663] Very Good Very Good Ideal     Good      Ideal     Very Good
## [16669] Very Good Ideal     Premium   Premium   Ideal     Very Good
## [16675] Premium   Very Good Very Good Very Good Good      Good     
## [16681] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [16687] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [16693] Very Good Good      Very Good Ideal     Good      Premium  
## [16699] Very Good Good      Premium   Ideal     Very Good Ideal    
## [16705] Very Good Premium   Very Good Good      Good      Very Good
## [16711] Very Good Ideal     Ideal     Ideal     Ideal     Ideal    
## [16717] Ideal     Ideal     Very Good Very Good Ideal     Ideal    
## [16723] Ideal     Ideal     Ideal     Premium   Ideal     Very Good
## [16729] Good      Premium   Very Good Very Good Very Good Good     
## [16735] Premium   Good      Good      Good      Premium   Very Good
## [16741] Premium   Very Good Ideal     Premium   Very Good Ideal    
## [16747] Premium   Premium   Very Good Very Good Premium   Ideal    
## [16753] Premium   Premium   Ideal     Very Good Ideal     Ideal    
## [16759] Very Good Very Good Premium   Premium   Ideal     Ideal    
## [16765] Ideal     Ideal     Very Good Ideal     Very Good Ideal    
## [16771] Premium   Good      Ideal     Ideal     Ideal     Premium  
## [16777] Ideal     Premium   Premium   Ideal     Very Good Ideal    
## [16783] Premium   Premium   Very Good Good      Good      Ideal    
## [16789] Premium   Ideal     Ideal     Ideal     Premium   Very Good
## [16795] Very Good Premium   Premium   Premium   Ideal     Very Good
## [16801] Very Good Premium   Ideal     Very Good Premium   Premium  
## [16807] Premium   Very Good Good      Very Good Ideal     Ideal    
## [16813] Very Good Premium   Very Good Very Good Good      Good     
## [16819] Premium   Premium   Ideal     Premium   Very Good Good     
## [16825] Premium   Premium   Premium   Very Good Good      Premium  
## [16831] Ideal     Premium   Premium   Very Good Very Good Ideal    
## [16837] Ideal     Premium   Ideal     Fair      Very Good Premium  
## [16843] Premium   Premium   Ideal     Good      Premium   Ideal    
## [16849] Ideal     Premium   Premium   Premium   Ideal     Premium  
## [16855] Premium   Very Good Premium   Fair      Ideal     Ideal    
## [16861] Premium   Ideal     Good      Ideal     Ideal     Premium  
## [16867] Premium   Premium   Ideal     Good      Very Good Very Good
## [16873] Ideal     Very Good Very Good Very Good Very Good Premium  
## [16879] Ideal     Ideal     Ideal     Fair      Premium   Ideal    
## [16885] Very Good Good      Ideal     Ideal     Ideal     Ideal    
## [16891] Premium   Premium   Ideal     Very Good Ideal     Good     
## [16897] Premium   Very Good Very Good Ideal     Good      Good     
## [16903] Good      Premium   Premium   Ideal     Good      Premium  
## [16909] Premium   Ideal     Very Good Ideal     Premium   Premium  
## [16915] Very Good Fair      Ideal     Very Good Very Good Ideal    
## [16921] Premium   Premium   Premium   Ideal     Premium   Ideal    
## [16927] Very Good Ideal     Premium   Good      Fair      Ideal    
## [16933] Premium   Very Good Very Good Ideal     Ideal     Premium  
## [16939] Very Good Very Good Very Good Premium   Very Good Very Good
## [16945] Very Good Ideal     Premium   Ideal     Very Good Very Good
## [16951] Good      Very Good Ideal     Very Good Very Good Very Good
## [16957] Ideal     Premium   Premium   Premium   Ideal     Ideal    
## [16963] Ideal     Good      Very Good Premium   Premium   Very Good
## [16969] Ideal     Very Good Very Good Very Good Premium   Premium  
## [16975] Ideal     Premium   Ideal     Very Good Very Good Very Good
## [16981] Premium   Good      Premium   Premium   Ideal     Ideal    
## [16987] Very Good Very Good Ideal     Very Good Premium   Ideal    
## [16993] Very Good Premium   Premium   Very Good Very Good Very Good
## [16999] Premium   Premium   Ideal     Ideal     Ideal     Premium  
## [17005] Premium   Ideal     Ideal     Premium   Ideal     Premium  
## [17011] Very Good Ideal     Very Good Ideal     Ideal     Good     
## [17017] Ideal     Very Good Good      Ideal     Ideal     Ideal    
## [17023] Very Good Ideal     Fair      Good      Ideal     Ideal    
## [17029] Good      Ideal     Ideal     Very Good Very Good Ideal    
## [17035] Good      Premium   Ideal     Ideal     Ideal     Premium  
## [17041] Very Good Very Good Very Good Very Good Ideal     Ideal    
## [17047] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [17053] Ideal     Ideal     Ideal     Ideal     Good      Ideal    
## [17059] Premium   Ideal     Premium   Premium   Premium   Premium  
## [17065] Ideal     Very Good Premium   Premium   Premium   Very Good
## [17071] Premium   Ideal     Very Good Ideal     Very Good Premium  
## [17077] Very Good Ideal     Ideal     Ideal     Very Good Very Good
## [17083] Ideal     Premium   Very Good Very Good Very Good Very Good
## [17089] Very Good Ideal     Premium   Premium   Ideal     Fair     
## [17095] Premium   Very Good Fair      Fair      Ideal     Ideal    
## [17101] Very Good Very Good Very Good Very Good Very Good Very Good
## [17107] Premium   Premium   Good      Premium   Very Good Very Good
## [17113] Good      Very Good Ideal     Very Good Premium   Good     
## [17119] Ideal     Good      Very Good Ideal     Very Good Ideal    
## [17125] Ideal     Ideal     Ideal     Very Good Premium   Ideal    
## [17131] Very Good Ideal     Ideal     Ideal     Ideal     Ideal    
## [17137] Premium   Fair      Fair      Premium   Very Good Good     
## [17143] Premium   Good      Premium   Good      Very Good Premium  
## [17149] Ideal     Very Good Good      Good      Very Good Very Good
## [17155] Premium   Ideal     Premium   Good      Very Good Premium  
## [17161] Ideal     Premium   Ideal     Premium   Premium   Good     
## [17167] Very Good Ideal     Ideal     Ideal     Premium   Ideal    
## [17173] Ideal     Ideal     Ideal     Good      Premium   Ideal    
## [17179] Ideal     Ideal     Very Good Fair      Premium   Good     
## [17185] Very Good Premium   Very Good Very Good Fair      Ideal    
## [17191] Very Good Very Good Very Good Ideal     Premium   Very Good
## [17197] Fair      Good      Ideal     Very Good Ideal     Very Good
## [17203] Very Good Ideal     Ideal     Good      Premium   Premium  
## [17209] Very Good Ideal     Premium   Very Good Premium   Good     
## [17215] Premium   Very Good Very Good Ideal     Very Good Premium  
## [17221] Ideal     Very Good Fair      Ideal     Ideal     Premium  
## [17227] Premium   Premium   Very Good Ideal     Ideal     Premium  
## [17233] Premium   Ideal     Very Good Very Good Premium   Very Good
## [17239] Ideal     Ideal     Good      Premium   Premium   Premium  
## [17245] Ideal     Ideal     Premium   Ideal     Very Good Very Good
## [17251] Very Good Ideal     Fair      Very Good Premium   Good     
## [17257] Premium   Ideal     Premium   Premium   Premium   Very Good
## [17263] Very Good Very Good Ideal     Good      Premium   Ideal    
## [17269] Ideal     Very Good Very Good Very Good Very Good Premium  
## [17275] Very Good Ideal     Premium   Ideal     Ideal     Premium  
## [17281] Premium   Ideal     Fair      Ideal     Premium   Ideal    
## [17287] Premium   Very Good Good      Ideal     Very Good Ideal    
## [17293] Ideal     Very Good Premium   Very Good Ideal     Premium  
## [17299] Ideal     Premium   Premium   Good      Ideal     Ideal    
## [17305] Premium   Ideal     Premium   Good      Ideal     Premium  
## [17311] Very Good Premium   Very Good Good      Ideal     Ideal    
## [17317] Very Good Premium   Very Good Ideal     Premium   Ideal    
## [17323] Premium   Premium   Ideal     Very Good Ideal     Premium  
## [17329] Ideal     Ideal     Very Good Ideal     Very Good Ideal    
## [17335] Premium   Very Good Ideal     Ideal     Very Good Premium  
## [17341] Ideal     Good      Ideal     Ideal     Very Good Ideal    
## [17347] Very Good Ideal     Ideal     Premium   Very Good Premium  
## [17353] Premium   Good      Premium   Premium   Ideal     Premium  
## [17359] Good      Ideal     Premium   Ideal     Ideal     Ideal    
## [17365] Very Good Good      Premium   Premium   Premium   Good     
## [17371] Ideal     Ideal     Good      Premium   Premium   Premium  
## [17377] Premium   Premium   Ideal     Premium   Premium   Premium  
## [17383] Premium   Premium   Good      Ideal     Premium   Premium  
## [17389] Ideal     Ideal     Ideal     Ideal     Ideal     Good     
## [17395] Ideal     Ideal     Ideal     Premium   Ideal     Ideal    
## [17401] Good      Premium   Ideal     Good      Premium   Ideal    
## [17407] Very Good Very Good Ideal     Very Good Premium   Premium  
## [17413] Very Good Premium   Very Good Fair      Premium   Very Good
## [17419] Ideal     Ideal     Ideal     Ideal     Ideal     Good     
## [17425] Premium   Premium   Ideal     Ideal     Very Good Very Good
## [17431] Very Good Very Good Premium   Premium   Premium   Ideal    
## [17437] Premium   Ideal     Very Good Premium   Ideal     Very Good
## [17443] Very Good Good      Premium   Very Good Very Good Very Good
## [17449] Good      Ideal     Very Good Ideal     Good      Premium  
## [17455] Good      Premium   Very Good Very Good Very Good Good     
## [17461] Premium   Very Good Ideal     Very Good Premium   Premium  
## [17467] Very Good Premium   Premium   Good      Very Good Ideal    
## [17473] Ideal     Ideal     Ideal     Good      Ideal     Premium  
## [17479] Ideal     Premium   Premium   Fair      Very Good Premium  
## [17485] Good      Ideal     Good      Premium   Ideal     Good     
## [17491] Good      Ideal     Ideal     Ideal     Very Good Premium  
## [17497] Premium   Premium   Premium   Premium   Ideal     Ideal    
## [17503] Ideal     Very Good Ideal     Ideal     Ideal     Ideal    
## [17509] Ideal     Very Good Very Good Premium   Good      Ideal    
## [17515] Ideal     Ideal     Premium   Premium   Good      Ideal    
## [17521] Good      Very Good Very Good Good      Ideal     Premium  
## [17527] Ideal     Ideal     Premium   Fair      Premium   Premium  
## [17533] Premium   Premium   Ideal     Premium   Fair      Ideal    
## [17539] Premium   Premium   Ideal     Ideal     Ideal     Ideal    
## [17545] Good      Premium   Premium   Premium   Good      Premium  
## [17551] Ideal     Ideal     Good      Ideal     Very Good Very Good
## [17557] Good      Ideal     Premium   Ideal     Fair      Ideal    
## [17563] Premium   Very Good Very Good Ideal     Ideal     Ideal    
## [17569] Premium   Ideal     Ideal     Ideal     Very Good Fair     
## [17575] Premium   Ideal     Ideal     Ideal     Very Good Ideal    
## [17581] Very Good Ideal     Ideal     Ideal     Ideal     Premium  
## [17587] Ideal     Premium   Premium   Very Good Ideal     Fair     
## [17593] Ideal     Fair      Good      Ideal     Ideal     Ideal    
## [17599] Premium   Very Good Ideal     Ideal     Ideal     Premium  
## [17605] Premium   Premium   Good      Premium   Ideal     Ideal    
## [17611] Premium   Very Good Very Good Very Good Very Good Very Good
## [17617] Premium   Very Good Good      Fair      Ideal     Ideal    
## [17623] Premium   Good      Good      Very Good Very Good Premium  
## [17629] Fair      Ideal     Premium   Ideal     Ideal     Ideal    
## [17635] Premium   Very Good Good      Ideal     Premium   Premium  
## [17641] Very Good Very Good Premium   Very Good Very Good Ideal    
## [17647] Ideal     Ideal     Very Good Ideal     Ideal     Ideal    
## [17653] Ideal     Very Good Ideal     Good      Very Good Premium  
## [17659] Ideal     Premium   Ideal     Very Good Ideal     Very Good
## [17665] Ideal     Ideal     Ideal     Ideal     Premium   Premium  
## [17671] Ideal     Very Good Very Good Very Good Ideal     Premium  
## [17677] Very Good Ideal     Good      Very Good Premium   Very Good
## [17683] Ideal     Premium   Ideal     Ideal     Premium   Premium  
## [17689] Premium   Premium   Very Good Fair      Very Good Premium  
## [17695] Ideal     Good      Very Good Very Good Ideal     Very Good
## [17701] Ideal     Ideal     Premium   Ideal     Good      Premium  
## [17707] Very Good Very Good Very Good Very Good Ideal     Ideal    
## [17713] Ideal     Ideal     Ideal     Ideal     Good      Fair     
## [17719] Very Good Fair      Very Good Very Good Very Good Very Good
## [17725] Very Good Ideal     Ideal     Ideal     Very Good Very Good
## [17731] Good      Good      Ideal     Premium   Very Good Fair     
## [17737] Ideal     Premium   Premium   Very Good Very Good Premium  
## [17743] Very Good Good      Premium   Very Good Premium   Ideal    
## [17749] Ideal     Very Good Very Good Good      Very Good Ideal    
## [17755] Ideal     Very Good Very Good Very Good Premium   Very Good
## [17761] Very Good Good      Ideal     Premium   Premium   Good     
## [17767] Premium   Very Good Ideal     Ideal     Ideal     Premium  
## [17773] Ideal     Ideal     Ideal     Ideal     Good      Very Good
## [17779] Premium   Premium   Ideal     Good      Premium   Very Good
## [17785] Premium   Premium   Ideal     Very Good Ideal     Premium  
## [17791] Premium   Premium   Premium   Ideal     Very Good Very Good
## [17797] Very Good Very Good Ideal     Ideal     Premium   Very Good
## [17803] Very Good Good      Good      Very Good Ideal     Ideal    
## [17809] Good      Very Good Ideal     Very Good Ideal     Ideal    
## [17815] Ideal     Premium   Ideal     Ideal     Premium   Fair     
## [17821] Ideal     Ideal     Fair      Ideal     Premium   Fair     
## [17827] Fair      Very Good Very Good Ideal     Premium   Good     
## [17833] Ideal     Very Good Premium   Ideal     Ideal     Ideal    
## [17839] Very Good Very Good Premium   Ideal     Good      Good     
## [17845] Good      Ideal     Ideal     Ideal     Ideal     Good     
## [17851] Good      Premium   Very Good Ideal     Premium   Premium  
## [17857] Very Good Premium   Premium   Ideal     Very Good Ideal    
## [17863] Good      Good      Good      Ideal     Ideal     Ideal    
## [17869] Good      Very Good Fair      Ideal     Premium   Ideal    
## [17875] Fair      Good      Very Good Very Good Ideal     Good     
## [17881] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [17887] Ideal     Premium   Ideal     Ideal     Premium   Ideal    
## [17893] Ideal     Ideal     Premium   Premium   Very Good Premium  
## [17899] Good      Very Good Ideal     Ideal     Ideal     Very Good
## [17905] Ideal     Ideal     Ideal     Premium   Good      Very Good
## [17911] Premium   Good      Very Good Very Good Very Good Premium  
## [17917] Ideal     Very Good Premium   Ideal     Good      Ideal    
## [17923] Premium   Very Good Premium   Ideal     Ideal     Ideal    
## [17929] Premium   Premium   Ideal     Very Good Ideal     Ideal    
## [17935] Very Good Premium   Premium   Premium   Very Good Premium  
## [17941] Ideal     Good      Ideal     Premium   Good      Very Good
## [17947] Ideal     Premium   Ideal     Good      Ideal     Premium  
## [17953] Ideal     Premium   Very Good Very Good Fair      Very Good
## [17959] Premium   Premium   Fair      Premium   Premium   Very Good
## [17965] Good      Premium   Ideal     Ideal     Premium   Ideal    
## [17971] Premium   Ideal     Very Good Premium   Very Good Ideal    
## [17977] Very Good Ideal     Very Good Ideal     Ideal     Premium  
## [17983] Premium   Premium   Ideal     Ideal     Ideal     Premium  
## [17989] Ideal     Very Good Ideal     Very Good Premium   Premium  
## [17995] Very Good Ideal     Premium   Very Good Very Good Very Good
## [18001] Good      Premium   Premium   Very Good Very Good Very Good
## [18007] Premium   Very Good Fair      Premium   Ideal     Ideal    
## [18013] Ideal     Premium   Premium   Ideal     Very Good Ideal    
## [18019] Very Good Ideal     Ideal     Good      Very Good Good     
## [18025] Fair      Premium   Good      Very Good Ideal     Good     
## [18031] Ideal     Premium   Premium   Premium   Ideal     Ideal    
## [18037] Premium   Very Good Very Good Ideal     Very Good Good     
## [18043] Premium   Premium   Very Good Very Good Ideal     Ideal    
## [18049] Premium   Very Good Ideal     Ideal     Ideal     Ideal    
## [18055] Ideal     Very Good Very Good Ideal     Ideal     Premium  
## [18061] Premium   Premium   Very Good Good      Ideal     Ideal    
## [18067] Ideal     Fair      Ideal     Ideal     Ideal     Ideal    
## [18073] Fair      Premium   Premium   Good      Fair      Good     
## [18079] Very Good Premium   Very Good Good      Very Good Good     
## [18085] Premium   Very Good Premium   Premium   Premium   Premium  
## [18091] Very Good Very Good Ideal     Good      Premium   Premium  
## [18097] Premium   Very Good Premium   Premium   Very Good Premium  
## [18103] Premium   Ideal     Ideal     Ideal     Ideal     Ideal    
## [18109] Ideal     Premium   Premium   Premium   Ideal     Ideal    
## [18115] Very Good Good      Very Good Fair      Ideal     Premium  
## [18121] Fair      Ideal     Premium   Ideal     Fair      Premium  
## [18127] Premium   Ideal     Very Good Good      Premium   Premium  
## [18133] Premium   Premium   Very Good Ideal     Very Good Very Good
## [18139] Very Good Ideal     Ideal     Ideal     Good      Very Good
## [18145] Premium   Premium   Premium   Premium   Premium   Ideal    
## [18151] Ideal     Ideal     Premium   Premium   Ideal     Ideal    
## [18157] Ideal     Very Good Premium   Premium   Good      Premium  
## [18163] Ideal     Ideal     Very Good Very Good Good      Premium  
## [18169] Ideal     Ideal     Premium   Premium   Very Good Very Good
## [18175] Ideal     Good      Ideal     Very Good Good      Good     
## [18181] Ideal     Very Good Ideal     Premium   Premium   Very Good
## [18187] Very Good Ideal     Ideal     Ideal     Ideal     Very Good
## [18193] Premium   Good      Good      Ideal     Premium   Ideal    
## [18199] Ideal     Very Good Ideal     Fair      Fair      Very Good
## [18205] Premium   Premium   Premium   Premium   Premium   Ideal    
## [18211] Ideal     Premium   Fair      Very Good Very Good Ideal    
## [18217] Premium   Premium   Good      Premium   Ideal     Premium  
## [18223] Premium   Premium   Premium   Good      Premium   Very Good
## [18229] Ideal     Very Good Ideal     Premium   Premium   Premium  
## [18235] Ideal     Premium   Very Good Very Good Premium   Very Good
## [18241] Ideal     Ideal     Ideal     Premium   Very Good Ideal    
## [18247] Very Good Ideal     Ideal     Ideal     Ideal     Very Good
## [18253] Premium   Ideal     Very Good Very Good Very Good Ideal    
## [18259] Ideal     Ideal     Very Good Very Good Premium   Very Good
## [18265] Good      Very Good Premium   Very Good Ideal     Premium  
## [18271] Premium   Ideal     Premium   Premium   Premium   Very Good
## [18277] Ideal     Ideal     Very Good Ideal     Ideal     Premium  
## [18283] Ideal     Very Good Very Good Ideal     Ideal     Good     
## [18289] Ideal     Ideal     Premium   Very Good Fair      Good     
## [18295] Good      Good      Very Good Premium   Very Good Very Good
## [18301] Very Good Premium   Ideal     Ideal     Ideal     Ideal    
## [18307] Very Good Premium   Premium   Very Good Ideal     Ideal    
## [18313] Very Good Premium   Very Good Very Good Very Good Ideal    
## [18319] Premium   Good      Very Good Very Good Very Good Ideal    
## [18325] Very Good Premium   Very Good Premium   Very Good Ideal    
## [18331] Ideal     Ideal     Premium   Ideal     Ideal     Very Good
## [18337] Ideal     Fair      Very Good Ideal     Ideal     Ideal    
## [18343] Good      Ideal     Ideal     Premium   Ideal     Ideal    
## [18349] Premium   Ideal     Premium   Premium   Premium   Good     
## [18355] Very Good Ideal     Good      Fair      Fair      Ideal    
## [18361] Premium   Very Good Premium   Very Good Premium   Very Good
## [18367] Ideal     Good      Good      Very Good Ideal     Premium  
## [18373] Good      Ideal     Very Good Ideal     Good      Good     
## [18379] Good      Good      Premium   Very Good Good      Ideal    
## [18385] Ideal     Ideal     Fair      Very Good Very Good Very Good
## [18391] Premium   Ideal     Ideal     Ideal     Premium   Good     
## [18397] Fair      Ideal     Very Good Very Good Ideal     Ideal    
## [18403] Premium   Premium   Premium   Ideal     Good      Very Good
## [18409] Ideal     Ideal     Very Good Premium   Premium   Ideal    
## [18415] Ideal     Premium   Premium   Premium   Premium   Very Good
## [18421] Ideal     Ideal     Ideal     Very Good Good      Good     
## [18427] Premium   Premium   Premium   Ideal     Very Good Good     
## [18433] Premium   Premium   Good      Ideal     Premium   Good     
## [18439] Premium   Good      Premium   Very Good Very Good Ideal    
## [18445] Premium   Premium   Very Good Ideal     Ideal     Very Good
## [18451] Very Good Very Good Ideal     Very Good Premium   Premium  
## [18457] Good      Ideal     Ideal     Premium   Very Good Fair     
## [18463] Very Good Premium   Very Good Ideal     Ideal     Ideal    
## [18469] Premium   Good      Ideal     Ideal     Ideal     Ideal    
## [18475] Very Good Premium   Premium   Ideal     Very Good Ideal    
## [18481] Ideal     Ideal     Premium   Premium   Premium   Premium  
## [18487] Very Good Ideal     Very Good Good      Good      Very Good
## [18493] Ideal     Premium   Very Good Ideal     Premium   Very Good
## [18499] Fair      Premium   Premium   Premium   Premium   Premium  
## [18505] Good      Very Good Very Good Ideal     Premium   Very Good
## [18511] Very Good Ideal     Ideal     Ideal     Ideal     Ideal    
## [18517] Ideal     Premium   Good      Very Good Very Good Premium  
## [18523] Ideal     Premium   Ideal     Ideal     Premium   Ideal    
## [18529] Ideal     Ideal     Fair      Premium   Premium   Premium  
## [18535] Premium   Premium   Premium   Good      Premium   Premium  
## [18541] Good      Very Good Premium   Premium   Fair      Very Good
## [18547] Premium   Premium   Ideal     Premium   Ideal     Ideal    
## [18553] Good      Ideal     Ideal     Premium   Ideal     Very Good
## [18559] Very Good Very Good Very Good Very Good Premium   Ideal    
## [18565] Ideal     Good      Premium   Ideal     Premium   Ideal    
## [18571] Premium   Very Good Premium   Ideal     Ideal     Premium  
## [18577] Premium   Good      Ideal     Premium   Very Good Premium  
## [18583] Ideal     Ideal     Very Good Very Good Very Good Very Good
## [18589] Very Good Ideal     Premium   Very Good Very Good Ideal    
## [18595] Ideal     Ideal     Premium   Premium   Premium   Ideal    
## [18601] Very Good Very Good Ideal     Very Good Very Good Ideal    
## [18607] Very Good Premium   Premium   Ideal     Ideal     Premium  
## [18613] Ideal     Ideal     Premium   Premium   Ideal     Ideal    
## [18619] Very Good Very Good Ideal     Ideal     Very Good Ideal    
## [18625] Very Good Ideal     Ideal     Premium   Good      Premium  
## [18631] Ideal     Ideal     Ideal     Premium   Ideal     Ideal    
## [18637] Good      Premium   Premium   Very Good Very Good Premium  
## [18643] Ideal     Ideal     Premium   Very Good Premium   Ideal    
## [18649] Premium   Ideal     Ideal     Ideal     Ideal     Premium  
## [18655] Ideal     Premium   Very Good Ideal     Premium   Premium  
## [18661] Ideal     Premium   Ideal     Ideal     Premium   Premium  
## [18667] Ideal     Ideal     Premium   Ideal     Ideal     Ideal    
## [18673] Ideal     Very Good Very Good Very Good Premium   Ideal    
## [18679] Very Good Premium   Premium   Ideal     Good      Ideal    
## [18685] Ideal     Good      Premium   Premium   Premium   Premium  
## [18691] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [18697] Good      Ideal     Good      Very Good Very Good Ideal    
## [18703] Good      Very Good Very Good Very Good Very Good Very Good
## [18709] Very Good Very Good Very Good Very Good Very Good Very Good
## [18715] Very Good Very Good Very Good Very Good Very Good Very Good
## [18721] Very Good Very Good Ideal     Premium   Premium   Fair     
## [18727] Very Good Ideal     Ideal     Premium   Ideal     Ideal    
## [18733] Very Good Premium   Very Good Ideal     Premium   Very Good
## [18739] Ideal     Ideal     Premium   Very Good Ideal     Ideal    
## [18745] Premium   Very Good Ideal     Premium   Very Good Ideal    
## [18751] Good      Premium   Premium   Good      Very Good Ideal    
## [18757] Ideal     Very Good Good      Premium   Very Good Ideal    
## [18763] Premium   Good      Ideal     Premium   Very Good Premium  
## [18769] Very Good Premium   Very Good Very Good Premium   Premium  
## [18775] Premium   Premium   Premium   Very Good Premium   Ideal    
## [18781] Ideal     Very Good Premium   Premium   Ideal     Very Good
## [18787] Very Good Premium   Premium   Premium   Very Good Ideal    
## [18793] Ideal     Ideal     Ideal     Ideal     Good      Ideal    
## [18799] Very Good Very Good Very Good Premium   Ideal     Ideal    
## [18805] Good      Good      Premium   Fair      Ideal     Ideal    
## [18811] Good      Ideal     Ideal     Ideal     Very Good Fair     
## [18817] Very Good Very Good Very Good Premium   Ideal     Very Good
## [18823] Very Good Ideal     Premium   Very Good Premium   Good     
## [18829] Very Good Premium   Premium   Very Good Ideal     Very Good
## [18835] Premium   Premium   Ideal     Good      Very Good Premium  
## [18841] Very Good Very Good Good      Very Good Very Good Ideal    
## [18847] Ideal     Good      Premium   Fair      Premium   Ideal    
## [18853] Premium   Very Good Premium   Premium   Ideal     Premium  
## [18859] Very Good Very Good Premium   Premium   Premium   Premium  
## [18865] Ideal     Very Good Good      Very Good Ideal     Good     
## [18871] Ideal     Very Good Very Good Ideal     Ideal     Premium  
## [18877] Good      Premium   Premium   Ideal     Very Good Ideal    
## [18883] Very Good Ideal     Very Good Premium   Ideal     Ideal    
## [18889] Premium   Fair      Very Good Very Good Ideal     Very Good
## [18895] Ideal     Very Good Ideal     Premium   Premium   Ideal    
## [18901] Premium   Premium   Premium   Ideal     Premium   Premium  
## [18907] Ideal     Premium   Very Good Premium   Premium   Ideal    
## [18913] Premium   Ideal     Premium   Premium   Premium   Premium  
## [18919] Ideal     Very Good Ideal     Good      Ideal     Very Good
## [18925] Ideal     Very Good Very Good Very Good Ideal     Very Good
## [18931] Premium   Very Good Premium   Premium   Ideal     Ideal    
## [18937] Premium   Ideal     Very Good Very Good Premium   Good     
## [18943] Very Good Very Good Ideal     Ideal     Ideal     Ideal    
## [18949] Ideal     Ideal     Ideal     Premium   Premium   Very Good
## [18955] Premium   Ideal     Very Good Premium   Premium   Premium  
## [18961] Very Good Good      Premium   Premium   Ideal     Ideal    
## [18967] Very Good Ideal     Ideal     Ideal     Ideal     Ideal    
## [18973] Ideal     Ideal     Premium   Ideal     Premium   Premium  
## [18979] Very Good Ideal     Premium   Premium   Very Good Ideal    
## [18985] Premium   Ideal     Good      Very Good Premium   Premium  
## [18991] Premium   Premium   Ideal     Fair      Ideal     Premium  
## [18997] Premium   Premium   Ideal     Premium   Premium   Premium  
## [19003] Very Good Ideal     Premium   Ideal     Good      Ideal    
## [19009] Very Good Very Good Premium   Ideal     Good      Very Good
## [19015] Very Good Fair      Premium   Premium   Very Good Very Good
## [19021] Very Good Ideal     Ideal     Ideal     Ideal     Ideal    
## [19027] Ideal     Good      Premium   Very Good Very Good Ideal    
## [19033] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [19039] Ideal     Ideal     Ideal     Ideal     Good      Good     
## [19045] Very Good Premium   Very Good Very Good Very Good Very Good
## [19051] Very Good Ideal     Ideal     Premium   Very Good Premium  
## [19057] Ideal     Very Good Ideal     Premium   Very Good Premium  
## [19063] Ideal     Very Good Good      Ideal     Premium   Very Good
## [19069] Good      Premium   Good      Premium   Premium   Fair     
## [19075] Ideal     Premium   Ideal     Premium   Ideal     Ideal    
## [19081] Premium   Fair      Premium   Premium   Ideal     Ideal    
## [19087] Very Good Good      Very Good Good      Good      Ideal    
## [19093] Very Good Premium   Premium   Good      Premium   Premium  
## [19099] Premium   Premium   Very Good Ideal     Ideal     Ideal    
## [19105] Premium   Premium   Good      Ideal     Very Good Very Good
## [19111] Very Good Ideal     Premium   Ideal     Premium   Ideal    
## [19117] Premium   Very Good Ideal     Ideal     Premium   Ideal    
## [19123] Ideal     Very Good Very Good Very Good Very Good Ideal    
## [19129] Good      Ideal     Premium   Very Good Ideal     Ideal    
## [19135] Ideal     Very Good Premium   Premium   Ideal     Premium  
## [19141] Premium   Premium   Very Good Very Good Ideal     Premium  
## [19147] Very Good Premium   Very Good Premium   Very Good Good     
## [19153] Fair      Good      Very Good Ideal     Ideal     Ideal    
## [19159] Ideal     Ideal     Fair      Premium   Premium   Ideal    
## [19165] Premium   Ideal     Ideal     Ideal     Premium   Ideal    
## [19171] Ideal     Very Good Very Good Very Good Fair      Ideal    
## [19177] Good      Ideal     Premium   Premium   Good      Ideal    
## [19183] Ideal     Fair      Ideal     Ideal     Ideal     Very Good
## [19189] Ideal     Ideal     Very Good Very Good Ideal     Premium  
## [19195] Very Good Ideal     Premium   Ideal     Good      Very Good
## [19201] Premium   Fair      Fair      Ideal     Premium   Very Good
## [19207] Premium   Premium   Premium   Ideal     Good      Ideal    
## [19213] Ideal     Premium   Very Good Very Good Good      Good     
## [19219] Fair      Ideal     Premium   Premium   Very Good Premium  
## [19225] Premium   Premium   Premium   Very Good Ideal     Ideal    
## [19231] Ideal     Very Good Ideal     Very Good Fair      Premium  
## [19237] Good      Very Good Very Good Very Good Premium   Very Good
## [19243] Ideal     Ideal     Ideal     Ideal     Good      Premium  
## [19249] Premium   Premium   Ideal     Very Good Ideal     Premium  
## [19255] Very Good Ideal     Premium   Ideal     Ideal     Ideal    
## [19261] Premium   Very Good Ideal     Ideal     Very Good Very Good
## [19267] Premium   Very Good Ideal     Ideal     Ideal     Premium  
## [19273] Ideal     Ideal     Ideal     Premium   Fair      Premium  
## [19279] Ideal     Very Good Premium   Premium   Good      Premium  
## [19285] Very Good Very Good Premium   Ideal     Fair      Premium  
## [19291] Good      Ideal     Ideal     Ideal     Good      Very Good
## [19297] Ideal     Ideal     Ideal     Premium   Premium   Ideal    
## [19303] Premium   Good      Premium   Very Good Very Good Ideal    
## [19309] Ideal     Premium   Premium   Ideal     Ideal     Ideal    
## [19315] Premium   Premium   Ideal     Ideal     Ideal     Premium  
## [19321] Very Good Good      Very Good Premium   Premium   Ideal    
## [19327] Ideal     Ideal     Ideal     Ideal     Ideal     Fair     
## [19333] Ideal     Ideal     Very Good Very Good Ideal     Very Good
## [19339] Ideal     Premium   Ideal     Good      Premium   Ideal    
## [19345] Fair      Premium   Fair      Good      Good      Very Good
## [19351] Ideal     Ideal     Ideal     Ideal     Good      Good     
## [19357] Good      Very Good Ideal     Very Good Ideal     Ideal    
## [19363] Ideal     Ideal     Very Good Very Good Good      Good     
## [19369] Good      Good      Ideal     Ideal     Ideal     Very Good
## [19375] Premium   Ideal     Good      Good      Good      Good     
## [19381] Ideal     Very Good Ideal     Very Good Premium   Ideal    
## [19387] Premium   Premium   Premium   Very Good Very Good Ideal    
## [19393] Very Good Premium   Very Good Ideal     Ideal     Ideal    
## [19399] Premium   Premium   Ideal     Ideal     Premium   Ideal    
## [19405] Ideal     Very Good Fair      Premium   Premium   Premium  
## [19411] Ideal     Ideal     Premium   Very Good Premium   Ideal    
## [19417] Very Good Ideal     Very Good Very Good Ideal     Premium  
## [19423] Ideal     Ideal     Good      Very Good Ideal     Very Good
## [19429] Ideal     Ideal     Good      Ideal     Very Good Very Good
## [19435] Ideal     Ideal     Very Good Very Good Ideal     Ideal    
## [19441] Ideal     Ideal     Ideal     Ideal     Ideal     Very Good
## [19447] Good      Ideal     Good      Ideal     Premium   Premium  
## [19453] Premium   Good      Ideal     Very Good Premium   Ideal    
## [19459] Very Good Good      Premium   Ideal     Good      Premium  
## [19465] Premium   Good      Good      Premium   Good      Premium  
## [19471] Premium   Ideal     Ideal     Ideal     Good      Premium  
## [19477] Ideal     Premium   Very Good Good      Premium   Premium  
## [19483] Ideal     Ideal     Ideal     Premium   Premium   Ideal    
## [19489] Very Good Ideal     Premium   Premium   Very Good Premium  
## [19495] Ideal     Very Good Ideal     Ideal     Ideal     Ideal    
## [19501] Ideal     Premium   Fair      Good      Premium   Very Good
## [19507] Very Good Ideal     Premium   Premium   Good      Premium  
## [19513] Ideal     Good      Premium   Very Good Ideal     Premium  
## [19519] Very Good Very Good Premium   Premium   Ideal     Ideal    
## [19525] Ideal     Ideal     Good      Ideal     Very Good Premium  
## [19531] Premium   Good      Good      Very Good Ideal     Ideal    
## [19537] Ideal     Premium   Ideal     Very Good Ideal     Very Good
## [19543] Ideal     Premium   Premium   Premium   Good      Premium  
## [19549] Premium   Ideal     Premium   Premium   Very Good Very Good
## [19555] Ideal     Ideal     Premium   Premium   Premium   Premium  
## [19561] Fair      Good      Very Good Very Good Ideal     Ideal    
## [19567] Very Good Very Good Premium   Premium   Premium   Ideal    
## [19573] Ideal     Ideal     Premium   Premium   Premium   Premium  
## [19579] Ideal     Good      Very Good Ideal     Good      Ideal    
## [19585] Premium   Very Good Very Good Very Good Premium   Fair     
## [19591] Very Good Ideal     Ideal     Very Good Premium   Very Good
## [19597] Good      Ideal     Premium   Ideal     Very Good Premium  
## [19603] Premium   Ideal     Very Good Ideal     Ideal     Very Good
## [19609] Ideal     Ideal     Ideal     Ideal     Very Good Premium  
## [19615] Very Good Ideal     Ideal     Very Good Ideal     Ideal    
## [19621] Ideal     Ideal     Good      Very Good Good      Good     
## [19627] Ideal     Good      Good      Very Good Ideal     Ideal    
## [19633] Ideal     Ideal     Ideal     Good      Premium   Ideal    
## [19639] Ideal     Premium   Very Good Very Good Ideal     Very Good
## [19645] Good      Premium   Premium   Premium   Fair      Very Good
## [19651] Ideal     Premium   Fair      Very Good Ideal     Premium  
## [19657] Premium   Ideal     Very Good Ideal     Very Good Ideal    
## [19663] Good      Ideal     Ideal     Ideal     Good      Ideal    
## [19669] Ideal     Premium   Ideal     Good      Premium   Ideal    
## [19675] Good      Ideal     Ideal     Very Good Ideal     Ideal    
## [19681] Ideal     Very Good Very Good Ideal     Premium   Very Good
## [19687] Very Good Ideal     Ideal     Ideal     Ideal     Ideal    
## [19693] Ideal     Ideal     Ideal     Ideal     Ideal     Very Good
## [19699] Premium   Ideal     Ideal     Very Good Very Good Premium  
## [19705] Premium   Premium   Premium   Good      Very Good Ideal    
## [19711] Ideal     Premium   Fair      Fair      Ideal     Very Good
## [19717] Ideal     Very Good Very Good Ideal     Premium   Premium  
## [19723] Premium   Ideal     Ideal     Ideal     Very Good Ideal    
## [19729] Ideal     Premium   Ideal     Ideal     Ideal     Premium  
## [19735] Very Good Ideal     Ideal     Premium   Ideal     Ideal    
## [19741] Premium   Very Good Good      Ideal     Premium   Very Good
## [19747] Good      Premium   Premium   Very Good Very Good Ideal    
## [19753] Premium   Premium   Ideal     Ideal     Premium   Premium  
## [19759] Ideal     Premium   Very Good Premium   Premium   Premium  
## [19765] Premium   Very Good Ideal     Ideal     Good      Very Good
## [19771] Premium   Ideal     Ideal     Premium   Ideal     Ideal    
## [19777] Premium   Ideal     Ideal     Good      Very Good Ideal    
## [19783] Ideal     Very Good Premium   Fair      Very Good Very Good
## [19789] Ideal     Good      Premium   Very Good Fair      Premium  
## [19795] Ideal     Ideal     Premium   Ideal     Ideal     Premium  
## [19801] Premium   Premium   Ideal     Premium   Good      Premium  
## [19807] Ideal     Very Good Very Good Good      Very Good Very Good
## [19813] Ideal     Very Good Good      Premium   Premium   Good     
## [19819] Ideal     Ideal     Good      Fair      Premium   Very Good
## [19825] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [19831] Ideal     Premium   Ideal     Ideal     Premium   Ideal    
## [19837] Very Good Ideal     Ideal     Ideal     Premium   Very Good
## [19843] Very Good Ideal     Very Good Ideal     Very Good Very Good
## [19849] Premium   Very Good Premium   Very Good Ideal     Ideal    
## [19855] Ideal     Ideal     Good      Very Good Very Good Ideal    
## [19861] Ideal     Ideal     Premium   Premium   Ideal     Very Good
## [19867] Premium   Premium   Ideal     Ideal     Premium   Premium  
## [19873] Premium   Premium   Very Good Ideal     Ideal     Ideal    
## [19879] Good      Ideal     Ideal     Very Good Premium   Premium  
## [19885] Very Good Ideal     Very Good Very Good Good      Good     
## [19891] Premium   Ideal     Very Good Good      Very Good Very Good
## [19897] Premium   Premium   Premium   Premium   Ideal     Ideal    
## [19903] Ideal     Ideal     Very Good Ideal     Premium   Very Good
## [19909] Ideal     Premium   Premium   Ideal     Premium   Premium  
## [19915] Very Good Premium   Good      Ideal     Premium   Good     
## [19921] Very Good Premium   Premium   Very Good Premium   Ideal    
## [19927] Very Good Premium   Premium   Premium   Ideal     Premium  
## [19933] Ideal     Ideal     Ideal     Ideal     Premium   Ideal    
## [19939] Ideal     Premium   Ideal     Premium   Ideal     Premium  
## [19945] Very Good Ideal     Premium   Good      Good      Ideal    
## [19951] Ideal     Premium   Ideal     Premium   Ideal     Premium  
## [19957] Ideal     Very Good Ideal     Ideal     Ideal     Premium  
## [19963] Premium   Very Good Very Good Very Good Ideal     Good     
## [19969] Ideal     Very Good Good      Premium   Very Good Premium  
## [19975] Good      Ideal     Premium   Very Good Premium   Ideal    
## [19981] Very Good Very Good Premium   Very Good Fair      Ideal    
## [19987] Ideal     Premium   Premium   Ideal     Ideal     Very Good
## [19993] Premium   Ideal     Premium   Good      Very Good Very Good
## [19999] Very Good Very Good Premium   Ideal     Very Good Ideal    
## [20005] Premium   Ideal     Ideal     Ideal     Ideal     Ideal    
## [20011] Very Good Good      Premium   Very Good Premium   Very Good
## [20017] Very Good Good      Premium   Good      Good      Very Good
## [20023] Ideal     Very Good Very Good Good      Premium   Very Good
## [20029] Very Good Very Good Very Good Very Good Very Good Premium  
## [20035] Very Good Very Good Ideal     Very Good Good      Premium  
## [20041] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [20047] Ideal     Ideal     Ideal     Ideal     Premium   Very Good
## [20053] Premium   Ideal     Ideal     Very Good Very Good Ideal    
## [20059] Good      Ideal     Very Good Very Good Premium   Ideal    
## [20065] Ideal     Ideal     Good      Ideal     Ideal     Ideal    
## [20071] Very Good Premium   Very Good Good      Premium   Ideal    
## [20077] Very Good Very Good Ideal     Good      Ideal     Ideal    
## [20083] Ideal     Very Good Premium   Premium   Good      Very Good
## [20089] Ideal     Premium   Ideal     Premium   Good      Premium  
## [20095] Premium   Premium   Ideal     Very Good Ideal     Very Good
## [20101] Very Good Premium   Fair      Very Good Premium   Premium  
## [20107] Very Good Good      Ideal     Ideal     Premium   Very Good
## [20113] Very Good Premium   Premium   Very Good Ideal     Ideal    
## [20119] Ideal     Premium   Premium   Ideal     Ideal     Premium  
## [20125] Ideal     Very Good Premium   Ideal     Ideal     Very Good
## [20131] Ideal     Ideal     Premium   Premium   Premium   Premium  
## [20137] Ideal     Ideal     Ideal     Premium   Very Good Premium  
## [20143] Premium   Ideal     Premium   Premium   Premium   Premium  
## [20149] Premium   Premium   Ideal     Ideal     Ideal     Ideal    
## [20155] Premium   Ideal     Ideal     Ideal     Premium   Ideal    
## [20161] Premium   Ideal     Ideal     Very Good Ideal     Very Good
## [20167] Very Good Ideal     Premium   Premium   Very Good Good     
## [20173] Ideal     Very Good Fair      Very Good Ideal     Ideal    
## [20179] Premium   Very Good Premium   Premium   Premium   Premium  
## [20185] Premium   Ideal     Very Good Premium   Very Good Very Good
## [20191] Very Good Ideal     Very Good Ideal     Very Good Ideal    
## [20197] Ideal     Very Good Ideal     Premium   Ideal     Ideal    
## [20203] Premium   Ideal     Very Good Very Good Very Good Ideal    
## [20209] Ideal     Ideal     Ideal     Premium   Ideal     Good     
## [20215] Ideal     Ideal     Premium   Very Good Ideal     Premium  
## [20221] Premium   Premium   Very Good Very Good Very Good Very Good
## [20227] Ideal     Ideal     Premium   Very Good Very Good Very Good
## [20233] Ideal     Very Good Premium   Fair      Very Good Premium  
## [20239] Ideal     Premium   Premium   Good      Ideal     Ideal    
## [20245] Fair      Premium   Ideal     Ideal     Ideal     Premium  
## [20251] Fair      Ideal     Premium   Very Good Ideal     Premium  
## [20257] Ideal     Ideal     Ideal     Premium   Ideal     Premium  
## [20263] Premium   Ideal     Very Good Very Good Premium   Premium  
## [20269] Ideal     Ideal     Premium   Premium   Premium   Ideal    
## [20275] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [20281] Very Good Good      Premium   Very Good Good      Premium  
## [20287] Ideal     Very Good Good      Very Good Ideal     Ideal    
## [20293] Premium   Ideal     Ideal     Ideal     Very Good Fair     
## [20299] Very Good Very Good Ideal     Fair      Fair      Premium  
## [20305] Premium   Ideal     Good      Premium   Premium   Very Good
## [20311] Ideal     Ideal     Ideal     Ideal     Ideal     Good     
## [20317] Premium   Good      Ideal     Premium   Very Good Good     
## [20323] Good      Good      Premium   Premium   Very Good Ideal    
## [20329] Premium   Premium   Good      Ideal     Good      Ideal    
## [20335] Premium   Ideal     Premium   Fair      Premium   Premium  
## [20341] Premium   Premium   Good      Ideal     Good      Ideal    
## [20347] Ideal     Ideal     Premium   Very Good Premium   Very Good
## [20353] Premium   Good      Good      Good      Ideal     Ideal    
## [20359] Premium   Premium   Ideal     Ideal     Very Good Ideal    
## [20365] Premium   Premium   Ideal     Ideal     Premium   Ideal    
## [20371] Very Good Premium   Good      Premium   Very Good Very Good
## [20377] Ideal     Ideal     Very Good Ideal     Premium   Ideal    
## [20383] Good      Very Good Premium   Premium   Premium   Premium  
## [20389] Good      Premium   Ideal     Premium   Good      Very Good
## [20395] Very Good Very Good Premium   Ideal     Premium   Ideal    
## [20401] Ideal     Premium   Premium   Very Good Premium   Premium  
## [20407] Very Good Ideal     Premium   Ideal     Very Good Ideal    
## [20413] Premium   Ideal     Ideal     Premium   Ideal     Ideal    
## [20419] Ideal     Premium   Good      Ideal     Ideal     Ideal    
## [20425] Fair      Premium   Good      Good      Ideal     Very Good
## [20431] Ideal     Very Good Very Good Very Good Ideal     Very Good
## [20437] Very Good Ideal     Very Good Very Good Premium   Fair     
## [20443] Premium   Premium   Fair      Good      Ideal     Premium  
## [20449] Ideal     Very Good Premium   Ideal     Ideal     Premium  
## [20455] Premium   Very Good Ideal     Ideal     Premium   Fair     
## [20461] Ideal     Ideal     Fair      Premium   Very Good Premium  
## [20467] Ideal     Premium   Premium   Ideal     Premium   Premium  
## [20473] Ideal     Very Good Premium   Premium   Fair      Ideal    
## [20479] Ideal     Premium   Very Good Good      Premium   Premium  
## [20485] Premium   Ideal     Premium   Premium   Good      Premium  
## [20491] Ideal     Premium   Very Good Premium   Ideal     Very Good
## [20497] Premium   Ideal     Ideal     Ideal     Good      Very Good
## [20503] Ideal     Premium   Very Good Premium   Ideal     Premium  
## [20509] Premium   Premium   Premium   Ideal     Very Good Premium  
## [20515] Ideal     Premium   Ideal     Very Good Ideal     Premium  
## [20521] Very Good Very Good Ideal     Good      Premium   Premium  
## [20527] Premium   Ideal     Premium   Good      Premium   Ideal    
## [20533] Very Good Premium   Premium   Ideal     Premium   Ideal    
## [20539] Ideal     Ideal     Good      Premium   Ideal     Ideal    
## [20545] Ideal     Premium   Good      Very Good Premium   Ideal    
## [20551] Ideal     Fair      Very Good Ideal     Premium   Very Good
## [20557] Premium   Very Good Premium   Premium   Ideal     Very Good
## [20563] Premium   Premium   Premium   Premium   Ideal     Premium  
## [20569] Fair      Premium   Premium   Very Good Ideal     Ideal    
## [20575] Ideal     Premium   Premium   Premium   Premium   Ideal    
## [20581] Ideal     Premium   Ideal     Very Good Very Good Ideal    
## [20587] Very Good Ideal     Ideal     Very Good Very Good Premium  
## [20593] Premium   Ideal     Good      Very Good Good      Ideal    
## [20599] Good      Ideal     Premium   Very Good Ideal     Good     
## [20605] Good      Good      Very Good Ideal     Premium   Premium  
## [20611] Premium   Premium   Very Good Very Good Ideal     Ideal    
## [20617] Good      Very Good Premium   Very Good Premium   Ideal    
## [20623] Good      Ideal     Very Good Very Good Ideal     Ideal    
## [20629] Premium   Premium   Ideal     Premium   Ideal     Very Good
## [20635] Ideal     Very Good Very Good Premium   Premium   Premium  
## [20641] Ideal     Very Good Ideal     Ideal     Very Good Premium  
## [20647] Premium   Fair      Very Good Very Good Very Good Very Good
## [20653] Ideal     Good      Good      Ideal     Premium   Ideal    
## [20659] Ideal     Very Good Very Good Ideal     Premium   Very Good
## [20665] Ideal     Good      Very Good Very Good Ideal     Ideal    
## [20671] Premium   Fair      Ideal     Ideal     Ideal     Good     
## [20677] Ideal     Ideal     Premium   Ideal     Ideal     Ideal    
## [20683] Good      Very Good Premium   Premium   Very Good Premium  
## [20689] Premium   Ideal     Ideal     Ideal     Premium   Ideal    
## [20695] Ideal     Ideal     Very Good Good      Very Good Very Good
## [20701] Very Good Very Good Ideal     Premium   Ideal     Ideal    
## [20707] Premium   Ideal     Premium   Premium   Premium   Very Good
## [20713] Ideal     Ideal     Ideal     Very Good Ideal     Good     
## [20719] Premium   Very Good Premium   Premium   Ideal     Very Good
## [20725] Very Good Ideal     Premium   Ideal     Premium   Good     
## [20731] Good      Good      Ideal     Ideal     Good      Ideal    
## [20737] Ideal     Ideal     Premium   Ideal     Premium   Ideal    
## [20743] Ideal     Ideal     Ideal     Very Good Ideal     Ideal    
## [20749] Premium   Ideal     Very Good Good      Ideal     Very Good
## [20755] Good      Premium   Fair      Premium   Very Good Ideal    
## [20761] Premium   Ideal     Very Good Very Good Ideal     Premium  
## [20767] Good      Ideal     Very Good Ideal     Premium   Very Good
## [20773] Ideal     Good      Ideal     Premium   Very Good Ideal    
## [20779] Premium   Ideal     Premium   Premium   Premium   Premium  
## [20785] Very Good Premium   Ideal     Premium   Premium   Good     
## [20791] Good      Ideal     Very Good Very Good Ideal     Ideal    
## [20797] Ideal     Premium   Premium   Ideal     Ideal     Very Good
## [20803] Very Good Very Good Ideal     Ideal     Very Good Ideal    
## [20809] Premium   Premium   Ideal     Premium   Ideal     Ideal    
## [20815] Premium   Very Good Ideal     Fair      Very Good Ideal    
## [20821] Very Good Premium   Premium   Premium   Ideal     Very Good
## [20827] Very Good Premium   Premium   Ideal     Ideal     Premium  
## [20833] Very Good Ideal     Very Good Very Good Premium   Ideal    
## [20839] Good      Very Good Good      Ideal     Very Good Good     
## [20845] Ideal     Good      Very Good Premium   Good      Premium  
## [20851] Premium   Ideal     Ideal     Ideal     Ideal     Very Good
## [20857] Very Good Ideal     Ideal     Ideal     Premium   Premium  
## [20863] Ideal     Premium   Premium   Premium   Ideal     Premium  
## [20869] Good      Very Good Very Good Ideal     Ideal     Ideal    
## [20875] Ideal     Very Good Ideal     Fair      Ideal     Good     
## [20881] Premium   Ideal     Ideal     Premium   Very Good Premium  
## [20887] Premium   Very Good Premium   Ideal     Ideal     Ideal    
## [20893] Premium   Good      Very Good Fair      Ideal     Premium  
## [20899] Good      Ideal     Ideal     Ideal     Very Good Ideal    
## [20905] Ideal     Premium   Premium   Good      Premium   Very Good
## [20911] Ideal     Ideal     Premium   Very Good Very Good Good     
## [20917] Ideal     Ideal     Ideal     Ideal     Premium   Very Good
## [20923] Ideal     Ideal     Premium   Premium   Very Good Very Good
## [20929] Very Good Ideal     Premium   Ideal     Very Good Very Good
## [20935] Ideal     Very Good Ideal     Ideal     Premium   Ideal    
## [20941] Very Good Premium   Premium   Very Good Very Good Good     
## [20947] Ideal     Premium   Ideal     Premium   Very Good Very Good
## [20953] Ideal     Ideal     Good      Very Good Ideal     Premium  
## [20959] Premium   Good      Very Good Ideal     Ideal     Good     
## [20965] Very Good Ideal     Premium   Ideal     Very Good Very Good
## [20971] Very Good Very Good Very Good Premium   Ideal     Ideal    
## [20977] Ideal     Good      Very Good Premium   Ideal     Ideal    
## [20983] Premium   Very Good Premium   Very Good Very Good Ideal    
## [20989] Very Good Very Good Premium   Premium   Premium   Ideal    
## [20995] Ideal     Ideal     Ideal     Ideal     Premium   Ideal    
## [21001] Ideal     Premium   Premium   Good      Very Good Very Good
## [21007] Ideal     Ideal     Very Good Premium   Premium   Ideal    
## [21013] Ideal     Premium   Premium   Premium   Ideal     Very Good
## [21019] Premium   Ideal     Ideal     Ideal     Ideal     Premium  
## [21025] Premium   Ideal     Good      Ideal     Very Good Ideal    
## [21031] Very Good Good      Ideal     Premium   Ideal     Very Good
## [21037] Good      Premium   Very Good Very Good Ideal     Premium  
## [21043] Good      Premium   Good      Ideal     Premium   Ideal    
## [21049] Ideal     Ideal     Premium   Premium   Very Good Premium  
## [21055] Ideal     Ideal     Premium   Ideal     Ideal     Good     
## [21061] Ideal     Very Good Premium   Premium   Ideal     Ideal    
## [21067] Very Good Ideal     Ideal     Ideal     Very Good Very Good
## [21073] Ideal     Ideal     Ideal     Ideal     Ideal     Very Good
## [21079] Premium   Premium   Ideal     Very Good Ideal     Good     
## [21085] Premium   Very Good Premium   Ideal     Good      Ideal    
## [21091] Ideal     Very Good Premium   Ideal     Ideal     Premium  
## [21097] Premium   Premium   Premium   Ideal     Premium   Ideal    
## [21103] Premium   Ideal     Ideal     Ideal     Very Good Good     
## [21109] Ideal     Premium   Ideal     Very Good Very Good Good     
## [21115] Good      Ideal     Ideal     Ideal     Very Good Premium  
## [21121] Very Good Very Good Ideal     Ideal     Ideal     Very Good
## [21127] Ideal     Premium   Ideal     Ideal     Very Good Premium  
## [21133] Fair      Premium   Ideal     Ideal     Premium   Good     
## [21139] Premium   Premium   Ideal     Ideal     Ideal     Premium  
## [21145] Good      Premium   Good      Ideal     Very Good Premium  
## [21151] Premium   Premium   Premium   Very Good Ideal     Premium  
## [21157] Fair      Premium   Ideal     Ideal     Ideal     Premium  
## [21163] Premium   Ideal     Very Good Good      Ideal     Premium  
## [21169] Premium   Premium   Premium   Very Good Ideal     Very Good
## [21175] Very Good Ideal     Ideal     Premium   Premium   Ideal    
## [21181] Premium   Ideal     Ideal     Very Good Ideal     Good     
## [21187] Very Good Ideal     Ideal     Premium   Premium   Very Good
## [21193] Ideal     Good      Very Good Very Good Very Good Very Good
## [21199] Ideal     Ideal     Very Good Premium   Ideal     Very Good
## [21205] Ideal     Ideal     Ideal     Ideal     Ideal     Very Good
## [21211] Ideal     Premium   Very Good Very Good Ideal     Ideal    
## [21217] Ideal     Premium   Ideal     Premium   Ideal     Good     
## [21223] Premium   Premium   Premium   Ideal     Ideal     Premium  
## [21229] Ideal     Ideal     Very Good Premium   Ideal     Good     
## [21235] Premium   Ideal     Very Good Premium   Premium   Premium  
## [21241] Fair      Very Good Very Good Ideal     Ideal     Good     
## [21247] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [21253] Ideal     Good      Ideal     Ideal     Very Good Ideal    
## [21259] Ideal     Premium   Premium   Premium   Ideal     Ideal    
## [21265] Very Good Ideal     Very Good Premium   Ideal     Premium  
## [21271] Premium   Ideal     Ideal     Ideal     Very Good Premium  
## [21277] Ideal     Ideal     Ideal     Very Good Premium   Very Good
## [21283] Ideal     Premium   Premium   Ideal     Very Good Ideal    
## [21289] Ideal     Premium   Ideal     Ideal     Premium   Ideal    
## [21295] Premium   Premium   Very Good Very Good Very Good Good     
## [21301] Fair      Ideal     Ideal     Ideal     Ideal     Ideal    
## [21307] Ideal     Ideal     Very Good Ideal     Ideal     Ideal    
## [21313] Very Good Premium   Good      Ideal     Premium   Ideal    
## [21319] Good      Premium   Very Good Ideal     Premium   Ideal    
## [21325] Premium   Premium   Very Good Very Good Very Good Premium  
## [21331] Very Good Very Good Premium   Premium   Ideal     Premium  
## [21337] Premium   Ideal     Very Good Ideal     Ideal     Ideal    
## [21343] Very Good Premium   Very Good Ideal     Premium   Very Good
## [21349] Premium   Very Good Ideal     Premium   Very Good Very Good
## [21355] Premium   Ideal     Ideal     Ideal     Very Good Premium  
## [21361] Ideal     Good      Good      Very Good Ideal     Ideal    
## [21367] Ideal     Ideal     Ideal     Premium   Ideal     Ideal    
## [21373] Very Good Premium   Very Good Very Good Ideal     Ideal    
## [21379] Very Good Ideal     Very Good Ideal     Very Good Ideal    
## [21385] Premium   Very Good Very Good Ideal     Very Good Ideal    
## [21391] Premium   Ideal     Very Good Premium   Very Good Premium  
## [21397] Very Good Very Good Premium   Ideal     Premium   Very Good
## [21403] Ideal     Ideal     Premium   Very Good Ideal     Ideal    
## [21409] Ideal     Very Good Ideal     Premium   Ideal     Premium  
## [21415] Ideal     Premium   Good      Premium   Premium   Premium  
## [21421] Very Good Ideal     Ideal     Ideal     Ideal     Very Good
## [21427] Ideal     Ideal     Ideal     Premium   Very Good Good     
## [21433] Ideal     Very Good Ideal     Ideal     Premium   Premium  
## [21439] Premium   Good      Ideal     Ideal     Ideal     Ideal    
## [21445] Ideal     Very Good Ideal     Premium   Fair      Premium  
## [21451] Premium   Premium   Very Good Very Good Ideal     Ideal    
## [21457] Ideal     Fair      Premium   Good      Very Good Very Good
## [21463] Premium   Premium   Ideal     Ideal     Ideal     Ideal    
## [21469] Ideal     Ideal     Good      Ideal     Ideal     Premium  
## [21475] Ideal     Very Good Premium   Very Good Ideal     Premium  
## [21481] Very Good Premium   Ideal     Premium   Premium   Ideal    
## [21487] Ideal     Premium   Good      Very Good Ideal     Ideal    
## [21493] Very Good Ideal     Ideal     Very Good Ideal     Very Good
## [21499] Ideal     Ideal     Premium   Premium   Premium   Ideal    
## [21505] Ideal     Ideal     Ideal     Premium   Ideal     Ideal    
## [21511] Premium   Very Good Good      Fair      Very Good Ideal    
## [21517] Premium   Very Good Ideal     Ideal     Good      Ideal    
## [21523] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [21529] Premium   Very Good Premium   Ideal     Very Good Ideal    
## [21535] Ideal     Ideal     Very Good Premium   Ideal     Ideal    
## [21541] Very Good Very Good Premium   Very Good Ideal     Ideal    
## [21547] Premium   Premium   Ideal     Good      Premium   Very Good
## [21553] Premium   Premium   Premium   Premium   Ideal     Premium  
## [21559] Good      Ideal     Premium   Very Good Ideal     Very Good
## [21565] Very Good Premium   Very Good Very Good Ideal     Ideal    
## [21571] Good      Premium   Good      Fair      Very Good Very Good
## [21577] Ideal     Good      Premium   Fair      Premium   Premium  
## [21583] Very Good Very Good Very Good Ideal     Ideal     Premium  
## [21589] Very Good Very Good Ideal     Ideal     Ideal     Premium  
## [21595] Premium   Very Good Premium   Ideal     Premium   Premium  
## [21601] Ideal     Good      Good      Premium   Ideal     Ideal    
## [21607] Premium   Very Good Ideal     Premium   Fair      Very Good
## [21613] Premium   Very Good Very Good Very Good Premium   Ideal    
## [21619] Premium   Ideal     Premium   Premium   Ideal     Ideal    
## [21625] Ideal     Premium   Premium   Good      Premium   Premium  
## [21631] Ideal     Premium   Very Good Premium   Very Good Very Good
## [21637] Premium   Premium   Ideal     Good      Very Good Ideal    
## [21643] Very Good Ideal     Premium   Premium   Very Good Very Good
## [21649] Very Good Good      Ideal     Ideal     Very Good Premium  
## [21655] Ideal     Premium   Ideal     Premium   Ideal     Premium  
## [21661] Very Good Ideal     Premium   Premium   Ideal     Ideal    
## [21667] Premium   Premium   Good      Premium   Premium   Very Good
## [21673] Good      Good      Premium   Premium   Very Good Very Good
## [21679] Very Good Ideal     Very Good Ideal     Good      Premium  
## [21685] Good      Ideal     Ideal     Ideal     Ideal     Ideal    
## [21691] Premium   Good      Ideal     Premium   Premium   Very Good
## [21697] Ideal     Premium   Premium   Ideal     Very Good Very Good
## [21703] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [21709] Ideal     Ideal     Ideal     Fair      Fair      Very Good
## [21715] Very Good Very Good Ideal     Ideal     Ideal     Ideal    
## [21721] Very Good Premium   Very Good Good      Very Good Very Good
## [21727] Premium   Ideal     Ideal     Very Good Ideal     Ideal    
## [21733] Ideal     Ideal     Very Good Very Good Fair      Very Good
## [21739] Very Good Very Good Ideal     Very Good Premium   Ideal    
## [21745] Ideal     Very Good Very Good Very Good Ideal     Very Good
## [21751] Ideal     Very Good Ideal     Premium   Good      Very Good
## [21757] Premium   Very Good Fair      Ideal     Very Good Good     
## [21763] Ideal     Premium   Premium   Ideal     Ideal     Good     
## [21769] Premium   Premium   Premium   Ideal     Ideal     Very Good
## [21775] Very Good Ideal     Very Good Ideal     Premium   Premium  
## [21781] Premium   Premium   Premium   Premium   Very Good Ideal    
## [21787] Fair      Ideal     Ideal     Very Good Premium   Ideal    
## [21793] Ideal     Ideal     Good      Very Good Ideal     Ideal    
## [21799] Ideal     Premium   Ideal     Very Good Premium   Premium  
## [21805] Premium   Very Good Good      Ideal     Ideal     Good     
## [21811] Premium   Premium   Very Good Ideal     Ideal     Ideal    
## [21817] Ideal     Ideal     Premium   Ideal     Ideal     Good     
## [21823] Premium   Very Good Very Good Premium   Ideal     Good     
## [21829] Good      Very Good Very Good Very Good Premium   Premium  
## [21835] Ideal     Ideal     Ideal     Very Good Ideal     Ideal    
## [21841] Premium   Ideal     Premium   Very Good Very Good Premium  
## [21847] Ideal     Very Good Good      Premium   Premium   Ideal    
## [21853] Ideal     Premium   Premium   Ideal     Premium   Premium  
## [21859] Ideal     Ideal     Ideal     Very Good Premium   Ideal    
## [21865] Premium   Very Good Premium   Ideal     Ideal     Ideal    
## [21871] Premium   Ideal     Very Good Premium   Very Good Ideal    
## [21877] Very Good Good      Ideal     Premium   Premium   Ideal    
## [21883] Premium   Premium   Ideal     Good      Very Good Ideal    
## [21889] Premium   Very Good Premium   Premium   Very Good Premium  
## [21895] Ideal     Very Good Very Good Ideal     Premium   Very Good
## [21901] Premium   Very Good Very Good Ideal     Premium   Very Good
## [21907] Ideal     Very Good Ideal     Very Good Good      Premium  
## [21913] Ideal     Premium   Very Good Very Good Premium   Ideal    
## [21919] Ideal     Very Good Premium   Fair      Very Good Ideal    
## [21925] Very Good Ideal     Good      Good      Ideal     Ideal    
## [21931] Very Good Ideal     Very Good Very Good Premium   Fair     
## [21937] Ideal     Ideal     Premium   Premium   Very Good Premium  
## [21943] Ideal     Premium   Ideal     Premium   Very Good Very Good
## [21949] Very Good Ideal     Very Good Premium   Premium   Premium  
## [21955] Premium   Premium   Very Good Premium   Premium   Very Good
## [21961] Premium   Very Good Very Good Premium   Very Good Very Good
## [21967] Good      Ideal     Ideal     Premium   Premium   Premium  
## [21973] Premium   Ideal     Premium   Very Good Very Good Ideal    
## [21979] Very Good Premium   Ideal     Very Good Ideal     Ideal    
## [21985] Premium   Very Good Ideal     Premium   Premium   Premium  
## [21991] Premium   Ideal     Ideal     Premium   Ideal     Premium  
## [21997] Premium   Premium   Premium   Very Good Ideal     Premium  
## [22003] Ideal     Ideal     Fair      Very Good Premium   Ideal    
## [22009] Premium   Very Good Good      Good      Ideal     Very Good
## [22015] Ideal     Premium   Premium   Premium   Ideal     Premium  
## [22021] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [22027] Ideal     Ideal     Ideal     Good      Ideal     Very Good
## [22033] Ideal     Good      Premium   Ideal     Premium   Ideal    
## [22039] Ideal     Premium   Premium   Ideal     Very Good Premium  
## [22045] Premium   Premium   Good      Ideal     Good      Good     
## [22051] Premium   Fair      Ideal     Ideal     Premium   Premium  
## [22057] Good      Ideal     Good      Very Good Premium   Good     
## [22063] Very Good Very Good Ideal     Premium   Very Good Premium  
## [22069] Premium   Premium   Ideal     Ideal     Very Good Premium  
## [22075] Ideal     Ideal     Good      Premium   Very Good Ideal    
## [22081] Ideal     Ideal     Good      Ideal     Ideal     Very Good
## [22087] Very Good Ideal     Premium   Premium   Very Good Very Good
## [22093] Ideal     Very Good Ideal     Premium   Very Good Premium  
## [22099] Ideal     Ideal     Ideal     Good      Ideal     Very Good
## [22105] Very Good Ideal     Very Good Good      Premium   Ideal    
## [22111] Premium   Ideal     Ideal     Very Good Very Good Ideal    
## [22117] Very Good Ideal     Very Good Premium   Ideal     Premium  
## [22123] Very Good Premium   Ideal     Ideal     Premium   Very Good
## [22129] Premium   Very Good Premium   Good      Ideal     Very Good
## [22135] Very Good Premium   Premium   Ideal     Very Good Ideal    
## [22141] Premium   Premium   Good      Ideal     Premium   Fair     
## [22147] Fair      Good      Good      Very Good Very Good Ideal    
## [22153] Ideal     Premium   Ideal     Ideal     Ideal     Ideal    
## [22159] Very Good Premium   Very Good Premium   Very Good Very Good
## [22165] Fair      Premium   Ideal     Ideal     Ideal     Premium  
## [22171] Premium   Ideal     Very Good Ideal     Ideal     Very Good
## [22177] Very Good Good      Fair      Ideal     Premium   Premium  
## [22183] Premium   Premium   Ideal     Ideal     Ideal     Premium  
## [22189] Premium   Premium   Very Good Very Good Ideal     Ideal    
## [22195] Ideal     Premium   Ideal     Ideal     Very Good Ideal    
## [22201] Ideal     Ideal     Ideal     Premium   Ideal     Very Good
## [22207] Ideal     Good      Ideal     Premium   Very Good Ideal    
## [22213] Premium   Ideal     Premium   Good      Premium   Ideal    
## [22219] Very Good Good      Premium   Ideal     Premium   Very Good
## [22225] Ideal     Premium   Ideal     Very Good Ideal     Ideal    
## [22231] Very Good Very Good Ideal     Premium   Ideal     Good     
## [22237] Good      Premium   Premium   Premium   Ideal     Very Good
## [22243] Ideal     Ideal     Premium   Ideal     Good      Premium  
## [22249] Ideal     Ideal     Ideal     Premium   Premium   Ideal    
## [22255] Very Good Ideal     Ideal     Ideal     Ideal     Ideal    
## [22261] Very Good Ideal     Premium   Premium   Ideal     Ideal    
## [22267] Very Good Ideal     Ideal     Very Good Very Good Ideal    
## [22273] Very Good Premium   Premium   Premium   Very Good Premium  
## [22279] Premium   Ideal     Ideal     Premium   Ideal     Premium  
## [22285] Ideal     Very Good Premium   Ideal     Ideal     Premium  
## [22291] Ideal     Very Good Very Good Premium   Very Good Premium  
## [22297] Ideal     Ideal     Ideal     Ideal     Very Good Very Good
## [22303] Ideal     Premium   Very Good Good      Premium   Ideal    
## [22309] Premium   Very Good Ideal     Ideal     Fair      Ideal    
## [22315] Very Good Very Good Ideal     Ideal     Ideal     Premium  
## [22321] Ideal     Very Good Ideal     Premium   Ideal     Ideal    
## [22327] Ideal     Ideal     Very Good Fair      Premium   Very Good
## [22333] Ideal     Ideal     Very Good Ideal     Ideal     Premium  
## [22339] Premium   Premium   Ideal     Premium   Ideal     Ideal    
## [22345] Ideal     Premium   Ideal     Premium   Ideal     Premium  
## [22351] Very Good Ideal     Ideal     Premium   Ideal     Premium  
## [22357] Ideal     Ideal     Ideal     Ideal     Premium   Very Good
## [22363] Premium   Ideal     Very Good Ideal     Very Good Premium  
## [22369] Very Good Premium   Premium   Very Good Ideal     Premium  
## [22375] Ideal     Ideal     Very Good Very Good Very Good Fair     
## [22381] Premium   Ideal     Premium   Very Good Ideal     Premium  
## [22387] Premium   Ideal     Ideal     Premium   Premium   Good     
## [22393] Good      Premium   Very Good Ideal     Premium   Premium  
## [22399] Very Good Premium   Ideal     Good      Premium   Premium  
## [22405] Good      Good      Very Good Premium   Very Good Premium  
## [22411] Premium   Very Good Ideal     Very Good Premium   Ideal    
## [22417] Ideal     Ideal     Ideal     Ideal     Premium   Very Good
## [22423] Very Good Very Good Ideal     Fair      Very Good Ideal    
## [22429] Premium   Ideal     Ideal     Ideal     Premium   Ideal    
## [22435] Very Good Very Good Ideal     Premium   Very Good Very Good
## [22441] Premium   Premium   Ideal     Ideal     Premium   Very Good
## [22447] Very Good Very Good Very Good Premium   Good      Premium  
## [22453] Very Good Premium   Ideal     Ideal     Premium   Ideal    
## [22459] Fair      Premium   Fair      Premium   Premium   Ideal    
## [22465] Premium   Very Good Ideal     Premium   Very Good Very Good
## [22471] Premium   Very Good Very Good Very Good Very Good Premium  
## [22477] Ideal     Premium   Ideal     Ideal     Ideal     Premium  
## [22483] Premium   Ideal     Good      Very Good Premium   Very Good
## [22489] Premium   Ideal     Very Good Ideal     Very Good Good     
## [22495] Premium   Premium   Ideal     Premium   Ideal     Ideal    
## [22501] Ideal     Premium   Ideal     Premium   Ideal     Ideal    
## [22507] Very Good Ideal     Premium   Very Good Premium   Premium  
## [22513] Ideal     Very Good Premium   Ideal     Fair      Ideal    
## [22519] Ideal     Ideal     Very Good Premium   Ideal     Ideal    
## [22525] Fair      Premium   Good      Very Good Ideal     Premium  
## [22531] Ideal     Ideal     Premium   Premium   Very Good Very Good
## [22537] Ideal     Premium   Ideal     Ideal     Fair      Premium  
## [22543] Very Good Premium   Ideal     Premium   Ideal     Premium  
## [22549] Premium   Very Good Ideal     Ideal     Ideal     Premium  
## [22555] Very Good Ideal     Ideal     Ideal     Very Good Premium  
## [22561] Very Good Ideal     Ideal     Ideal     Good      Very Good
## [22567] Good      Premium   Premium   Ideal     Premium   Ideal    
## [22573] Fair      Premium   Premium   Premium   Very Good Good     
## [22579] Premium   Ideal     Very Good Ideal     Very Good Ideal    
## [22585] Ideal     Ideal     Good      Premium   Premium   Very Good
## [22591] Ideal     Ideal     Ideal     Fair      Very Good Ideal    
## [22597] Ideal     Very Good Very Good Very Good Premium   Premium  
## [22603] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [22609] Very Good Ideal     Ideal     Premium   Very Good Good     
## [22615] Good      Premium   Premium   Premium   Premium   Good     
## [22621] Fair      Very Good Very Good Ideal     Very Good Very Good
## [22627] Very Good Very Good Ideal     Very Good Very Good Very Good
## [22633] Ideal     Ideal     Ideal     Premium   Ideal     Ideal    
## [22639] Premium   Ideal     Premium   Premium   Ideal     Good     
## [22645] Ideal     Very Good Ideal     Premium   Ideal     Good     
## [22651] Good      Very Good Ideal     Very Good Very Good Premium  
## [22657] Premium   Ideal     Very Good Premium   Very Good Ideal    
## [22663] Ideal     Premium   Very Good Very Good Ideal     Fair     
## [22669] Premium   Premium   Premium   Very Good Ideal     Very Good
## [22675] Ideal     Ideal     Ideal     Ideal     Very Good Ideal    
## [22681] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [22687] Ideal     Good      Premium   Very Good Ideal     Very Good
## [22693] Very Good Very Good Very Good Ideal     Premium   Ideal    
## [22699] Premium   Good      Ideal     Fair      Ideal     Ideal    
## [22705] Ideal     Ideal     Premium   Premium   Very Good Ideal    
## [22711] Ideal     Premium   Very Good Very Good Very Good Premium  
## [22717] Ideal     Ideal     Ideal     Premium   Premium   Ideal    
## [22723] Ideal     Premium   Ideal     Ideal     Premium   Premium  
## [22729] Ideal     Premium   Premium   Fair      Premium   Ideal    
## [22735] Fair      Ideal     Ideal     Premium   Very Good Very Good
## [22741] Ideal     Fair      Ideal     Good      Premium   Ideal    
## [22747] Premium   Very Good Good      Premium   Premium   Good     
## [22753] Ideal     Very Good Fair      Fair      Very Good Premium  
## [22759] Premium   Ideal     Ideal     Very Good Ideal     Ideal    
## [22765] Ideal     Ideal     Ideal     Ideal     Very Good Good     
## [22771] Ideal     Good      Ideal     Ideal     Premium   Very Good
## [22777] Ideal     Very Good Very Good Ideal     Premium   Premium  
## [22783] Ideal     Ideal     Ideal     Ideal     Very Good Premium  
## [22789] Ideal     Premium   Premium   Premium   Good      Premium  
## [22795] Ideal     Ideal     Very Good Very Good Ideal     Very Good
## [22801] Premium   Premium   Ideal     Ideal     Premium   Premium  
## [22807] Premium   Ideal     Premium   Premium   Fair      Very Good
## [22813] Ideal     Very Good Ideal     Ideal     Premium   Ideal    
## [22819] Ideal     Ideal     Ideal     Premium   Very Good Ideal    
## [22825] Ideal     Ideal     Ideal     Ideal     Premium   Ideal    
## [22831] Ideal     Good      Premium   Premium   Good      Ideal    
## [22837] Premium   Ideal     Very Good Ideal     Premium   Premium  
## [22843] Premium   Premium   Premium   Good      Premium   Ideal    
## [22849] Good      Ideal     Ideal     Fair      Fair      Ideal    
## [22855] Premium   Premium   Very Good Very Good Very Good Very Good
## [22861] Ideal     Premium   Good      Premium   Very Good Ideal    
## [22867] Ideal     Premium   Premium   Very Good Premium   Fair     
## [22873] Premium   Premium   Premium   Very Good Ideal     Ideal    
## [22879] Ideal     Ideal     Very Good Ideal     Premium   Ideal    
## [22885] Premium   Very Good Premium   Premium   Ideal     Premium  
## [22891] Premium   Very Good Ideal     Ideal     Premium   Ideal    
## [22897] Very Good Ideal     Good      Ideal     Premium   Premium  
## [22903] Premium   Ideal     Ideal     Very Good Ideal     Ideal    
## [22909] Very Good Premium   Premium   Ideal     Very Good Fair     
## [22915] Premium   Ideal     Premium   Very Good Ideal     Ideal    
## [22921] Ideal     Very Good Premium   Premium   Very Good Very Good
## [22927] Premium   Ideal     Very Good Ideal     Very Good Ideal    
## [22933] Ideal     Very Good Ideal     Premium   Premium   Premium  
## [22939] Premium   Good      Ideal     Ideal     Premium   Ideal    
## [22945] Premium   Very Good Ideal     Premium   Ideal     Good     
## [22951] Ideal     Premium   Ideal     Premium   Ideal     Very Good
## [22957] Premium   Premium   Fair      Premium   Ideal     Ideal    
## [22963] Very Good Ideal     Ideal     Very Good Ideal     Very Good
## [22969] Very Good Ideal     Ideal     Premium   Premium   Premium  
## [22975] Premium   Good      Premium   Very Good Ideal     Ideal    
## [22981] Very Good Premium   Premium   Ideal     Ideal     Good     
## [22987] Premium   Very Good Ideal     Ideal     Ideal     Good     
## [22993] Premium   Ideal     Ideal     Premium   Premium   Premium  
## [22999] Ideal     Ideal     Ideal     Premium   Very Good Premium  
## [23005] Premium   Ideal     Premium   Premium   Ideal     Very Good
## [23011] Premium   Premium   Premium   Premium   Very Good Premium  
## [23017] Premium   Very Good Ideal     Premium   Fair      Ideal    
## [23023] Premium   Ideal     Very Good Very Good Very Good Ideal    
## [23029] Ideal     Ideal     Premium   Premium   Premium   Ideal    
## [23035] Premium   Ideal     Ideal     Premium   Ideal     Very Good
## [23041] Ideal     Very Good Ideal     Ideal     Good      Very Good
## [23047] Premium   Ideal     Ideal     Ideal     Very Good Ideal    
## [23053] Ideal     Premium   Good      Very Good Premium   Premium  
## [23059] Very Good Premium   Ideal     Ideal     Very Good Premium  
## [23065] Very Good Premium   Good      Fair      Premium   Ideal    
## [23071] Premium   Ideal     Premium   Premium   Premium   Ideal    
## [23077] Premium   Premium   Premium   Very Good Premium   Ideal    
## [23083] Ideal     Very Good Ideal     Ideal     Ideal     Premium  
## [23089] Premium   Premium   Ideal     Premium   Ideal     Ideal    
## [23095] Good      Ideal     Premium   Very Good Premium   Ideal    
## [23101] Ideal     Ideal     Premium   Fair      Ideal     Premium  
## [23107] Very Good Premium   Ideal     Very Good Ideal     Ideal    
## [23113] Very Good Ideal     Premium   Ideal     Ideal     Very Good
## [23119] Premium   Ideal     Ideal     Premium   Ideal     Ideal    
## [23125] Premium   Ideal     Ideal     Premium   Very Good Fair     
## [23131] Premium   Ideal     Ideal     Premium   Ideal     Fair     
## [23137] Fair      Ideal     Ideal     Premium   Very Good Very Good
## [23143] Premium   Premium   Premium   Ideal     Premium   Premium  
## [23149] Very Good Ideal     Premium   Ideal     Ideal     Premium  
## [23155] Very Good Ideal     Very Good Premium   Ideal     Very Good
## [23161] Premium   Premium   Premium   Very Good Good      Ideal    
## [23167] Ideal     Good      Good      Ideal     Ideal     Premium  
## [23173] Premium   Premium   Premium   Fair      Premium   Ideal    
## [23179] Very Good Premium   Premium   Very Good Very Good Premium  
## [23185] Ideal     Very Good Premium   Ideal     Ideal     Ideal    
## [23191] Ideal     Premium   Ideal     Premium   Premium   Fair     
## [23197] Ideal     Premium   Ideal     Ideal     Ideal     Premium  
## [23203] Premium   Ideal     Good      Fair      Very Good Very Good
## [23209] Very Good Ideal     Ideal     Very Good Premium   Ideal    
## [23215] Ideal     Good      Ideal     Ideal     Premium   Premium  
## [23221] Ideal     Premium   Ideal     Premium   Ideal     Premium  
## [23227] Premium   Ideal     Ideal     Premium   Very Good Ideal    
## [23233] Good      Ideal     Ideal     Premium   Ideal     Premium  
## [23239] Ideal     Premium   Premium   Good      Ideal     Premium  
## [23245] Premium   Ideal     Ideal     Premium   Ideal     Premium  
## [23251] Premium   Ideal     Very Good Premium   Very Good Premium  
## [23257] Very Good Premium   Fair      Very Good Very Good Premium  
## [23263] Premium   Very Good Premium   Premium   Ideal     Very Good
## [23269] Premium   Premium   Very Good Very Good Ideal     Good     
## [23275] Ideal     Ideal     Premium   Premium   Very Good Ideal    
## [23281] Ideal     Very Good Good      Premium   Ideal     Premium  
## [23287] Good      Premium   Good      Ideal     Ideal     Premium  
## [23293] Fair      Premium   Ideal     Very Good Premium   Very Good
## [23299] Very Good Premium   Ideal     Ideal     Premium   Good     
## [23305] Ideal     Very Good Ideal     Premium   Premium   Very Good
## [23311] Premium   Very Good Very Good Fair      Premium   Premium  
## [23317] Premium   Premium   Ideal     Premium   Good      Ideal    
## [23323] Ideal     Good      Premium   Premium   Premium   Ideal    
## [23329] Premium   Premium   Ideal     Ideal     Ideal     Very Good
## [23335] Ideal     Premium   Premium   Very Good Ideal     Premium  
## [23341] Very Good Very Good Very Good Ideal     Ideal     Ideal    
## [23347] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [23353] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [23359] Ideal     Ideal     Very Good Very Good Ideal     Ideal    
## [23365] Ideal     Ideal     Ideal     Ideal     Good      Good     
## [23371] Ideal     Ideal     Premium   Premium   Ideal     Very Good
## [23377] Ideal     Premium   Premium   Very Good Premium   Good     
## [23383] Ideal     Very Good Ideal     Premium   Good      Good     
## [23389] Very Good Very Good Ideal     Very Good Ideal     Very Good
## [23395] Very Good Premium   Premium   Very Good Ideal     Ideal    
## [23401] Ideal     Premium   Very Good Very Good Very Good Premium  
## [23407] Premium   Good      Good      Ideal     Premium   Ideal    
## [23413] Ideal     Very Good Ideal     Ideal     Very Good Ideal    
## [23419] Very Good Ideal     Premium   Premium   Premium   Ideal    
## [23425] Very Good Very Good Ideal     Ideal     Ideal     Ideal    
## [23431] Very Good Premium   Ideal     Very Good Ideal     Premium  
## [23437] Very Good Premium   Ideal     Very Good Ideal     Ideal    
## [23443] Premium   Premium   Premium   Ideal     Ideal     Premium  
## [23449] Ideal     Premium   Ideal     Very Good Very Good Ideal    
## [23455] Ideal     Very Good Very Good Very Good Ideal     Ideal    
## [23461] Premium   Premium   Very Good Ideal     Premium   Very Good
## [23467] Good      Premium   Premium   Ideal     Ideal     Premium  
## [23473] Ideal     Ideal     Premium   Very Good Very Good Ideal    
## [23479] Very Good Premium   Premium   Premium   Premium   Premium  
## [23485] Good      Ideal     Premium   Premium   Ideal     Fair     
## [23491] Premium   Premium   Ideal     Ideal     Ideal     Very Good
## [23497] Ideal     Good      Good      Premium   Very Good Ideal    
## [23503] Ideal     Ideal     Very Good Good      Very Good Premium  
## [23509] Very Good Premium   Premium   Good      Premium   Good     
## [23515] Ideal     Ideal     Premium   Good      Ideal     Ideal    
## [23521] Good      Ideal     Very Good Ideal     Ideal     Ideal    
## [23527] Ideal     Ideal     Very Good Very Good Premium   Premium  
## [23533] Very Good Premium   Ideal     Premium   Premium   Ideal    
## [23539] Premium   Good      Premium   Ideal     Good      Very Good
## [23545] Good      Ideal     Very Good Ideal     Very Good Very Good
## [23551] Very Good Good      Very Good Premium   Very Good Premium  
## [23557] Premium   Premium   Very Good Premium   Ideal     Ideal    
## [23563] Ideal     Premium   Ideal     Premium   Ideal     Good     
## [23569] Good      Premium   Premium   Ideal     Ideal     Very Good
## [23575] Ideal     Very Good Ideal     Premium   Premium   Very Good
## [23581] Ideal     Very Good Ideal     Fair      Ideal     Very Good
## [23587] Very Good Ideal     Very Good Ideal     Ideal     Premium  
## [23593] Ideal     Ideal     Ideal     Very Good Premium   Ideal    
## [23599] Premium   Very Good Very Good Ideal     Ideal     Very Good
## [23605] Premium   Ideal     Premium   Ideal     Premium   Ideal    
## [23611] Premium   Very Good Premium   Very Good Good      Ideal    
## [23617] Ideal     Ideal     Ideal     Premium   Premium   Premium  
## [23623] Premium   Ideal     Very Good Very Good Premium   Ideal    
## [23629] Ideal     Very Good Premium   Ideal     Premium   Premium  
## [23635] Fair      Very Good Ideal     Premium   Premium   Premium  
## [23641] Premium   Premium   Premium   Very Good Fair      Very Good
## [23647] Ideal     Very Good Ideal     Ideal     Very Good Very Good
## [23653] Very Good Ideal     Good      Ideal     Good      Very Good
## [23659] Ideal     Very Good Premium   Premium   Ideal     Premium  
## [23665] Premium   Premium   Ideal     Very Good Ideal     Very Good
## [23671] Good      Very Good Premium   Premium   Premium   Very Good
## [23677] Ideal     Premium   Ideal     Ideal     Premium   Very Good
## [23683] Very Good Premium   Fair      Premium   Good      Good     
## [23689] Premium   Very Good Good      Very Good Good      Very Good
## [23695] Very Good Premium   Very Good Very Good Ideal     Ideal    
## [23701] Ideal     Premium   Premium   Very Good Good      Ideal    
## [23707] Very Good Good      Very Good Ideal     Ideal     Ideal    
## [23713] Good      Very Good Very Good Very Good Very Good Very Good
## [23719] Very Good Very Good Very Good Very Good Very Good Very Good
## [23725] Very Good Very Good Very Good Very Good Ideal     Ideal    
## [23731] Premium   Very Good Premium   Very Good Very Good Very Good
## [23737] Ideal     Premium   Very Good Premium   Premium   Very Good
## [23743] Very Good Premium   Ideal     Very Good Premium   Premium  
## [23749] Premium   Premium   Good      Very Good Premium   Premium  
## [23755] Ideal     Very Good Good      Premium   Very Good Ideal    
## [23761] Ideal     Premium   Premium   Good      Premium   Good     
## [23767] Ideal     Premium   Very Good Premium   Very Good Good     
## [23773] Ideal     Premium   Very Good Ideal     Very Good Ideal    
## [23779] Premium   Ideal     Ideal     Very Good Ideal     Very Good
## [23785] Very Good Ideal     Premium   Ideal     Ideal     Premium  
## [23791] Very Good Premium   Good      Ideal     Premium   Premium  
## [23797] Good      Premium   Ideal     Ideal     Premium   Premium  
## [23803] Premium   Very Good Premium   Very Good Premium   Premium  
## [23809] Ideal     Good      Ideal     Premium   Premium   Very Good
## [23815] Very Good Premium   Very Good Ideal     Ideal     Premium  
## [23821] Ideal     Ideal     Premium   Ideal     Ideal     Premium  
## [23827] Premium   Premium   Premium   Premium   Very Good Premium  
## [23833] Ideal     Very Good Very Good Ideal     Ideal     Good     
## [23839] Ideal     Very Good Very Good Fair      Ideal     Very Good
## [23845] Premium   Ideal     Ideal     Very Good Very Good Fair     
## [23851] Very Good Ideal     Ideal     Very Good Ideal     Premium  
## [23857] Fair      Premium   Good      Ideal     Ideal     Good     
## [23863] Ideal     Ideal     Premium   Very Good Premium   Premium  
## [23869] Premium   Ideal     Premium   Fair      Premium   Ideal    
## [23875] Very Good Very Good Premium   Premium   Very Good Ideal    
## [23881] Ideal     Ideal     Premium   Ideal     Premium   Premium  
## [23887] Premium   Premium   Very Good Ideal     Ideal     Good     
## [23893] Ideal     Very Good Very Good Ideal     Very Good Ideal    
## [23899] Very Good Ideal     Ideal     Very Good Premium   Very Good
## [23905] Ideal     Premium   Very Good Very Good Premium   Very Good
## [23911] Ideal     Premium   Ideal     Ideal     Good      Ideal    
## [23917] Ideal     Very Good Premium   Premium   Ideal     Premium  
## [23923] Premium   Premium   Ideal     Premium   Ideal     Ideal    
## [23929] Premium   Ideal     Very Good Ideal     Ideal     Premium  
## [23935] Premium   Ideal     Premium   Premium   Ideal     Premium  
## [23941] Ideal     Ideal     Premium   Premium   Premium   Ideal    
## [23947] Premium   Ideal     Very Good Very Good Premium   Premium  
## [23953] Ideal     Ideal     Ideal     Premium   Ideal     Very Good
## [23959] Ideal     Very Good Very Good Very Good Premium   Ideal    
## [23965] Ideal     Very Good Very Good Premium   Ideal     Very Good
## [23971] Premium   Very Good Premium   Ideal     Ideal     Premium  
## [23977] Good      Ideal     Good      Good      Ideal     Ideal    
## [23983] Premium   Very Good Ideal     Ideal     Premium   Ideal    
## [23989] Ideal     Premium   Very Good Premium   Ideal     Ideal    
## [23995] Ideal     Ideal     Very Good Premium   Ideal     Ideal    
## [24001] Ideal     Ideal     Premium   Premium   Ideal     Ideal    
## [24007] Ideal     Very Good Very Good Ideal     Premium   Ideal    
## [24013] Premium   Ideal     Ideal     Ideal     Ideal     Ideal    
## [24019] Ideal     Ideal     Very Good Very Good Very Good Very Good
## [24025] Good      Premium   Premium   Ideal     Very Good Premium  
## [24031] Ideal     Ideal     Ideal     Ideal     Good      Good     
## [24037] Premium   Very Good Premium   Premium   Very Good Premium  
## [24043] Very Good Good      Very Good Very Good Ideal     Premium  
## [24049] Very Good Very Good Very Good Very Good Ideal     Ideal    
## [24055] Ideal     Ideal     Ideal     Premium   Premium   Ideal    
## [24061] Ideal     Very Good Ideal     Ideal     Ideal     Ideal    
## [24067] Premium   Premium   Ideal     Premium   Premium   Ideal    
## [24073] Very Good Good      Ideal     Very Good Very Good Good     
## [24079] Premium   Very Good Ideal     Premium   Ideal     Premium  
## [24085] Very Good Premium   Very Good Ideal     Premium   Premium  
## [24091] Premium   Ideal     Premium   Ideal     Premium   Fair     
## [24097] Ideal     Very Good Good      Ideal     Very Good Ideal    
## [24103] Very Good Very Good Good      Premium   Premium   Premium  
## [24109] Ideal     Ideal     Ideal     Very Good Premium   Premium  
## [24115] Premium   Premium   Very Good Very Good Ideal     Very Good
## [24121] Good      Ideal     Ideal     Very Good Good      Very Good
## [24127] Very Good Very Good Very Good Ideal     Very Good Premium  
## [24133] Very Good Premium   Premium   Premium   Premium   Premium  
## [24139] Premium   Premium   Premium   Premium   Good      Very Good
## [24145] Very Good Ideal     Ideal     Ideal     Very Good Premium  
## [24151] Good      Ideal     Premium   Premium   Ideal     Ideal    
## [24157] Ideal     Premium   Premium   Ideal     Premium   Premium  
## [24163] Premium   Premium   Very Good Premium   Ideal     Very Good
## [24169] Very Good Ideal     Premium   Premium   Premium   Ideal    
## [24175] Ideal     Ideal     Very Good Very Good Very Good Ideal    
## [24181] Ideal     Premium   Premium   Premium   Good      Very Good
## [24187] Ideal     Ideal     Very Good Premium   Premium   Very Good
## [24193] Premium   Very Good Ideal     Premium   Premium   Very Good
## [24199] Very Good Very Good Premium   Ideal     Ideal     Very Good
## [24205] Very Good Very Good Ideal     Very Good Premium   Very Good
## [24211] Ideal     Premium   Ideal     Very Good Premium   Premium  
## [24217] Ideal     Premium   Premium   Premium   Ideal     Premium  
## [24223] Ideal     Premium   Ideal     Premium   Good      Premium  
## [24229] Very Good Good      Very Good Premium   Ideal     Ideal    
## [24235] Ideal     Ideal     Premium   Premium   Premium   Ideal    
## [24241] Ideal     Fair      Very Good Premium   Fair      Premium  
## [24247] Ideal     Ideal     Premium   Very Good Ideal     Ideal    
## [24253] Ideal     Ideal     Ideal     Good      Ideal     Ideal    
## [24259] Ideal     Very Good Ideal     Very Good Ideal     Good     
## [24265] Good      Premium   Premium   Very Good Ideal     Premium  
## [24271] Ideal     Premium   Premium   Premium   Premium   Fair     
## [24277] Premium   Ideal     Fair      Premium   Premium   Very Good
## [24283] Premium   Premium   Very Good Premium   Premium   Premium  
## [24289] Ideal     Premium   Ideal     Ideal     Premium   Ideal    
## [24295] Premium   Ideal     Ideal     Ideal     Good      Very Good
## [24301] Good      Premium   Premium   Very Good Ideal     Very Good
## [24307] Good      Premium   Fair      Ideal     Premium   Good     
## [24313] Very Good Ideal     Very Good Ideal     Ideal     Ideal    
## [24319] Premium   Ideal     Ideal     Premium   Ideal     Good     
## [24325] Very Good Fair      Ideal     Premium   Ideal     Ideal    
## [24331] Ideal     Fair      Premium   Very Good Very Good Premium  
## [24337] Very Good Ideal     Fair      Ideal     Ideal     Premium  
## [24343] Ideal     Very Good Fair      Very Good Ideal     Very Good
## [24349] Good      Ideal     Ideal     Fair      Very Good Ideal    
## [24355] Premium   Ideal     Ideal     Very Good Very Good Ideal    
## [24361] Ideal     Ideal     Ideal     Ideal     Very Good Premium  
## [24367] Very Good Ideal     Premium   Ideal     Ideal     Ideal    
## [24373] Ideal     Ideal     Ideal     Ideal     Good      Ideal    
## [24379] Ideal     Good      Fair      Premium   Ideal     Very Good
## [24385] Good      Good      Good      Very Good Very Good Very Good
## [24391] Ideal     Very Good Ideal     Ideal     Premium   Premium  
## [24397] Fair      Very Good Very Good Ideal     Ideal     Very Good
## [24403] Premium   Premium   Premium   Ideal     Very Good Very Good
## [24409] Ideal     Fair      Very Good Ideal     Ideal     Good     
## [24415] Very Good Premium   Premium   Ideal     Premium   Premium  
## [24421] Ideal     Very Good Premium   Ideal     Premium   Ideal    
## [24427] Ideal     Premium   Good      Premium   Ideal     Premium  
## [24433] Very Good Very Good Ideal     Ideal     Premium   Premium  
## [24439] Good      Ideal     Premium   Premium   Very Good Ideal    
## [24445] Ideal     Ideal     Premium   Ideal     Good      Ideal    
## [24451] Premium   Ideal     Very Good Ideal     Premium   Ideal    
## [24457] Ideal     Ideal     Ideal     Premium   Good      Ideal    
## [24463] Ideal     Very Good Ideal     Ideal     Very Good Ideal    
## [24469] Premium   Ideal     Premium   Premium   Premium   Premium  
## [24475] Ideal     Ideal     Ideal     Ideal     Premium   Ideal    
## [24481] Premium   Good      Good      Ideal     Premium   Good     
## [24487] Premium   Very Good Ideal     Ideal     Fair      Very Good
## [24493] Premium   Very Good Premium   Premium   Very Good Very Good
## [24499] Ideal     Ideal     Very Good Very Good Good      Ideal    
## [24505] Premium   Good      Premium   Premium   Fair      Ideal    
## [24511] Very Good Ideal     Premium   Premium   Ideal     Very Good
## [24517] Good      Very Good Fair      Premium   Ideal     Ideal    
## [24523] Ideal     Fair      Premium   Ideal     Ideal     Very Good
## [24529] Premium   Ideal     Premium   Very Good Premium   Premium  
## [24535] Premium   Ideal     Good      Ideal     Fair      Ideal    
## [24541] Very Good Premium   Premium   Ideal     Premium   Ideal    
## [24547] Premium   Ideal     Very Good Very Good Very Good Ideal    
## [24553] Ideal     Premium   Ideal     Very Good Premium   Very Good
## [24559] Very Good Ideal     Ideal     Ideal     Premium   Very Good
## [24565] Premium   Ideal     Very Good Premium   Ideal     Ideal    
## [24571] Very Good Good      Ideal     Premium   Premium   Very Good
## [24577] Very Good Very Good Very Good Premium   Ideal     Premium  
## [24583] Ideal     Very Good Premium   Very Good Premium   Ideal    
## [24589] Premium   Premium   Premium   Very Good Very Good Ideal    
## [24595] Ideal     Ideal     Very Good Ideal     Fair      Ideal    
## [24601] Ideal     Very Good Very Good Good      Premium   Fair     
## [24607] Good      Good      Premium   Premium   Ideal     Very Good
## [24613] Ideal     Premium   Premium   Ideal     Good      Ideal    
## [24619] Ideal     Fair      Premium   Ideal     Ideal     Ideal    
## [24625] Ideal     Premium   Ideal     Good      Premium   Good     
## [24631] Good      Very Good Very Good Premium   Premium   Premium  
## [24637] Ideal     Ideal     Ideal     Premium   Premium   Very Good
## [24643] Very Good Ideal     Premium   Good      Premium   Premium  
## [24649] Very Good Ideal     Ideal     Ideal     Premium   Premium  
## [24655] Ideal     Ideal     Very Good Premium   Very Good Very Good
## [24661] Premium   Ideal     Ideal     Fair      Very Good Premium  
## [24667] Premium   Good      Very Good Premium   Very Good Fair     
## [24673] Ideal     Premium   Ideal     Ideal     Ideal     Premium  
## [24679] Premium   Very Good Very Good Premium   Ideal     Very Good
## [24685] Premium   Very Good Good      Ideal     Premium   Ideal    
## [24691] Good      Good      Very Good Good      Very Good Ideal    
## [24697] Very Good Very Good Very Good Very Good Good      Ideal    
## [24703] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [24709] Ideal     Premium   Premium   Very Good Very Good Very Good
## [24715] Very Good Very Good Very Good Very Good Very Good Ideal    
## [24721] Ideal     Ideal     Ideal     Very Good Very Good Ideal    
## [24727] Premium   Very Good Premium   Good      Premium   Premium  
## [24733] Premium   Premium   Premium   Premium   Premium   Very Good
## [24739] Premium   Premium   Premium   Ideal     Premium   Premium  
## [24745] Very Good Ideal     Premium   Ideal     Premium   Very Good
## [24751] Very Good Premium   Premium   Good      Ideal     Very Good
## [24757] Ideal     Very Good Premium   Very Good Ideal     Premium  
## [24763] Premium   Premium   Premium   Premium   Premium   Premium  
## [24769] Ideal     Premium   Premium   Premium   Very Good Premium  
## [24775] Ideal     Ideal     Ideal     Ideal     Good      Premium  
## [24781] Very Good Ideal     Ideal     Good      Ideal     Ideal    
## [24787] Premium   Good      Very Good Ideal     Premium   Ideal    
## [24793] Ideal     Ideal     Very Good Ideal     Premium   Premium  
## [24799] Ideal     Premium   Premium   Ideal     Ideal     Good     
## [24805] Very Good Ideal     Premium   Premium   Very Good Ideal    
## [24811] Premium   Premium   Very Good Ideal     Ideal     Very Good
## [24817] Premium   Premium   Very Good Premium   Premium   Very Good
## [24823] Premium   Ideal     Very Good Premium   Premium   Premium  
## [24829] Very Good Premium   Very Good Premium   Premium   Ideal    
## [24835] Premium   Ideal     Good      Premium   Ideal     Very Good
## [24841] Good      Ideal     Good      Very Good Premium   Premium  
## [24847] Ideal     Ideal     Premium   Very Good Ideal     Ideal    
## [24853] Ideal     Premium   Ideal     Ideal     Ideal     Premium  
## [24859] Premium   Very Good Good      Very Good Fair      Fair     
## [24865] Ideal     Premium   Very Good Good      Very Good Premium  
## [24871] Good      Premium   Ideal     Ideal     Premium   Premium  
## [24877] Premium   Ideal     Premium   Premium   Premium   Premium  
## [24883] Good      Very Good Premium   Good      Premium   Premium  
## [24889] Premium   Ideal     Very Good Good      Ideal     Premium  
## [24895] Very Good Ideal     Ideal     Premium   Premium   Premium  
## [24901] Premium   Ideal     Premium   Ideal     Very Good Good     
## [24907] Very Good Premium   Ideal     Very Good Ideal     Ideal    
## [24913] Premium   Ideal     Ideal     Premium   Ideal     Premium  
## [24919] Premium   Premium   Ideal     Very Good Premium   Fair     
## [24925] Premium   Very Good Ideal     Ideal     Premium   Ideal    
## [24931] Very Good Very Good Fair      Premium   Premium   Ideal    
## [24937] Premium   Ideal     Premium   Ideal     Ideal     Ideal    
## [24943] Fair      Premium   Premium   Ideal     Premium   Ideal    
## [24949] Premium   Premium   Premium   Ideal     Premium   Premium  
## [24955] Ideal     Ideal     Ideal     Premium   Ideal     Fair     
## [24961] Premium   Very Good Ideal     Premium   Ideal     Premium  
## [24967] Premium   Ideal     Ideal     Good      Very Good Ideal    
## [24973] Ideal     Premium   Premium   Ideal     Good      Premium  
## [24979] Ideal     Ideal     Premium   Premium   Very Good Very Good
## [24985] Ideal     Very Good Good      Good      Very Good Very Good
## [24991] Very Good Fair      Very Good Premium   Premium   Ideal    
## [24997] Ideal     Premium   Good      Very Good Ideal     Premium  
## [25003] Ideal     Ideal     Ideal     Very Good Ideal     Premium  
## [25009] Very Good Good      Good      Premium   Ideal     Good     
## [25015] Premium   Fair      Premium   Ideal     Very Good Very Good
## [25021] Ideal     Ideal     Ideal     Ideal     Fair      Premium  
## [25027] Premium   Premium   Very Good Very Good Very Good Very Good
## [25033] Ideal     Ideal     Ideal     Good      Good      Premium  
## [25039] Premium   Premium   Premium   Ideal     Good      Premium  
## [25045] Ideal     Very Good Ideal     Very Good Very Good Premium  
## [25051] Very Good Premium   Ideal     Premium   Premium   Premium  
## [25057] Very Good Premium   Ideal     Premium   Premium   Very Good
## [25063] Premium   Very Good Fair      Good      Fair      Ideal    
## [25069] Ideal     Ideal     Ideal     Ideal     Very Good Good     
## [25075] Very Good Ideal     Very Good Premium   Premium   Premium  
## [25081] Premium   Good      Ideal     Premium   Very Good Premium  
## [25087] Very Good Premium   Premium   Ideal     Premium   Good     
## [25093] Very Good Very Good Good      Fair      Very Good Ideal    
## [25099] Very Good Premium   Very Good Ideal     Very Good Very Good
## [25105] Ideal     Premium   Premium   Premium   Ideal     Ideal    
## [25111] Good      Ideal     Ideal     Good      Very Good Very Good
## [25117] Ideal     Ideal     Ideal     Premium   Premium   Very Good
## [25123] Ideal     Premium   Ideal     Premium   Premium   Premium  
## [25129] Premium   Ideal     Good      Very Good Premium   Very Good
## [25135] Very Good Very Good Premium   Fair      Premium   Premium  
## [25141] Ideal     Very Good Premium   Premium   Ideal     Ideal    
## [25147] Premium   Premium   Premium   Good      Premium   Fair     
## [25153] Ideal     Very Good Ideal     Very Good Premium   Premium  
## [25159] Very Good Premium   Premium   Very Good Premium   Premium  
## [25165] Premium   Premium   Fair      Premium   Ideal     Premium  
## [25171] Premium   Ideal     Very Good Premium   Very Good Premium  
## [25177] Ideal     Ideal     Fair      Fair      Premium   Premium  
## [25183] Premium   Ideal     Ideal     Ideal     Premium   Ideal    
## [25189] Ideal     Premium   Very Good Very Good Good      Premium  
## [25195] Premium   Premium   Ideal     Premium   Very Good Very Good
## [25201] Ideal     Ideal     Very Good Very Good Premium   Ideal    
## [25207] Ideal     Ideal     Very Good Premium   Ideal     Very Good
## [25213] Premium   Very Good Premium   Premium   Ideal     Very Good
## [25219] Good      Ideal     Premium   Ideal     Ideal     Premium  
## [25225] Very Good Fair      Premium   Premium   Ideal     Premium  
## [25231] Premium   Premium   Ideal     Ideal     Premium   Good     
## [25237] Ideal     Fair      Very Good Good      Premium   Ideal    
## [25243] Good      Fair      Premium   Very Good Premium   Very Good
## [25249] Premium   Very Good Premium   Ideal     Premium   Ideal    
## [25255] Ideal     Ideal     Ideal     Premium   Premium   Ideal    
## [25261] Very Good Very Good Good      Ideal     Good      Very Good
## [25267] Premium   Ideal     Premium   Ideal     Ideal     Premium  
## [25273] Ideal     Good      Premium   Premium   Premium   Ideal    
## [25279] Very Good Ideal     Good      Ideal     Premium   Premium  
## [25285] Ideal     Premium   Premium   Very Good Premium   Very Good
## [25291] Premium   Premium   Premium   Very Good Ideal     Ideal    
## [25297] Premium   Very Good Good      Very Good Very Good Ideal    
## [25303] Ideal     Very Good Good      Good      Premium   Premium  
## [25309] Ideal     Premium   Premium   Premium   Premium   Good     
## [25315] Ideal     Ideal     Ideal     Premium   Very Good Premium  
## [25321] Good      Ideal     Ideal     Ideal     Premium   Good     
## [25327] Ideal     Good      Very Good Ideal     Premium   Ideal    
## [25333] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [25339] Premium   Premium   Good      Ideal     Premium   Very Good
## [25345] Premium   Ideal     Very Good Ideal     Very Good Premium  
## [25351] Good      Very Good Very Good Very Good Very Good Very Good
## [25357] Very Good Very Good Very Good Very Good Very Good Very Good
## [25363] Very Good Very Good Very Good Very Good Very Good Very Good
## [25369] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [25375] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [25381] Ideal     Premium   Premium   Very Good Premium   Premium  
## [25387] Ideal     Very Good Fair      Ideal     Good      Ideal    
## [25393] Premium   Very Good Ideal     Ideal     Premium   Premium  
## [25399] Premium   Ideal     Premium   Premium   Ideal     Very Good
## [25405] Very Good Ideal     Very Good Premium   Premium   Premium  
## [25411] Ideal     Very Good Ideal     Premium   Good      Ideal    
## [25417] Good      Very Good Ideal     Premium   Premium   Very Good
## [25423] Very Good Premium   Very Good Very Good Ideal     Premium  
## [25429] Ideal     Very Good Premium   Premium   Ideal     Premium  
## [25435] Ideal     Premium   Premium   Premium   Very Good Premium  
## [25441] Fair      Premium   Ideal     Ideal     Premium   Premium  
## [25447] Fair      Premium   Premium   Premium   Very Good Fair     
## [25453] Premium   Good      Premium   Premium   Ideal     Very Good
## [25459] Good      Ideal     Premium   Premium   Premium   Very Good
## [25465] Ideal     Premium   Premium   Very Good Premium   Ideal    
## [25471] Premium   Very Good Premium   Premium   Ideal     Very Good
## [25477] Ideal     Ideal     Premium   Good      Good      Very Good
## [25483] Very Good Premium   Ideal     Good      Ideal     Premium  
## [25489] Very Good Ideal     Ideal     Ideal     Premium   Ideal    
## [25495] Very Good Ideal     Good      Ideal     Premium   Premium  
## [25501] Ideal     Premium   Premium   Premium   Very Good Very Good
## [25507] Premium   Ideal     Ideal     Premium   Premium   Ideal    
## [25513] Very Good Very Good Ideal     Premium   Premium   Ideal    
## [25519] Ideal     Premium   Premium   Premium   Premium   Very Good
## [25525] Very Good Very Good Ideal     Premium   Premium   Ideal    
## [25531] Premium   Good      Premium   Premium   Premium   Very Good
## [25537] Ideal     Premium   Very Good Premium   Premium   Ideal    
## [25543] Very Good Premium   Premium   Ideal     Premium   Premium  
## [25549] Ideal     Ideal     Very Good Very Good Ideal     Very Good
## [25555] Ideal     Premium   Premium   Premium   Ideal     Premium  
## [25561] Premium   Ideal     Good      Premium   Very Good Fair     
## [25567] Very Good Very Good Premium   Ideal     Ideal     Premium  
## [25573] Premium   Very Good Ideal     Premium   Ideal     Very Good
## [25579] Ideal     Ideal     Premium   Premium   Good      Very Good
## [25585] Good      Ideal     Premium   Ideal     Ideal     Ideal    
## [25591] Fair      Ideal     Very Good Premium   Premium   Good     
## [25597] Premium   Premium   Good      Ideal     Premium   Premium  
## [25603] Ideal     Premium   Premium   Ideal     Premium   Very Good
## [25609] Good      Very Good Ideal     Ideal     Premium   Very Good
## [25615] Ideal     Ideal     Ideal     Premium   Premium   Premium  
## [25621] Very Good Premium   Ideal     Ideal     Very Good Ideal    
## [25627] Ideal     Ideal     Premium   Premium   Premium   Ideal    
## [25633] Ideal     Premium   Ideal     Premium   Ideal     Ideal    
## [25639] Very Good Very Good Very Good Ideal     Ideal     Ideal    
## [25645] Ideal     Ideal     Premium   Premium   Premium   Premium  
## [25651] Premium   Very Good Ideal     Very Good Premium   Ideal    
## [25657] Premium   Very Good Good      Premium   Premium   Premium  
## [25663] Premium   Ideal     Ideal     Ideal     Premium   Ideal    
## [25669] Premium   Premium   Very Good Ideal     Ideal     Ideal    
## [25675] Good      Very Good Very Good Ideal     Premium   Premium  
## [25681] Ideal     Ideal     Very Good Ideal     Very Good Ideal    
## [25687] Good      Premium   Ideal     Very Good Very Good Very Good
## [25693] Very Good Ideal     Ideal     Fair      Ideal     Very Good
## [25699] Premium   Premium   Good      Ideal     Very Good Ideal    
## [25705] Ideal     Premium   Very Good Ideal     Ideal     Ideal    
## [25711] Premium   Ideal     Ideal     Ideal     Very Good Ideal    
## [25717] Ideal     Very Good Ideal     Premium   Very Good Ideal    
## [25723] Ideal     Ideal     Very Good Fair      Very Good Premium  
## [25729] Ideal     Fair      Good      Good      Premium   Premium  
## [25735] Premium   Premium   Premium   Premium   Premium   Ideal    
## [25741] Very Good Ideal     Ideal     Ideal     Premium   Premium  
## [25747] Very Good Premium   Fair      Ideal     Ideal     Ideal    
## [25753] Very Good Premium   Very Good Good      Very Good Premium  
## [25759] Premium   Premium   Very Good Very Good Premium   Premium  
## [25765] Good      Ideal     Very Good Ideal     Premium   Premium  
## [25771] Very Good Ideal     Very Good Ideal     Very Good Ideal    
## [25777] Good      Premium   Very Good Premium   Fair      Ideal    
## [25783] Premium   Ideal     Premium   Very Good Ideal     Very Good
## [25789] Premium   Premium   Good      Ideal     Very Good Ideal    
## [25795] Ideal     Ideal     Ideal     Good      Premium   Premium  
## [25801] Very Good Very Good Fair      Very Good Ideal     Premium  
## [25807] Very Good Very Good Very Good Very Good Ideal     Premium  
## [25813] Very Good Very Good Premium   Very Good Premium   Ideal    
## [25819] Premium   Ideal     Premium   Very Good Very Good Ideal    
## [25825] Premium   Premium   Premium   Ideal     Good      Premium  
## [25831] Ideal     Ideal     Fair      Premium   Premium   Premium  
## [25837] Ideal     Premium   Premium   Good      Ideal     Very Good
## [25843] Premium   Very Good Premium   Premium   Very Good Ideal    
## [25849] Premium   Premium   Good      Premium   Good      Premium  
## [25855] Very Good Premium   Ideal     Very Good Good      Premium  
## [25861] Very Good Premium   Premium   Premium   Premium   Fair     
## [25867] Premium   Premium   Good      Ideal     Ideal     Premium  
## [25873] Ideal     Good      Premium   Good      Premium   Fair     
## [25879] Very Good Ideal     Premium   Premium   Ideal     Good     
## [25885] Ideal     Premium   Ideal     Premium   Ideal     Very Good
## [25891] Very Good Premium   Very Good Very Good Very Good Very Good
## [25897] Ideal     Premium   Ideal     Fair      Premium   Ideal    
## [25903] Premium   Ideal     Premium   Ideal     Ideal     Very Good
## [25909] Ideal     Good      Very Good Premium   Very Good Premium  
## [25915] Ideal     Premium   Very Good Ideal     Premium   Premium  
## [25921] Good      Ideal     Very Good Very Good Good      Good     
## [25927] Very Good Very Good Very Good Good      Ideal     Very Good
## [25933] Ideal     Good      Premium   Premium   Premium   Premium  
## [25939] Premium   Very Good Premium   Ideal     Ideal     Premium  
## [25945] Very Good Premium   Premium   Ideal     Ideal     Good     
## [25951] Very Good Ideal     Very Good Ideal     Ideal     Premium  
## [25957] Ideal     Very Good Ideal     Ideal     Ideal     Very Good
## [25963] Good      Premium   Premium   Premium   Fair      Premium  
## [25969] Very Good Premium   Ideal     Premium   Good      Very Good
## [25975] Premium   Premium   Ideal     Premium   Premium   Premium  
## [25981] Premium   Premium   Ideal     Very Good Premium   Ideal    
## [25987] Ideal     Ideal     Ideal     Premium   Premium   Ideal    
## [25993] Ideal     Premium   Premium   Premium   Ideal     Very Good
## [25999] Premium   Premium   Very Good Ideal     Very Good Premium  
## [26005] Ideal     Good      Premium   Premium   Premium   Ideal    
## [26011] Ideal     Ideal     Ideal     Premium   Good      Very Good
## [26017] Very Good Ideal     Ideal     Ideal     Ideal     Premium  
## [26023] Ideal     Ideal     Ideal     Ideal     Ideal     Very Good
## [26029] Very Good Ideal     Ideal     Ideal     Ideal     Premium  
## [26035] Very Good Ideal     Premium   Good      Very Good Ideal    
## [26041] Premium   Very Good Very Good Ideal     Ideal     Ideal    
## [26047] Ideal     Premium   Very Good Premium   Ideal     Premium  
## [26053] Ideal     Good      Very Good Ideal     Ideal     Very Good
## [26059] Good      Very Good Ideal     Ideal     Premium   Very Good
## [26065] Ideal     Ideal     Fair      Good      Ideal     Ideal    
## [26071] Premium   Premium   Premium   Ideal     Ideal     Premium  
## [26077] Ideal     Very Good Ideal     Very Good Very Good Premium  
## [26083] Ideal     Premium   Ideal     Premium   Premium   Premium  
## [26089] Premium   Premium   Ideal     Fair      Very Good Very Good
## [26095] Very Good Premium   Good      Premium   Ideal     Fair     
## [26101] Very Good Very Good Premium   Ideal     Premium   Premium  
## [26107] Good      Premium   Ideal     Ideal     Premium   Ideal    
## [26113] Good      Ideal     Premium   Good      Premium   Good     
## [26119] Very Good Premium   Ideal     Premium   Ideal     Premium  
## [26125] Premium   Premium   Very Good Premium   Ideal     Premium  
## [26131] Premium   Good      Premium   Very Good Ideal     Ideal    
## [26137] Premium   Premium   Ideal     Ideal     Very Good Ideal    
## [26143] Very Good Very Good Ideal     Ideal     Premium   Premium  
## [26149] Very Good Ideal     Premium   Premium   Ideal     Premium  
## [26155] Very Good Good      Ideal     Very Good Premium   Ideal    
## [26161] Premium   Premium   Very Good Ideal     Ideal     Premium  
## [26167] Ideal     Premium   Premium   Premium   Ideal     Very Good
## [26173] Ideal     Very Good Ideal     Ideal     Very Good Premium  
## [26179] Premium   Premium   Premium   Premium   Premium   Premium  
## [26185] Ideal     Very Good Premium   Ideal     Fair      Fair     
## [26191] Premium   Very Good Premium   Premium   Fair      Ideal    
## [26197] Very Good Premium   Ideal     Ideal     Premium   Ideal    
## [26203] Fair      Ideal     Very Good Good      Ideal     Ideal    
## [26209] Very Good Very Good Premium   Premium   Ideal     Premium  
## [26215] Ideal     Ideal     Ideal     Premium   Very Good Very Good
## [26221] Good      Good      Fair      Ideal     Ideal     Good     
## [26227] Very Good Premium   Premium   Premium   Ideal     Premium  
## [26233] Very Good Premium   Very Good Very Good Premium   Premium  
## [26239] Premium   Very Good Ideal     Premium   Very Good Premium  
## [26245] Ideal     Very Good Ideal     Very Good Fair      Very Good
## [26251] Ideal     Very Good Ideal     Premium   Good      Premium  
## [26257] Ideal     Ideal     Premium   Ideal     Ideal     Ideal    
## [26263] Good      Ideal     Fair      Ideal     Ideal     Ideal    
## [26269] Very Good Ideal     Very Good Premium   Very Good Ideal    
## [26275] Premium   Very Good Ideal     Premium   Fair      Good     
## [26281] Ideal     Very Good Very Good Very Good Premium   Very Good
## [26287] Ideal     Fair      Premium   Ideal     Ideal     Ideal    
## [26293] Good      Good      Ideal     Premium   Ideal     Ideal    
## [26299] Premium   Premium   Premium   Premium   Ideal     Ideal    
## [26305] Premium   Premium   Very Good Premium   Premium   Premium  
## [26311] Very Good Ideal     Ideal     Premium   Premium   Very Good
## [26317] Very Good Ideal     Ideal     Ideal     Fair      Very Good
## [26323] Very Good Ideal     Fair      Very Good Ideal     Good     
## [26329] Premium   Premium   Very Good Very Good Ideal     Premium  
## [26335] Ideal     Ideal     Ideal     Ideal     Very Good Ideal    
## [26341] Ideal     Ideal     Ideal     Ideal     Ideal     Very Good
## [26347] Ideal     Very Good Ideal     Premium   Very Good Ideal    
## [26353] Ideal     Very Good Premium   Ideal     Ideal     Ideal    
## [26359] Very Good Premium   Premium   Ideal     Very Good Premium  
## [26365] Very Good Premium   Ideal     Good      Very Good Ideal    
## [26371] Ideal     Very Good Ideal     Very Good Ideal     Good     
## [26377] Premium   Very Good Ideal     Ideal     Very Good Premium  
## [26383] Ideal     Ideal     Very Good Very Good Ideal     Good     
## [26389] Fair      Premium   Premium   Premium   Premium   Good     
## [26395] Very Good Premium   Premium   Premium   Premium   Premium  
## [26401] Premium   Very Good Very Good Good      Premium   Ideal    
## [26407] Very Good Good      Very Good Premium   Very Good Premium  
## [26413] Very Good Very Good Premium   Premium   Premium   Premium  
## [26419] Ideal     Ideal     Premium   Very Good Ideal     Very Good
## [26425] Premium   Very Good Ideal     Very Good Ideal     Premium  
## [26431] Good      Fair      Very Good Premium   Ideal     Premium  
## [26437] Ideal     Ideal     Good      Premium   Premium   Premium  
## [26443] Premium   Very Good Very Good Premium   Very Good Ideal    
## [26449] Premium   Ideal     Premium   Very Good Ideal     Ideal    
## [26455] Premium   Ideal     Ideal     Premium   Good      Premium  
## [26461] Good      Ideal     Premium   Premium   Premium   Premium  
## [26467] Good      Ideal     Very Good Very Good Very Good Premium  
## [26473] Ideal     Ideal     Ideal     Ideal     Good      Very Good
## [26479] Very Good Premium   Premium   Premium   Very Good Very Good
## [26485] Premium   Very Good Very Good Very Good Good      Very Good
## [26491] Fair      Very Good Premium   Very Good Premium   Premium  
## [26497] Premium   Premium   Ideal     Very Good Premium   Premium  
## [26503] Very Good Premium   Very Good Ideal     Very Good Ideal    
## [26509] Ideal     Premium   Very Good Ideal     Ideal     Good     
## [26515] Good      Premium   Very Good Very Good Ideal     Premium  
## [26521] Very Good Premium   Ideal     Premium   Very Good Ideal    
## [26527] Ideal     Premium   Ideal     Very Good Ideal     Premium  
## [26533] Premium   Very Good Premium   Premium   Premium   Premium  
## [26539] Premium   Good      Very Good Premium   Very Good Premium  
## [26545] Good      Ideal     Good      Good      Premium   Premium  
## [26551] Ideal     Premium   Premium   Good      Good      Very Good
## [26557] Ideal     Ideal     Ideal     Good      Premium   Premium  
## [26563] Ideal     Premium   Ideal     Ideal     Ideal     Premium  
## [26569] Premium   Ideal     Premium   Premium   Fair      Very Good
## [26575] Very Good Premium   Premium   Ideal     Ideal     Good     
## [26581] Very Good Good      Good      Ideal     Ideal     Ideal    
## [26587] Very Good Good      Premium   Premium   Premium   Ideal    
## [26593] Premium   Ideal     Ideal     Ideal     Very Good Ideal    
## [26599] Ideal     Premium   Ideal     Premium   Ideal     Ideal    
## [26605] Premium   Ideal     Ideal     Very Good Very Good Premium  
## [26611] Premium   Ideal     Very Good Fair      Ideal     Premium  
## [26617] Premium   Ideal     Very Good Very Good Very Good Ideal    
## [26623] Fair      Very Good Premium   Very Good Premium   Premium  
## [26629] Very Good Ideal     Good      Premium   Very Good Ideal    
## [26635] Good      Very Good Ideal     Premium   Good      Very Good
## [26641] Very Good Very Good Good      Premium   Premium   Ideal    
## [26647] Ideal     Premium   Premium   Good      Very Good Ideal    
## [26653] Ideal     Premium   Very Good Very Good Very Good Ideal    
## [26659] Ideal     Ideal     Ideal     Premium   Very Good Premium  
## [26665] Premium   Premium   Very Good Ideal     Premium   Premium  
## [26671] Good      Very Good Very Good Ideal     Ideal     Ideal    
## [26677] Ideal     Very Good Very Good Very Good Very Good Very Good
## [26683] Ideal     Ideal     Ideal     Ideal     Ideal     Very Good
## [26689] Good      Very Good Very Good Very Good Very Good Very Good
## [26695] Very Good Ideal     Ideal     Ideal     Premium   Premium  
## [26701] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [26707] Very Good Very Good Ideal     Ideal     Very Good Premium  
## [26713] Ideal     Premium   Good      Ideal     Very Good Premium  
## [26719] Ideal     Good      Very Good Very Good Ideal     Good     
## [26725] Ideal     Very Good Ideal     Ideal     Very Good Premium  
## [26731] Ideal     Premium   Fair      Ideal     Good      Ideal    
## [26737] Ideal     Premium   Good      Ideal     Premium   Ideal    
## [26743] Premium   Very Good Ideal     Good      Premium   Ideal    
## [26749] Ideal     Very Good Premium   Very Good Ideal     Premium  
## [26755] Ideal     Ideal     Premium   Good      Ideal     Ideal    
## [26761] Ideal     Very Good Very Good Good      Premium   Premium  
## [26767] Ideal     Ideal     Good      Ideal     Premium   Very Good
## [26773] Premium   Very Good Very Good Ideal     Ideal     Good     
## [26779] Very Good Premium   Very Good Ideal     Premium   Premium  
## [26785] Premium   Ideal     Premium   Very Good Premium   Very Good
## [26791] Ideal     Ideal     Premium   Good      Ideal     Ideal    
## [26797] Premium   Premium   Ideal     Premium   Very Good Very Good
## [26803] Ideal     Premium   Ideal     Ideal     Ideal     Good     
## [26809] Very Good Very Good Premium   Premium   Ideal     Ideal    
## [26815] Ideal     Ideal     Premium   Ideal     Ideal     Premium  
## [26821] Very Good Very Good Ideal     Very Good Ideal     Ideal    
## [26827] Premium   Ideal     Very Good Good      Premium   Premium  
## [26833] Premium   Ideal     Good      Premium   Premium   Good     
## [26839] Very Good Premium   Very Good Ideal     Premium   Ideal    
## [26845] Good      Premium   Premium   Premium   Premium   Premium  
## [26851] Premium   Premium   Premium   Premium   Premium   Premium  
## [26857] Premium   Ideal     Premium   Premium   Very Good Premium  
## [26863] Ideal     Good      Ideal     Ideal     Very Good Ideal    
## [26869] Premium   Ideal     Premium   Ideal     Very Good Ideal    
## [26875] Premium   Premium   Very Good Ideal     Very Good Premium  
## [26881] Ideal     Very Good Premium   Ideal     Very Good Ideal    
## [26887] Very Good Good      Premium   Premium   Premium   Premium  
## [26893] Very Good Very Good Very Good Premium   Premium   Premium  
## [26899] Premium   Premium   Premium   Ideal     Ideal     Very Good
## [26905] Ideal     Very Good Very Good Very Good Ideal     Ideal    
## [26911] Ideal     Premium   Ideal     Premium   Premium   Premium  
## [26917] Ideal     Premium   Premium   Premium   Ideal     Ideal    
## [26923] Premium   Very Good Ideal     Very Good Premium   Premium  
## [26929] Premium   Premium   Ideal     Ideal     Premium   Fair     
## [26935] Premium   Premium   Ideal     Ideal     Very Good Premium  
## [26941] Premium   Premium   Premium   Premium   Very Good Very Good
## [26947] Ideal     Ideal     Ideal     Premium   Premium   Premium  
## [26953] Premium   Very Good Very Good Very Good Fair      Very Good
## [26959] Very Good Premium   Ideal     Very Good Premium   Ideal    
## [26965] Premium   Ideal     Premium   Ideal     Ideal     Ideal    
## [26971] Ideal     Good      Ideal     Very Good Premium   Very Good
## [26977] Premium   Very Good Ideal     Very Good Premium   Ideal    
## [26983] Very Good Premium   Ideal     Premium   Very Good Premium  
## [26989] Ideal     Ideal     Very Good Premium   Very Good Good     
## [26995] Ideal     Very Good Premium   Premium   Very Good Fair     
## [27001] Ideal     Good      Ideal     Ideal     Good      Premium  
## [27007] Very Good Ideal     Ideal     Ideal     Premium   Premium  
## [27013] Very Good Ideal     Ideal     Premium   Premium   Premium  
## [27019] Good      Very Good Premium   Premium   Very Good Ideal    
## [27025] Very Good Premium   Premium   Premium   Premium   Premium  
## [27031] Ideal     Very Good Ideal     Ideal     Ideal     Premium  
## [27037] Ideal     Ideal     Premium   Very Good Ideal     Ideal    
## [27043] Premium   Very Good Ideal     Ideal     Very Good Ideal    
## [27049] Premium   Very Good Good      Ideal     Ideal     Ideal    
## [27055] Ideal     Premium   Premium   Premium   Good      Very Good
## [27061] Premium   Ideal     Premium   Premium   Ideal     Very Good
## [27067] Premium   Premium   Premium   Premium   Ideal     Premium  
## [27073] Ideal     Ideal     Premium   Very Good Premium   Premium  
## [27079] Ideal     Very Good Premium   Ideal     Ideal     Ideal    
## [27085] Premium   Premium   Very Good Ideal     Ideal     Premium  
## [27091] Ideal     Ideal     Ideal     Premium   Ideal     Premium  
## [27097] Premium   Premium   Premium   Ideal     Very Good Ideal    
## [27103] Very Good Ideal     Premium   Ideal     Premium   Premium  
## [27109] Very Good Ideal     Ideal     Premium   Premium   Very Good
## [27115] Very Good Premium   Very Good Premium   Premium   Premium  
## [27121] Ideal     Very Good Ideal     Premium   Good      Very Good
## [27127] Very Good Very Good Premium   Ideal     Fair      Ideal    
## [27133] Good      Premium   Good      Ideal     Ideal     Premium  
## [27139] Ideal     Ideal     Premium   Very Good Premium   Premium  
## [27145] Ideal     Good      Ideal     Ideal     Very Good Very Good
## [27151] Premium   Ideal     Ideal     Ideal     Premium   Premium  
## [27157] Good      Ideal     Premium   Ideal     Ideal     Fair     
## [27163] Good      Ideal     Fair      Ideal     Premium   Very Good
## [27169] Very Good Premium   Ideal     Very Good Premium   Ideal    
## [27175] Ideal     Good      Very Good Good      Premium   Premium  
## [27181] Ideal     Premium   Ideal     Ideal     Ideal     Very Good
## [27187] Premium   Premium   Very Good Very Good Good      Premium  
## [27193] Premium   Ideal     Premium   Good      Good      Ideal    
## [27199] Very Good Premium   Very Good Premium   Ideal     Premium  
## [27205] Premium   Ideal     Premium   Very Good Good      Ideal    
## [27211] Ideal     Premium   Premium   Very Good Ideal     Premium  
## [27217] Ideal     Premium   Ideal     Ideal     Ideal     Very Good
## [27223] Premium   Premium   Very Good Very Good Ideal     Premium  
## [27229] Premium   Ideal     Very Good Good      Very Good Good     
## [27235] Ideal     Premium   Premium   Ideal     Premium   Good     
## [27241] Ideal     Premium   Ideal     Premium   Premium   Premium  
## [27247] Premium   Ideal     Premium   Very Good Ideal     Ideal    
## [27253] Premium   Premium   Premium   Very Good Premium   Premium  
## [27259] Premium   Very Good Ideal     Very Good Ideal     Premium  
## [27265] Very Good Very Good Premium   Very Good Ideal     Ideal    
## [27271] Fair      Premium   Very Good Premium   Premium   Very Good
## [27277] Ideal     Ideal     Good      Premium   Premium   Ideal    
## [27283] Very Good Very Good Ideal     Very Good Very Good Premium  
## [27289] Very Good Good      Good      Ideal     Premium   Ideal    
## [27295] Premium   Premium   Very Good Premium   Premium   Premium  
## [27301] Very Good Ideal     Ideal     Very Good Premium   Good     
## [27307] Premium   Ideal     Very Good Premium   Ideal     Ideal    
## [27313] Premium   Premium   Ideal     Premium   Very Good Ideal    
## [27319] Premium   Premium   Premium   Ideal     Premium   Premium  
## [27325] Ideal     Ideal     Good      Very Good Ideal     Very Good
## [27331] Ideal     Very Good Very Good Ideal     Ideal     Premium  
## [27337] Premium   Ideal     Ideal     Premium   Very Good Very Good
## [27343] Ideal     Ideal     Good      Good      Premium   Premium  
## [27349] Premium   Very Good Ideal     Fair      Very Good Good     
## [27355] Premium   Good      Very Good Premium   Ideal     Premium  
## [27361] Good      Ideal     Ideal     Premium   Ideal     Very Good
## [27367] Ideal     Premium   Ideal     Ideal     Ideal     Ideal    
## [27373] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [27379] Premium   Very Good Very Good Good      Very Good Very Good
## [27385] Very Good Ideal     Very Good Ideal     Premium   Ideal    
## [27391] Premium   Ideal     Very Good Very Good Very Good Very Good
## [27397] Ideal     Ideal     Premium   Ideal     Ideal     Premium  
## [27403] Premium   Ideal     Fair      Very Good Ideal     Premium  
## [27409] Ideal     Premium   Premium   Very Good Premium   Very Good
## [27415] Ideal     Fair      Premium   Good      Good      Ideal    
## [27421] Fair      Premium   Very Good Good      Very Good Very Good
## [27427] Ideal     Premium   Premium   Premium   Ideal     Very Good
## [27433] Ideal     Premium   Ideal     Ideal     Ideal     Ideal    
## [27439] Ideal     Premium   Very Good Very Good Good      Very Good
## [27445] Good      Very Good Premium   Ideal     Ideal     Very Good
## [27451] Premium   Premium   Very Good Very Good Ideal     Very Good
## [27457] Ideal     Very Good Very Good Premium   Premium   Premium  
## [27463] Very Good Very Good Very Good Premium   Ideal     Ideal    
## [27469] Very Good Very Good Very Good Ideal     Ideal     Ideal    
## [27475] Premium   Premium   Premium   Very Good Very Good Ideal    
## [27481] Ideal     Premium   Premium   Ideal     Ideal     Ideal    
## [27487] Very Good Ideal     Ideal     Premium   Ideal     Ideal    
## [27493] Very Good Ideal     Premium   Ideal     Ideal     Premium  
## [27499] Ideal     Premium   Ideal     Premium   Premium   Premium  
## [27505] Very Good Very Good Premium   Very Good Premium   Very Good
## [27511] Very Good Very Good Good      Premium   Premium   Fair     
## [27517] Fair      Good      Good      Premium   Ideal     Premium  
## [27523] Premium   Premium   Very Good Ideal     Premium   Ideal    
## [27529] Ideal     Ideal     Premium   Very Good Ideal     Premium  
## [27535] Premium   Ideal     Ideal     Ideal     Premium   Ideal    
## [27541] Ideal     Ideal     Ideal     Fair      Premium   Ideal    
## [27547] Premium   Ideal     Premium   Ideal     Good      Premium  
## [27553] Good      Ideal     Premium   Ideal     Ideal     Very Good
## [27559] Premium   Ideal     Premium   Ideal     Ideal     Very Good
## [27565] Premium   Premium   Ideal     Good      Good      Premium  
## [27571] Very Good Ideal     Ideal     Very Good Very Good Premium  
## [27577] Very Good Very Good Ideal     Premium   Ideal     Premium  
## [27583] Premium   Premium   Premium   Very Good Very Good Very Good
## [27589] Premium   Ideal     Premium   Ideal     Premium   Ideal    
## [27595] Ideal     Ideal     Premium   Ideal     Premium   Premium  
## [27601] Ideal     Ideal     Ideal     Good      Ideal     Good     
## [27607] Good      Ideal     Premium   Very Good Premium   Very Good
## [27613] Premium   Premium   Very Good Premium   Good      Very Good
## [27619] Very Good Very Good Very Good Ideal     Very Good Fair     
## [27625] Premium   Very Good Premium   Very Good Ideal     Ideal    
## [27631] Fair      Ideal     Ideal     Good      Ideal     Very Good
## [27637] Very Good Very Good Premium   Premium   Premium   Good     
## [27643] Very Good Fair      Ideal     Good      Fair      Premium  
## [27649] Ideal     Good      Premium   Premium   Ideal     Ideal    
## [27655] Ideal     Premium   Ideal     Premium   Good      Very Good
## [27661] Good      Very Good Ideal     Premium   Premium   Ideal    
## [27667] Ideal     Ideal     Premium   Very Good Premium   Ideal    
## [27673] Good      Very Good Premium   Very Good Ideal     Ideal    
## [27679] Ideal     Premium   Premium   Premium   Good      Very Good
## [27685] Premium   Premium   Premium   Premium   Ideal     Premium  
## [27691] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [27697] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [27703] Ideal     Good      Ideal     Ideal     Premium   Good     
## [27709] Ideal     Ideal     Premium   Premium   Premium   Ideal    
## [27715] Premium   Premium   Premium   Ideal     Premium   Premium  
## [27721] Very Good Premium   Ideal     Ideal     Very Good Premium  
## [27727] Premium   Ideal     Ideal     Very Good Ideal     Premium  
## [27733] Ideal     Very Good Ideal     Ideal     Very Good Premium  
## [27739] Ideal     Good      Premium   Ideal     Premium   Premium  
## [27745] Premium   Very Good Ideal     Ideal     Very Good Premium  
## [27751] Very Good Premium   Very Good Ideal     Ideal     Premium  
## [27757] Premium   Ideal     Premium   Premium   Premium   Ideal    
## [27763] Premium   Premium   Premium   Ideal     Premium   Premium  
## [27769] Premium   Premium   Ideal     Ideal     Premium   Premium  
## [27775] Premium   Ideal     Ideal     Ideal     Ideal     Premium  
## [27781] Premium   Very Good Good      Ideal     Very Good Premium  
## [27787] Premium   Ideal     Ideal     Premium   Very Good Premium  
## [27793] Ideal     Very Good Very Good Premium   Very Good Premium  
## [27799] Ideal     Very Good Premium   Ideal     Ideal     Premium  
## [27805] Very Good Very Good Ideal     Ideal     Ideal     Ideal    
## [27811] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [27817] Ideal     Good      Premium   Premium   Premium   Very Good
## [27823] Premium   Premium   Premium   Premium   Very Good Premium  
## [27829] Very Good Ideal     Very Good Very Good Very Good Very Good
## [27835] Very Good Good      Ideal     Ideal     Ideal     Ideal    
## [27841] Good      Premium   Premium   Very Good Premium   Very Good
## [27847] Ideal     Ideal     Very Good Ideal     Ideal     Ideal    
## [27853] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [27859] Ideal     Ideal     Good      Ideal     Ideal     Good     
## [27865] Ideal     Very Good Very Good Ideal     Good      Premium  
## [27871] Good      Good      Very Good Premium   Very Good Ideal    
## [27877] Ideal     Ideal     Ideal     Ideal     Ideal     Good     
## [27883] Very Good Very Good Ideal     Ideal     Ideal     Ideal    
## [27889] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [27895] Ideal     Ideal     Very Good Good      Good      Good     
## [27901] Good      Very Good Very Good Very Good Very Good Ideal    
## [27907] Ideal     Very Good Very Good Very Good Very Good Very Good
## [27913] Very Good Ideal     Ideal     Ideal     Very Good Ideal    
## [27919] Good      Ideal     Very Good Very Good Very Good Ideal    
## [27925] Ideal     Ideal     Premium   Premium   Ideal     Good     
## [27931] Good      Very Good Very Good Ideal     Ideal     Ideal    
## [27937] Ideal     Ideal     Very Good Very Good Very Good Very Good
## [27943] Very Good Very Good Very Good Ideal     Premium   Good     
## [27949] Very Good Very Good Very Good Very Good Ideal     Ideal    
## [27955] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [27961] Premium   Ideal     Very Good Very Good Premium   Ideal    
## [27967] Ideal     Ideal     Premium   Premium   Premium   Ideal    
## [27973] Premium   Ideal     Ideal     Very Good Ideal     Very Good
## [27979] Ideal     Ideal     Ideal     Ideal     Very Good Premium  
## [27985] Ideal     Ideal     Ideal     Very Good Very Good Ideal    
## [27991] Ideal     Premium   Premium   Ideal     Premium   Ideal    
## [27997] Very Good Premium   Premium   Premium   Very Good Ideal    
## [28003] Premium   Premium   Ideal     Premium   Ideal     Ideal    
## [28009] Premium   Premium   Ideal     Very Good Premium   Ideal    
## [28015] Ideal     Ideal     Very Good Ideal     Ideal     Ideal    
## [28021] Very Good Premium   Ideal     Very Good Ideal     Ideal    
## [28027] Very Good Premium   Premium   Ideal     Ideal     Very Good
## [28033] Very Good Ideal     Ideal     Ideal     Ideal     Very Good
## [28039] Ideal     Good      Fair      Good      Ideal     Very Good
## [28045] Very Good Very Good Very Good Very Good Very Good Very Good
## [28051] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [28057] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [28063] Ideal     Ideal     Good      Ideal     Ideal     Very Good
## [28069] Very Good Very Good Ideal     Good      Premium   Ideal    
## [28075] Very Good Ideal     Very Good Ideal     Ideal     Ideal    
## [28081] Ideal     Ideal     Ideal     Premium   Premium   Very Good
## [28087] Very Good Very Good Fair      Good      Ideal     Ideal    
## [28093] Premium   Ideal     Premium   Premium   Ideal     Very Good
## [28099] Very Good Very Good Very Good Ideal     Ideal     Premium  
## [28105] Good      Very Good Premium   Ideal     Ideal     Ideal    
## [28111] Premium   Ideal     Good      Ideal     Ideal     Ideal    
## [28117] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [28123] Very Good Premium   Very Good Good      Very Good Very Good
## [28129] Very Good Very Good Very Good Very Good Very Good Very Good
## [28135] Very Good Ideal     Ideal     Ideal     Ideal     Ideal    
## [28141] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [28147] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [28153] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [28159] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [28165] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [28171] Ideal     Good      Premium   Premium   Very Good Ideal    
## [28177] Very Good Very Good Ideal     Premium   Premium   Premium  
## [28183] Ideal     Premium   Premium   Premium   Good      Very Good
## [28189] Premium   Very Good Very Good Good      Premium   Ideal    
## [28195] Very Good Premium   Very Good Premium   Ideal     Premium  
## [28201] Premium   Very Good Very Good Premium   Very Good Very Good
## [28207] Ideal     Very Good Good      Premium   Ideal     Very Good
## [28213] Premium   Premium   Good      Very Good Very Good Premium  
## [28219] Ideal     Premium   Very Good Ideal     Ideal     Premium  
## [28225] Ideal     Good      Ideal     Premium   Premium   Very Good
## [28231] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [28237] Ideal     Very Good Ideal     Ideal     Ideal     Premium  
## [28243] Premium   Very Good Ideal     Ideal     Ideal     Premium  
## [28249] Ideal     Ideal     Premium   Ideal     Premium   Premium  
## [28255] Ideal     Premium   Ideal     Premium   Ideal     Premium  
## [28261] Very Good Very Good Ideal     Good      Good      Ideal    
## [28267] Very Good Ideal     Ideal     Good      Fair      Good     
## [28273] Very Good Very Good Very Good Ideal     Ideal     Very Good
## [28279] Very Good Premium   Good      Ideal     Ideal     Ideal    
## [28285] Premium   Ideal     Very Good Very Good Ideal     Ideal    
## [28291] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [28297] Ideal     Ideal     Ideal     Good      Good      Very Good
## [28303] Ideal     Very Good Good      Very Good Very Good Very Good
## [28309] Very Good Good      Premium   Good      Good      Ideal    
## [28315] Premium   Good      Very Good Very Good Premium   Very Good
## [28321] Very Good Premium   Ideal     Very Good Very Good Ideal    
## [28327] Premium   Ideal     Ideal     Ideal     Ideal     Ideal    
## [28333] Premium   Ideal     Premium   Ideal     Premium   Premium  
## [28339] Premium   Very Good Ideal     Ideal     Good      Ideal    
## [28345] Premium   Ideal     Premium   Ideal     Ideal     Very Good
## [28351] Very Good Very Good Very Good Very Good Ideal     Ideal    
## [28357] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [28363] Ideal     Good      Good      Ideal     Ideal     Ideal    
## [28369] Very Good Very Good Ideal     Ideal     Ideal     Ideal    
## [28375] Premium   Premium   Premium   Premium   Premium   Premium  
## [28381] Premium   Very Good Very Good Premium   Ideal     Premium  
## [28387] Premium   Premium   Ideal     Ideal     Ideal     Premium  
## [28393] Good      Ideal     Premium   Premium   Premium   Premium  
## [28399] Very Good Very Good Premium   Good      Very Good Ideal    
## [28405] Very Good Premium   Premium   Premium   Ideal     Ideal    
## [28411] Ideal     Premium   Ideal     Ideal     Ideal     Good     
## [28417] Good      Good      Good      Ideal     Good      Very Good
## [28423] Very Good Ideal     Ideal     Ideal     Ideal     Ideal    
## [28429] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [28435] Ideal     Ideal     Ideal     Ideal     Ideal     Very Good
## [28441] Very Good Very Good Ideal     Ideal     Ideal     Ideal    
## [28447] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [28453] Ideal     Ideal     Very Good Good      Very Good Very Good
## [28459] Very Good Very Good Very Good Very Good Very Good Very Good
## [28465] Very Good Very Good Very Good Ideal     Ideal     Ideal    
## [28471] Ideal     Premium   Premium   Premium   Ideal     Ideal    
## [28477] Ideal     Premium   Very Good Premium   Very Good Very Good
## [28483] Very Good Very Good Premium   Premium   Good      Very Good
## [28489] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [28495] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [28501] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [28507] Ideal     Fair      Premium   Premium   Ideal     Premium  
## [28513] Very Good Ideal     Premium   Ideal     Ideal     Ideal    
## [28519] Very Good Very Good Very Good Very Good Ideal     Ideal    
## [28525] Ideal     Very Good Very Good Very Good Very Good Ideal    
## [28531] Ideal     Ideal     Ideal     Ideal     Fair      Premium  
## [28537] Very Good Premium   Premium   Premium   Premium   Premium  
## [28543] Good      Premium   Premium   Premium   Premium   Premium  
## [28549] Very Good Premium   Premium   Ideal     Premium   Ideal    
## [28555] Premium   Ideal     Premium   Ideal     Ideal     Ideal    
## [28561] Very Good Ideal     Premium   Premium   Ideal     Premium  
## [28567] Very Good Premium   Good      Ideal     Premium   Very Good
## [28573] Ideal     Premium   Premium   Very Good Ideal     Premium  
## [28579] Very Good Ideal     Premium   Premium   Ideal     Ideal    
## [28585] Premium   Good      Ideal     Ideal     Ideal     Ideal    
## [28591] Ideal     Premium   Ideal     Ideal     Premium   Premium  
## [28597] Very Good Very Good Very Good Ideal     Very Good Premium  
## [28603] Ideal     Premium   Premium   Very Good Premium   Ideal    
## [28609] Premium   Premium   Ideal     Ideal     Very Good Ideal    
## [28615] Good      Premium   Good      Premium   Good      Very Good
## [28621] Premium   Very Good Very Good Ideal     Ideal     Ideal    
## [28627] Very Good Very Good Ideal     Ideal     Ideal     Ideal    
## [28633] Ideal     Premium   Very Good Premium   Very Good Very Good
## [28639] Very Good Good      Ideal     Premium   Very Good Premium  
## [28645] Ideal     Very Good Ideal     Ideal     Very Good Very Good
## [28651] Ideal     Premium   Premium   Very Good Very Good Very Good
## [28657] Ideal     Ideal     Ideal     Good      Ideal     Very Good
## [28663] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [28669] Premium   Ideal     Ideal     Ideal     Ideal     Ideal    
## [28675] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [28681] Ideal     Ideal     Ideal     Ideal     Good      Good     
## [28687] Ideal     Good      Very Good Very Good Very Good Very Good
## [28693] Very Good Very Good Ideal     Ideal     Ideal     Good     
## [28699] Premium   Very Good Very Good Very Good Very Good Very Good
## [28705] Very Good Ideal     Ideal     Ideal     Ideal     Good     
## [28711] Very Good Premium   Premium   Premium   Premium   Premium  
## [28717] Premium   Good      Very Good Premium   Ideal     Very Good
## [28723] Premium   Premium   Ideal     Ideal     Very Good Ideal    
## [28729] Ideal     Ideal     Ideal     Premium   Ideal     Ideal    
## [28735] Ideal     Ideal     Ideal     Ideal     Premium   Premium  
## [28741] Ideal     Premium   Ideal     Ideal     Ideal     Ideal    
## [28747] Ideal     Ideal     Ideal     Ideal     Premium   Very Good
## [28753] Very Good Ideal     Ideal     Very Good Very Good Ideal    
## [28759] Ideal     Very Good Ideal     Ideal     Ideal     Premium  
## [28765] Premium   Ideal     Ideal     Premium   Premium   Ideal    
## [28771] Ideal     Very Good Very Good Very Good Very Good Ideal    
## [28777] Ideal     Ideal     Good      Good      Ideal     Premium  
## [28783] Very Good Ideal     Good      Very Good Premium   Premium  
## [28789] Good      Very Good Premium   Very Good Premium   Very Good
## [28795] Very Good Very Good Very Good Very Good Very Good Very Good
## [28801] Very Good Very Good Very Good Very Good Ideal     Ideal    
## [28807] Ideal     Ideal     Ideal     Good      Premium   Good     
## [28813] Very Good Ideal     Ideal     Ideal     Premium   Ideal    
## [28819] Ideal     Ideal     Ideal     Very Good Good      Premium  
## [28825] Premium   Very Good Premium   Premium   Very Good Ideal    
## [28831] Very Good Premium   Very Good Ideal     Ideal     Premium  
## [28837] Very Good Premium   Good      Ideal     Ideal     Very Good
## [28843] Very Good Very Good Ideal     Very Good Premium   Very Good
## [28849] Premium   Premium   Very Good Ideal     Ideal     Very Good
## [28855] Premium   Premium   Premium   Ideal     Very Good Ideal    
## [28861] Ideal     Ideal     Ideal     Ideal     Ideal     Good     
## [28867] Very Good Premium   Ideal     Premium   Premium   Premium  
## [28873] Premium   Ideal     Ideal     Ideal     Ideal     Ideal    
## [28879] Premium   Ideal     Good      Premium   Very Good Good     
## [28885] Ideal     Ideal     Very Good Good      Premium   Very Good
## [28891] Very Good Very Good Premium   Premium   Very Good Very Good
## [28897] Good      Premium   Good      Ideal     Very Good Very Good
## [28903] Very Good Good      Very Good Ideal     Very Good Very Good
## [28909] Good      Very Good Premium   Very Good Ideal     Very Good
## [28915] Ideal     Good      Premium   Ideal     Very Good Good     
## [28921] Ideal     Good      Ideal     Ideal     Very Good Ideal    
## [28927] Premium   Good      Very Good Very Good Premium   Good     
## [28933] Premium   Very Good Premium   Premium   Good      Good     
## [28939] Very Good Premium   Good      Ideal     Very Good Ideal    
## [28945] Good      Good      Ideal     Ideal     Premium   Very Good
## [28951] Very Good Good      Good      Very Good Very Good Premium  
## [28957] Good      Premium   Premium   Very Good Ideal     Very Good
## [28963] Premium   Very Good Premium   Ideal     Very Good Premium  
## [28969] Premium   Premium   Very Good Very Good Good      Very Good
## [28975] Premium   Ideal     Premium   Good      Very Good Very Good
## [28981] Very Good Ideal     Ideal     Ideal     Very Good Good     
## [28987] Ideal     Good      Ideal     Ideal     Premium   Ideal    
## [28993] Very Good Ideal     Ideal     Premium   Ideal     Good     
## [28999] Premium   Ideal     Ideal     Ideal     Premium   Very Good
## [29005] Very Good Ideal     Premium   Very Good Premium   Very Good
## [29011] Very Good Ideal     Very Good Very Good Ideal     Ideal    
## [29017] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [29023] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [29029] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [29035] Ideal     Ideal     Good      Good      Good      Good     
## [29041] Very Good Good      Premium   Good      Ideal     Very Good
## [29047] Good      Good      Very Good Premium   Very Good Very Good
## [29053] Very Good Good      Very Good Good      Very Good Good     
## [29059] Good      Good      Very Good Good      Very Good Good     
## [29065] Good      Good      Good      Very Good Very Good Ideal    
## [29071] Ideal     Premium   Very Good Very Good Ideal     Ideal    
## [29077] Ideal     Ideal     Premium   Very Good Very Good Ideal    
## [29083] Ideal     Premium   Very Good Premium   Premium   Ideal    
## [29089] Premium   Good      Ideal     Very Good Premium   Premium  
## [29095] Premium   Ideal     Premium   Premium   Ideal     Ideal    
## [29101] Ideal     Premium   Premium   Ideal     Ideal     Ideal    
## [29107] Ideal     Premium   Premium   Premium   Premium   Very Good
## [29113] Ideal     Premium   Very Good Premium   Premium   Ideal    
## [29119] Very Good Good      Good      Premium   Very Good Ideal    
## [29125] Ideal     Ideal     Very Good Very Good Very Good Very Good
## [29131] Ideal     Ideal     Very Good Very Good Very Good Very Good
## [29137] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [29143] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [29149] Ideal     Ideal     Premium   Very Good Premium   Very Good
## [29155] Very Good Ideal     Ideal     Ideal     Premium   Premium  
## [29161] Premium   Premium   Very Good Good      Fair      Ideal    
## [29167] Good      Very Good Very Good Very Good Very Good Very Good
## [29173] Very Good Very Good Ideal     Ideal     Ideal     Ideal    
## [29179] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [29185] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [29191] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [29197] Ideal     Very Good Good      Ideal     Ideal     Very Good
## [29203] Very Good Good      Ideal     Ideal     Very Good Very Good
## [29209] Good      Ideal     Ideal     Ideal     Ideal     Ideal    
## [29215] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [29221] Ideal     Ideal     Premium   Ideal     Premium   Ideal    
## [29227] Fair      Very Good Very Good Very Good Very Good Very Good
## [29233] Very Good Ideal     Ideal     Ideal     Ideal     Good     
## [29239] Good      Very Good Very Good Premium   Very Good Very Good
## [29245] Ideal     Good      Very Good Ideal     Ideal     Ideal    
## [29251] Very Good Very Good Ideal     Ideal     Ideal     Ideal    
## [29257] Ideal     Ideal     Ideal     Ideal     Premium   Premium  
## [29263] Premium   Premium   Ideal     Premium   Very Good Very Good
## [29269] Very Good Premium   Ideal     Premium   Good      Ideal    
## [29275] Premium   Premium   Premium   Premium   Ideal     Premium  
## [29281] Very Good Very Good Very Good Very Good Ideal     Ideal    
## [29287] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [29293] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [29299] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [29305] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [29311] Premium   Premium   Premium   Premium   Ideal     Ideal    
## [29317] Ideal     Premium   Ideal     Ideal     Ideal     Ideal    
## [29323] Premium   Premium   Premium   Very Good Ideal     Ideal    
## [29329] Premium   Premium   Very Good Good      Premium   Ideal    
## [29335] Premium   Premium   Good      Premium   Very Good Ideal    
## [29341] Premium   Premium   Ideal     Ideal     Premium   Ideal    
## [29347] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [29353] Ideal     Premium   Ideal     Ideal     Very Good Premium  
## [29359] Premium   Ideal     Ideal     Very Good Premium   Premium  
## [29365] Premium   Premium   Ideal     Good      Ideal     Ideal    
## [29371] Very Good Very Good Premium   Premium   Very Good Very Good
## [29377] Ideal     Premium   Very Good Very Good Very Good Premium  
## [29383] Ideal     Ideal     Premium   Premium   Very Good Very Good
## [29389] Very Good Very Good Very Good Very Good Very Good Good     
## [29395] Good      Premium   Premium   Premium   Premium   Ideal    
## [29401] Ideal     Very Good Ideal     Ideal     Ideal     Ideal    
## [29407] Ideal     Ideal     Ideal     Ideal     Premium   Premium  
## [29413] Premium   Very Good Very Good Very Good Very Good Ideal    
## [29419] Premium   Very Good Ideal     Ideal     Very Good Very Good
## [29425] Ideal     Ideal     Very Good Very Good Good      Very Good
## [29431] Premium   Premium   Ideal     Very Good Good      Very Good
## [29437] Premium   Premium   Premium   Very Good Very Good Very Good
## [29443] Very Good Ideal     Fair      Very Good Ideal     Ideal    
## [29449] Ideal     Ideal     Ideal     Ideal     Good      Very Good
## [29455] Premium   Ideal     Ideal     Ideal     Very Good Very Good
## [29461] Ideal     Ideal     Premium   Ideal     Premium   Ideal    
## [29467] Ideal     Ideal     Very Good Very Good Ideal     Ideal    
## [29473] Ideal     Ideal     Premium   Premium   Premium   Ideal    
## [29479] Premium   Premium   Ideal     Ideal     Ideal     Very Good
## [29485] Very Good Ideal     Very Good Ideal     Ideal     Ideal    
## [29491] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [29497] Ideal     Premium   Very Good Ideal     Ideal     Ideal    
## [29503] Very Good Premium   Premium   Very Good Ideal     Ideal    
## [29509] Ideal     Premium   Ideal     Very Good Ideal     Ideal    
## [29515] Very Good Very Good Ideal     Ideal     Ideal     Ideal    
## [29521] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [29527] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [29533] Very Good Very Good Very Good Very Good Ideal     Ideal    
## [29539] Ideal     Ideal     Ideal     Ideal     Fair      Premium  
## [29545] Good      Good      Very Good Very Good Very Good Ideal    
## [29551] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [29557] Ideal     Ideal     Premium   Premium   Premium   Good     
## [29563] Very Good Very Good Good      Good      Ideal     Good     
## [29569] Very Good Good      Ideal     Very Good Good      Good     
## [29575] Very Good Ideal     Very Good Premium   Very Good Very Good
## [29581] Very Good Very Good Good      Good      Ideal     Ideal    
## [29587] Ideal     Ideal     Ideal     Good      Ideal     Ideal    
## [29593] Good      Good      Premium   Ideal     Ideal     Very Good
## [29599] Premium   Premium   Very Good Ideal     Ideal     Very Good
## [29605] Ideal     Ideal     Ideal     Ideal     Premium   Premium  
## [29611] Very Good Very Good Very Good Very Good Very Good Ideal    
## [29617] Ideal     Ideal     Ideal     Ideal     Good      Good     
## [29623] Ideal     Good      Very Good Ideal     Good      Good     
## [29629] Premium   Very Good Very Good Very Good Ideal     Ideal    
## [29635] Ideal     Ideal     Good      Very Good Very Good Very Good
## [29641] Ideal     Premium   Premium   Very Good Ideal     Very Good
## [29647] Premium   Ideal     Ideal     Premium   Ideal     Ideal    
## [29653] Ideal     Premium   Ideal     Very Good Very Good Ideal    
## [29659] Ideal     Ideal     Premium   Ideal     Premium   Ideal    
## [29665] Ideal     Ideal     Ideal     Ideal     Very Good Ideal    
## [29671] Very Good Very Good Very Good Very Good Very Good Very Good
## [29677] Very Good Very Good Ideal     Ideal     Ideal     Ideal    
## [29683] Ideal     Ideal     Ideal     Ideal     Very Good Very Good
## [29689] Very Good Very Good Ideal     Premium   Ideal     Premium  
## [29695] Very Good Premium   Good      Ideal     Premium   Ideal    
## [29701] Ideal     Good      Ideal     Ideal     Ideal     Very Good
## [29707] Very Good Ideal     Premium   Ideal     Ideal     Good     
## [29713] Good      Ideal     Very Good Good      Ideal     Ideal    
## [29719] Ideal     Good      Ideal     Good      Very Good Premium  
## [29725] Premium   Premium   Ideal     Ideal     Very Good Good     
## [29731] Ideal     Premium   Very Good Good      Very Good Ideal    
## [29737] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [29743] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [29749] Premium   Very Good Very Good Very Good Ideal     Ideal    
## [29755] Premium   Premium   Premium   Premium   Ideal     Premium  
## [29761] Premium   Premium   Premium   Ideal     Premium   Premium  
## [29767] Ideal     Premium   Ideal     Premium   Ideal     Ideal    
## [29773] Premium   Ideal     Very Good Ideal     Premium   Premium  
## [29779] Premium   Good      Premium   Ideal     Ideal     Ideal    
## [29785] Premium   Ideal     Ideal     Ideal     Premium   Ideal    
## [29791] Ideal     Ideal     Premium   Ideal     Premium   Premium  
## [29797] Ideal     Ideal     Ideal     Very Good Ideal     Premium  
## [29803] Premium   Very Good Premium   Ideal     Premium   Very Good
## [29809] Good      Premium   Premium   Ideal     Ideal     Premium  
## [29815] Premium   Premium   Very Good Premium   Very Good Premium  
## [29821] Premium   Premium   Good      Premium   Very Good Very Good
## [29827] Very Good Very Good Very Good Ideal     Ideal     Ideal    
## [29833] Ideal     Ideal     Ideal     Premium   Premium   Premium  
## [29839] Ideal     Ideal     Ideal     Very Good Very Good Ideal    
## [29845] Premium   Premium   Ideal     Premium   Premium   Ideal    
## [29851] Premium   Ideal     Very Good Very Good Premium   Ideal    
## [29857] Ideal     Very Good Ideal     Premium   Ideal     Ideal    
## [29863] Ideal     Very Good Very Good Premium   Ideal     Very Good
## [29869] Ideal     Premium   Very Good Premium   Ideal     Ideal    
## [29875] Ideal     Premium   Ideal     Very Good Ideal     Very Good
## [29881] Very Good Very Good Ideal     Ideal     Ideal     Ideal    
## [29887] Ideal     Very Good Ideal     Very Good Ideal     Ideal    
## [29893] Premium   Ideal     Premium   Premium   Premium   Premium  
## [29899] Ideal     Very Good Very Good Very Good Very Good Ideal    
## [29905] Ideal     Ideal     Ideal     Ideal     Good      Very Good
## [29911] Very Good Ideal     Very Good Premium   Very Good Ideal    
## [29917] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [29923] Ideal     Ideal     Ideal     Ideal     Ideal     Good     
## [29929] Premium   Fair      Very Good Ideal     Ideal     Premium  
## [29935] Good      Ideal     Very Good Very Good Very Good Ideal    
## [29941] Ideal     Very Good Ideal     Premium   Very Good Very Good
## [29947] Ideal     Ideal     Very Good Ideal     Premium   Ideal    
## [29953] Very Good Good      Good      Ideal     Premium   Good     
## [29959] Ideal     Ideal     Ideal     Ideal     Premium   Very Good
## [29965] Very Good Very Good Good      Ideal     Very Good Very Good
## [29971] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [29977] Ideal     Ideal     Very Good Very Good Ideal     Ideal    
## [29983] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [29989] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [29995] Ideal     Premium   Premium   Ideal     Ideal     Very Good
## [30001] Ideal     Ideal     Premium   Ideal     Ideal     Premium  
## [30007] Very Good Very Good Very Good Very Good Very Good Very Good
## [30013] Ideal     Ideal     Ideal     Very Good Premium   Good     
## [30019] Very Good Very Good Good      Very Good Ideal     Ideal    
## [30025] Ideal     Ideal     Ideal     Ideal     Good      Premium  
## [30031] Good      Very Good Good      Ideal     Ideal     Good     
## [30037] Very Good Very Good Very Good Ideal     Ideal     Ideal    
## [30043] Good      Good      Fair      Premium   Very Good Premium  
## [30049] Premium   Very Good Very Good Premium   Premium   Premium  
## [30055] Premium   Premium   Premium   Very Good Ideal     Premium  
## [30061] Premium   Ideal     Ideal     Ideal     Ideal     Ideal    
## [30067] Good      Ideal     Ideal     Premium   Premium   Ideal    
## [30073] Premium   Premium   Ideal     Premium   Very Good Good     
## [30079] Ideal     Premium   Premium   Ideal     Premium   Ideal    
## [30085] Ideal     Ideal     Ideal     Premium   Ideal     Premium  
## [30091] Premium   Ideal     Premium   Ideal     Premium   Premium  
## [30097] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [30103] Very Good Premium   Premium   Premium   Premium   Premium  
## [30109] Premium   Premium   Premium   Ideal     Premium   Ideal    
## [30115] Ideal     Premium   Ideal     Premium   Ideal     Ideal    
## [30121] Premium   Ideal     Ideal     Ideal     Ideal     Premium  
## [30127] Ideal     Premium   Ideal     Premium   Ideal     Premium  
## [30133] Very Good Premium   Premium   Premium   Premium   Premium  
## [30139] Premium   Premium   Premium   Premium   Premium   Very Good
## [30145] Premium   Ideal     Premium   Premium   Premium   Very Good
## [30151] Very Good Premium   Premium   Very Good Premium   Very Good
## [30157] Ideal     Good      Good      Very Good Good      Very Good
## [30163] Good      Very Good Very Good Good      Very Good Very Good
## [30169] Very Good Very Good Very Good Ideal     Ideal     Ideal    
## [30175] Ideal     Ideal     Ideal     Good      Good      Premium  
## [30181] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [30187] Ideal     Fair      Ideal     Premium   Very Good Very Good
## [30193] Very Good Good      Good      Premium   Good      Premium  
## [30199] Premium   Good      Good      Very Good Very Good Ideal    
## [30205] Ideal     Ideal     Ideal     Ideal     Good      Ideal    
## [30211] Ideal     Ideal     Ideal     Premium   Ideal     Premium  
## [30217] Ideal     Ideal     Ideal     Very Good Premium   Premium  
## [30223] Premium   Ideal     Premium   Ideal     Premium   Ideal    
## [30229] Premium   Ideal     Ideal     Ideal     Ideal     Ideal    
## [30235] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [30241] Good      Ideal     Ideal     Ideal     Ideal     Good     
## [30247] Very Good Ideal     Premium   Premium   Premium   Good     
## [30253] Very Good Premium   Very Good Very Good Very Good Very Good
## [30259] Very Good Very Good Ideal     Ideal     Ideal     Ideal    
## [30265] Ideal     Ideal     Good      Very Good Premium   Premium  
## [30271] Good      Very Good Very Good Ideal     Ideal     Ideal    
## [30277] Good      Very Good Very Good Very Good Very Good Very Good
## [30283] Very Good Very Good Premium   Very Good Very Good Very Good
## [30289] Premium   Good      Very Good Good      Good      Ideal    
## [30295] Premium   Very Good Very Good Premium   Ideal     Very Good
## [30301] Premium   Very Good Very Good Very Good Premium   Very Good
## [30307] Very Good Ideal     Ideal     Ideal     Ideal     Ideal    
## [30313] Ideal     Ideal     Premium   Premium   Ideal     Premium  
## [30319] Premium   Very Good Very Good Ideal     Premium   Very Good
## [30325] Ideal     Premium   Ideal     Premium   Premium   Ideal    
## [30331] Premium   Very Good Ideal     Ideal     Premium   Ideal    
## [30337] Premium   Premium   Premium   Ideal     Fair      Fair     
## [30343] Ideal     Premium   Ideal     Premium   Very Good Ideal    
## [30349] Very Good Very Good Ideal     Ideal     Ideal     Ideal    
## [30355] Very Good Very Good Very Good Ideal     Ideal     Ideal    
## [30361] Premium   Premium   Ideal     Premium   Premium   Very Good
## [30367] Premium   Ideal     Ideal     Very Good Ideal     Ideal    
## [30373] Premium   Good      Fair      Very Good Very Good Ideal    
## [30379] Good      Very Good Very Good Very Good Very Good Very Good
## [30385] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [30391] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [30397] Premium   Very Good Ideal     Ideal     Premium   Premium  
## [30403] Ideal     Premium   Ideal     Ideal     Premium   Ideal    
## [30409] Very Good Good      Ideal     Premium   Premium   Ideal    
## [30415] Ideal     Ideal     Premium   Ideal     Ideal     Very Good
## [30421] Ideal     Very Good Ideal     Premium   Ideal     Ideal    
## [30427] Premium   Very Good Ideal     Ideal     Very Good Ideal    
## [30433] Ideal     Ideal     Ideal     Very Good Ideal     Ideal    
## [30439] Ideal     Very Good Ideal     Very Good Premium   Very Good
## [30445] Very Good Very Good Ideal     Ideal     Ideal     Ideal    
## [30451] Ideal     Ideal     Ideal     Good      Ideal     Ideal    
## [30457] Premium   Premium   Very Good Premium   Ideal     Ideal    
## [30463] Ideal     Good      Very Good Premium   Ideal     Very Good
## [30469] Very Good Very Good Very Good Good      Ideal     Ideal    
## [30475] Ideal     Premium   Ideal     Premium   Very Good Premium  
## [30481] Ideal     Premium   Premium   Ideal     Premium   Ideal    
## [30487] Ideal     Ideal     Ideal     Premium   Ideal     Ideal    
## [30493] Premium   Ideal     Ideal     Ideal     Ideal     Premium  
## [30499] Ideal     Ideal     Ideal     Ideal     Premium   Ideal    
## [30505] Premium   Ideal     Good      Ideal     Ideal     Ideal    
## [30511] Premium   Ideal     Very Good Very Good Ideal     Good     
## [30517] Very Good Ideal     Premium   Very Good Ideal     Ideal    
## [30523] Ideal     Ideal     Ideal     Good      Ideal     Very Good
## [30529] Very Good Premium   Premium   Ideal     Ideal     Good     
## [30535] Very Good Good      Very Good Ideal     Very Good Ideal    
## [30541] Premium   Premium   Premium   Ideal     Ideal     Ideal    
## [30547] Ideal     Premium   Ideal     Ideal     Ideal     Premium  
## [30553] Very Good Ideal     Ideal     Premium   Ideal     Ideal    
## [30559] Premium   Ideal     Ideal     Ideal     Ideal     Ideal    
## [30565] Premium   Ideal     Ideal     Ideal     Ideal     Ideal    
## [30571] Ideal     Ideal     Ideal     Ideal     Very Good Ideal    
## [30577] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [30583] Ideal     Ideal     Ideal     Ideal     Premium   Ideal    
## [30589] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [30595] Ideal     Ideal     Premium   Ideal     Ideal     Premium  
## [30601] Very Good Very Good Premium   Ideal     Very Good Very Good
## [30607] Good      Ideal     Very Good Very Good Very Good Good     
## [30613] Good      Very Good Very Good Very Good Very Good Very Good
## [30619] Premium   Very Good Good      Very Good Very Good Premium  
## [30625] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [30631] Ideal     Ideal     Ideal     Very Good Ideal     Ideal    
## [30637] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [30643] Fair      Good      Very Good Ideal     Ideal     Ideal    
## [30649] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [30655] Ideal     Very Good Very Good Ideal     Ideal     Very Good
## [30661] Premium   Ideal     Premium   Premium   Very Good Premium  
## [30667] Very Good Good      Premium   Very Good Very Good Very Good
## [30673] Very Good Premium   Very Good Premium   Premium   Very Good
## [30679] Premium   Ideal     Ideal     Ideal     Very Good Very Good
## [30685] Premium   Ideal     Good      Good      Very Good Premium  
## [30691] Ideal     Premium   Very Good Premium   Very Good Ideal    
## [30697] Premium   Very Good Very Good Very Good Good      Premium  
## [30703] Very Good Very Good Premium   Ideal     Premium   Good     
## [30709] Very Good Ideal     Ideal     Ideal     Ideal     Ideal    
## [30715] Ideal     Ideal     Ideal     Good      Fair      Fair     
## [30721] Good      Very Good Good      Good      Very Good Good     
## [30727] Good      Very Good Very Good Ideal     Ideal     Ideal    
## [30733] Ideal     Good      Good      Good      Premium   Good     
## [30739] Good      Premium   Good      Good      Ideal     Premium  
## [30745] Good      Good      Very Good Very Good Ideal     Ideal    
## [30751] Ideal     Fair      Premium   Premium   Premium   Ideal    
## [30757] Premium   Ideal     Premium   Premium   Premium   Premium  
## [30763] Very Good Ideal     Premium   Premium   Premium   Very Good
## [30769] Very Good Very Good Very Good Ideal     Ideal     Ideal    
## [30775] Ideal     Ideal     Ideal     Ideal     Premium   Ideal    
## [30781] Ideal     Premium   Very Good Very Good Very Good Very Good
## [30787] Very Good Very Good Ideal     Ideal     Ideal     Ideal    
## [30793] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [30799] Ideal     Ideal     Ideal     Ideal     Good      Good     
## [30805] Good      Very Good Very Good Fair      Ideal     Ideal    
## [30811] Ideal     Ideal     Ideal     Ideal     Good      Premium  
## [30817] Ideal     Very Good Ideal     Premium   Good      Very Good
## [30823] Premium   Ideal     Premium   Ideal     Premium   Premium  
## [30829] Ideal     Premium   Premium   Premium   Good      Premium  
## [30835] Ideal     Premium   Premium   Premium   Ideal     Ideal    
## [30841] Premium   Ideal     Ideal     Ideal     Premium   Premium  
## [30847] Ideal     Premium   Premium   Premium   Premium   Ideal    
## [30853] Premium   Ideal     Premium   Premium   Premium   Ideal    
## [30859] Premium   Ideal     Ideal     Ideal     Premium   Premium  
## [30865] Ideal     Premium   Premium   Premium   Ideal     Premium  
## [30871] Ideal     Ideal     Ideal     Premium   Premium   Premium  
## [30877] Premium   Premium   Premium   Premium   Premium   Ideal    
## [30883] Premium   Ideal     Ideal     Ideal     Ideal     Premium  
## [30889] Premium   Very Good Ideal     Very Good Very Good Ideal    
## [30895] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [30901] Good      Ideal     Ideal     Ideal     Ideal     Ideal    
## [30907] Ideal     Ideal     Ideal     Very Good Ideal     Ideal    
## [30913] Ideal     Premium   Ideal     Ideal     Premium   Ideal    
## [30919] Ideal     Ideal     Ideal     Very Good Very Good Premium  
## [30925] Ideal     Ideal     Very Good Premium   Ideal     Premium  
## [30931] Ideal     Ideal     Very Good Very Good Very Good Very Good
## [30937] Very Good Very Good Very Good Very Good Very Good Very Good
## [30943] Very Good Ideal     Ideal     Ideal     Ideal     Very Good
## [30949] Premium   Very Good Very Good Ideal     Very Good Ideal    
## [30955] Very Good Premium   Ideal     Ideal     Ideal     Very Good
## [30961] Very Good Ideal     Ideal     Ideal     Ideal     Premium  
## [30967] Very Good Ideal     Premium   Premium   Premium   Ideal    
## [30973] Very Good Very Good Very Good Premium   Ideal     Premium  
## [30979] Premium   Very Good Premium   Good      Premium   Good     
## [30985] Premium   Very Good Ideal     Very Good Very Good Premium  
## [30991] Ideal     Premium   Premium   Premium   Ideal     Ideal    
## [30997] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [31003] Very Good Good      Ideal     Ideal     Ideal     Ideal    
## [31009] Premium   Ideal     Premium   Premium   Ideal     Good     
## [31015] Very Good Very Good Very Good Ideal     Good      Ideal    
## [31021] Premium   Good      Premium   Premium   Ideal     Premium  
## [31027] Premium   Ideal     Ideal     Premium   Ideal     Ideal    
## [31033] Very Good Premium   Good      Good      Premium   Very Good
## [31039] Ideal     Ideal     Ideal     Very Good Ideal     Ideal    
## [31045] Good      Ideal     Premium   Ideal     Very Good Ideal    
## [31051] Ideal     Ideal     Ideal     Ideal     Ideal     Very Good
## [31057] Very Good Ideal     Ideal     Ideal     Good      Premium  
## [31063] Ideal     Good      Ideal     Ideal     Very Good Premium  
## [31069] Ideal     Premium   Premium   Ideal     Ideal     Premium  
## [31075] Good      Very Good Very Good Premium   Ideal     Very Good
## [31081] Very Good Premium   Very Good Ideal     Ideal     Premium  
## [31087] Ideal     Good      Very Good Ideal     Very Good Very Good
## [31093] Premium   Ideal     Very Good Very Good Very Good Very Good
## [31099] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [31105] Ideal     Ideal     Premium   Premium   Very Good Very Good
## [31111] Very Good Very Good Very Good Very Good Very Good Ideal    
## [31117] Ideal     Ideal     Ideal     Ideal     Good      Good     
## [31123] Ideal     Very Good Very Good Very Good Very Good Ideal    
## [31129] Very Good Very Good Very Good Ideal     Ideal     Ideal    
## [31135] Ideal     Ideal     Ideal     Very Good Very Good Very Good
## [31141] Very Good Very Good Very Good Very Good Ideal     Ideal    
## [31147] Good      Very Good Good      Premium   Ideal     Good     
## [31153] Premium   Premium   Ideal     Premium   Good      Ideal    
## [31159] Ideal     Very Good Very Good Good      Very Good Very Good
## [31165] Ideal     Premium   Good      Very Good Good      Premium  
## [31171] Premium   Premium   Good      Very Good Ideal     Ideal    
## [31177] Ideal     Ideal     Good      Good      Premium   Ideal    
## [31183] Premium   Ideal     Ideal     Premium   Ideal     Ideal    
## [31189] Very Good Good      Ideal     Very Good Premium   Premium  
## [31195] Premium   Ideal     Ideal     Ideal     Ideal     Premium  
## [31201] Ideal     Premium   Ideal     Premium   Premium   Ideal    
## [31207] Ideal     Premium   Premium   Ideal     Ideal     Ideal    
## [31213] Premium   Premium   Premium   Ideal     Ideal     Very Good
## [31219] Very Good Ideal     Ideal     Ideal     Very Good Very Good
## [31225] Good      Very Good Premium   Ideal     Good      Ideal    
## [31231] Fair      Good      Good      Very Good Good      Very Good
## [31237] Very Good Ideal     Very Good Ideal     Ideal     Ideal    
## [31243] Ideal     Ideal     Fair      Premium   Very Good Premium  
## [31249] Ideal     Ideal     Good      Ideal     Ideal     Ideal    
## [31255] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [31261] Good      Very Good Ideal     Premium   Good      Very Good
## [31267] Very Good Ideal     Ideal     Very Good Ideal     Very Good
## [31273] Good      Ideal     Premium   Very Good Ideal     Premium  
## [31279] Good      Ideal     Premium   Very Good Ideal     Ideal    
## [31285] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [31291] Ideal     Ideal     Ideal     Ideal     Very Good Ideal    
## [31297] Premium   Ideal     Ideal     Ideal     Ideal     Very Good
## [31303] Very Good Ideal     Ideal     Ideal     Ideal     Ideal    
## [31309] Ideal     Premium   Ideal     Ideal     Ideal     Ideal    
## [31315] Ideal     Premium   Ideal     Ideal     Ideal     Ideal    
## [31321] Premium   Ideal     Ideal     Ideal     Ideal     Ideal    
## [31327] Ideal     Very Good Very Good Ideal     Ideal     Ideal    
## [31333] Ideal     Ideal     Ideal     Ideal     Premium   Premium  
## [31339] Premium   Very Good Very Good Very Good Very Good Ideal    
## [31345] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [31351] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [31357] Ideal     Ideal     Good      Ideal     Ideal     Very Good
## [31363] Very Good Ideal     Ideal     Ideal     Ideal     Ideal    
## [31369] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [31375] Ideal     Good      Very Good Ideal     Premium   Very Good
## [31381] Ideal     Ideal     Ideal     Premium   Very Good Very Good
## [31387] Very Good Ideal     Ideal     Ideal     Ideal     Good     
## [31393] Ideal     Premium   Very Good Very Good Very Good Ideal    
## [31399] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [31405] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [31411] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [31417] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [31423] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [31429] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [31435] Ideal     Ideal     Ideal     Ideal     Good      Very Good
## [31441] Ideal     Ideal     Ideal     Ideal     Premium   Good     
## [31447] Very Good Ideal     Premium   Very Good Ideal     Ideal    
## [31453] Ideal     Ideal     Premium   Premium   Good      Ideal    
## [31459] Premium   Premium   Good      Very Good Very Good Premium  
## [31465] Very Good Very Good Premium   Premium   Premium   Premium  
## [31471] Premium   Ideal     Ideal     Premium   Premium   Ideal    
## [31477] Premium   Ideal     Ideal     Premium   Very Good Premium  
## [31483] Premium   Ideal     Premium   Premium   Premium   Ideal    
## [31489] Ideal     Ideal     Ideal     Premium   Premium   Ideal    
## [31495] Ideal     Ideal     Ideal     Ideal     Premium   Ideal    
## [31501] Ideal     Premium   Ideal     Premium   Ideal     Ideal    
## [31507] Premium   Premium   Very Good Premium   Ideal     Premium  
## [31513] Premium   Ideal     Ideal     Premium   Ideal     Ideal    
## [31519] Premium   Ideal     Ideal     Ideal     Ideal     Very Good
## [31525] Premium   Very Good Ideal     Premium   Ideal     Ideal    
## [31531] Ideal     Ideal     Ideal     Premium   Very Good Ideal    
## [31537] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [31543] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [31549] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [31555] Ideal     Ideal     Very Good Very Good Very Good Premium  
## [31561] Very Good Very Good Premium   Ideal     Ideal     Ideal    
## [31567] Ideal     Premium   Ideal     Premium   Very Good Very Good
## [31573] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [31579] Ideal     Ideal     Premium   Ideal     Ideal     Very Good
## [31585] Ideal     Ideal     Premium   Ideal     Premium   Very Good
## [31591] Ideal     Premium   Premium   Premium   Premium   Ideal    
## [31597] Premium   Ideal     Very Good Ideal     Premium   Premium  
## [31603] Ideal     Premium   Good      Very Good Good      Premium  
## [31609] Premium   Very Good Very Good Fair      Very Good Ideal    
## [31615] Ideal     Fair      Ideal     Very Good Very Good Very Good
## [31621] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [31627] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [31633] Ideal     Ideal     Ideal     Very Good Premium   Very Good
## [31639] Good      Premium   Very Good Ideal     Premium   Very Good
## [31645] Ideal     Ideal     Ideal     Ideal     Very Good Very Good
## [31651] Ideal     Premium   Premium   Premium   Premium   Ideal    
## [31657] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [31663] Ideal     Ideal     Ideal     Ideal     Premium   Ideal    
## [31669] Very Good Premium   Ideal     Very Good Ideal     Ideal    
## [31675] Ideal     Ideal     Ideal     Premium   Premium   Ideal    
## [31681] Ideal     Ideal     Ideal     Ideal     Premium   Premium  
## [31687] Ideal     Ideal     Very Good Very Good Very Good Ideal    
## [31693] Ideal     Ideal     Premium   Ideal     Ideal     Premium  
## [31699] Ideal     Premium   Premium   Premium   Premium   Premium  
## [31705] Ideal     Premium   Premium   Premium   Premium   Very Good
## [31711] Premium   Ideal     Ideal     Ideal     Ideal     Ideal    
## [31717] Ideal     Ideal     Good      Very Good Fair      Very Good
## [31723] Very Good Very Good Very Good Very Good Very Good Ideal    
## [31729] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [31735] Ideal     Ideal     Ideal     Good      Premium   Very Good
## [31741] Ideal     Premium   Premium   Ideal     Ideal     Premium  
## [31747] Very Good Very Good Premium   Ideal     Premium   Premium  
## [31753] Ideal     Premium   Premium   Ideal     Premium   Premium  
## [31759] Ideal     Ideal     Premium   Premium   Ideal     Ideal    
## [31765] Ideal     Ideal     Premium   Ideal     Premium   Ideal    
## [31771] Premium   Premium   Premium   Very Good Ideal     Ideal    
## [31777] Very Good Premium   Very Good Very Good Very Good Very Good
## [31783] Ideal     Ideal     Premium   Ideal     Ideal     Very Good
## [31789] Ideal     Very Good Premium   Good      Premium   Premium  
## [31795] Ideal     Ideal     Very Good Premium   Ideal     Ideal    
## [31801] Ideal     Very Good Very Good Very Good Very Good Very Good
## [31807] Ideal     Ideal     Ideal     Very Good Good      Ideal    
## [31813] Good      Premium   Premium   Very Good Ideal     Ideal    
## [31819] Ideal     Ideal     Ideal     Premium   Good      Premium  
## [31825] Ideal     Premium   Very Good Ideal     Good      Very Good
## [31831] Ideal     Premium   Ideal     Ideal     Very Good Very Good
## [31837] Good      Ideal     Very Good Ideal     Premium   Ideal    
## [31843] Very Good Ideal     Premium   Premium   Ideal     Ideal    
## [31849] Ideal     Ideal     Good      Premium   Ideal     Ideal    
## [31855] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [31861] Ideal     Ideal     Premium   Premium   Premium   Premium  
## [31867] Ideal     Premium   Good      Premium   Premium   Premium  
## [31873] Ideal     Very Good Good      Ideal     Very Good Ideal    
## [31879] Very Good Ideal     Premium   Good      Premium   Ideal    
## [31885] Premium   Premium   Premium   Premium   Ideal     Ideal    
## [31891] Ideal     Ideal     Ideal     Premium   Premium   Premium  
## [31897] Ideal     Premium   Premium   Ideal     Premium   Good     
## [31903] Premium   Ideal     Premium   Ideal     Premium   Premium  
## [31909] Ideal     Ideal     Ideal     Premium   Good      Very Good
## [31915] Premium   Premium   Premium   Very Good Good      Ideal    
## [31921] Very Good Very Good Premium   Premium   Ideal     Very Good
## [31927] Good      Premium   Premium   Ideal     Ideal     Ideal    
## [31933] Premium   Ideal     Ideal     Premium   Ideal     Ideal    
## [31939] Ideal     Premium   Ideal     Ideal     Ideal     Ideal    
## [31945] Premium   Ideal     Premium   Ideal     Premium   Ideal    
## [31951] Very Good Premium   Ideal     Ideal     Ideal     Ideal    
## [31957] Ideal     Ideal     Ideal     Ideal     Ideal     Good     
## [31963] Premium   Very Good Ideal     Very Good Ideal     Premium  
## [31969] Premium   Ideal     Ideal     Premium   Very Good Very Good
## [31975] Very Good Very Good Very Good Very Good Very Good Very Good
## [31981] Premium   Premium   Ideal     Ideal     Ideal     Premium  
## [31987] Premium   Ideal     Ideal     Ideal     Premium   Premium  
## [31993] Premium   Premium   Premium   Fair      Ideal     Premium  
## [31999] Ideal     Ideal     Very Good Very Good Ideal     Ideal    
## [32005] Ideal     Ideal     Good      Ideal     Ideal     Very Good
## [32011] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [32017] Premium   Very Good Very Good Very Good Very Good Very Good
## [32023] Very Good Ideal     Ideal     Ideal     Ideal     Ideal    
## [32029] Ideal     Premium   Premium   Ideal     Premium   Very Good
## [32035] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [32041] Ideal     Ideal     Ideal     Ideal     Ideal     Good     
## [32047] Very Good Very Good Very Good Ideal     Ideal     Ideal    
## [32053] Ideal     Ideal     Good      Premium   Ideal     Premium  
## [32059] Premium   Premium   Premium   Premium   Ideal     Ideal    
## [32065] Premium   Ideal     Ideal     Premium   Ideal     Ideal    
## [32071] Premium   Ideal     Good      Very Good Ideal     Ideal    
## [32077] Ideal     Ideal     Ideal     Premium   Very Good Very Good
## [32083] Ideal     Ideal     Ideal     Ideal     Premium   Ideal    
## [32089] Ideal     Ideal     Premium   Ideal     Ideal     Ideal    
## [32095] Ideal     Premium   Ideal     Ideal     Ideal     Ideal    
## [32101] Ideal     Ideal     Ideal     Premium   Ideal     Ideal    
## [32107] Very Good Ideal     Ideal     Ideal     Ideal     Very Good
## [32113] Ideal     Ideal     Ideal     Ideal     Good      Very Good
## [32119] Very Good Very Good Very Good Ideal     Ideal     Ideal    
## [32125] Good      Good      Premium   Very Good Good      Very Good
## [32131] Good      Premium   Premium   Ideal     Very Good Ideal    
## [32137] Ideal     Ideal     Premium   Good      Ideal     Good     
## [32143] Ideal     Ideal     Ideal     Very Good Ideal     Good     
## [32149] Very Good Very Good Ideal     Ideal     Ideal     Ideal    
## [32155] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [32161] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [32167] Ideal     Ideal     Good      Premium   Premium   Very Good
## [32173] Very Good Good      Very Good Good      Very Good Premium  
## [32179] Good      Very Good Very Good Premium   Good      Very Good
## [32185] Very Good Ideal     Ideal     Ideal     Ideal     Ideal    
## [32191] Premium   Premium   Premium   Premium   Ideal     Premium  
## [32197] Ideal     Premium   Ideal     Premium   Ideal     Premium  
## [32203] Premium   Premium   Premium   Premium   Premium   Very Good
## [32209] Ideal     Good      Premium   Ideal     Ideal     Ideal    
## [32215] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [32221] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [32227] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [32233] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [32239] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [32245] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [32251] Ideal     Very Good Premium   Premium   Ideal     Ideal    
## [32257] Ideal     Very Good Ideal     Ideal     Very Good Very Good
## [32263] Ideal     Very Good Ideal     Ideal     Good      Ideal    
## [32269] Ideal     Ideal     Ideal     Very Good Very Good Very Good
## [32275] Premium   Premium   Ideal     Ideal     Very Good Ideal    
## [32281] Very Good Very Good Very Good Very Good Ideal     Ideal    
## [32287] Ideal     Ideal     Ideal     Good      Premium   Premium  
## [32293] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [32299] Ideal     Ideal     Ideal     Premium   Premium   Good     
## [32305] Very Good Ideal     Ideal     Ideal     Ideal     Ideal    
## [32311] Very Good Very Good Ideal     Premium   Ideal     Ideal    
## [32317] Premium   Premium   Good      Good      Premium   Premium  
## [32323] Very Good Very Good Ideal     Good      Premium   Good     
## [32329] Ideal     Ideal     Ideal     Premium   Ideal     Very Good
## [32335] Very Good Ideal     Ideal     Very Good Premium   Good     
## [32341] Very Good Very Good Ideal     Ideal     Very Good Ideal    
## [32347] Ideal     Very Good Ideal     Ideal     Premium   Ideal    
## [32353] Premium   Premium   Ideal     Premium   Very Good Good     
## [32359] Premium   Premium   Premium   Ideal     Very Good Very Good
## [32365] Very Good Ideal     Ideal     Ideal     Ideal     Ideal    
## [32371] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [32377] Ideal     Fair      Very Good Very Good Premium   Premium  
## [32383] Ideal     Ideal     Ideal     Ideal     Very Good Very Good
## [32389] Very Good Very Good Very Good Very Good Very Good Ideal    
## [32395] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [32401] Premium   Premium   Premium   Premium   Premium   Premium  
## [32407] Ideal     Ideal     Ideal     Premium   Ideal     Ideal    
## [32413] Ideal     Good      Ideal     Ideal     Premium   Ideal    
## [32419] Very Good Ideal     Premium   Ideal     Premium   Premium  
## [32425] Very Good Very Good Ideal     Ideal     Ideal     Ideal    
## [32431] Ideal     Ideal     Premium   Ideal     Ideal     Premium  
## [32437] Ideal     Ideal     Very Good Premium   Very Good Premium  
## [32443] Very Good Premium   Very Good Very Good Very Good Very Good
## [32449] Very Good Ideal     Very Good Very Good Premium   Very Good
## [32455] Premium   Premium   Very Good Ideal     Ideal     Ideal    
## [32461] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [32467] Ideal     Very Good Ideal     Fair      Fair      Premium  
## [32473] Very Good Very Good Very Good Very Good Very Good Very Good
## [32479] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [32485] Ideal     Ideal     Premium   Very Good Ideal     Ideal    
## [32491] Ideal     Ideal     Ideal     Ideal     Ideal     Good     
## [32497] Premium   Premium   Very Good Premium   Premium   Very Good
## [32503] Very Good Very Good Ideal     Ideal     Fair      Very Good
## [32509] Premium   Good      Very Good Good      Ideal     Very Good
## [32515] Good      Premium   Good      Very Good Very Good Ideal    
## [32521] Premium   Ideal     Very Good Ideal     Ideal     Ideal    
## [32527] Very Good Premium   Premium   Very Good Very Good Very Good
## [32533] Ideal     Premium   Very Good Ideal     Premium   Ideal    
## [32539] Ideal     Premium   Ideal     Ideal     Ideal     Ideal    
## [32545] Ideal     Ideal     Ideal     Premium   Ideal     Ideal    
## [32551] Fair      Premium   Ideal     Very Good Ideal     Ideal    
## [32557] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [32563] Ideal     Ideal     Ideal     Ideal     Very Good Premium  
## [32569] Ideal     Very Good Ideal     Very Good Premium   Ideal    
## [32575] Very Good Very Good Very Good Ideal     Ideal     Ideal    
## [32581] Good      Good      Premium   Ideal     Ideal     Ideal    
## [32587] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [32593] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [32599] Ideal     Ideal     Ideal     Premium   Ideal     Premium  
## [32605] Premium   Ideal     Premium   Ideal     Premium   Ideal    
## [32611] Ideal     Ideal     Ideal     Good      Very Good Very Good
## [32617] Very Good Ideal     Ideal     Ideal     Ideal     Ideal    
## [32623] Ideal     Good      Very Good Very Good Very Good Very Good
## [32629] Very Good Good      Good      Good      Very Good Ideal    
## [32635] Ideal     Ideal     Ideal     Ideal     Ideal     Very Good
## [32641] Ideal     Ideal     Premium   Premium   Ideal     Premium  
## [32647] Ideal     Ideal     Ideal     Premium   Ideal     Ideal    
## [32653] Ideal     Premium   Premium   Premium   Good      Good     
## [32659] Very Good Premium   Ideal     Premium   Premium   Premium  
## [32665] Premium   Ideal     Ideal     Premium   Ideal     Premium  
## [32671] Premium   Ideal     Ideal     Ideal     Ideal     Premium  
## [32677] Premium   Premium   Premium   Premium   Premium   Premium  
## [32683] Premium   Premium   Ideal     Premium   Very Good Very Good
## [32689] Very Good Good      Very Good Premium   Good      Very Good
## [32695] Premium   Ideal     Premium   Premium   Ideal     Ideal    
## [32701] Ideal     Ideal     Ideal     Ideal     Premium   Premium  
## [32707] Ideal     Premium   Premium   Ideal     Ideal     Ideal    
## [32713] Very Good Ideal     Premium   Premium   Premium   Premium  
## [32719] Ideal     Premium   Ideal     Premium   Ideal     Premium  
## [32725] Ideal     Ideal     Premium   Ideal     Premium   Ideal    
## [32731] Ideal     Premium   Very Good Good      Premium   Premium  
## [32737] Ideal     Ideal     Premium   Ideal     Ideal     Premium  
## [32743] Premium   Ideal     Ideal     Ideal     Ideal     Ideal    
## [32749] Ideal     Ideal     Ideal     Premium   Premium   Premium  
## [32755] Very Good Ideal     Premium   Premium   Premium   Premium  
## [32761] Premium   Premium   Premium   Ideal     Ideal     Ideal    
## [32767] Ideal     Premium   Premium   Ideal     Ideal     Ideal    
## [32773] Ideal     Premium   Ideal     Premium   Premium   Premium  
## [32779] Premium   Premium   Ideal     Premium   Ideal     Ideal    
## [32785] Ideal     Very Good Very Good Ideal     Ideal     Ideal    
## [32791] Ideal     Premium   Ideal     Ideal     Ideal     Ideal    
## [32797] Very Good Ideal     Ideal     Ideal     Ideal     Very Good
## [32803] Ideal     Ideal     Ideal     Ideal     Premium   Ideal    
## [32809] Ideal     Ideal     Ideal     Ideal     Ideal     Very Good
## [32815] Ideal     Ideal     Ideal     Ideal     Premium   Premium  
## [32821] Ideal     Ideal     Ideal     Ideal     Premium   Ideal    
## [32827] Ideal     Very Good Ideal     Ideal     Ideal     Ideal    
## [32833] Premium   Premium   Ideal     Ideal     Ideal     Ideal    
## [32839] Ideal     Ideal     Premium   Premium   Ideal     Ideal    
## [32845] Ideal     Ideal     Very Good Ideal     Very Good Premium  
## [32851] Good      Ideal     Premium   Good      Very Good Good     
## [32857] Very Good Ideal     Premium   Very Good Very Good Ideal    
## [32863] Ideal     Ideal     Ideal     Very Good Very Good Ideal    
## [32869] Ideal     Ideal     Ideal     Ideal     Good      Ideal    
## [32875] Ideal     Ideal     Ideal     Ideal     Premium   Premium  
## [32881] Very Good Ideal     Premium   Premium   Premium   Very Good
## [32887] Premium   Ideal     Premium   Premium   Premium   Ideal    
## [32893] Ideal     Ideal     Premium   Very Good Ideal     Ideal    
## [32899] Very Good Premium   Very Good Fair      Premium   Ideal    
## [32905] Premium   Ideal     Ideal     Ideal     Ideal     Ideal    
## [32911] Very Good Premium   Ideal     Very Good Very Good Ideal    
## [32917] Ideal     Ideal     Ideal     Very Good Very Good Ideal    
## [32923] Ideal     Ideal     Premium   Ideal     Ideal     Premium  
## [32929] Premium   Premium   Very Good Ideal     Very Good Ideal    
## [32935] Ideal     Premium   Premium   Ideal     Premium   Ideal    
## [32941] Ideal     Ideal     Very Good Premium   Ideal     Ideal    
## [32947] Very Good Ideal     Very Good Ideal     Ideal     Good     
## [32953] Very Good Premium   Very Good Very Good Very Good Very Good
## [32959] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [32965] Premium   Premium   Very Good Good      Good      Very Good
## [32971] Very Good Very Good Very Good Ideal     Ideal     Very Good
## [32977] Ideal     Ideal     Ideal     Very Good Ideal     Ideal    
## [32983] Ideal     Very Good Premium   Ideal     Premium   Premium  
## [32989] Ideal     Premium   Premium   Premium   Premium   Premium  
## [32995] Premium   Premium   Premium   Very Good Premium   Ideal    
## [33001] Very Good Ideal     Ideal     Ideal     Ideal     Ideal    
## [33007] Ideal     Fair      Premium   Very Good Good      Very Good
## [33013] Very Good Very Good Very Good Good      Fair      Ideal    
## [33019] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [33025] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [33031] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [33037] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [33043] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [33049] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [33055] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [33061] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [33067] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [33073] Ideal     Ideal     Good      Ideal     Ideal     Premium  
## [33079] Premium   Premium   Premium   Premium   Premium   Ideal    
## [33085] Good      Ideal     Very Good Very Good Ideal     Ideal    
## [33091] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [33097] Ideal     Ideal     Ideal     Ideal     Premium   Very Good
## [33103] Good      Ideal     Ideal     Ideal     Ideal     Ideal    
## [33109] Good      Premium   Premium   Premium   Premium   Premium  
## [33115] Fair      Ideal     Ideal     Ideal     Ideal     Ideal    
## [33121] Premium   Ideal     Ideal     Very Good Very Good Very Good
## [33127] Good      Ideal     Premium   Ideal     Very Good Ideal    
## [33133] Premium   Ideal     Ideal     Premium   Premium   Ideal    
## [33139] Very Good Premium   Good      Ideal     Ideal     Premium  
## [33145] Premium   Good      Ideal     Ideal     Very Good Premium  
## [33151] Premium   Premium   Ideal     Premium   Premium   Very Good
## [33157] Very Good Ideal     Ideal     Premium   Ideal     Premium  
## [33163] Ideal     Very Good Very Good Ideal     Ideal     Ideal    
## [33169] Ideal     Ideal     Premium   Very Good Very Good Ideal    
## [33175] Premium   Ideal     Very Good Premium   Very Good Very Good
## [33181] Very Good Good      Premium   Very Good Fair      Very Good
## [33187] Good      Very Good Very Good Very Good Very Good Ideal    
## [33193] Premium   Very Good Premium   Ideal     Very Good Ideal    
## [33199] Ideal     Premium   Fair      Ideal     Very Good Ideal    
## [33205] Ideal     Ideal     Ideal     Ideal     Ideal     Good     
## [33211] Premium   Very Good Ideal     Ideal     Ideal     Ideal    
## [33217] Premium   Premium   Very Good Ideal     Very Good Ideal    
## [33223] Premium   Very Good Very Good Ideal     Very Good Very Good
## [33229] Ideal     Ideal     Very Good Premium   Ideal     Good     
## [33235] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [33241] Ideal     Ideal     Ideal     Ideal     Very Good Very Good
## [33247] Very Good Very Good Ideal     Ideal     Ideal     Ideal    
## [33253] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [33259] Good      Good      Premium   Ideal     Ideal     Ideal    
## [33265] Ideal     Ideal     Ideal     Ideal     Ideal     Good     
## [33271] Premium   Good      Very Good Good      Very Good Ideal    
## [33277] Ideal     Good      Good      Good      Very Good Premium  
## [33283] Good      Very Good Very Good Very Good Good      Very Good
## [33289] Ideal     Premium   Very Good Very Good Premium   Premium  
## [33295] Very Good Premium   Good      Ideal     Good      Ideal    
## [33301] Good      Premium   Good      Very Good Very Good Ideal    
## [33307] Ideal     Good      Good      Good      Very Good Ideal    
## [33313] Ideal     Premium   Ideal     Premium   Premium   Premium  
## [33319] Ideal     Ideal     Premium   Ideal     Premium   Ideal    
## [33325] Ideal     Ideal     Ideal     Ideal     Ideal     Fair     
## [33331] Ideal     Very Good Good      Very Good Ideal     Premium  
## [33337] Premium   Ideal     Ideal     Premium   Premium   Very Good
## [33343] Premium   Ideal     Premium   Premium   Ideal     Premium  
## [33349] Premium   Ideal     Premium   Ideal     Premium   Ideal    
## [33355] Very Good Premium   Premium   Good      Ideal     Good     
## [33361] Very Good Very Good Ideal     Ideal     Ideal     Premium  
## [33367] Ideal     Very Good Very Good Very Good Ideal     Premium  
## [33373] Very Good Premium   Ideal     Ideal     Good      Ideal    
## [33379] Premium   Ideal     Premium   Premium   Good      Ideal    
## [33385] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [33391] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [33397] Ideal     Ideal     Ideal     Ideal     Ideal     Fair     
## [33403] Ideal     Premium   Ideal     Ideal     Ideal     Ideal    
## [33409] Premium   Premium   Premium   Ideal     Good      Premium  
## [33415] Premium   Premium   Premium   Very Good Premium   Ideal    
## [33421] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [33427] Premium   Ideal     Ideal     Ideal     Ideal     Premium  
## [33433] Premium   Premium   Ideal     Ideal     Ideal     Premium  
## [33439] Premium   Ideal     Ideal     Ideal     Ideal     Ideal    
## [33445] Ideal     Premium   Ideal     Ideal     Ideal     Ideal    
## [33451] Ideal     Premium   Premium   Premium   Ideal     Ideal    
## [33457] Very Good Ideal     Ideal     Premium   Premium   Premium  
## [33463] Premium   Ideal     Premium   Premium   Ideal     Premium  
## [33469] Ideal     Premium   Ideal     Ideal     Ideal     Ideal    
## [33475] Premium   Ideal     Ideal     Premium   Ideal     Ideal    
## [33481] Premium   Ideal     Premium   Premium   Premium   Ideal    
## [33487] Premium   Ideal     Premium   Ideal     Ideal     Ideal    
## [33493] Ideal     Premium   Premium   Ideal     Ideal     Ideal    
## [33499] Ideal     Ideal     Ideal     Very Good Very Good Ideal    
## [33505] Ideal     Premium   Ideal     Premium   Very Good Very Good
## [33511] Ideal     Ideal     Ideal     Premium   Very Good Very Good
## [33517] Premium   Ideal     Ideal     Ideal     Ideal     Ideal    
## [33523] Ideal     Fair      Ideal     Ideal     Ideal     Ideal    
## [33529] Premium   Very Good Ideal     Ideal     Premium   Ideal    
## [33535] Ideal     Premium   Ideal     Ideal     Ideal     Premium  
## [33541] Ideal     Ideal     Ideal     Ideal     Ideal     Good     
## [33547] Ideal     Premium   Ideal     Very Good Ideal     Ideal    
## [33553] Ideal     Ideal     Fair      Ideal     Premium   Premium  
## [33559] Premium   Ideal     Ideal     Premium   Good      Premium  
## [33565] Premium   Good      Premium   Ideal     Premium   Premium  
## [33571] Very Good Ideal     Very Good Very Good Very Good Very Good
## [33577] Very Good Ideal     Ideal     Ideal     Ideal     Ideal    
## [33583] Premium   Ideal     Very Good Very Good Premium   Ideal    
## [33589] Fair      Ideal     Premium   Premium   Premium   Premium  
## [33595] Premium   Premium   Ideal     Ideal     Premium   Ideal    
## [33601] Very Good Very Good Very Good Very Good Ideal     Good     
## [33607] Very Good Ideal     Ideal     Ideal     Good      Very Good
## [33613] Good      Very Good Good      Premium   Premium   Premium  
## [33619] Good      Very Good Very Good Ideal     Ideal     Ideal    
## [33625] Ideal     Ideal     Very Good Premium   Premium   Very Good
## [33631] Premium   Premium   Premium   Premium   Ideal     Premium  
## [33637] Ideal     Premium   Premium   Premium   Premium   Ideal    
## [33643] Premium   Ideal     Very Good Ideal     Very Good Very Good
## [33649] Very Good Ideal     Ideal     Ideal     Premium   Premium  
## [33655] Very Good Very Good Ideal     Ideal     Very Good Very Good
## [33661] Very Good Ideal     Ideal     Ideal     Premium   Very Good
## [33667] Very Good Very Good Ideal     Ideal     Good      Ideal    
## [33673] Premium   Very Good Ideal     Ideal     Ideal     Ideal    
## [33679] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [33685] Very Good Ideal     Very Good Very Good Very Good Ideal    
## [33691] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [33697] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [33703] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [33709] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [33715] Ideal     Ideal     Ideal     Ideal     Ideal     Good     
## [33721] Premium   Premium   Very Good Very Good Premium   Ideal    
## [33727] Premium   Very Good Ideal     Ideal     Ideal     Ideal    
## [33733] Ideal     Ideal     Ideal     Very Good Ideal     Very Good
## [33739] Very Good Ideal     Ideal     Ideal     Ideal     Ideal    
## [33745] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [33751] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [33757] Ideal     Ideal     Fair      Very Good Premium   Very Good
## [33763] Very Good Very Good Ideal     Premium   Premium   Very Good
## [33769] Very Good Ideal     Ideal     Ideal     Ideal     Ideal    
## [33775] Ideal     Very Good Very Good Ideal     Ideal     Premium  
## [33781] Ideal     Very Good Ideal     Very Good Ideal     Premium  
## [33787] Premium   Premium   Ideal     Premium   Ideal     Premium  
## [33793] Premium   Premium   Ideal     Premium   Ideal     Ideal    
## [33799] Ideal     Premium   Very Good Ideal     Ideal     Premium  
## [33805] Very Good Premium   Good      Ideal     Ideal     Premium  
## [33811] Ideal     Premium   Premium   Ideal     Ideal     Good     
## [33817] Premium   Good      Premium   Ideal     Ideal     Very Good
## [33823] Ideal     Ideal     Ideal     Very Good Very Good Ideal    
## [33829] Ideal     Ideal     Ideal     Ideal     Very Good Very Good
## [33835] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [33841] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [33847] Ideal     Premium   Premium   Premium   Ideal     Premium  
## [33853] Ideal     Premium   Premium   Premium   Premium   Premium  
## [33859] Ideal     Ideal     Premium   Premium   Ideal     Premium  
## [33865] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [33871] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [33877] Ideal     Premium   Ideal     Premium   Ideal     Ideal    
## [33883] Premium   Ideal     Ideal     Ideal     Ideal     Ideal    
## [33889] Ideal     Ideal     Ideal     Premium   Ideal     Premium  
## [33895] Ideal     Premium   Ideal     Ideal     Ideal     Ideal    
## [33901] Ideal     Ideal     Premium   Premium   Premium   Premium  
## [33907] Premium   Ideal     Ideal     Premium   Ideal     Premium  
## [33913] Premium   Premium   Ideal     Premium   Ideal     Premium  
## [33919] Premium   Good      Ideal     Premium   Ideal     Ideal    
## [33925] Ideal     Ideal     Very Good Premium   Very Good Good     
## [33931] Very Good Ideal     Ideal     Premium   Very Good Premium  
## [33937] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [33943] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [33949] Ideal     Very Good Very Good Very Good Very Good Very Good
## [33955] Very Good Very Good Very Good Very Good Very Good Ideal    
## [33961] Very Good Very Good Very Good Premium   Very Good Ideal    
## [33967] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [33973] Ideal     Good      Ideal     Premium   Premium   Very Good
## [33979] Very Good Very Good Very Good Good      Good      Premium  
## [33985] Premium   Ideal     Ideal     Premium   Very Good Ideal    
## [33991] Premium   Ideal     Ideal     Ideal     Premium   Ideal    
## [33997] Premium   Ideal     Premium   Ideal     Ideal     Very Good
## [34003] Very Good Ideal     Ideal     Premium   Premium   Ideal    
## [34009] Premium   Very Good Ideal     Ideal     Ideal     Good     
## [34015] Premium   Ideal     Premium   Ideal     Very Good Ideal    
## [34021] Ideal     Ideal     Ideal     Ideal     Good      Very Good
## [34027] Premium   Very Good Very Good Very Good Very Good Very Good
## [34033] Very Good Very Good Very Good Ideal     Ideal     Ideal    
## [34039] Ideal     Ideal     Ideal     Ideal     Ideal     Very Good
## [34045] Ideal     Ideal     Ideal     Good      Very Good Premium  
## [34051] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [34057] Premium   Premium   Ideal     Ideal     Ideal     Ideal    
## [34063] Ideal     Ideal     Ideal     Very Good Premium   Premium  
## [34069] Premium   Fair      Premium   Ideal     Very Good Very Good
## [34075] Very Good Ideal     Ideal     Ideal     Ideal     Ideal    
## [34081] Ideal     Premium   Ideal     Ideal     Ideal     Ideal    
## [34087] Premium   Premium   Ideal     Very Good Premium   Ideal    
## [34093] Ideal     Ideal     Ideal     Ideal     Premium   Premium  
## [34099] Premium   Premium   Very Good Ideal     Ideal     Ideal    
## [34105] Very Good Very Good Ideal     Ideal     Ideal     Good     
## [34111] Premium   Ideal     Ideal     Ideal     Ideal     Ideal    
## [34117] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [34123] Ideal     Ideal     Ideal     Premium   Very Good Very Good
## [34129] Very Good Ideal     Ideal     Ideal     Ideal     Ideal    
## [34135] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [34141] Ideal     Ideal     Premium   Very Good Ideal     Ideal    
## [34147] Ideal     Ideal     Ideal     Ideal     Premium   Premium  
## [34153] Premium   Ideal     Ideal     Ideal     Premium   Premium  
## [34159] Ideal     Ideal     Premium   Premium   Ideal     Premium  
## [34165] Ideal     Ideal     Ideal     Ideal     Premium   Premium  
## [34171] Ideal     Ideal     Premium   Premium   Premium   Premium  
## [34177] Premium   Ideal     Ideal     Ideal     Ideal     Premium  
## [34183] Premium   Premium   Premium   Very Good Premium   Ideal    
## [34189] Ideal     Premium   Ideal     Ideal     Premium   Premium  
## [34195] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [34201] Ideal     Ideal     Ideal     Premium   Premium   Premium  
## [34207] Premium   Premium   Premium   Premium   Ideal     Ideal    
## [34213] Ideal     Very Good Premium   Very Good Ideal     Good     
## [34219] Ideal     Premium   Ideal     Premium   Premium   Premium  
## [34225] Premium   Good      Premium   Premium   Premium   Premium  
## [34231] Premium   Ideal     Very Good Premium   Premium   Premium  
## [34237] Premium   Premium   Premium   Premium   Premium   Premium  
## [34243] Premium   Ideal     Premium   Very Good Very Good Premium  
## [34249] Ideal     Ideal     Ideal     Fair      Ideal     Very Good
## [34255] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [34261] Ideal     Ideal     Good      Premium   Very Good Premium  
## [34267] Premium   Very Good Ideal     Ideal     Ideal     Very Good
## [34273] Very Good Ideal     Ideal     Good      Premium   Very Good
## [34279] Very Good Ideal     Premium   Good      Premium   Very Good
## [34285] Ideal     Very Good Very Good Ideal     Premium   Good     
## [34291] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [34297] Ideal     Fair      Good      Very Good Very Good Very Good
## [34303] Ideal     Ideal     Ideal     Ideal     Good      Ideal    
## [34309] Ideal     Ideal     Ideal     Very Good Good      Very Good
## [34315] Very Good Very Good Very Good Very Good Very Good Very Good
## [34321] Ideal     Premium   Ideal     Ideal     Premium   Premium  
## [34327] Very Good Ideal     Ideal     Ideal     Very Good Premium  
## [34333] Very Good Very Good Very Good Ideal     Ideal     Ideal    
## [34339] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [34345] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [34351] Premium   Premium   Ideal     Very Good Very Good Very Good
## [34357] Very Good Very Good Very Good Good      Ideal     Ideal    
## [34363] Premium   Very Good Ideal     Ideal     Premium   Premium  
## [34369] Ideal     Ideal     Ideal     Ideal     Very Good Ideal    
## [34375] Ideal     Ideal     Very Good Ideal     Ideal     Premium  
## [34381] Very Good Very Good Very Good Ideal     Ideal     Very Good
## [34387] Premium   Ideal     Ideal     Ideal     Ideal     Very Good
## [34393] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [34399] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [34405] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [34411] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [34417] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [34423] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [34429] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [34435] Ideal     Ideal     Premium   Premium   Ideal     Ideal    
## [34441] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [34447] Ideal     Ideal     Ideal     Premium   Premium   Ideal    
## [34453] Premium   Ideal     Premium   Very Good Very Good Very Good
## [34459] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [34465] Ideal     Ideal     Ideal     Ideal     Very Good Ideal    
## [34471] Ideal     Very Good Very Good Premium   Ideal     Very Good
## [34477] Ideal     Premium   Very Good Ideal     Premium   Very Good
## [34483] Premium   Very Good Very Good Premium   Premium   Premium  
## [34489] Premium   Premium   Premium   Premium   Ideal     Ideal    
## [34495] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [34501] Good      Premium   Very Good Very Good Very Good Very Good
## [34507] Very Good Ideal     Premium   Ideal     Premium   Ideal    
## [34513] Very Good Premium   Premium   Ideal     Ideal     Ideal    
## [34519] Ideal     Premium   Premium   Premium   Premium   Very Good
## [34525] Very Good Ideal     Premium   Ideal     Premium   Ideal    
## [34531] Ideal     Ideal     Ideal     Very Good Ideal     Ideal    
## [34537] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [34543] Ideal     Very Good Ideal     Good      Ideal     Premium  
## [34549] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [34555] Premium   Ideal     Premium   Very Good Very Good Very Good
## [34561] Very Good Very Good Very Good Ideal     Very Good Very Good
## [34567] Good      Ideal     Ideal     Ideal     Ideal     Ideal    
## [34573] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [34579] Premium   Premium   Premium   Fair      Very Good Very Good
## [34585] Very Good Ideal     Ideal     Ideal     Ideal     Ideal    
## [34591] Ideal     Ideal     Ideal     Ideal     Very Good Very Good
## [34597] Very Good Very Good Very Good Very Good Very Good Very Good
## [34603] Very Good Very Good Very Good Very Good Very Good Ideal    
## [34609] Ideal     Ideal     Good      Premium   Ideal     Ideal    
## [34615] Good      Premium   Ideal     Ideal     Very Good Premium  
## [34621] Ideal     Ideal     Premium   Premium   Premium   Ideal    
## [34627] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [34633] Ideal     Premium   Ideal     Ideal     Ideal     Premium  
## [34639] Ideal     Ideal     Ideal     Premium   Premium   Ideal    
## [34645] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [34651] Ideal     Ideal     Premium   Ideal     Premium   Ideal    
## [34657] Ideal     Ideal     Premium   Ideal     Ideal     Ideal    
## [34663] Premium   Premium   Ideal     Ideal     Premium   Premium  
## [34669] Premium   Ideal     Ideal     Premium   Very Good Very Good
## [34675] Very Good Good      Good      Ideal     Premium   Ideal    
## [34681] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [34687] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [34693] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [34699] Good      Premium   Very Good Ideal     Ideal     Ideal    
## [34705] Premium   Premium   Ideal     Ideal     Ideal     Ideal    
## [34711] Ideal     Premium   Premium   Premium   Premium   Premium  
## [34717] Premium   Premium   Good      Premium   Very Good Ideal    
## [34723] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [34729] Ideal     Fair      Very Good Ideal     Premium   Ideal    
## [34735] Ideal     Ideal     Very Good Ideal     Premium   Ideal    
## [34741] Ideal     Ideal     Ideal     Ideal     Good      Very Good
## [34747] Premium   Ideal     Ideal     Ideal     Premium   Ideal    
## [34753] Ideal     Premium   Premium   Premium   Premium   Premium  
## [34759] Premium   Ideal     Premium   Ideal     Premium   Premium  
## [34765] Premium   Ideal     Premium   Ideal     Premium   Premium  
## [34771] Ideal     Ideal     Premium   Premium   Premium   Ideal    
## [34777] Premium   Premium   Ideal     Premium   Premium   Premium  
## [34783] Ideal     Ideal     Very Good Premium   Ideal     Ideal    
## [34789] Ideal     Ideal     Premium   Premium   Good      Ideal    
## [34795] Premium   Ideal     Good      Good      Ideal     Ideal    
## [34801] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [34807] Very Good Very Good Ideal     Ideal     Very Good Premium  
## [34813] Very Good Ideal     Very Good Very Good Ideal     Premium  
## [34819] Premium   Premium   Premium   Very Good Ideal     Ideal    
## [34825] Very Good Very Good Ideal     Ideal     Premium   Premium  
## [34831] Premium   Very Good Very Good Ideal     Premium   Premium  
## [34837] Premium   Premium   Premium   Premium   Ideal     Ideal    
## [34843] Ideal     Premium   Premium   Ideal     Ideal     Ideal    
## [34849] Very Good Ideal     Premium   Ideal     Premium   Ideal    
## [34855] Premium   Good      Ideal     Premium   Good      Ideal    
## [34861] Premium   Premium   Premium   Premium   Premium   Premium  
## [34867] Premium   Very Good Premium   Very Good Premium   Very Good
## [34873] Good      Premium   Premium   Very Good Very Good Ideal    
## [34879] Good      Premium   Ideal     Very Good Premium   Premium  
## [34885] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [34891] Premium   Ideal     Ideal     Very Good Ideal     Ideal    
## [34897] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [34903] Ideal     Ideal     Premium   Premium   Premium   Premium  
## [34909] Ideal     Ideal     Very Good Premium   Ideal     Ideal    
## [34915] Ideal     Premium   Ideal     Ideal     Premium   Ideal    
## [34921] Very Good Very Good Ideal     Good      Good      Good     
## [34927] Good      Good      Good      Good      Very Good Very Good
## [34933] Good      Ideal     Good      Very Good Good      Ideal    
## [34939] Good      Good      Very Good Very Good Very Good Very Good
## [34945] Good      Good      Good      Premium   Very Good Ideal    
## [34951] Very Good Very Good Very Good Very Good Very Good Very Good
## [34957] Ideal     Good      Very Good Premium   Very Good Ideal    
## [34963] Ideal     Premium   Premium   Very Good Very Good Very Good
## [34969] Very Good Very Good Very Good Very Good Very Good Very Good
## [34975] Good      Good      Good      Ideal     Ideal     Good     
## [34981] Ideal     Ideal     Premium   Premium   Premium   Premium  
## [34987] Premium   Ideal     Ideal     Ideal     Premium   Very Good
## [34993] Ideal     Ideal     Very Good Premium   Ideal     Ideal    
## [34999] Ideal     Ideal     Premium   Premium   Very Good Ideal    
## [35005] Premium   Premium   Very Good Very Good Premium   Good     
## [35011] Ideal     Very Good Premium   Premium   Good      Premium  
## [35017] Premium   Very Good Premium   Very Good Very Good Premium  
## [35023] Very Good Good      Good      Very Good Premium   Good     
## [35029] Good      Good      Very Good Ideal     Very Good Good     
## [35035] Premium   Ideal     Premium   Very Good Very Good Ideal    
## [35041] Ideal     Ideal     Ideal     Good      Premium   Ideal    
## [35047] Ideal     Very Good Ideal     Ideal     Ideal     Ideal    
## [35053] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [35059] Ideal     Premium   Very Good Ideal     Ideal     Ideal    
## [35065] Ideal     Ideal     Ideal     Very Good Very Good Very Good
## [35071] Very Good Very Good Very Good Very Good Very Good Very Good
## [35077] Very Good Very Good Very Good Very Good Ideal     Ideal    
## [35083] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [35089] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [35095] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [35101] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [35107] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [35113] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [35119] Good      Ideal     Premium   Ideal     Ideal     Ideal    
## [35125] Ideal     Ideal     Premium   Premium   Very Good Ideal    
## [35131] Ideal     Ideal     Very Good Very Good Very Good Very Good
## [35137] Ideal     Very Good Very Good Premium   Premium   Ideal    
## [35143] Good      Ideal     Very Good Very Good Ideal     Ideal    
## [35149] Very Good Ideal     Ideal     Ideal     Ideal     Ideal    
## [35155] Ideal     Very Good Very Good Ideal     Premium   Premium  
## [35161] Good      Ideal     Ideal     Ideal     Ideal     Ideal    
## [35167] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [35173] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [35179] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [35185] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [35191] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [35197] Ideal     Ideal     Good      Good      Good      Ideal    
## [35203] Ideal     Very Good Premium   Very Good Premium   Ideal    
## [35209] Premium   Premium   Ideal     Ideal     Very Good Ideal    
## [35215] Ideal     Ideal     Ideal     Ideal     Good      Very Good
## [35221] Very Good Ideal     Ideal     Ideal     Ideal     Ideal    
## [35227] Ideal     Ideal     Ideal     Premium   Good      Fair     
## [35233] Fair      Very Good Ideal     Good      Ideal     Premium  
## [35239] Very Good Ideal     Premium   Premium   Premium   Very Good
## [35245] Very Good Ideal     Ideal     Premium   Very Good Ideal    
## [35251] Ideal     Very Good Very Good Premium   Good      Very Good
## [35257] Ideal     Ideal     Ideal     Ideal     Premium   Ideal    
## [35263] Good      Ideal     Ideal     Ideal     Ideal     Premium  
## [35269] Very Good Ideal     Ideal     Ideal     Ideal     Premium  
## [35275] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [35281] Premium   Good      Premium   Premium   Premium   Premium  
## [35287] Very Good Premium   Very Good Premium   Premium   Premium  
## [35293] Very Good Very Good Premium   Ideal     Premium   Very Good
## [35299] Very Good Very Good Premium   Very Good Good      Good     
## [35305] Ideal     Premium   Ideal     Very Good Very Good Premium  
## [35311] Very Good Ideal     Ideal     Ideal     Ideal     Ideal    
## [35317] Ideal     Ideal     Ideal     Very Good Very Good Very Good
## [35323] Ideal     Ideal     Very Good Very Good Very Good Ideal    
## [35329] Good      Ideal     Ideal     Ideal     Ideal     Ideal    
## [35335] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [35341] Premium   Ideal     Ideal     Very Good Ideal     Very Good
## [35347] Premium   Ideal     Premium   Premium   Ideal     Premium  
## [35353] Ideal     Ideal     Ideal     Premium   Premium   Premium  
## [35359] Premium   Premium   Premium   Premium   Ideal     Premium  
## [35365] Premium   Premium   Premium   Ideal     Premium   Premium  
## [35371] Very Good Premium   Ideal     Very Good Ideal     Ideal    
## [35377] Very Good Ideal     Ideal     Premium   Very Good Ideal    
## [35383] Very Good Very Good Premium   Ideal     Premium   Premium  
## [35389] Ideal     Ideal     Very Good Premium   Ideal     Very Good
## [35395] Very Good Ideal     Good      Premium   Very Good Very Good
## [35401] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [35407] Ideal     Ideal     Premium   Ideal     Ideal     Premium  
## [35413] Premium   Ideal     Ideal     Premium   Very Good Premium  
## [35419] Premium   Premium   Premium   Premium   Premium   Premium  
## [35425] Premium   Ideal     Ideal     Ideal     Premium   Ideal    
## [35431] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [35437] Premium   Ideal     Ideal     Ideal     Premium   Ideal    
## [35443] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [35449] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [35455] Ideal     Premium   Ideal     Premium   Ideal     Premium  
## [35461] Premium   Ideal     Ideal     Ideal     Ideal     Ideal    
## [35467] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [35473] Premium   Ideal     Ideal     Ideal     Ideal     Ideal    
## [35479] Ideal     Very Good Ideal     Ideal     Ideal     Ideal    
## [35485] Good      Premium   Fair      Very Good Very Good Good     
## [35491] Ideal     Ideal     Ideal     Very Good Good      Ideal    
## [35497] Very Good Very Good Very Good Premium   Premium   Ideal    
## [35503] Very Good Ideal     Premium   Premium   Ideal     Good     
## [35509] Premium   Very Good Premium   Good      Very Good Ideal    
## [35515] Ideal     Ideal     Very Good Ideal     Very Good Ideal    
## [35521] Very Good Very Good Very Good Very Good Premium   Ideal    
## [35527] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [35533] Premium   Premium   Good      Premium   Very Good Very Good
## [35539] Very Good Ideal     Ideal     Ideal     Ideal     Ideal    
## [35545] Ideal     Premium   Ideal     Premium   Ideal     Premium  
## [35551] Ideal     Premium   Ideal     Ideal     Premium   Premium  
## [35557] Ideal     Premium   Premium   Premium   Premium   Ideal    
## [35563] Premium   Ideal     Ideal     Ideal     Ideal     Ideal    
## [35569] Ideal     Ideal     Ideal     Premium   Ideal     Premium  
## [35575] Ideal     Ideal     Ideal     Premium   Ideal     Ideal    
## [35581] Premium   Ideal     Premium   Ideal     Premium   Ideal    
## [35587] Ideal     Very Good Premium   Ideal     Premium   Premium  
## [35593] Ideal     Premium   Ideal     Premium   Premium   Ideal    
## [35599] Ideal     Ideal     Ideal     Premium   Premium   Premium  
## [35605] Very Good Premium   Very Good Very Good Ideal     Premium  
## [35611] Ideal     Very Good Good      Premium   Ideal     Premium  
## [35617] Very Good Ideal     Good      Good      Very Good Premium  
## [35623] Very Good Very Good Premium   Very Good Good      Good     
## [35629] Premium   Very Good Very Good Very Good Good      Very Good
## [35635] Ideal     Ideal     Very Good Very Good Ideal     Ideal    
## [35641] Premium   Ideal     Ideal     Ideal     Ideal     Premium  
## [35647] Ideal     Ideal     Ideal     Premium   Ideal     Premium  
## [35653] Ideal     Very Good Ideal     Premium   Very Good Ideal    
## [35659] Very Good Premium   Good      Very Good Fair      Premium  
## [35665] Ideal     Ideal     Very Good Ideal     Very Good Very Good
## [35671] Very Good Good      Ideal     Ideal     Ideal     Very Good
## [35677] Very Good Very Good Very Good Very Good Ideal     Ideal    
## [35683] Premium   Good      Ideal     Ideal     Premium   Premium  
## [35689] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [35695] Premium   Ideal     Premium   Ideal     Premium   Ideal    
## [35701] Premium   Premium   Ideal     Ideal     Premium   Ideal    
## [35707] Ideal     Ideal     Premium   Ideal     Ideal     Premium  
## [35713] Premium   Ideal     Ideal     Ideal     Ideal     Premium  
## [35719] Ideal     Premium   Ideal     Ideal     Ideal     Ideal    
## [35725] Premium   Ideal     Ideal     Ideal     Ideal     Premium  
## [35731] Premium   Ideal     Ideal     Premium   Ideal     Premium  
## [35737] Ideal     Ideal     Premium   Premium   Premium   Ideal    
## [35743] Premium   Premium   Ideal     Ideal     Premium   Premium  
## [35749] Premium   Premium   Premium   Ideal     Premium   Fair     
## [35755] Ideal     Very Good Very Good Very Good Very Good Ideal    
## [35761] Premium   Very Good Very Good Premium   Very Good Ideal    
## [35767] Premium   Ideal     Ideal     Premium   Very Good Very Good
## [35773] Ideal     Premium   Good      Very Good Ideal     Very Good
## [35779] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [35785] Ideal     Ideal     Ideal     Ideal     Good      Good     
## [35791] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [35797] Ideal     Ideal     Very Good Very Good Very Good Very Good
## [35803] Premium   Ideal     Ideal     Ideal     Ideal     Ideal    
## [35809] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [35815] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [35821] Ideal     Ideal     Ideal     Ideal     Good      Ideal    
## [35827] Premium   Very Good Very Good Good      Ideal     Ideal    
## [35833] Ideal     Ideal     Ideal     Very Good Ideal     Ideal    
## [35839] Ideal     Ideal     Ideal     Very Good Very Good Premium  
## [35845] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [35851] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [35857] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [35863] Ideal     Ideal     Good      Good      Good      Very Good
## [35869] Very Good Very Good Ideal     Ideal     Premium   Ideal    
## [35875] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [35881] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [35887] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [35893] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [35899] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [35905] Ideal     Ideal     Premium   Ideal     Ideal     Ideal    
## [35911] Ideal     Ideal     Ideal     Ideal     Fair      Premium  
## [35917] Ideal     Premium   Ideal     Premium   Ideal     Ideal    
## [35923] Ideal     Premium   Premium   Ideal     Premium   Premium  
## [35929] Ideal     Ideal     Premium   Premium   Premium   Very Good
## [35935] Ideal     Ideal     Premium   Very Good Ideal     Very Good
## [35941] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [35947] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [35953] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [35959] Very Good Very Good Very Good Ideal     Ideal     Ideal    
## [35965] Good      Good      Good      Ideal     Ideal     Ideal    
## [35971] Ideal     Premium   Very Good Very Good Ideal     Very Good
## [35977] Very Good Very Good Very Good Ideal     Premium   Premium  
## [35983] Ideal     Very Good Ideal     Ideal     Very Good Very Good
## [35989] Ideal     Very Good Ideal     Ideal     Ideal     Ideal    
## [35995] Ideal     Ideal     Very Good Premium   Premium   Ideal    
## [36001] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [36007] Ideal     Ideal     Ideal     Good      Very Good Ideal    
## [36013] Very Good Premium   Very Good Ideal     Premium   Very Good
## [36019] Premium   Premium   Fair      Very Good Ideal     Ideal    
## [36025] Ideal     Ideal     Good      Good      Premium   Premium  
## [36031] Premium   Premium   Premium   Premium   Premium   Premium  
## [36037] Ideal     Premium   Premium   Premium   Premium   Premium  
## [36043] Ideal     Fair      Ideal     Very Good Ideal     Ideal    
## [36049] Premium   Very Good Ideal     Ideal     Premium   Ideal    
## [36055] Good      Ideal     Premium   Very Good Ideal     Ideal    
## [36061] Ideal     Premium   Ideal     Ideal     Ideal     Ideal    
## [36067] Ideal     Premium   Very Good Very Good Ideal     Premium  
## [36073] Good      Very Good Very Good Premium   Very Good Very Good
## [36079] Very Good Very Good Good      Ideal     Ideal     Ideal    
## [36085] Ideal     Premium   Ideal     Premium   Premium   Premium  
## [36091] Very Good Very Good Ideal     Good      Very Good Ideal    
## [36097] Ideal     Premium   Ideal     Very Good Ideal     Ideal    
## [36103] Ideal     Ideal     Premium   Very Good Very Good Very Good
## [36109] Very Good Good      Premium   Premium   Premium   Premium  
## [36115] Premium   Very Good Very Good Very Good Ideal     Premium  
## [36121] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [36127] Ideal     Premium   Ideal     Ideal     Premium   Ideal    
## [36133] Ideal     Ideal     Premium   Premium   Ideal     Ideal    
## [36139] Ideal     Ideal     Ideal     Premium   Ideal     Ideal    
## [36145] Ideal     Premium   Ideal     Ideal     Ideal     Premium  
## [36151] Ideal     Ideal     Ideal     Ideal     Premium   Ideal    
## [36157] Premium   Ideal     Ideal     Very Good Ideal     Ideal    
## [36163] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [36169] Ideal     Ideal     Ideal     Ideal     Ideal     Good     
## [36175] Premium   Very Good Very Good Ideal     Ideal     Ideal    
## [36181] Ideal     Premium   Premium   Premium   Premium   Very Good
## [36187] Very Good Very Good Very Good Premium   Good      Ideal    
## [36193] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [36199] Ideal     Ideal     Ideal     Good      Ideal     Very Good
## [36205] Very Good Very Good Ideal     Premium   Ideal     Premium  
## [36211] Ideal     Premium   Premium   Premium   Ideal     Premium  
## [36217] Premium   Ideal     Ideal     Premium   Premium   Premium  
## [36223] Ideal     Ideal     Premium   Very Good Premium   Very Good
## [36229] Very Good Ideal     Very Good Premium   Ideal     Ideal    
## [36235] Ideal     Ideal     Premium   Premium   Ideal     Ideal    
## [36241] Ideal     Very Good Ideal     Premium   Ideal     Ideal    
## [36247] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [36253] Good      Ideal     Ideal     Premium   Ideal     Ideal    
## [36259] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [36265] Ideal     Ideal     Good      Ideal     Ideal     Premium  
## [36271] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [36277] Ideal     Ideal     Very Good Good      Good      Premium  
## [36283] Premium   Ideal     Ideal     Good      Very Good Premium  
## [36289] Premium   Ideal     Very Good Good      Very Good Ideal    
## [36295] Very Good Premium   Very Good Premium   Premium   Very Good
## [36301] Ideal     Ideal     Premium   Premium   Premium   Ideal    
## [36307] Ideal     Ideal     Premium   Ideal     Premium   Ideal    
## [36313] Premium   Premium   Ideal     Ideal     Premium   Ideal    
## [36319] Premium   Ideal     Ideal     Ideal     Ideal     Ideal    
## [36325] Ideal     Ideal     Ideal     Premium   Premium   Ideal    
## [36331] Premium   Ideal     Premium   Premium   Premium   Ideal    
## [36337] Ideal     Premium   Ideal     Premium   Ideal     Ideal    
## [36343] Ideal     Premium   Ideal     Very Good Very Good Very Good
## [36349] Ideal     Ideal     Good      Premium   Very Good Very Good
## [36355] Ideal     Fair      Very Good Very Good Very Good Ideal    
## [36361] Ideal     Good      Ideal     Ideal     Ideal     Ideal    
## [36367] Ideal     Ideal     Premium   Ideal     Ideal     Ideal    
## [36373] Ideal     Ideal     Premium   Premium   Premium   Ideal    
## [36379] Ideal     Ideal     Very Good Premium   Premium   Premium  
## [36385] Premium   Premium   Premium   Ideal     Premium   Premium  
## [36391] Ideal     Premium   Ideal     Ideal     Ideal     Good     
## [36397] Ideal     Ideal     Ideal     Ideal     Premium   Premium  
## [36403] Premium   Premium   Ideal     Ideal     Premium   Ideal    
## [36409] Ideal     Ideal     Ideal     Premium   Premium   Ideal    
## [36415] Ideal     Premium   Premium   Ideal     Premium   Very Good
## [36421] Premium   Premium   Good      Ideal     Ideal     Ideal    
## [36427] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [36433] Ideal     Ideal     Ideal     Premium   Ideal     Ideal    
## [36439] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [36445] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [36451] Premium   Ideal     Ideal     Premium   Ideal     Ideal    
## [36457] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [36463] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [36469] Very Good Ideal     Good      Ideal     Ideal     Ideal    
## [36475] Very Good Ideal     Ideal     Premium   Ideal     Ideal    
## [36481] Very Good Very Good Ideal     Ideal     Ideal     Ideal    
## [36487] Ideal     Ideal     Ideal     Ideal     Ideal     Good     
## [36493] Very Good Very Good Very Good Very Good Ideal     Ideal    
## [36499] Ideal     Premium   Very Good Very Good Ideal     Fair     
## [36505] Ideal     Premium   Good      Ideal     Ideal     Ideal    
## [36511] Ideal     Ideal     Ideal     Good      Premium   Premium  
## [36517] Premium   Premium   Premium   Premium   Ideal     Very Good
## [36523] Ideal     Good      Premium   Premium   Ideal     Ideal    
## [36529] Ideal     Ideal     Ideal     Premium   Premium   Premium  
## [36535] Premium   Ideal     Premium   Good      Premium   Premium  
## [36541] Premium   Premium   Premium   Ideal     Premium   Good     
## [36547] Premium   Premium   Premium   Premium   Premium   Ideal    
## [36553] Ideal     Ideal     Premium   Ideal     Premium   Premium  
## [36559] Ideal     Premium   Ideal     Premium   Ideal     Ideal    
## [36565] Premium   Ideal     Premium   Premium   Ideal     Good     
## [36571] Fair      Premium   Fair      Fair      Ideal     Ideal    
## [36577] Premium   Very Good Premium   Premium   Ideal     Ideal    
## [36583] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [36589] Ideal     Ideal     Ideal     Very Good Very Good Premium  
## [36595] Ideal     Ideal     Premium   Ideal     Ideal     Ideal    
## [36601] Ideal     Good      Ideal     Ideal     Ideal     Ideal    
## [36607] Very Good Very Good Very Good Ideal     Ideal     Premium  
## [36613] Premium   Premium   Premium   Very Good Ideal     Ideal    
## [36619] Premium   Very Good Very Good Very Good Very Good Very Good
## [36625] Very Good Very Good Very Good Very Good Very Good Very Good
## [36631] Ideal     Ideal     Premium   Very Good Very Good Very Good
## [36637] Very Good Good      Very Good Ideal     Ideal     Ideal    
## [36643] Ideal     Premium   Good      Good      Premium   Very Good
## [36649] Very Good Very Good Premium   Premium   Premium   Premium  
## [36655] Very Good Good      Ideal     Ideal     Ideal     Ideal    
## [36661] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [36667] Ideal     Ideal     Ideal     Ideal     Premium   Premium  
## [36673] Fair      Ideal     Premium   Ideal     Premium   Ideal    
## [36679] Ideal     Ideal     Very Good Very Good Premium   Ideal    
## [36685] Ideal     Ideal     Premium   Premium   Premium   Premium  
## [36691] Premium   Fair      Very Good Ideal     Ideal     Ideal    
## [36697] Ideal     Ideal     Ideal     Good      Ideal     Ideal    
## [36703] Very Good Premium   Premium   Ideal     Ideal     Ideal    
## [36709] Ideal     Ideal     Ideal     Ideal     Premium   Ideal    
## [36715] Premium   Premium   Very Good Very Good Very Good Very Good
## [36721] Very Good Very Good Ideal     Very Good Ideal     Ideal    
## [36727] Very Good Premium   Very Good Ideal     Premium   Premium  
## [36733] Ideal     Ideal     Ideal     Very Good Ideal     Ideal    
## [36739] Premium   Premium   Ideal     Ideal     Ideal     Ideal    
## [36745] Good      Good      Ideal     Ideal     Ideal     Ideal    
## [36751] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [36757] Ideal     Ideal     Premium   Ideal     Very Good Very Good
## [36763] Ideal     Ideal     Ideal     Premium   Ideal     Premium  
## [36769] Ideal     Ideal     Ideal     Premium   Ideal     Premium  
## [36775] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [36781] Very Good Ideal     Premium   Ideal     Ideal     Premium  
## [36787] Premium   Ideal     Ideal     Ideal     Premium   Ideal    
## [36793] Ideal     Premium   Ideal     Ideal     Premium   Ideal    
## [36799] Premium   Premium   Premium   Premium   Ideal     Premium  
## [36805] Ideal     Ideal     Ideal     Premium   Ideal     Ideal    
## [36811] Ideal     Premium   Good      Ideal     Very Good Premium  
## [36817] Premium   Fair      Fair      Fair      Fair      Fair     
## [36823] Very Good Very Good Ideal     Very Good Good      Ideal    
## [36829] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [36835] Good      Premium   Premium   Premium   Premium   Premium  
## [36841] Very Good Premium   Premium   Ideal     Premium   Premium  
## [36847] Ideal     Premium   Ideal     Premium   Very Good Premium  
## [36853] Premium   Premium   Ideal     Very Good Good      Good     
## [36859] Very Good Ideal     Ideal     Premium   Ideal     Ideal    
## [36865] Premium   Very Good Ideal     Ideal     Premium   Ideal    
## [36871] Very Good Premium   Ideal     Ideal     Very Good Ideal    
## [36877] Premium   Ideal     Ideal     Ideal     Premium   Ideal    
## [36883] Ideal     Very Good Good      Ideal     Ideal     Ideal    
## [36889] Ideal     Good      Ideal     Ideal     Premium   Ideal    
## [36895] Premium   Ideal     Ideal     Ideal     Ideal     Very Good
## [36901] Ideal     Ideal     Ideal     Premium   Fair      Ideal    
## [36907] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [36913] Premium   Ideal     Ideal     Premium   Premium   Ideal    
## [36919] Premium   Ideal     Premium   Premium   Ideal     Ideal    
## [36925] Ideal     Premium   Premium   Premium   Ideal     Ideal    
## [36931] Very Good Very Good Very Good Very Good Very Good Very Good
## [36937] Very Good Very Good Very Good Very Good Very Good Very Good
## [36943] Ideal     Ideal     Ideal     Good      Ideal     Ideal    
## [36949] Very Good Very Good Very Good Very Good Ideal     Ideal    
## [36955] Ideal     Ideal     Ideal     Ideal     Good      Very Good
## [36961] Premium   Premium   Premium   Premium   Premium   Ideal    
## [36967] Premium   Ideal     Premium   Ideal     Very Good Ideal    
## [36973] Ideal     Good      Premium   Very Good Very Good Premium  
## [36979] Good      Ideal     Ideal     Ideal     Ideal     Ideal    
## [36985] Very Good Ideal     Good      Premium   Premium   Ideal    
## [36991] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [36997] Ideal     Ideal     Very Good Premium   Good      Premium  
## [37003] Very Good Premium   Ideal     Ideal     Premium   Ideal    
## [37009] Ideal     Premium   Ideal     Premium   Very Good Premium  
## [37015] Ideal     Premium   Premium   Ideal     Ideal     Ideal    
## [37021] Premium   Premium   Premium   Premium   Ideal     Premium  
## [37027] Ideal     Good      Premium   Premium   Very Good Premium  
## [37033] Premium   Premium   Very Good Very Good Very Good Premium  
## [37039] Very Good Good      Ideal     Ideal     Ideal     Ideal    
## [37045] Ideal     Good      Ideal     Ideal     Ideal     Premium  
## [37051] Very Good Very Good Very Good Ideal     Ideal     Ideal    
## [37057] Ideal     Ideal     Premium   Premium   Premium   Premium  
## [37063] Ideal     Premium   Premium   Premium   Premium   Ideal    
## [37069] Ideal     Good      Ideal     Good      Ideal     Premium  
## [37075] Premium   Premium   Premium   Ideal     Ideal     Good     
## [37081] Very Good Very Good Premium   Premium   Premium   Premium  
## [37087] Premium   Ideal     Ideal     Premium   Premium   Ideal    
## [37093] Ideal     Ideal     Very Good Ideal     Ideal     Premium  
## [37099] Ideal     Ideal     Good      Very Good Premium   Ideal    
## [37105] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [37111] Premium   Fair      Very Good Ideal     Ideal     Ideal    
## [37117] Good      Good      Premium   Premium   Ideal     Ideal    
## [37123] Ideal     Very Good Very Good Very Good Ideal     Very Good
## [37129] Premium   Ideal     Premium   Ideal     Ideal     Ideal    
## [37135] Ideal     Ideal     Ideal     Ideal     Premium   Premium  
## [37141] Very Good Ideal     Ideal     Ideal     Premium   Ideal    
## [37147] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [37153] Ideal     Premium   Ideal     Ideal     Ideal     Ideal    
## [37159] Ideal     Premium   Ideal     Ideal     Ideal     Ideal    
## [37165] Ideal     Ideal     Ideal     Premium   Ideal     Ideal    
## [37171] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [37177] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [37183] Premium   Fair      Premium   Ideal     Ideal     Ideal    
## [37189] Premium   Ideal     Ideal     Ideal     Ideal     Ideal    
## [37195] Ideal     Very Good Very Good Ideal     Ideal     Good     
## [37201] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [37207] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [37213] Ideal     Ideal     Ideal     Ideal     Good      Good     
## [37219] Very Good Fair      Ideal     Ideal     Ideal     Ideal    
## [37225] Ideal     Premium   Ideal     Premium   Ideal     Ideal    
## [37231] Ideal     Premium   Premium   Ideal     Ideal     Ideal    
## [37237] Ideal     Premium   Premium   Ideal     Premium   Good     
## [37243] Premium   Very Good Very Good Very Good Ideal     Good     
## [37249] Very Good Very Good Very Good Ideal     Ideal     Ideal    
## [37255] Ideal     Ideal     Premium   Ideal     Premium   Premium  
## [37261] Premium   Very Good Very Good Premium   Very Good Ideal    
## [37267] Ideal     Ideal     Very Good Ideal     Ideal     Ideal    
## [37273] Ideal     Ideal     Ideal     Good      Very Good Very Good
## [37279] Very Good Very Good Ideal     Ideal     Ideal     Good     
## [37285] Good      Very Good Very Good Very Good Very Good Very Good
## [37291] Good      Premium   Good      Very Good Good      Ideal    
## [37297] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [37303] Premium   Fair      Ideal     Ideal     Ideal     Ideal    
## [37309] Ideal     Ideal     Premium   Ideal     Ideal     Ideal    
## [37315] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [37321] Ideal     Premium   Good      Premium   Very Good Ideal    
## [37327] Premium   Premium   Ideal     Fair      Very Good Very Good
## [37333] Ideal     Ideal     Very Good Ideal     Premium   Ideal    
## [37339] Ideal     Good      Good      Ideal     Ideal     Ideal    
## [37345] Ideal     Premium   Ideal     Very Good Premium   Very Good
## [37351] Very Good Very Good Ideal     Very Good Premium   Ideal    
## [37357] Premium   Very Good Premium   Premium   Ideal     Premium  
## [37363] Premium   Premium   Premium   Premium   Premium   Premium  
## [37369] Very Good Ideal     Premium   Premium   Premium   Premium  
## [37375] Ideal     Premium   Ideal     Premium   Very Good Ideal    
## [37381] Ideal     Very Good Premium   Ideal     Ideal     Ideal    
## [37387] Ideal     Ideal     Premium   Ideal     Ideal     Ideal    
## [37393] Ideal     Good      Ideal     Fair      Good      Very Good
## [37399] Very Good Ideal     Ideal     Ideal     Ideal     Ideal    
## [37405] Ideal     Ideal     Ideal     Ideal     Premium   Ideal    
## [37411] Premium   Premium   Premium   Premium   Ideal     Premium  
## [37417] Premium   Ideal     Premium   Ideal     Ideal     Ideal    
## [37423] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [37429] Premium   Ideal     Premium   Ideal     Premium   Good     
## [37435] Ideal     Ideal     Ideal     Premium   Ideal     Ideal    
## [37441] Ideal     Ideal     Good      Premium   Very Good Ideal    
## [37447] Very Good Ideal     Ideal     Ideal     Ideal     Premium  
## [37453] Ideal     Premium   Premium   Ideal     Ideal     Premium  
## [37459] Premium   Ideal     Ideal     Premium   Ideal     Premium  
## [37465] Ideal     Ideal     Premium   Ideal     Ideal     Ideal    
## [37471] Premium   Ideal     Premium   Ideal     Premium   Ideal    
## [37477] Premium   Ideal     Ideal     Ideal     Premium   Premium  
## [37483] Ideal     Premium   Premium   Premium   Premium   Ideal    
## [37489] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [37495] Premium   Premium   Premium   Premium   Premium   Premium  
## [37501] Ideal     Premium   Fair      Good      Premium   Fair     
## [37507] Good      Very Good Ideal     Ideal     Ideal     Ideal    
## [37513] Premium   Ideal     Ideal     Very Good Ideal     Good     
## [37519] Good      Very Good Very Good Premium   Good      Ideal    
## [37525] Ideal     Ideal     Ideal     Ideal     Ideal     Good     
## [37531] Good      Very Good Very Good Very Good Ideal     Premium  
## [37537] Ideal     Ideal     Very Good Ideal     Ideal     Ideal    
## [37543] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [37549] Ideal     Ideal     Ideal     Premium   Premium   Ideal    
## [37555] Ideal     Ideal     Ideal     Premium   Ideal     Premium  
## [37561] Premium   Premium   Ideal     Good      Ideal     Ideal    
## [37567] Ideal     Premium   Good      Ideal     Premium   Very Good
## [37573] Good      Ideal     Ideal     Ideal     Ideal     Ideal    
## [37579] Ideal     Premium   Ideal     Premium   Ideal     Ideal    
## [37585] Premium   Ideal     Ideal     Ideal     Premium   Ideal    
## [37591] Very Good Very Good Very Good Very Good Very Good Very Good
## [37597] Very Good Very Good Very Good Very Good Very Good Very Good
## [37603] Very Good Very Good Very Good Very Good Very Good Very Good
## [37609] Very Good Very Good Very Good Very Good Very Good Very Good
## [37615] Very Good Very Good Very Good Very Good Very Good Very Good
## [37621] Premium   Premium   Premium   Premium   Premium   Premium  
## [37627] Premium   Ideal     Ideal     Ideal     Premium   Premium  
## [37633] Premium   Premium   Premium   Ideal     Premium   Premium  
## [37639] Ideal     Premium   Premium   Premium   Premium   Premium  
## [37645] Ideal     Premium   Premium   Very Good Very Good Very Good
## [37651] Ideal     Good      Ideal     Ideal     Ideal     Ideal    
## [37657] Ideal     Premium   Premium   Premium   Premium   Premium  
## [37663] Ideal     Premium   Premium   Ideal     Premium   Ideal    
## [37669] Premium   Premium   Premium   Ideal     Premium   Very Good
## [37675] Ideal     Fair      Fair      Fair      Ideal     Premium  
## [37681] Ideal     Very Good Ideal     Ideal     Premium   Premium  
## [37687] Very Good Good      Ideal     Premium   Very Good Ideal    
## [37693] Ideal     Ideal     Premium   Ideal     Premium   Ideal    
## [37699] Fair      Good      Fair      Fair      Very Good Very Good
## [37705] Very Good Very Good Very Good Very Good Ideal     Premium  
## [37711] Premium   Premium   Premium   Very Good Ideal     Ideal    
## [37717] Premium   Ideal     Premium   Ideal     Ideal     Ideal    
## [37723] Ideal     Premium   Premium   Ideal     Premium   Very Good
## [37729] Ideal     Good      Very Good Premium   Ideal     Fair     
## [37735] Very Good Good      Ideal     Ideal     Ideal     Ideal    
## [37741] Ideal     Ideal     Ideal     Premium   Premium   Fair     
## [37747] Premium   Very Good Very Good Ideal     Ideal     Ideal    
## [37753] Premium   Ideal     Ideal     Premium   Premium   Premium  
## [37759] Ideal     Premium   Very Good Ideal     Ideal     Ideal    
## [37765] Ideal     Ideal     Premium   Premium   Ideal     Premium  
## [37771] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [37777] Premium   Premium   Ideal     Very Good Very Good Very Good
## [37783] Premium   Ideal     Very Good Very Good Ideal     Good     
## [37789] Ideal     Premium   Premium   Ideal     Ideal     Ideal    
## [37795] Ideal     Premium   Ideal     Ideal     Ideal     Ideal    
## [37801] Ideal     Ideal     Good      Very Good Very Good Ideal    
## [37807] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [37813] Good      Fair      Premium   Ideal     Very Good Good     
## [37819] Good      Ideal     Ideal     Ideal     Ideal     Ideal    
## [37825] Premium   Premium   Ideal     Ideal     Ideal     Ideal    
## [37831] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [37837] Premium   Ideal     Ideal     Ideal     Ideal     Ideal    
## [37843] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [37849] Ideal     Premium   Ideal     Fair      Premium   Very Good
## [37855] Ideal     Ideal     Premium   Premium   Ideal     Premium  
## [37861] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [37867] Fair      Very Good Ideal     Ideal     Ideal     Ideal    
## [37873] Ideal     Ideal     Ideal     Ideal     Premium   Very Good
## [37879] Very Good Ideal     Very Good Ideal     Premium   Very Good
## [37885] Ideal     Ideal     Premium   Ideal     Ideal     Ideal    
## [37891] Premium   Very Good Ideal     Very Good Ideal     Ideal    
## [37897] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [37903] Ideal     Premium   Ideal     Ideal     Ideal     Ideal    
## [37909] Ideal     Ideal     Ideal     Very Good Very Good Premium  
## [37915] Ideal     Ideal     Ideal     Premium   Ideal     Ideal    
## [37921] Very Good Very Good Very Good Very Good Very Good Ideal    
## [37927] Good      Good      Very Good Premium   Premium   Very Good
## [37933] Very Good Very Good Very Good Very Good Very Good Ideal    
## [37939] Ideal     Ideal     Ideal     Premium   Very Good Very Good
## [37945] Good      Premium   Very Good Very Good Very Good Very Good
## [37951] Premium   Premium   Ideal     Very Good Ideal     Ideal    
## [37957] Ideal     Ideal     Premium   Ideal     Ideal     Ideal    
## [37963] Premium   Ideal     Very Good Ideal     Very Good Ideal    
## [37969] Ideal     Ideal     Very Good Ideal     Premium   Ideal    
## [37975] Good      Very Good Premium   Very Good Very Good Very Good
## [37981] Very Good Ideal     Ideal     Ideal     Ideal     Ideal    
## [37987] Ideal     Ideal     Ideal     Ideal     Premium   Ideal    
## [37993] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [37999] Ideal     Ideal     Premium   Premium   Premium   Ideal    
## [38005] Premium   Very Good Ideal     Premium   Premium   Premium  
## [38011] Very Good Very Good Good      Good      Ideal     Premium  
## [38017] Premium   Premium   Premium   Premium   Very Good Very Good
## [38023] Premium   Premium   Very Good Premium   Ideal     Ideal    
## [38029] Very Good Ideal     Ideal     Ideal     Ideal     Ideal    
## [38035] Ideal     Ideal     Very Good Premium   Premium   Premium  
## [38041] Fair      Fair      Premium   Ideal     Ideal     Ideal    
## [38047] Very Good Premium   Premium   Premium   Premium   Premium  
## [38053] Very Good Fair      Ideal     Good      Premium   Premium  
## [38059] Ideal     Ideal     Ideal     Ideal     Ideal     Good     
## [38065] Ideal     Premium   Ideal     Premium   Premium   Ideal    
## [38071] Premium   Premium   Ideal     Premium   Ideal     Premium  
## [38077] Premium   Ideal     Ideal     Premium   Premium   Premium  
## [38083] Ideal     Ideal     Premium   Ideal     Ideal     Ideal    
## [38089] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [38095] Premium   Ideal     Premium   Ideal     Premium   Premium  
## [38101] Ideal     Ideal     Ideal     Premium   Premium   Premium  
## [38107] Premium   Premium   Ideal     Premium   Ideal     Ideal    
## [38113] Premium   Ideal     Ideal     Premium   Ideal     Ideal    
## [38119] Ideal     Premium   Premium   Premium   Premium   Ideal    
## [38125] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [38131] Very Good Premium   Very Good Premium   Ideal     Very Good
## [38137] Premium   Premium   Premium   Premium   Ideal     Very Good
## [38143] Premium   Ideal     Premium   Very Good Very Good Very Good
## [38149] Very Good Premium   Premium   Ideal     Good      Fair     
## [38155] Premium   Ideal     Ideal     Very Good Very Good Ideal    
## [38161] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [38167] Ideal     Ideal     Ideal     Very Good Ideal     Premium  
## [38173] Ideal     Fair      Very Good Ideal     Premium   Ideal    
## [38179] Ideal     Ideal     Ideal     Premium   Ideal     Premium  
## [38185] Ideal     Ideal     Ideal     Premium   Ideal     Premium  
## [38191] Ideal     Ideal     Premium   Very Good Very Good Very Good
## [38197] Good      Premium   Premium   Ideal     Very Good Very Good
## [38203] Ideal     Premium   Premium   Premium   Premium   Ideal    
## [38209] Premium   Ideal     Ideal     Premium   Premium   Premium  
## [38215] Premium   Premium   Ideal     Ideal     Premium   Ideal    
## [38221] Premium   Premium   Very Good Very Good Very Good Premium  
## [38227] Ideal     Ideal     Ideal     Ideal     Ideal     Good     
## [38233] Ideal     Ideal     Ideal     Ideal     Very Good Very Good
## [38239] Very Good Very Good Ideal     Ideal     Ideal     Ideal    
## [38245] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [38251] Ideal     Very Good Premium   Very Good Premium   Very Good
## [38257] Very Good Very Good Ideal     Ideal     Ideal     Premium  
## [38263] Premium   Very Good Ideal     Good      Very Good Very Good
## [38269] Ideal     Ideal     Ideal     Ideal     Ideal     Very Good
## [38275] Premium   Premium   Premium   Premium   Premium   Premium  
## [38281] Very Good Very Good Very Good Very Good Very Good Very Good
## [38287] Very Good Very Good Very Good Very Good Very Good Very Good
## [38293] Very Good Ideal     Ideal     Ideal     Ideal     Good     
## [38299] Ideal     Ideal     Ideal     Very Good Ideal     Ideal    
## [38305] Very Good Good      Very Good Good      Good      Premium  
## [38311] Ideal     Ideal     Ideal     Premium   Good      Very Good
## [38317] Very Good Ideal     Ideal     Ideal     Very Good Very Good
## [38323] Very Good Ideal     Ideal     Ideal     Ideal     Premium  
## [38329] Premium   Ideal     Ideal     Ideal     Ideal     Ideal    
## [38335] Premium   Ideal     Premium   Ideal     Premium   Ideal    
## [38341] Premium   Premium   Ideal     Premium   Ideal     Ideal    
## [38347] Ideal     Premium   Premium   Ideal     Premium   Premium  
## [38353] Very Good Ideal     Ideal     Ideal     Ideal     Ideal    
## [38359] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [38365] Premium   Very Good Premium   Ideal     Good      Very Good
## [38371] Good      Very Good Ideal     Ideal     Premium   Premium  
## [38377] Very Good Ideal     Ideal     Ideal     Premium   Premium  
## [38383] Ideal     Ideal     Ideal     Premium   Ideal     Ideal    
## [38389] Ideal     Ideal     Ideal     Premium   Premium   Ideal    
## [38395] Ideal     Premium   Premium   Premium   Premium   Premium  
## [38401] Premium   Ideal     Premium   Ideal     Ideal     Premium  
## [38407] Ideal     Premium   Ideal     Premium   Premium   Ideal    
## [38413] Premium   Premium   Premium   Very Good Ideal     Ideal    
## [38419] Ideal     Premium   Very Good Premium   Premium   Very Good
## [38425] Premium   Premium   Ideal     Ideal     Ideal     Ideal    
## [38431] Premium   Ideal     Premium   Ideal     Ideal     Premium  
## [38437] Premium   Premium   Good      Very Good Ideal     Ideal    
## [38443] Fair      Ideal     Very Good Ideal     Ideal     Ideal    
## [38449] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [38455] Premium   Good      Ideal     Ideal     Premium   Premium  
## [38461] Very Good Very Good Very Good Ideal     Ideal     Ideal    
## [38467] Ideal     Premium   Ideal     Fair      Premium   Very Good
## [38473] Very Good Ideal     Ideal     Ideal     Ideal     Ideal    
## [38479] Ideal     Very Good Premium   Ideal     Ideal     Premium  
## [38485] Ideal     Ideal     Ideal     Very Good Ideal     Ideal    
## [38491] Ideal     Ideal     Premium   Very Good Ideal     Ideal    
## [38497] Ideal     Ideal     Ideal     Ideal     Very Good Ideal    
## [38503] Ideal     Good      Premium   Fair      Premium   Fair     
## [38509] Ideal     Premium   Fair      Ideal     Very Good Very Good
## [38515] Ideal     Ideal     Ideal     Good      Ideal     Ideal    
## [38521] Ideal     Premium   Ideal     Ideal     Ideal     Ideal    
## [38527] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [38533] Premium   Very Good Ideal     Ideal     Ideal     Premium  
## [38539] Premium   Ideal     Ideal     Premium   Ideal     Ideal    
## [38545] Ideal     Ideal     Ideal     Ideal     Ideal     Fair     
## [38551] Premium   Very Good Good      Ideal     Ideal     Ideal    
## [38557] Ideal     Ideal     Very Good Premium   Fair      Ideal    
## [38563] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [38569] Ideal     Ideal     Ideal     Premium   Premium   Premium  
## [38575] Premium   Premium   Premium   Very Good Ideal     Ideal    
## [38581] Ideal     Premium   Premium   Fair      Premium   Ideal    
## [38587] Ideal     Ideal     Premium   Premium   Very Good Ideal    
## [38593] Ideal     Good      Fair      Premium   Ideal     Ideal    
## [38599] Ideal     Ideal     Premium   Ideal     Premium   Very Good
## [38605] Ideal     Ideal     Ideal     Ideal     Very Good Ideal    
## [38611] Premium   Premium   Premium   Very Good Very Good Very Good
## [38617] Ideal     Ideal     Ideal     Very Good Good      Ideal    
## [38623] Good      Premium   Ideal     Ideal     Good      Good     
## [38629] Ideal     Very Good Premium   Ideal     Very Good Very Good
## [38635] Very Good Premium   Very Good Very Good Ideal     Very Good
## [38641] Premium   Fair      Ideal     Ideal     Ideal     Ideal    
## [38647] Premium   Premium   Ideal     Ideal     Premium   Premium  
## [38653] Ideal     Ideal     Premium   Ideal     Ideal     Ideal    
## [38659] Premium   Ideal     Premium   Premium   Ideal     Ideal    
## [38665] Premium   Ideal     Ideal     Premium   Premium   Premium  
## [38671] Good      Premium   Ideal     Premium   Very Good Very Good
## [38677] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [38683] Ideal     Ideal     Ideal     Ideal     Good      Ideal    
## [38689] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [38695] Premium   Premium   Ideal     Premium   Premium   Premium  
## [38701] Ideal     Premium   Ideal     Ideal     Premium   Premium  
## [38707] Ideal     Ideal     Premium   Very Good Very Good Premium  
## [38713] Ideal     Ideal     Ideal     Ideal     Good      Ideal    
## [38719] Very Good Good      Ideal     Ideal     Premium   Very Good
## [38725] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [38731] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [38737] Ideal     Ideal     Premium   Very Good Very Good Very Good
## [38743] Premium   Ideal     Very Good Ideal     Ideal     Ideal    
## [38749] Premium   Ideal     Ideal     Premium   Ideal     Ideal    
## [38755] Ideal     Ideal     Very Good Very Good Very Good Ideal    
## [38761] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [38767] Ideal     Ideal     Ideal     Premium   Ideal     Premium  
## [38773] Ideal     Premium   Ideal     Premium   Premium   Ideal    
## [38779] Ideal     Premium   Ideal     Premium   Ideal     Premium  
## [38785] Premium   Ideal     Ideal     Premium   Premium   Ideal    
## [38791] Premium   Premium   Ideal     Ideal     Premium   Premium  
## [38797] Ideal     Ideal     Ideal     Ideal     Very Good Ideal    
## [38803] Very Good Ideal     Premium   Premium   Premium   Premium  
## [38809] Premium   Very Good Ideal     Premium   Ideal     Premium  
## [38815] Premium   Premium   Premium   Ideal     Very Good Premium  
## [38821] Good      Very Good Very Good Very Good Very Good Ideal    
## [38827] Ideal     Ideal     Ideal     Premium   Ideal     Ideal    
## [38833] Ideal     Ideal     Very Good Very Good Good      Very Good
## [38839] Very Good Ideal     Fair      Fair      Very Good Ideal    
## [38845] Ideal     Fair      Premium   Ideal     Ideal     Premium  
## [38851] Premium   Premium   Premium   Ideal     Premium   Ideal    
## [38857] Premium   Premium   Premium   Premium   Ideal     Premium  
## [38863] Premium   Ideal     Ideal     Premium   Premium   Premium  
## [38869] Premium   Premium   Premium   Premium   Premium   Premium  
## [38875] Premium   Ideal     Premium   Premium   Ideal     Premium  
## [38881] Ideal     Ideal     Ideal     Premium   Ideal     Ideal    
## [38887] Premium   Premium   Good      Ideal     Very Good Very Good
## [38893] Ideal     Ideal     Ideal     Fair      Ideal     Ideal    
## [38899] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [38905] Premium   Premium   Premium   Ideal     Ideal     Ideal    
## [38911] Very Good Ideal     Good      Ideal     Ideal     Ideal    
## [38917] Ideal     Ideal     Ideal     Premium   Ideal     Ideal    
## [38923] Premium   Premium   Premium   Premium   Good      Very Good
## [38929] Very Good Very Good Premium   Premium   Ideal     Ideal    
## [38935] Very Good Very Good Very Good Ideal     Ideal     Premium  
## [38941] Premium   Very Good Premium   Ideal     Premium   Ideal    
## [38947] Very Good Ideal     Good      Ideal     Good      Very Good
## [38953] Good      Good      Very Good Good      Good      Premium  
## [38959] Good      Premium   Good      Ideal     Ideal     Very Good
## [38965] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [38971] Very Good Very Good Ideal     Very Good Ideal     Premium  
## [38977] Ideal     Ideal     Ideal     Ideal     Good      Premium  
## [38983] Ideal     Premium   Premium   Premium   Ideal     Premium  
## [38989] Ideal     Ideal     Premium   Very Good Very Good Ideal    
## [38995] Ideal     Ideal     Ideal     Premium   Very Good Premium  
## [39001] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [39007] Ideal     Premium   Ideal     Ideal     Very Good Ideal    
## [39013] Premium   Ideal     Ideal     Very Good Ideal     Very Good
## [39019] Premium   Premium   Premium   Very Good Premium   Ideal    
## [39025] Ideal     Ideal     Very Good Ideal     Good      Ideal    
## [39031] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [39037] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [39043] Good      Premium   Premium   Very Good Very Good Very Good
## [39049] Ideal     Premium   Ideal     Ideal     Ideal     Ideal    
## [39055] Very Good Good      Very Good Premium   Premium   Ideal    
## [39061] Fair      Very Good Very Good Ideal     Ideal     Ideal    
## [39067] Ideal     Ideal     Ideal     Ideal     Good      Good     
## [39073] Ideal     Very Good Premium   Ideal     Ideal     Fair     
## [39079] Premium   Premium   Ideal     Very Good Premium   Ideal    
## [39085] Good      Ideal     Ideal     Good      Ideal     Ideal    
## [39091] Ideal     Ideal     Premium   Very Good Good      Premium  
## [39097] Ideal     Premium   Good      Very Good Very Good Ideal    
## [39103] Ideal     Premium   Ideal     Very Good Ideal     Premium  
## [39109] Premium   Ideal     Ideal     Premium   Premium   Ideal    
## [39115] Premium   Ideal     Premium   Premium   Premium   Premium  
## [39121] Premium   Premium   Premium   Ideal     Ideal     Premium  
## [39127] Ideal     Premium   Ideal     Premium   Premium   Premium  
## [39133] Ideal     Premium   Premium   Ideal     Ideal     Ideal    
## [39139] Premium   Premium   Premium   Premium   Ideal     Premium  
## [39145] Ideal     Ideal     Ideal     Ideal     Ideal     Very Good
## [39151] Ideal     Ideal     Ideal     Premium   Premium   Ideal    
## [39157] Ideal     Ideal     Ideal     Premium   Ideal     Premium  
## [39163] Ideal     Ideal     Good      Premium   Premium   Premium  
## [39169] Ideal     Ideal     Premium   Ideal     Ideal     Premium  
## [39175] Premium   Ideal     Ideal     Ideal     Premium   Ideal    
## [39181] Ideal     Ideal     Ideal     Ideal     Ideal     Very Good
## [39187] Premium   Premium   Ideal     Ideal     Ideal     Ideal    
## [39193] Ideal     Ideal     Ideal     Premium   Premium   Ideal    
## [39199] Premium   Premium   Good      Ideal     Premium   Premium  
## [39205] Very Good Ideal     Ideal     Ideal     Ideal     Ideal    
## [39211] Very Good Ideal     Very Good Very Good Very Good Ideal    
## [39217] Ideal     Ideal     Ideal     Good      Premium   Ideal    
## [39223] Fair      Ideal     Very Good Ideal     Ideal     Premium  
## [39229] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [39235] Ideal     Ideal     Ideal     Very Good Very Good Very Good
## [39241] Very Good Ideal     Premium   Very Good Premium   Ideal    
## [39247] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [39253] Ideal     Fair      Ideal     Ideal     Ideal     Premium  
## [39259] Ideal     Ideal     Ideal     Ideal     Premium   Premium  
## [39265] Premium   Ideal     Premium   Ideal     Premium   Premium  
## [39271] Ideal     Ideal     Ideal     Good      Ideal     Good     
## [39277] Ideal     Ideal     Good      Good      Ideal     Ideal    
## [39283] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [39289] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [39295] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [39301] Premium   Premium   Premium   Ideal     Ideal     Ideal    
## [39307] Fair      Fair      Fair      Fair      Premium   Premium  
## [39313] Premium   Ideal     Very Good Very Good Ideal     Ideal    
## [39319] Fair      Very Good Premium   Ideal     Good      Very Good
## [39325] Ideal     Ideal     Fair      Ideal     Premium   Premium  
## [39331] Premium   Ideal     Ideal     Premium   Ideal     Good     
## [39337] Very Good Premium   Premium   Premium   Ideal     Good     
## [39343] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [39349] Ideal     Ideal     Good      Premium   Very Good Ideal    
## [39355] Premium   Premium   Ideal     Premium   Ideal     Premium  
## [39361] Premium   Very Good Premium   Premium   Premium   Very Good
## [39367] Premium   Premium   Ideal     Ideal     Very Good Very Good
## [39373] Premium   Ideal     Ideal     Ideal     Ideal     Ideal    
## [39379] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [39385] Very Good Fair      Ideal     Premium   Ideal     Ideal    
## [39391] Ideal     Ideal     Good      Ideal     Ideal     Premium  
## [39397] Ideal     Ideal     Ideal     Ideal     Premium   Ideal    
## [39403] Ideal     Ideal     Premium   Ideal     Ideal     Premium  
## [39409] Premium   Premium   Ideal     Ideal     Ideal     Ideal    
## [39415] Ideal     Ideal     Premium   Ideal     Ideal     Ideal    
## [39421] Premium   Ideal     Ideal     Ideal     Premium   Ideal    
## [39427] Ideal     Premium   Ideal     Premium   Ideal     Premium  
## [39433] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [39439] Premium   Ideal     Ideal     Ideal     Premium   Premium  
## [39445] Ideal     Ideal     Premium   Ideal     Good      Very Good
## [39451] Very Good Good      Very Good Good      Ideal     Ideal    
## [39457] Ideal     Ideal     Very Good Very Good Ideal     Ideal    
## [39463] Ideal     Premium   Fair      Very Good Very Good Very Good
## [39469] Ideal     Ideal     Premium   Premium   Very Good Premium  
## [39475] Ideal     Premium   Ideal     Premium   Ideal     Premium  
## [39481] Ideal     Ideal     Ideal     Ideal     Premium   Ideal    
## [39487] Very Good Ideal     Very Good Very Good Premium   Premium  
## [39493] Ideal     Ideal     Ideal     Ideal     Premium   Premium  
## [39499] Premium   Ideal     Ideal     Very Good Ideal     Very Good
## [39505] Ideal     Ideal     Ideal     Good      Good      Ideal    
## [39511] Ideal     Ideal     Ideal     Premium   Ideal     Ideal    
## [39517] Ideal     Premium   Premium   Premium   Premium   Very Good
## [39523] Ideal     Premium   Premium   Premium   Ideal     Very Good
## [39529] Premium   Very Good Premium   Good      Premium   Premium  
## [39535] Ideal     Premium   Premium   Ideal     Premium   Premium  
## [39541] Ideal     Premium   Premium   Ideal     Ideal     Premium  
## [39547] Premium   Ideal     Ideal     Ideal     Ideal     Ideal    
## [39553] Premium   Ideal     Ideal     Premium   Ideal     Ideal    
## [39559] Ideal     Premium   Fair      Ideal     Premium   Premium  
## [39565] Premium   Good      Very Good Ideal     Very Good Ideal    
## [39571] Ideal     Premium   Premium   Premium   Very Good Ideal    
## [39577] Ideal     Ideal     Ideal     Premium   Ideal     Good     
## [39583] Ideal     Premium   Ideal     Premium   Premium   Ideal    
## [39589] Very Good Very Good Ideal     Ideal     Ideal     Ideal    
## [39595] Ideal     Premium   Ideal     Ideal     Ideal     Ideal    
## [39601] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [39607] Premium   Premium   Very Good Ideal     Very Good Ideal    
## [39613] Ideal     Ideal     Ideal     Good      Premium   Very Good
## [39619] Good      Very Good Premium   Very Good Very Good Good     
## [39625] Very Good Premium   Ideal     Ideal     Very Good Good     
## [39631] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [39637] Ideal     Premium   Premium   Premium   Premium   Ideal    
## [39643] Ideal     Premium   Ideal     Premium   Fair      Very Good
## [39649] Very Good Very Good Very Good Ideal     Ideal     Premium  
## [39655] Premium   Premium   Ideal     Premium   Ideal     Premium  
## [39661] Ideal     Ideal     Ideal     Premium   Premium   Premium  
## [39667] Ideal     Ideal     Ideal     Ideal     Premium   Ideal    
## [39673] Premium   Ideal     Premium   Premium   Ideal     Premium  
## [39679] Fair      Ideal     Premium   Premium   Ideal     Very Good
## [39685] Ideal     Premium   Very Good Premium   Ideal     Premium  
## [39691] Ideal     Premium   Ideal     Very Good Premium   Good     
## [39697] Good      Good      Ideal     Ideal     Premium   Ideal    
## [39703] Ideal     Fair      Very Good Very Good Good      Ideal    
## [39709] Ideal     Premium   Premium   Good      Ideal     Fair     
## [39715] Very Good Good      Very Good Good      Very Good Very Good
## [39721] Premium   Ideal     Good      Good      Very Good Ideal    
## [39727] Ideal     Very Good Ideal     Ideal     Premium   Premium  
## [39733] Very Good Very Good Ideal     Ideal     Ideal     Ideal    
## [39739] Fair      Ideal     Premium   Premium   Ideal     Good     
## [39745] Premium   Ideal     Ideal     Good      Ideal     Premium  
## [39751] Ideal     Ideal     Fair      Ideal     Ideal     Ideal    
## [39757] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [39763] Premium   Ideal     Premium   Ideal     Ideal     Ideal    
## [39769] Ideal     Premium   Ideal     Ideal     Very Good Premium  
## [39775] Premium   Premium   Premium   Ideal     Ideal     Premium  
## [39781] Ideal     Ideal     Premium   Ideal     Ideal     Very Good
## [39787] Premium   Ideal     Ideal     Ideal     Ideal     Premium  
## [39793] Ideal     Ideal     Premium   Ideal     Ideal     Ideal    
## [39799] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [39805] Ideal     Ideal     Ideal     Premium   Premium   Premium  
## [39811] Very Good Ideal     Ideal     Ideal     Ideal     Ideal    
## [39817] Ideal     Very Good Premium   Ideal     Premium   Ideal    
## [39823] Premium   Premium   Premium   Premium   Premium   Premium  
## [39829] Ideal     Premium   Premium   Premium   Very Good Very Good
## [39835] Very Good Ideal     Very Good Very Good Ideal     Ideal    
## [39841] Good      Fair      Very Good Ideal     Ideal     Premium  
## [39847] Very Good Very Good Ideal     Ideal     Ideal     Ideal    
## [39853] Ideal     Good      Ideal     Ideal     Ideal     Ideal    
## [39859] Ideal     Ideal     Very Good Ideal     Ideal     Ideal    
## [39865] Ideal     Good      Premium   Fair      Ideal     Ideal    
## [39871] Ideal     Premium   Premium   Fair      Very Good Ideal    
## [39877] Ideal     Premium   Ideal     Ideal     Ideal     Ideal    
## [39883] Ideal     Ideal     Ideal     Premium   Premium   Ideal    
## [39889] Premium   Ideal     Ideal     Premium   Ideal     Ideal    
## [39895] Premium   Ideal     Premium   Ideal     Ideal     Ideal    
## [39901] Ideal     Ideal     Ideal     Ideal     Premium   Premium  
## [39907] Fair      Very Good Very Good Ideal     Ideal     Ideal    
## [39913] Ideal     Fair      Very Good Very Good Ideal     Very Good
## [39919] Ideal     Premium   Ideal     Ideal     Premium   Premium  
## [39925] Premium   Very Good Ideal     Ideal     Ideal     Ideal    
## [39931] Very Good Ideal     Premium   Good      Very Good Very Good
## [39937] Very Good Very Good Very Good Very Good Very Good Very Good
## [39943] Very Good Good      Good      Good      Good      Good     
## [39949] Good      Good      Premium   Ideal     Ideal     Good     
## [39955] Ideal     Good      Premium   Ideal     Good      Premium  
## [39961] Good      Very Good Premium   Premium   Very Good Very Good
## [39967] Ideal     Ideal     Ideal     Very Good Very Good Very Good
## [39973] Good      Fair      Ideal     Ideal     Ideal     Ideal    
## [39979] Good      Ideal     Premium   Premium   Ideal     Premium  
## [39985] Premium   Ideal     Ideal     Ideal     Premium   Premium  
## [39991] Ideal     Ideal     Premium   Premium   Ideal     Ideal    
## [39997] Ideal     Premium   Ideal     Ideal     Premium   Ideal    
## [40003] Ideal     Premium   Very Good Ideal     Ideal     Premium  
## [40009] Premium   Ideal     Ideal     Ideal     Premium   Premium  
## [40015] Very Good Very Good Ideal     Very Good Good      Very Good
## [40021] Ideal     Ideal     Ideal     Premium   Very Good Fair     
## [40027] Very Good Premium   Ideal     Ideal     Ideal     Ideal    
## [40033] Premium   Good      Very Good Very Good Very Good Premium  
## [40039] Good      Very Good Ideal     Ideal     Good      Very Good
## [40045] Premium   Good      Ideal     Ideal     Ideal     Premium  
## [40051] Premium   Premium   Premium   Premium   Premium   Ideal    
## [40057] Ideal     Ideal     Ideal     Good      Premium   Premium  
## [40063] Premium   Premium   Premium   Premium   Premium   Premium  
## [40069] Premium   Premium   Premium   Very Good Ideal     Ideal    
## [40075] Ideal     Ideal     Premium   Premium   Ideal     Premium  
## [40081] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [40087] Premium   Premium   Premium   Premium   Ideal     Premium  
## [40093] Premium   Ideal     Ideal     Ideal     Ideal     Ideal    
## [40099] Premium   Ideal     Ideal     Ideal     Premium   Premium  
## [40105] Premium   Premium   Fair      Fair      Fair      Ideal    
## [40111] Ideal     Premium   Very Good Ideal     Ideal     Premium  
## [40117] Ideal     Premium   Ideal     Ideal     Premium   Very Good
## [40123] Ideal     Ideal     Premium   Ideal     Ideal     Ideal    
## [40129] Ideal     Premium   Premium   Ideal     Ideal     Premium  
## [40135] Premium   Ideal     Ideal     Premium   Very Good Fair     
## [40141] Premium   Very Good Very Good Very Good Premium   Good     
## [40147] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [40153] Premium   Ideal     Premium   Ideal     Premium   Ideal    
## [40159] Ideal     Premium   Good      Ideal     Premium   Premium  
## [40165] Premium   Very Good Very Good Ideal     Very Good Premium  
## [40171] Premium   Premium   Premium   Premium   Ideal     Ideal    
## [40177] Ideal     Ideal     Premium   Very Good Premium   Good     
## [40183] Good      Ideal     Ideal     Good      Very Good Very Good
## [40189] Ideal     Ideal     Ideal     Good      Good      Premium  
## [40195] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [40201] Premium   Very Good Very Good Very Good Good      Very Good
## [40207] Premium   Ideal     Ideal     Premium   Premium   Fair     
## [40213] Ideal     Ideal     Premium   Ideal     Very Good Premium  
## [40219] Ideal     Ideal     Very Good Premium   Very Good Very Good
## [40225] Premium   Premium   Premium   Ideal     Ideal     Ideal    
## [40231] Premium   Ideal     Ideal     Premium   Very Good Ideal    
## [40237] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [40243] Ideal     Ideal     Ideal     Ideal     Premium   Good     
## [40249] Very Good Premium   Premium   Ideal     Ideal     Ideal    
## [40255] Ideal     Ideal     Premium   Ideal     Premium   Premium  
## [40261] Premium   Premium   Very Good Ideal     Very Good Very Good
## [40267] Ideal     Very Good Very Good Ideal     Very Good Ideal    
## [40273] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [40279] Very Good Ideal     Ideal     Ideal     Ideal     Ideal    
## [40285] Ideal     Ideal     Ideal     Very Good Ideal     Ideal    
## [40291] Ideal     Very Good Ideal     Ideal     Ideal     Ideal    
## [40297] Ideal     Ideal     Ideal     Premium   Premium   Premium  
## [40303] Ideal     Ideal     Premium   Ideal     Premium   Premium  
## [40309] Premium   Ideal     Premium   Ideal     Premium   Premium  
## [40315] Premium   Premium   Ideal     Ideal     Premium   Ideal    
## [40321] Very Good Premium   Ideal     Ideal     Premium   Ideal    
## [40327] Ideal     Good      Good      Fair      Ideal     Very Good
## [40333] Ideal     Ideal     Very Good Good      Ideal     Very Good
## [40339] Very Good Premium   Premium   Ideal     Premium   Ideal    
## [40345] Ideal     Good      Ideal     Premium   Ideal     Ideal    
## [40351] Ideal     Premium   Premium   Ideal     Premium   Premium  
## [40357] Premium   Premium   Premium   Ideal     Premium   Ideal    
## [40363] Ideal     Premium   Good      Ideal     Ideal     Ideal    
## [40369] Ideal     Ideal     Good      Premium   Very Good Premium  
## [40375] Very Good Good      Ideal     Ideal     Ideal     Ideal    
## [40381] Ideal     Ideal     Premium   Good      Very Good Ideal    
## [40387] Ideal     Ideal     Premium   Good      Very Good Ideal    
## [40393] Ideal     Very Good Very Good Very Good Very Good Ideal    
## [40399] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [40405] Ideal     Ideal     Ideal     Premium   Very Good Fair     
## [40411] Premium   Ideal     Ideal     Ideal     Good      Good     
## [40417] Ideal     Ideal     Good      Fair      Fair      Fair     
## [40423] Fair      Good      Ideal     Very Good Ideal     Ideal    
## [40429] Good      Ideal     Ideal     Ideal     Good      Fair     
## [40435] Ideal     Good      Premium   Ideal     Good      Fair     
## [40441] Good      Premium   Good      Premium   Premium   Fair     
## [40447] Very Good Ideal     Premium   Very Good Very Good Fair     
## [40453] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [40459] Ideal     Ideal     Good      Ideal     Ideal     Ideal    
## [40465] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [40471] Ideal     Premium   Premium   Good      Premium   Premium  
## [40477] Very Good Premium   Good      Good      Very Good Ideal    
## [40483] Ideal     Good      Good      Premium   Fair      Very Good
## [40489] Ideal     Premium   Ideal     Ideal     Premium   Ideal    
## [40495] Premium   Ideal     Ideal     Ideal     Ideal     Premium  
## [40501] Premium   Ideal     Very Good Very Good Premium   Ideal    
## [40507] Ideal     Premium   Very Good Ideal     Good      Good     
## [40513] Very Good Good      Good      Ideal     Ideal     Ideal    
## [40519] Ideal     Premium   Very Good Good      Ideal     Ideal    
## [40525] Very Good Very Good Good      Good      Ideal     Ideal    
## [40531] Fair      Fair      Very Good Premium   Ideal     Ideal    
## [40537] Ideal     Ideal     Premium   Premium   Ideal     Premium  
## [40543] Premium   Very Good Very Good Good      Ideal     Ideal    
## [40549] Good      Ideal     Very Good Ideal     Ideal     Very Good
## [40555] Ideal     Ideal     Ideal     Ideal     Very Good Very Good
## [40561] Very Good Ideal     Very Good Ideal     Ideal     Premium  
## [40567] Ideal     Ideal     Ideal     Ideal     Very Good Ideal    
## [40573] Premium   Very Good Premium   Ideal     Ideal     Very Good
## [40579] Very Good Ideal     Ideal     Ideal     Premium   Premium  
## [40585] Premium   Premium   Premium   Ideal     Premium   Premium  
## [40591] Ideal     Fair      Ideal     Ideal     Ideal     Ideal    
## [40597] Ideal     Fair      Premium   Ideal     Very Good Very Good
## [40603] Very Good Very Good Ideal     Ideal     Ideal     Ideal    
## [40609] Ideal     Ideal     Ideal     Ideal     Good      Good     
## [40615] Good      Very Good Very Good Very Good Very Good Very Good
## [40621] Ideal     Premium   Ideal     Ideal     Ideal     Ideal    
## [40627] Ideal     Premium   Premium   Ideal     Ideal     Ideal    
## [40633] Ideal     Ideal     Ideal     Premium   Premium   Premium  
## [40639] Premium   Premium   Ideal     Ideal     Premium   Very Good
## [40645] Very Good Very Good Ideal     Very Good Premium   Ideal    
## [40651] Very Good Ideal     Premium   Ideal     Ideal     Very Good
## [40657] Very Good Good      Premium   Ideal     Ideal     Ideal    
## [40663] Ideal     Ideal     Ideal     Good      Ideal     Ideal    
## [40669] Premium   Premium   Premium   Premium   Ideal     Fair     
## [40675] Premium   Premium   Good      Good      Premium   Premium  
## [40681] Ideal     Good      Very Good Premium   Good      Good     
## [40687] Premium   Fair      Premium   Fair      Very Good Good     
## [40693] Good      Ideal     Good      Ideal     Ideal     Ideal    
## [40699] Ideal     Ideal     Ideal     Very Good Ideal     Ideal    
## [40705] Ideal     Ideal     Ideal     Fair      Fair      Premium  
## [40711] Good      Premium   Premium   Very Good Very Good Very Good
## [40717] Very Good Fair      Ideal     Ideal     Ideal     Ideal    
## [40723] Ideal     Very Good Very Good Very Good Very Good Ideal    
## [40729] Ideal     Ideal     Very Good Ideal     Ideal     Ideal    
## [40735] Very Good Ideal     Ideal     Ideal     Ideal     Good     
## [40741] Good      Very Good Ideal     Ideal     Ideal     Ideal    
## [40747] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [40753] Premium   Premium   Premium   Ideal     Ideal     Ideal    
## [40759] Premium   Premium   Very Good Good      Very Good Ideal    
## [40765] Very Good Premium   Fair      Ideal     Ideal     Ideal    
## [40771] Good      Fair      Good      Premium   Very Good Very Good
## [40777] Very Good Very Good Premium   Ideal     Premium   Good     
## [40783] Very Good Good      Ideal     Ideal     Ideal     Ideal    
## [40789] Ideal     Ideal     Ideal     Ideal     Ideal     Good     
## [40795] Good      Very Good Good      Ideal     Premium   Ideal    
## [40801] Ideal     Ideal     Ideal     Good      Ideal     Premium  
## [40807] Premium   Very Good Very Good Good      Ideal     Ideal    
## [40813] Ideal     Good      Ideal     Ideal     Ideal     Ideal    
## [40819] Premium   Ideal     Ideal     Premium   Ideal     Premium  
## [40825] Ideal     Very Good Premium   Very Good Very Good Very Good
## [40831] Ideal     Very Good Ideal     Very Good Good      Good     
## [40837] Ideal     Good      Premium   Premium   Very Good Very Good
## [40843] Very Good Very Good Very Good Premium   Premium   Ideal    
## [40849] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [40855] Ideal     Very Good Very Good Ideal     Ideal     Ideal    
## [40861] Ideal     Fair      Very Good Very Good Very Good Ideal    
## [40867] Ideal     Ideal     Good      Premium   Ideal     Ideal    
## [40873] Good      Fair      Fair      Very Good Ideal     Ideal    
## [40879] Ideal     Ideal     Very Good Ideal     Ideal     Ideal    
## [40885] Premium   Premium   Premium   Premium   Premium   Fair     
## [40891] Fair      Ideal     Premium   Ideal     Ideal     Ideal    
## [40897] Ideal     Ideal     Ideal     Very Good Ideal     Ideal    
## [40903] Ideal     Ideal     Very Good Premium   Very Good Very Good
## [40909] Very Good Ideal     Good      Premium   Ideal     Ideal    
## [40915] Ideal     Premium   Premium   Ideal     Ideal     Ideal    
## [40921] Very Good Very Good Ideal     Ideal     Ideal     Ideal    
## [40927] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [40933] Ideal     Ideal     Good      Good      Ideal     Very Good
## [40939] Very Good Very Good Ideal     Ideal     Ideal     Premium  
## [40945] Very Good Very Good Good      Good      Good      Good     
## [40951] Premium   Ideal     Ideal     Ideal     Premium   Ideal    
## [40957] Ideal     Ideal     Premium   Very Good Ideal     Ideal    
## [40963] Premium   Good      Ideal     Premium   Very Good Very Good
## [40969] Good      Good      Good      Ideal     Ideal     Ideal    
## [40975] Ideal     Good      Premium   Ideal     Ideal     Premium  
## [40981] Fair      Very Good Very Good Ideal     Good      Premium  
## [40987] Ideal     Ideal     Premium   Premium   Ideal     Premium  
## [40993] Premium   Premium   Premium   Premium   Very Good Very Good
## [40999] Ideal     Good      Very Good Ideal     Ideal     Ideal    
## [41005] Ideal     Ideal     Very Good Very Good Ideal     Premium  
## [41011] Very Good Ideal     Ideal     Ideal     Very Good Good     
## [41017] Ideal     Ideal     Premium   Premium   Premium   Very Good
## [41023] Very Good Very Good Very Good Ideal     Ideal     Ideal    
## [41029] Ideal     Premium   Ideal     Ideal     Ideal     Ideal    
## [41035] Ideal     Very Good Ideal     Premium   Ideal     Ideal    
## [41041] Ideal     Ideal     Very Good Ideal     Ideal     Ideal    
## [41047] Ideal     Ideal     Ideal     Premium   Fair      Very Good
## [41053] Good      Very Good Very Good Ideal     Very Good Very Good
## [41059] Very Good Very Good Good      Premium   Good      Ideal    
## [41065] Very Good Premium   Ideal     Ideal     Ideal     Ideal    
## [41071] Ideal     Premium   Ideal     Ideal     Premium   Premium  
## [41077] Premium   Premium   Ideal     Ideal     Premium   Premium  
## [41083] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [41089] Ideal     Ideal     Ideal     Ideal     Very Good Very Good
## [41095] Good      Premium   Ideal     Ideal     Ideal     Ideal    
## [41101] Good      Fair      Very Good Very Good Very Good Very Good
## [41107] Ideal     Ideal     Ideal     Premium   Ideal     Premium  
## [41113] Premium   Good      Premium   Ideal     Premium   Ideal    
## [41119] Ideal     Ideal     Ideal     Ideal     Premium   Premium  
## [41125] Premium   Fair      Premium   Premium   Very Good Very Good
## [41131] Ideal     Ideal     Very Good Very Good Good      Very Good
## [41137] Very Good Good      Good      Very Good Very Good Very Good
## [41143] Very Good Ideal     Very Good Good      Very Good Ideal    
## [41149] Ideal     Ideal     Fair      Good      Very Good Very Good
## [41155] Ideal     Ideal     Good      Good      Very Good Ideal    
## [41161] Ideal     Ideal     Very Good Very Good Ideal     Very Good
## [41167] Ideal     Premium   Premium   Ideal     Ideal     Premium  
## [41173] Ideal     Ideal     Ideal     Ideal     Premium   Premium  
## [41179] Premium   Premium   Premium   Premium   Premium   Ideal    
## [41185] Ideal     Ideal     Ideal     Very Good Premium   Ideal    
## [41191] Ideal     Ideal     Ideal     Good      Very Good Good     
## [41197] Very Good Ideal     Very Good Ideal     Ideal     Very Good
## [41203] Good      Ideal     Ideal     Ideal     Ideal     Premium  
## [41209] Premium   Ideal     Premium   Premium   Premium   Ideal    
## [41215] Ideal     Ideal     Premium   Premium   Premium   Ideal    
## [41221] Very Good Ideal     Ideal     Premium   Premium   Ideal    
## [41227] Ideal     Very Good Premium   Very Good Ideal     Ideal    
## [41233] Ideal     Ideal     Ideal     Premium   Ideal     Premium  
## [41239] Premium   Very Good Premium   Premium   Fair      Premium  
## [41245] Ideal     Ideal     Premium   Ideal     Premium   Very Good
## [41251] Very Good Very Good Very Good Very Good Ideal     Very Good
## [41257] Premium   Very Good Premium   Ideal     Premium   Very Good
## [41263] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [41269] Ideal     Ideal     Good      Ideal     Ideal     Ideal    
## [41275] Ideal     Ideal     Ideal     Good      Very Good Very Good
## [41281] Very Good Ideal     Ideal     Premium   Premium   Premium  
## [41287] Ideal     Ideal     Very Good Good      Ideal     Ideal    
## [41293] Good      Premium   Ideal     Ideal     Ideal     Ideal    
## [41299] Premium   Ideal     Ideal     Ideal     Ideal     Ideal    
## [41305] Ideal     Ideal     Ideal     Ideal     Premium   Very Good
## [41311] Good      Good      Ideal     Ideal     Ideal     Ideal    
## [41317] Very Good Very Good Very Good Good      Ideal     Ideal    
## [41323] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [41329] Ideal     Ideal     Good      Good      Good      Good     
## [41335] Ideal     Fair      Premium   Very Good Ideal     Ideal    
## [41341] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [41347] Very Good Very Good Premium   Good      Good      Premium  
## [41353] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [41359] Very Good Very Good Ideal     Premium   Very Good Premium  
## [41365] Premium   Premium   Very Good Very Good Very Good Very Good
## [41371] Good      Good      Very Good Premium   Good      Good     
## [41377] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [41383] Ideal     Premium   Ideal     Ideal     Ideal     Ideal    
## [41389] Ideal     Ideal     Premium   Premium   Premium   Very Good
## [41395] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [41401] Ideal     Ideal     Good      Very Good Ideal     Very Good
## [41407] Good      Very Good Premium   Premium   Fair      Very Good
## [41413] Good      Very Good Ideal     Ideal     Ideal     Good     
## [41419] Premium   Ideal     Premium   Very Good Ideal     Ideal    
## [41425] Ideal     Very Good Good      Very Good Very Good Premium  
## [41431] Premium   Premium   Fair      Good      Premium   Ideal    
## [41437] Ideal     Ideal     Good      Good      Ideal     Fair     
## [41443] Very Good Premium   Premium   Ideal     Ideal     Ideal    
## [41449] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [41455] Ideal     Good      Premium   Premium   Premium   Very Good
## [41461] Premium   Ideal     Ideal     Ideal     Ideal     Premium  
## [41467] Premium   Premium   Ideal     Ideal     Ideal     Ideal    
## [41473] Ideal     Ideal     Very Good Premium   Good      Premium  
## [41479] Premium   Very Good Very Good Premium   Ideal     Very Good
## [41485] Very Good Very Good Premium   Ideal     Ideal     Ideal    
## [41491] Ideal     Good      Good      Premium   Fair      Very Good
## [41497] Ideal     Ideal     Ideal     Premium   Premium   Very Good
## [41503] Ideal     Fair      Premium   Fair      Very Good Very Good
## [41509] Very Good Very Good Ideal     Premium   Ideal     Ideal    
## [41515] Ideal     Ideal     Ideal     Ideal     Very Good Premium  
## [41521] Ideal     Ideal     Ideal     Premium   Ideal     Premium  
## [41527] Ideal     Premium   Ideal     Ideal     Ideal     Premium  
## [41533] Good      Ideal     Ideal     Ideal     Ideal     Ideal    
## [41539] Ideal     Premium   Ideal     Premium   Good      Premium  
## [41545] Good      Premium   Good      Fair      Very Good Ideal    
## [41551] Very Good Ideal     Ideal     Ideal     Very Good Very Good
## [41557] Very Good Very Good Very Good Premium   Ideal     Ideal    
## [41563] Ideal     Fair      Premium   Very Good Premium   Premium  
## [41569] Premium   Very Good Ideal     Very Good Ideal     Ideal    
## [41575] Ideal     Ideal     Fair      Premium   Very Good Premium  
## [41581] Very Good Very Good Very Good Very Good Very Good Very Good
## [41587] Ideal     Ideal     Very Good Very Good Premium   Very Good
## [41593] Very Good Very Good Very Good Ideal     Ideal     Ideal    
## [41599] Ideal     Good      Very Good Very Good Ideal     Very Good
## [41605] Ideal     Very Good Very Good Premium   Premium   Good     
## [41611] Very Good Very Good Very Good Very Good Very Good Very Good
## [41617] Very Good Very Good Very Good Very Good Very Good Ideal    
## [41623] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [41629] Ideal     Good      Premium   Very Good Very Good Ideal    
## [41635] Ideal     Premium   Ideal     Premium   Premium   Premium  
## [41641] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [41647] Ideal     Fair      Ideal     Good      Fair      Ideal    
## [41653] Very Good Very Good Very Good Ideal     Ideal     Ideal    
## [41659] Very Good Good      Ideal     Ideal     Ideal     Ideal    
## [41665] Very Good Good      Ideal     Ideal     Ideal     Ideal    
## [41671] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [41677] Ideal     Ideal     Ideal     Very Good Ideal     Premium  
## [41683] Ideal     Ideal     Premium   Premium   Ideal     Premium  
## [41689] Premium   Premium   Ideal     Premium   Ideal     Good     
## [41695] Good      Premium   Very Good Premium   Good      Premium  
## [41701] Premium   Good      Very Good Good      Premium   Good     
## [41707] Premium   Premium   Very Good Very Good Very Good Ideal    
## [41713] Ideal     Ideal     Ideal     Good      Good      Ideal    
## [41719] Ideal     Ideal     Ideal     Very Good Very Good Ideal    
## [41725] Ideal     Ideal     Ideal     Premium   Ideal     Ideal    
## [41731] Ideal     Ideal     Ideal     Ideal     Premium   Ideal    
## [41737] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [41743] Premium   Very Good Very Good Good      Good      Good     
## [41749] Good      Very Good Very Good Very Good Premium   Very Good
## [41755] Ideal     Ideal     Premium   Very Good Ideal     Very Good
## [41761] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [41767] Ideal     Good      Very Good Ideal     Ideal     Ideal    
## [41773] Good      Premium   Ideal     Good      Very Good Very Good
## [41779] Premium   Ideal     Premium   Ideal     Ideal     Premium  
## [41785] Very Good Ideal     Ideal     Ideal     Premium   Premium  
## [41791] Very Good Very Good Good      Premium   Premium   Fair     
## [41797] Ideal     Very Good Very Good Very Good Very Good Very Good
## [41803] Very Good Very Good Very Good Premium   Premium   Ideal    
## [41809] Ideal     Ideal     Good      Ideal     Premium   Very Good
## [41815] Ideal     Ideal     Ideal     Premium   Ideal     Ideal    
## [41821] Fair      Ideal     Ideal     Premium   Very Good Very Good
## [41827] Ideal     Ideal     Ideal     Ideal     Ideal     Good     
## [41833] Premium   Ideal     Fair      Premium   Premium   Ideal    
## [41839] Premium   Very Good Ideal     Good      Ideal     Ideal    
## [41845] Good      Very Good Very Good Good      Ideal     Ideal    
## [41851] Ideal     Ideal     Ideal     Good      Ideal     Premium  
## [41857] Good      Ideal     Very Good Very Good Very Good Ideal    
## [41863] Ideal     Ideal     Ideal     Premium   Ideal     Premium  
## [41869] Ideal     Ideal     Premium   Premium   Ideal     Premium  
## [41875] Very Good Ideal     Fair      Fair      Good      Premium  
## [41881] Very Good Good      Ideal     Ideal     Ideal     Ideal    
## [41887] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [41893] Good      Good      Very Good Good      Very Good Ideal    
## [41899] Ideal     Ideal     Ideal     Very Good Very Good Very Good
## [41905] Very Good Very Good Very Good Very Good Ideal     Ideal    
## [41911] Ideal     Premium   Very Good Very Good Very Good Very Good
## [41917] Very Good Good      Fair      Good      Good      Ideal    
## [41923] Very Good Premium   Ideal     Ideal     Premium   Premium  
## [41929] Ideal     Ideal     Very Good Very Good Ideal     Premium  
## [41935] Premium   Very Good Ideal     Premium   Premium   Ideal    
## [41941] Very Good Very Good Premium   Very Good Premium   Very Good
## [41947] Good      Ideal     Good      Very Good Very Good Very Good
## [41953] Very Good Very Good Very Good Very Good Very Good Very Good
## [41959] Very Good Very Good Very Good Very Good Very Good Very Good
## [41965] Very Good Very Good Very Good Very Good Very Good Very Good
## [41971] Ideal     Premium   Ideal     Good      Ideal     Premium  
## [41977] Good      Ideal     Ideal     Ideal     Ideal     Ideal    
## [41983] Very Good Ideal     Ideal     Premium   Ideal     Premium  
## [41989] Premium   Ideal     Premium   Premium   Premium   Ideal    
## [41995] Premium   Ideal     Ideal     Ideal     Ideal     Ideal    
## [42001] Premium   Very Good Very Good Very Good Very Good Premium  
## [42007] Ideal     Ideal     Premium   Premium   Premium   Very Good
## [42013] Good      Ideal     Ideal     Ideal     Ideal     Ideal    
## [42019] Premium   Very Good Very Good Very Good Very Good Very Good
## [42025] Very Good Very Good Ideal     Ideal     Ideal     Premium  
## [42031] Good      Good      Ideal     Good      Good      Premium  
## [42037] Very Good Ideal     Ideal     Ideal     Ideal     Ideal    
## [42043] Good      Good      Good      Ideal     Very Good Premium  
## [42049] Ideal     Ideal     Good      Premium   Very Good Very Good
## [42055] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [42061] Premium   Very Good Very Good Ideal     Very Good Premium  
## [42067] Premium   Fair      Ideal     Premium   Very Good Good     
## [42073] Good      Ideal     Premium   Ideal     Ideal     Very Good
## [42079] Very Good Very Good Ideal     Premium   Premium   Ideal    
## [42085] Premium   Ideal     Ideal     Ideal     Ideal     Ideal    
## [42091] Ideal     Fair      Premium   Very Good Ideal     Premium  
## [42097] Ideal     Ideal     Very Good Ideal     Premium   Ideal    
## [42103] Good      Good      Fair      Very Good Ideal     Ideal    
## [42109] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [42115] Very Good Ideal     Very Good Good      Very Good Fair     
## [42121] Good      Ideal     Ideal     Ideal     Ideal     Premium  
## [42127] Ideal     Ideal     Premium   Ideal     Ideal     Ideal    
## [42133] Premium   Premium   Premium   Good      Ideal     Ideal    
## [42139] Good      Premium   Premium   Fair      Premium   Very Good
## [42145] Ideal     Ideal     Premium   Very Good Premium   Ideal    
## [42151] Ideal     Ideal     Good      Good      Premium   Very Good
## [42157] Very Good Very Good Very Good Very Good Very Good Ideal    
## [42163] Ideal     Ideal     Ideal     Premium   Ideal     Premium  
## [42169] Very Good Very Good Premium   Very Good Ideal     Premium  
## [42175] Ideal     Ideal     Ideal     Ideal     Good      Premium  
## [42181] Premium   Premium   Very Good Premium   Good      Ideal    
## [42187] Very Good Good      Premium   Premium   Premium   Fair     
## [42193] Premium   Premium   Premium   Very Good Good      Good     
## [42199] Ideal     Premium   Ideal     Ideal     Good      Very Good
## [42205] Very Good Good      Very Good Very Good Good      Very Good
## [42211] Ideal     Very Good Good      Ideal     Ideal     Ideal    
## [42217] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [42223] Ideal     Premium   Ideal     Premium   Premium   Premium  
## [42229] Premium   Premium   Premium   Very Good Very Good Ideal    
## [42235] Ideal     Ideal     Very Good Good      Good      Very Good
## [42241] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [42247] Ideal     Ideal     Ideal     Ideal     Ideal     Very Good
## [42253] Very Good Good      Good      Good      Fair      Premium  
## [42259] Good      Good      Good      Good      Premium   Premium  
## [42265] Ideal     Ideal     Ideal     Ideal     Premium   Premium  
## [42271] Very Good Very Good Very Good Very Good Very Good Very Good
## [42277] Very Good Very Good Very Good Ideal     Ideal     Ideal    
## [42283] Ideal     Ideal     Ideal     Ideal     Good      Good     
## [42289] Good      Premium   Premium   Ideal     Very Good Very Good
## [42295] Good      Very Good Ideal     Ideal     Premium   Very Good
## [42301] Premium   Ideal     Ideal     Ideal     Very Good Very Good
## [42307] Premium   Ideal     Ideal     Ideal     Ideal     Ideal    
## [42313] Premium   Premium   Ideal     Premium   Ideal     Very Good
## [42319] Ideal     Ideal     Ideal     Ideal     Very Good Ideal    
## [42325] Ideal     Very Good Ideal     Good      Ideal     Ideal    
## [42331] Ideal     Premium   Ideal     Ideal     Ideal     Ideal    
## [42337] Premium   Ideal     Very Good Very Good Ideal     Good     
## [42343] Ideal     Ideal     Good      Ideal     Ideal     Ideal    
## [42349] Premium   Premium   Ideal     Ideal     Ideal     Ideal    
## [42355] Ideal     Very Good Ideal     Very Good Very Good Premium  
## [42361] Premium   Good      Ideal     Ideal     Premium   Ideal    
## [42367] Very Good Very Good Good      Premium   Fair      Ideal    
## [42373] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [42379] Very Good Ideal     Premium   Fair      Ideal     Very Good
## [42385] Ideal     Ideal     Ideal     Good      Good      Premium  
## [42391] Very Good Ideal     Ideal     Good      Premium   Premium  
## [42397] Premium   Very Good Premium   Good      Ideal     Ideal    
## [42403] Ideal     Ideal     Premium   Very Good Premium   Very Good
## [42409] Ideal     Ideal     Ideal     Good      Very Good Very Good
## [42415] Very Good Very Good Very Good Very Good Ideal     Ideal    
## [42421] Premium   Premium   Good      Ideal     Good      Very Good
## [42427] Good      Ideal     Very Good Ideal     Ideal     Very Good
## [42433] Ideal     Premium   Ideal     Ideal     Ideal     Very Good
## [42439] Ideal     Ideal     Ideal     Ideal     Premium   Premium  
## [42445] Very Good Very Good Good      Ideal     Good      Ideal    
## [42451] Good      Good      Ideal     Ideal     Good      Very Good
## [42457] Premium   Good      Premium   Good      Very Good Ideal    
## [42463] Very Good Good      Very Good Premium   Premium   Ideal    
## [42469] Very Good Ideal     Ideal     Very Good Very Good Ideal    
## [42475] Ideal     Very Good Very Good Very Good Ideal     Ideal    
## [42481] Ideal     Ideal     Good      Good      Premium   Good     
## [42487] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [42493] Very Good Very Good Very Good Very Good Very Good Very Good
## [42499] Ideal     Ideal     Premium   Premium   Very Good Premium  
## [42505] Premium   Ideal     Very Good Very Good Ideal     Ideal    
## [42511] Ideal     Ideal     Good      Very Good Premium   Ideal    
## [42517] Ideal     Fair      Fair      Fair      Fair      Premium  
## [42523] Premium   Premium   Premium   Premium   Ideal     Ideal    
## [42529] Premium   Ideal     Ideal     Ideal     Premium   Ideal    
## [42535] Premium   Premium   Ideal     Ideal     Ideal     Premium  
## [42541] Premium   Good      Premium   Premium   Ideal     Ideal    
## [42547] Fair      Fair      Very Good Ideal     Very Good Very Good
## [42553] Ideal     Very Good Premium   Ideal     Ideal     Ideal    
## [42559] Ideal     Ideal     Ideal     Ideal     Ideal     Very Good
## [42565] Very Good Ideal     Ideal     Ideal     Ideal     Ideal    
## [42571] Ideal     Premium   Ideal     Fair      Good      Very Good
## [42577] Good      Premium   Ideal     Ideal     Ideal     Ideal    
## [42583] Very Good Ideal     Ideal     Very Good Ideal     Ideal    
## [42589] Ideal     Premium   Very Good Very Good Very Good Very Good
## [42595] Very Good Ideal     Ideal     Ideal     Ideal     Good     
## [42601] Premium   Ideal     Good      Ideal     Very Good Premium  
## [42607] Very Good Premium   Very Good Premium   Ideal     Very Good
## [42613] Premium   Premium   Good      Very Good Ideal     Premium  
## [42619] Premium   Ideal     Ideal     Very Good Very Good Premium  
## [42625] Very Good Ideal     Premium   Very Good Ideal     Ideal    
## [42631] Ideal     Good      Premium   Premium   Premium   Very Good
## [42637] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [42643] Premium   Fair      Premium   Ideal     Ideal     Very Good
## [42649] Ideal     Good      Premium   Ideal     Very Good Premium  
## [42655] Premium   Ideal     Ideal     Very Good Good      Ideal    
## [42661] Ideal     Ideal     Ideal     Ideal     Good      Ideal    
## [42667] Ideal     Premium   Ideal     Ideal     Ideal     Fair     
## [42673] Fair      Fair      Premium   Ideal     Very Good Very Good
## [42679] Ideal     Good      Very Good Very Good Very Good Very Good
## [42685] Very Good Very Good Very Good Ideal     Ideal     Ideal    
## [42691] Ideal     Ideal     Good      Ideal     Very Good Ideal    
## [42697] Ideal     Premium   Very Good Good      Very Good Premium  
## [42703] Premium   Good      Premium   Very Good Ideal     Good     
## [42709] Very Good Premium   Ideal     Very Good Ideal     Good     
## [42715] Ideal     Ideal     Ideal     Fair      Very Good Premium  
## [42721] Premium   Ideal     Ideal     Good      Premium   Ideal    
## [42727] Ideal     Ideal     Ideal     Ideal     Very Good Very Good
## [42733] Very Good Ideal     Ideal     Ideal     Very Good Good     
## [42739] Ideal     Ideal     Very Good Ideal     Good      Good     
## [42745] Premium   Good      Good      Premium   Good      Very Good
## [42751] Very Good Ideal     Ideal     Ideal     Ideal     Very Good
## [42757] Premium   Very Good Premium   Ideal     Very Good Ideal    
## [42763] Premium   Very Good Very Good Ideal     Ideal     Ideal    
## [42769] Ideal     Ideal     Premium   Very Good Ideal     Ideal    
## [42775] Good      Fair      Premium   Very Good Very Good Very Good
## [42781] Premium   Good      Ideal     Good      Premium   Good     
## [42787] Premium   Very Good Ideal     Ideal     Good      Good     
## [42793] Ideal     Ideal     Good      Fair      Premium   Very Good
## [42799] Very Good Very Good Ideal     Ideal     Premium   Ideal    
## [42805] Good      Ideal     Ideal     Ideal     Ideal     Ideal    
## [42811] Ideal     Ideal     Good      Very Good Very Good Very Good
## [42817] Very Good Premium   Very Good Ideal     Ideal     Ideal    
## [42823] Ideal     Good      Premium   Good      Ideal     Ideal    
## [42829] Premium   Premium   Very Good Very Good Ideal     Ideal    
## [42835] Ideal     Premium   Ideal     Premium   Ideal     Ideal    
## [42841] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [42847] Premium   Premium   Ideal     Ideal     Ideal     Ideal    
## [42853] Premium   Ideal     Premium   Ideal     Premium   Premium  
## [42859] Premium   Premium   Very Good Very Good Ideal     Ideal    
## [42865] Good      Very Good Ideal     Ideal     Ideal     Very Good
## [42871] Premium   Ideal     Ideal     Ideal     Ideal     Premium  
## [42877] Fair      Very Good Very Good Ideal     Ideal     Premium  
## [42883] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [42889] Ideal     Ideal     Premium   Ideal     Ideal     Ideal    
## [42895] Premium   Ideal     Ideal     Very Good Premium   Good     
## [42901] Good      Good      Good      Very Good Fair      Good     
## [42907] Premium   Premium   Very Good Premium   Ideal     Very Good
## [42913] Ideal     Ideal     Good      Good      Ideal     Good     
## [42919] Good      Ideal     Premium   Fair      Premium   Very Good
## [42925] Good      Ideal     Good      Good      Very Good Ideal    
## [42931] Premium   Ideal     Premium   Very Good Very Good Premium  
## [42937] Ideal     Premium   Good      Very Good Ideal     Premium  
## [42943] Premium   Premium   Premium   Very Good Very Good Premium  
## [42949] Very Good Ideal     Premium   Premium   Premium   Premium  
## [42955] Very Good Premium   Very Good Premium   Good      Premium  
## [42961] Ideal     Premium   Very Good Ideal     Ideal     Ideal    
## [42967] Good      Very Good Ideal     Premium   Ideal     Ideal    
## [42973] Ideal     Ideal     Premium   Very Good Premium   Ideal    
## [42979] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [42985] Ideal     Ideal     Fair      Fair      Good      Fair     
## [42991] Very Good Very Good Ideal     Ideal     Ideal     Ideal    
## [42997] Ideal     Very Good Very Good Ideal     Ideal     Ideal    
## [43003] Premium   Very Good Very Good Very Good Very Good Ideal    
## [43009] Very Good Premium   Ideal     Ideal     Very Good Very Good
## [43015] Very Good Ideal     Ideal     Ideal     Ideal     Ideal    
## [43021] Premium   Ideal     Ideal     Ideal     Very Good Premium  
## [43027] Premium   Ideal     Ideal     Very Good Very Good Fair     
## [43033] Very Good Good      Premium   Ideal     Ideal     Ideal    
## [43039] Ideal     Ideal     Premium   Very Good Very Good Ideal    
## [43045] Ideal     Ideal     Premium   Very Good Very Good Very Good
## [43051] Premium   Ideal     Ideal     Good      Ideal     Ideal    
## [43057] Ideal     Very Good Ideal     Ideal     Ideal     Ideal    
## [43063] Ideal     Ideal     Ideal     Ideal     Good      Premium  
## [43069] Very Good Very Good Very Good Premium   Very Good Very Good
## [43075] Very Good Ideal     Ideal     Good      Premium   Ideal    
## [43081] Ideal     Premium   Very Good Very Good Very Good Ideal    
## [43087] Very Good Premium   Premium   Ideal     Premium   Ideal    
## [43093] Very Good Ideal     Premium   Premium   Ideal     Ideal    
## [43099] Fair      Ideal     Premium   Very Good Ideal     Ideal    
## [43105] Ideal     Good      Very Good Ideal     Ideal     Ideal    
## [43111] Ideal     Ideal     Ideal     Ideal     Ideal     Very Good
## [43117] Good      Good      Ideal     Premium   Ideal     Premium  
## [43123] Premium   Ideal     Very Good Very Good Ideal     Very Good
## [43129] Very Good Very Good Premium   Premium   Good      Good     
## [43135] Premium   Very Good Good      Ideal     Ideal     Good     
## [43141] Ideal     Ideal     Premium   Very Good Very Good Very Good
## [43147] Very Good Very Good Ideal     Fair      Ideal     Ideal    
## [43153] Ideal     Ideal     Ideal     Good      Premium   Very Good
## [43159] Ideal     Very Good Ideal     Ideal     Premium   Ideal    
## [43165] Ideal     Ideal     Ideal     Ideal     Premium   Ideal    
## [43171] Premium   Premium   Premium   Premium   Premium   Very Good
## [43177] Good      Ideal     Ideal     Very Good Ideal     Very Good
## [43183] Very Good Very Good Very Good Very Good Ideal     Very Good
## [43189] Very Good Ideal     Ideal     Ideal     Very Good Ideal    
## [43195] Ideal     Ideal     Good      Premium   Good      Very Good
## [43201] Good      Premium   Very Good Premium   Ideal     Ideal    
## [43207] Ideal     Ideal     Good      Premium   Very Good Very Good
## [43213] Very Good Ideal     Ideal     Ideal     Fair      Fair     
## [43219] Good      Ideal     Ideal     Premium   Ideal     Premium  
## [43225] Ideal     Ideal     Ideal     Ideal     Premium   Ideal    
## [43231] Ideal     Premium   Very Good Very Good Fair      Ideal    
## [43237] Very Good Very Good Ideal     Ideal     Premium   Very Good
## [43243] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [43249] Premium   Ideal     Ideal     Premium   Premium   Ideal    
## [43255] Ideal     Ideal     Premium   Ideal     Ideal     Good     
## [43261] Very Good Very Good Premium   Very Good Premium   Premium  
## [43267] Ideal     Ideal     Premium   Premium   Good      Very Good
## [43273] Very Good Very Good Very Good Ideal     Good      Good     
## [43279] Very Good Premium   Premium   Premium   Premium   Premium  
## [43285] Very Good Ideal     Ideal     Ideal     Ideal     Ideal    
## [43291] Ideal     Very Good Very Good Very Good Ideal     Ideal    
## [43297] Ideal     Ideal     Ideal     Ideal     Ideal     Fair     
## [43303] Premium   Very Good Ideal     Very Good Very Good Very Good
## [43309] Very Good Premium   Premium   Good      Ideal     Ideal    
## [43315] Ideal     Ideal     Ideal     Good      Premium   Premium  
## [43321] Very Good Fair      Ideal     Very Good Very Good Ideal    
## [43327] Very Good Very Good Very Good Good      Very Good Ideal    
## [43333] Ideal     Ideal     Ideal     Ideal     Good      Very Good
## [43339] Ideal     Ideal     Premium   Premium   Good      Ideal    
## [43345] Good      Premium   Ideal     Ideal     Ideal     Premium  
## [43351] Very Good Good      Good      Good      Ideal     Good     
## [43357] Premium   Very Good Very Good Very Good Very Good Good     
## [43363] Very Good Very Good Ideal     Very Good Ideal     Ideal    
## [43369] Ideal     Ideal     Ideal     Ideal     Very Good Fair     
## [43375] Good      Ideal     Ideal     Premium   Premium   Very Good
## [43381] Ideal     Good      Ideal     Premium   Ideal     Good     
## [43387] Very Good Very Good Ideal     Ideal     Good      Ideal    
## [43393] Ideal     Ideal     Ideal     Ideal     Fair      Very Good
## [43399] Fair      Premium   Very Good Very Good Ideal     Ideal    
## [43405] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [43411] Ideal     Ideal     Premium   Very Good Ideal     Very Good
## [43417] Ideal     Ideal     Ideal     Premium   Very Good Premium  
## [43423] Premium   Premium   Fair      Fair      Very Good Very Good
## [43429] Very Good Very Good Very Good Very Good Premium   Premium  
## [43435] Ideal     Ideal     Good      Premium   Ideal     Premium  
## [43441] Good      Ideal     Premium   Premium   Premium   Premium  
## [43447] Premium   Good      Ideal     Premium   Very Good Very Good
## [43453] Good      Very Good Premium   Premium   Premium   Ideal    
## [43459] Very Good Very Good Very Good Very Good Ideal     Ideal    
## [43465] Ideal     Ideal     Ideal     Very Good Ideal     Ideal    
## [43471] Very Good Ideal     Ideal     Ideal     Good      Good     
## [43477] Very Good Very Good Premium   Premium   Fair      Premium  
## [43483] Very Good Ideal     Ideal     Ideal     Ideal     Premium  
## [43489] Ideal     Ideal     Premium   Premium   Premium   Premium  
## [43495] Ideal     Good      Premium   Good      Ideal     Very Good
## [43501] Ideal     Good      Ideal     Ideal     Premium   Ideal    
## [43507] Premium   Ideal     Very Good Very Good Very Good Good     
## [43513] Very Good Ideal     Good      Very Good Very Good Good     
## [43519] Ideal     Ideal     Ideal     Ideal     Very Good Very Good
## [43525] Ideal     Ideal     Very Good Very Good Very Good Good     
## [43531] Ideal     Premium   Very Good Ideal     Ideal     Ideal    
## [43537] Ideal     Ideal     Ideal     Ideal     Good      Good     
## [43543] Ideal     Premium   Premium   Ideal     Premium   Premium  
## [43549] Very Good Very Good Good      Ideal     Ideal     Ideal    
## [43555] Ideal     Ideal     Ideal     Ideal     Premium   Good     
## [43561] Good      Good      Fair      Ideal     Ideal     Ideal    
## [43567] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [43573] Premium   Ideal     Ideal     Ideal     Ideal     Premium  
## [43579] Premium   Ideal     Ideal     Ideal     Premium   Fair     
## [43585] Fair      Good      Very Good Premium   Ideal     Ideal    
## [43591] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [43597] Ideal     Ideal     Ideal     Ideal     Ideal     Very Good
## [43603] Good      Good      Good      Very Good Premium   Premium  
## [43609] Very Good Ideal     Very Good Good      Premium   Very Good
## [43615] Premium   Good      Very Good Ideal     Ideal     Ideal    
## [43621] Ideal     Premium   Very Good Ideal     Ideal     Ideal    
## [43627] Ideal     Premium   Very Good Premium   Premium   Premium  
## [43633] Ideal     Ideal     Very Good Premium   Very Good Ideal    
## [43639] Ideal     Premium   Good      Premium   Good      Ideal    
## [43645] Good      Premium   Premium   Premium   Good      Ideal    
## [43651] Ideal     Ideal     Ideal     Ideal     Ideal     Fair     
## [43657] Premium   Fair      Very Good Ideal     Good      Premium  
## [43663] Very Good Ideal     Very Good Ideal     Very Good Good     
## [43669] Ideal     Good      Very Good Premium   Very Good Very Good
## [43675] Very Good Very Good Ideal     Premium   Very Good Good     
## [43681] Very Good Ideal     Premium   Good      Good      Very Good
## [43687] Good      Good      Good      Very Good Very Good Good     
## [43693] Very Good Premium   Very Good Premium   Good      Good     
## [43699] Very Good Good      Very Good Very Good Very Good Good     
## [43705] Ideal     Fair      Very Good Premium   Premium   Premium  
## [43711] Ideal     Ideal     Ideal     Ideal     Good      Fair     
## [43717] Very Good Very Good Very Good Very Good Very Good Ideal    
## [43723] Ideal     Very Good Very Good Very Good Ideal     Very Good
## [43729] Ideal     Ideal     Very Good Ideal     Ideal     Ideal    
## [43735] Very Good Very Good Very Good Very Good Very Good Ideal    
## [43741] Ideal     Good      Very Good Ideal     Very Good Very Good
## [43747] Premium   Very Good Premium   Ideal     Very Good Very Good
## [43753] Ideal     Ideal     Premium   Ideal     Ideal     Premium  
## [43759] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [43765] Ideal     Ideal     Ideal     Ideal     Premium   Very Good
## [43771] Premium   Premium   Very Good Very Good Very Good Ideal    
## [43777] Good      Good      Fair      Ideal     Ideal     Ideal    
## [43783] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [43789] Ideal     Premium   Very Good Ideal     Very Good Very Good
## [43795] Good      Ideal     Ideal     Ideal     Ideal     Ideal    
## [43801] Ideal     Ideal     Ideal     Ideal     Ideal     Very Good
## [43807] Ideal     Premium   Premium   Ideal     Premium   Good     
## [43813] Ideal     Ideal     Ideal     Premium   Premium   Ideal    
## [43819] Ideal     Good      Premium   Ideal     Very Good Good     
## [43825] Very Good Premium   Premium   Very Good Good      Ideal    
## [43831] Premium   Very Good Very Good Ideal     Premium   Very Good
## [43837] Very Good Very Good Very Good Very Good Ideal     Ideal    
## [43843] Ideal     Ideal     Ideal     Ideal     Fair      Premium  
## [43849] Good      Good      Good      Good      Very Good Ideal    
## [43855] Premium   Ideal     Ideal     Ideal     Very Good Very Good
## [43861] Premium   Ideal     Ideal     Ideal     Ideal     Ideal    
## [43867] Ideal     Ideal     Ideal     Very Good Very Good Ideal    
## [43873] Ideal     Very Good Ideal     Ideal     Premium   Very Good
## [43879] Very Good Very Good Very Good Ideal     Very Good Ideal    
## [43885] Ideal     Ideal     Ideal     Very Good Ideal     Ideal    
## [43891] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [43897] Ideal     Premium   Ideal     Ideal     Ideal     Ideal    
## [43903] Ideal     Ideal     Ideal     Ideal     Very Good Very Good
## [43909] Very Good Ideal     Good      Premium   Good      Good     
## [43915] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [43921] Premium   Premium   Very Good Ideal     Ideal     Ideal    
## [43927] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [43933] Ideal     Ideal     Ideal     Good      Premium   Very Good
## [43939] Good      Good      Good      Ideal     Ideal     Ideal    
## [43945] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [43951] Premium   Very Good Good      Very Good Very Good Good     
## [43957] Ideal     Good      Premium   Ideal     Ideal     Ideal    
## [43963] Ideal     Good      Premium   Good      Very Good Ideal    
## [43969] Very Good Very Good Very Good Very Good Very Good Ideal    
## [43975] Ideal     Ideal     Ideal     Premium   Premium   Very Good
## [43981] Very Good Ideal     Very Good Very Good Ideal     Ideal    
## [43987] Ideal     Good      Premium   Premium   Very Good Premium  
## [43993] Very Good Very Good Ideal     Good      Ideal     Good     
## [43999] Premium   Ideal     Good      Good      Good      Good     
## [44005] Premium   Good      Very Good Very Good Very Good Premium  
## [44011] Premium   Ideal     Premium   Premium   Premium   Ideal    
## [44017] Premium   Premium   Ideal     Premium   Ideal     Premium  
## [44023] Ideal     Ideal     Ideal     Ideal     Premium   Ideal    
## [44029] Ideal     Premium   Premium   Very Good Good      Ideal    
## [44035] Ideal     Very Good Ideal     Premium   Ideal     Fair     
## [44041] Very Good Very Good Ideal     Very Good Ideal     Good     
## [44047] Premium   Very Good Very Good Good      Ideal     Ideal    
## [44053] Ideal     Ideal     Very Good Very Good Very Good Very Good
## [44059] Ideal     Premium   Very Good Ideal     Ideal     Ideal    
## [44065] Premium   Good      Very Good Very Good Ideal     Ideal    
## [44071] Ideal     Ideal     Premium   Very Good Premium   Premium  
## [44077] Ideal     Very Good Very Good Ideal     Very Good Very Good
## [44083] Very Good Very Good Very Good Good      Very Good Ideal    
## [44089] Ideal     Ideal     Ideal     Ideal     Good      Very Good
## [44095] Very Good Ideal     Ideal     Premium   Premium   Very Good
## [44101] Good      Premium   Ideal     Premium   Ideal     Premium  
## [44107] Very Good Ideal     Ideal     Ideal     Very Good Ideal    
## [44113] Ideal     Ideal     Premium   Ideal     Very Good Very Good
## [44119] Very Good Premium   Very Good Ideal     Very Good Premium  
## [44125] Premium   Premium   Ideal     Premium   Very Good Ideal    
## [44131] Ideal     Ideal     Very Good Very Good Very Good Good     
## [44137] Ideal     Good      Premium   Ideal     Ideal     Ideal    
## [44143] Ideal     Ideal     Very Good Premium   Very Good Very Good
## [44149] Ideal     Ideal     Ideal     Ideal     Good      Good     
## [44155] Good      Ideal     Ideal     Fair      Very Good Very Good
## [44161] Very Good Very Good Very Good Ideal     Ideal     Good     
## [44167] Good      Good      Very Good Ideal     Ideal     Ideal    
## [44173] Premium   Ideal     Very Good Ideal     Ideal     Ideal    
## [44179] Premium   Premium   Premium   Very Good Ideal     Very Good
## [44185] Very Good Very Good Premium   Ideal     Very Good Very Good
## [44191] Ideal     Premium   Very Good Premium   Premium   Very Good
## [44197] Ideal     Ideal     Ideal     Premium   Very Good Very Good
## [44203] Ideal     Good      Very Good Ideal     Ideal     Ideal    
## [44209] Ideal     Ideal     Ideal     Fair      Very Good Ideal    
## [44215] Ideal     Ideal     Ideal     Premium   Premium   Ideal    
## [44221] Premium   Premium   Ideal     Ideal     Premium   Ideal    
## [44227] Very Good Very Good Very Good Premium   Ideal     Premium  
## [44233] Ideal     Ideal     Premium   Good      Ideal     Ideal    
## [44239] Ideal     Ideal     Premium   Premium   Very Good Very Good
## [44245] Very Good Premium   Very Good Premium   Good      Ideal    
## [44251] Ideal     Premium   Very Good Good      Premium   Very Good
## [44257] Premium   Ideal     Ideal     Premium   Ideal     Ideal    
## [44263] Ideal     Very Good Ideal     Ideal     Ideal     Good     
## [44269] Premium   Ideal     Very Good Ideal     Ideal     Ideal    
## [44275] Premium   Good      Ideal     Very Good Very Good Premium  
## [44281] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [44287] Very Good Ideal     Premium   Very Good Premium   Premium  
## [44293] Premium   Premium   Premium   Ideal     Ideal     Premium  
## [44299] Premium   Premium   Fair      Very Good Very Good Very Good
## [44305] Ideal     Premium   Ideal     Premium   Premium   Premium  
## [44311] Premium   Premium   Ideal     Very Good Very Good Very Good
## [44317] Ideal     Good      Premium   Very Good Premium   Ideal    
## [44323] Ideal     Premium   Good      Premium   Very Good Ideal    
## [44329] Good      Premium   Ideal     Premium   Good      Premium  
## [44335] Premium   Premium   Very Good Very Good Very Good Very Good
## [44341] Premium   Very Good Very Good Very Good Very Good Ideal    
## [44347] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [44353] Ideal     Ideal     Very Good Ideal     Very Good Ideal    
## [44359] Very Good Ideal     Very Good Premium   Ideal     Premium  
## [44365] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [44371] Ideal     Ideal     Ideal     Ideal     Premium   Ideal    
## [44377] Very Good Very Good Ideal     Premium   Premium   Ideal    
## [44383] Good      Ideal     Premium   Ideal     Premium   Ideal    
## [44389] Ideal     Ideal     Very Good Very Good Premium   Ideal    
## [44395] Ideal     Premium   Very Good Premium   Premium   Very Good
## [44401] Premium   Ideal     Ideal     Ideal     Ideal     Ideal    
## [44407] Ideal     Ideal     Ideal     Ideal     Premium   Ideal    
## [44413] Ideal     Ideal     Ideal     Ideal     Premium   Very Good
## [44419] Ideal     Very Good Very Good Ideal     Premium   Ideal    
## [44425] Ideal     Premium   Ideal     Premium   Premium   Very Good
## [44431] Good      Ideal     Very Good Very Good Ideal     Ideal    
## [44437] Good      Premium   Ideal     Very Good Premium   Premium  
## [44443] Premium   Very Good Ideal     Ideal     Ideal     Ideal    
## [44449] Ideal     Ideal     Ideal     Ideal     Premium   Good     
## [44455] Very Good Very Good Very Good Very Good Very Good Very Good
## [44461] Ideal     Premium   Ideal     Ideal     Ideal     Premium  
## [44467] Ideal     Premium   Premium   Good      Ideal     Very Good
## [44473] Ideal     Premium   Ideal     Ideal     Ideal     Premium  
## [44479] Premium   Ideal     Ideal     Good      Ideal     Very Good
## [44485] Premium   Ideal     Very Good Ideal     Premium   Very Good
## [44491] Very Good Ideal     Ideal     Ideal     Very Good Very Good
## [44497] Very Good Ideal     Ideal     Ideal     Good      Very Good
## [44503] Premium   Very Good Ideal     Ideal     Ideal     Ideal    
## [44509] Ideal     Ideal     Very Good Ideal     Good      Premium  
## [44515] Ideal     Premium   Ideal     Premium   Premium   Ideal    
## [44521] Very Good Very Good Very Good Very Good Very Good Ideal    
## [44527] Ideal     Ideal     Ideal     Ideal     Good      Very Good
## [44533] Fair      Premium   Very Good Ideal     Very Good Ideal    
## [44539] Very Good Very Good Ideal     Ideal     Premium   Very Good
## [44545] Very Good Very Good Ideal     Ideal     Ideal     Ideal    
## [44551] Ideal     Ideal     Ideal     Good      Premium   Premium  
## [44557] Good      Ideal     Premium   Ideal     Premium   Premium  
## [44563] Good      Good      Ideal     Ideal     Very Good Very Good
## [44569] Premium   Premium   Premium   Very Good Very Good Very Good
## [44575] Ideal     Ideal     Very Good Good      Very Good Very Good
## [44581] Ideal     Ideal     Ideal     Good      Very Good Good     
## [44587] Ideal     Ideal     Ideal     Ideal     Premium   Premium  
## [44593] Ideal     Ideal     Ideal     Very Good Premium   Ideal    
## [44599] Ideal     Ideal     Premium   Ideal     Premium   Premium  
## [44605] Ideal     Premium   Premium   Premium   Ideal     Ideal    
## [44611] Ideal     Ideal     Ideal     Premium   Ideal     Premium  
## [44617] Ideal     Ideal     Ideal     Ideal     Very Good Ideal    
## [44623] Ideal     Ideal     Very Good Ideal     Premium   Ideal    
## [44629] Ideal     Ideal     Premium   Premium   Ideal     Very Good
## [44635] Ideal     Very Good Very Good Ideal     Ideal     Ideal    
## [44641] Very Good Very Good Good      Premium   Very Good Very Good
## [44647] Very Good Very Good Very Good Very Good Ideal     Ideal    
## [44653] Good      Ideal     Very Good Good      Premium   Ideal    
## [44659] Ideal     Premium   Premium   Good      Ideal     Ideal    
## [44665] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [44671] Very Good Very Good Ideal     Ideal     Ideal     Ideal    
## [44677] Ideal     Premium   Good      Very Good Ideal     Premium  
## [44683] Premium   Premium   Ideal     Good      Premium   Ideal    
## [44689] Ideal     Very Good Ideal     Very Good Very Good Ideal    
## [44695] Ideal     Premium   Ideal     Ideal     Ideal     Good     
## [44701] Good      Good      Good      Premium   Premium   Ideal    
## [44707] Ideal     Ideal     Ideal     Very Good Premium   Premium  
## [44713] Ideal     Ideal     Ideal     Premium   Premium   Very Good
## [44719] Ideal     Good      Very Good Ideal     Ideal     Ideal    
## [44725] Ideal     Very Good Very Good Very Good Ideal     Ideal    
## [44731] Ideal     Ideal     Ideal     Very Good Very Good Ideal    
## [44737] Premium   Ideal     Very Good Very Good Ideal     Ideal    
## [44743] Ideal     Very Good Ideal     Ideal     Ideal     Good     
## [44749] Ideal     Premium   Ideal     Premium   Premium   Ideal    
## [44755] Ideal     Premium   Ideal     Ideal     Very Good Ideal    
## [44761] Very Good Ideal     Very Good Ideal     Ideal     Ideal    
## [44767] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [44773] Ideal     Ideal     Premium   Very Good Ideal     Premium  
## [44779] Very Good Ideal     Premium   Premium   Very Good Ideal    
## [44785] Very Good Ideal     Very Good Premium   Premium   Premium  
## [44791] Very Good Ideal     Very Good Ideal     Very Good Ideal    
## [44797] Very Good Premium   Very Good Very Good Premium   Ideal    
## [44803] Very Good Very Good Premium   Very Good Premium   Premium  
## [44809] Ideal     Ideal     Very Good Ideal     Ideal     Ideal    
## [44815] Ideal     Ideal     Premium   Premium   Very Good Ideal    
## [44821] Very Good Premium   Ideal     Ideal     Ideal     Good     
## [44827] Good      Ideal     Ideal     Good      Ideal     Premium  
## [44833] Premium   Good      Premium   Very Good Ideal     Very Good
## [44839] Premium   Ideal     Ideal     Ideal     Ideal     Ideal    
## [44845] Ideal     Ideal     Good      Good      Fair      Very Good
## [44851] Ideal     Ideal     Ideal     Good      Very Good Very Good
## [44857] Very Good Very Good Premium   Ideal     Premium   Very Good
## [44863] Very Good Very Good Very Good Very Good Good      Ideal    
## [44869] Ideal     Ideal     Ideal     Very Good Very Good Very Good
## [44875] Ideal     Ideal     Ideal     Premium   Premium   Premium  
## [44881] Premium   Premium   Premium   Ideal     Ideal     Ideal    
## [44887] Very Good Ideal     Premium   Premium   Ideal     Ideal    
## [44893] Ideal     Very Good Very Good Very Good Premium   Very Good
## [44899] Very Good Ideal     Premium   Premium   Premium   Very Good
## [44905] Premium   Ideal     Ideal     Ideal     Premium   Very Good
## [44911] Ideal     Very Good Ideal     Ideal     Premium   Ideal    
## [44917] Ideal     Very Good Premium   Ideal     Ideal     Fair     
## [44923] Very Good Ideal     Ideal     Ideal     Ideal     Ideal    
## [44929] Ideal     Very Good Very Good Very Good Very Good Ideal    
## [44935] Ideal     Ideal     Ideal     Ideal     Fair      Ideal    
## [44941] Premium   Very Good Ideal     Ideal     Ideal     Premium  
## [44947] Premium   Good      Good      Good      Very Good Good     
## [44953] Ideal     Very Good Ideal     Very Good Very Good Ideal    
## [44959] Ideal     Ideal     Premium   Ideal     Very Good Premium  
## [44965] Good      Premium   Premium   Very Good Very Good Ideal    
## [44971] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [44977] Ideal     Ideal     Ideal     Ideal     Premium   Premium  
## [44983] Premium   Premium   Ideal     Premium   Ideal     Premium  
## [44989] Premium   Premium   Ideal     Good      Ideal     Premium  
## [44995] Premium   Premium   Very Good Premium   Very Good Very Good
## [45001] Ideal     Ideal     Very Good Ideal     Good      Very Good
## [45007] Premium   Ideal     Ideal     Ideal     Ideal     Ideal    
## [45013] Premium   Premium   Ideal     Ideal     Premium   Premium  
## [45019] Ideal     Ideal     Premium   Premium   Ideal     Ideal    
## [45025] Premium   Ideal     Ideal     Premium   Premium   Ideal    
## [45031] Ideal     Premium   Ideal     Premium   Good      Premium  
## [45037] Fair      Very Good Very Good Ideal     Very Good Ideal    
## [45043] Ideal     Ideal     Ideal     Very Good Very Good Ideal    
## [45049] Good      Premium   Premium   Very Good Very Good Ideal    
## [45055] Ideal     Very Good Premium   Fair      Premium   Premium  
## [45061] Very Good Fair      Very Good Ideal     Good      Premium  
## [45067] Fair      Very Good Ideal     Very Good Ideal     Good     
## [45073] Very Good Ideal     Ideal     Ideal     Ideal     Good     
## [45079] Ideal     Good      Premium   Ideal     Good      Premium  
## [45085] Very Good Very Good Ideal     Premium   Ideal     Ideal    
## [45091] Ideal     Good      Premium   Premium   Premium   Very Good
## [45097] Ideal     Very Good Very Good Very Good Ideal     Ideal    
## [45103] Ideal     Ideal     Ideal     Premium   Ideal     Ideal    
## [45109] Ideal     Ideal     Very Good Ideal     Ideal     Premium  
## [45115] Premium   Good      Good      Very Good Ideal     Ideal    
## [45121] Premium   Premium   Premium   Fair      Fair      Very Good
## [45127] Ideal     Ideal     Ideal     Ideal     Very Good Very Good
## [45133] Very Good Very Good Premium   Ideal     Premium   Ideal    
## [45139] Ideal     Very Good Premium   Ideal     Premium   Ideal    
## [45145] Premium   Ideal     Premium   Premium   Premium   Premium  
## [45151] Premium   Premium   Premium   Premium   Ideal     Ideal    
## [45157] Ideal     Premium   Premium   Premium   Premium   Premium  
## [45163] Ideal     Ideal     Ideal     Premium   Premium   Ideal    
## [45169] Premium   Premium   Premium   Premium   Very Good Ideal    
## [45175] Ideal     Ideal     Ideal     Very Good Ideal     Ideal    
## [45181] Very Good Ideal     Ideal     Ideal     Ideal     Ideal    
## [45187] Ideal     Premium   Very Good Ideal     Ideal     Premium  
## [45193] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [45199] Premium   Ideal     Premium   Premium   Premium   Premium  
## [45205] Very Good Ideal     Premium   Ideal     Ideal     Ideal    
## [45211] Ideal     Premium   Premium   Good      Very Good Ideal    
## [45217] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [45223] Ideal     Ideal     Very Good Very Good Premium   Ideal    
## [45229] Very Good Ideal     Ideal     Ideal     Good      Very Good
## [45235] Very Good Ideal     Very Good Very Good Very Good Ideal    
## [45241] Ideal     Very Good Ideal     Ideal     Ideal     Ideal    
## [45247] Ideal     Fair      Ideal     Premium   Fair      Good     
## [45253] Ideal     Ideal     Ideal     Good      Very Good Ideal    
## [45259] Ideal     Ideal     Very Good Ideal     Ideal     Ideal    
## [45265] Ideal     Ideal     Very Good Ideal     Premium   Ideal    
## [45271] Very Good Very Good Premium   Very Good Premium   Very Good
## [45277] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [45283] Ideal     Ideal     Ideal     Ideal     Premium   Ideal    
## [45289] Premium   Premium   Ideal     Premium   Ideal     Ideal    
## [45295] Ideal     Premium   Very Good Ideal     Very Good Very Good
## [45301] Very Good Premium   Very Good Ideal     Ideal     Ideal    
## [45307] Ideal     Ideal     Ideal     Good      Premium   Very Good
## [45313] Ideal     Ideal     Ideal     Very Good Very Good Premium  
## [45319] Ideal     Good      Ideal     Very Good Very Good Very Good
## [45325] Good      Good      Good      Ideal     Premium   Ideal    
## [45331] Very Good Good      Ideal     Premium   Good      Good     
## [45337] Premium   Ideal     Ideal     Ideal     Premium   Good     
## [45343] Very Good Very Good Very Good Very Good Ideal     Very Good
## [45349] Very Good Ideal     Ideal     Ideal     Premium   Premium  
## [45355] Ideal     Ideal     Ideal     Good      Premium   Ideal    
## [45361] Very Good Very Good Very Good Very Good Ideal     Ideal    
## [45367] Ideal     Ideal     Ideal     Ideal     Ideal     Good     
## [45373] Good      Ideal     Premium   Very Good Very Good Ideal    
## [45379] Very Good Ideal     Ideal     Premium   Premium   Premium  
## [45385] Very Good Ideal     Ideal     Very Good Ideal     Premium  
## [45391] Premium   Premium   Very Good Premium   Premium   Premium  
## [45397] Very Good Very Good Ideal     Ideal     Ideal     Ideal    
## [45403] Good      Very Good Good      Premium   Good      Ideal    
## [45409] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [45415] Good      Premium   Ideal     Ideal     Ideal     Ideal    
## [45421] Premium   Premium   Ideal     Ideal     Ideal     Fair     
## [45427] Very Good Ideal     Ideal     Ideal     Very Good Ideal    
## [45433] Ideal     Very Good Very Good Ideal     Ideal     Very Good
## [45439] Premium   Ideal     Ideal     Ideal     Fair      Very Good
## [45445] Premium   Ideal     Ideal     Good      Good      Ideal    
## [45451] Ideal     Very Good Very Good Premium   Very Good Very Good
## [45457] Ideal     Good      Premium   Ideal     Ideal     Ideal    
## [45463] Ideal     Very Good Very Good Ideal     Ideal     Premium  
## [45469] Ideal     Premium   Premium   Ideal     Ideal     Premium  
## [45475] Premium   Premium   Premium   Premium   Ideal     Ideal    
## [45481] Very Good Ideal     Ideal     Ideal     Ideal     Ideal    
## [45487] Ideal     Ideal     Good      Very Good Very Good Very Good
## [45493] Ideal     Ideal     Ideal     Ideal     Very Good Very Good
## [45499] Ideal     Ideal     Ideal     Ideal     Good      Ideal    
## [45505] Ideal     Premium   Very Good Very Good Very Good Ideal    
## [45511] Ideal     Ideal     Good      Premium   Ideal     Ideal    
## [45517] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [45523] Ideal     Ideal     Premium   Ideal     Ideal     Ideal    
## [45529] Ideal     Premium   Ideal     Good      Very Good Ideal    
## [45535] Fair      Ideal     Ideal     Ideal     Ideal     Premium  
## [45541] Premium   Ideal     Ideal     Ideal     Premium   Premium  
## [45547] Premium   Premium   Ideal     Ideal     Ideal     Premium  
## [45553] Premium   Premium   Ideal     Ideal     Premium   Very Good
## [45559] Very Good Very Good Good      Ideal     Good      Ideal    
## [45565] Ideal     Ideal     Ideal     Premium   Premium   Very Good
## [45571] Very Good Ideal     Ideal     Ideal     Very Good Premium  
## [45577] Ideal     Very Good Ideal     Ideal     Very Good Ideal    
## [45583] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [45589] Very Good Premium   Very Good Ideal     Ideal     Ideal    
## [45595] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [45601] Good      Good      Good      Premium   Ideal     Fair     
## [45607] Premium   Ideal     Ideal     Ideal     Ideal     Ideal    
## [45613] Ideal     Fair      Fair      Very Good Very Good Very Good
## [45619] Ideal     Ideal     Ideal     Premium   Ideal     Very Good
## [45625] Ideal     Ideal     Ideal     Fair      Very Good Ideal    
## [45631] Good      Very Good Very Good Very Good Very Good Very Good
## [45637] Very Good Very Good Very Good Very Good Ideal     Ideal    
## [45643] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [45649] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [45655] Premium   Very Good Very Good Very Good Very Good Ideal    
## [45661] Premium   Very Good Ideal     Good      Ideal     Ideal    
## [45667] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [45673] Ideal     Very Good Premium   Premium   Ideal     Ideal    
## [45679] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [45685] Good      Ideal     Ideal     Premium   Fair      Very Good
## [45691] Very Good Ideal     Ideal     Good      Fair      Very Good
## [45697] Very Good Premium   Ideal     Ideal     Ideal     Ideal    
## [45703] Ideal     Premium   Ideal     Ideal     Ideal     Ideal    
## [45709] Ideal     Ideal     Ideal     Premium   Premium   Ideal    
## [45715] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [45721] Premium   Ideal     Premium   Premium   Premium   Very Good
## [45727] Ideal     Premium   Premium   Premium   Good      Good     
## [45733] Ideal     Ideal     Ideal     Premium   Premium   Premium  
## [45739] Very Good Ideal     Ideal     Ideal     Good      Very Good
## [45745] Good      Ideal     Good      Fair      Ideal     Ideal    
## [45751] Ideal     Very Good Ideal     Premium   Premium   Fair     
## [45757] Ideal     Ideal     Fair      Fair      Very Good Very Good
## [45763] Very Good Very Good Ideal     Ideal     Good      Ideal    
## [45769] Ideal     Very Good Ideal     Premium   Ideal     Ideal    
## [45775] Very Good Premium   Ideal     Ideal     Ideal     Ideal    
## [45781] Ideal     Ideal     Good      Good      Premium   Premium  
## [45787] Premium   Ideal     Very Good Premium   Ideal     Ideal    
## [45793] Ideal     Very Good Ideal     Premium   Premium   Very Good
## [45799] Fair      Premium   Premium   Good      Premium   Premium  
## [45805] Premium   Very Good Ideal     Ideal     Good      Ideal    
## [45811] Ideal     Ideal     Very Good Premium   Very Good Ideal    
## [45817] Ideal     Ideal     Ideal     Ideal     Good      Good     
## [45823] Good      Ideal     Ideal     Premium   Premium   Very Good
## [45829] Fair      Fair      Ideal     Ideal     Ideal     Ideal    
## [45835] Good      Ideal     Ideal     Ideal     Ideal     Premium  
## [45841] Very Good Very Good Good      Good      Very Good Good     
## [45847] Very Good Ideal     Good      Ideal     Ideal     Ideal    
## [45853] Ideal     Good      Premium   Ideal     Very Good Premium  
## [45859] Very Good Very Good Fair      Premium   Premium   Very Good
## [45865] Premium   Fair      Very Good Ideal     Ideal     Very Good
## [45871] Premium   Premium   Ideal     Premium   Ideal     Very Good
## [45877] Very Good Premium   Premium   Ideal     Ideal     Ideal    
## [45883] Ideal     Very Good Ideal     Ideal     Premium   Ideal    
## [45889] Ideal     Very Good Ideal     Premium   Ideal     Premium  
## [45895] Premium   Ideal     Ideal     Ideal     Premium   Ideal    
## [45901] Ideal     Ideal     Ideal     Ideal     Premium   Good     
## [45907] Ideal     Premium   Fair      Premium   Premium   Ideal    
## [45913] Ideal     Premium   Ideal     Ideal     Ideal     Ideal    
## [45919] Ideal     Very Good Ideal     Ideal     Ideal     Ideal    
## [45925] Ideal     Ideal     Premium   Ideal     Very Good Ideal    
## [45931] Very Good Ideal     Ideal     Ideal     Very Good Very Good
## [45937] Very Good Very Good Ideal     Ideal     Ideal     Ideal    
## [45943] Very Good Very Good Very Good Ideal     Premium   Ideal    
## [45949] Ideal     Ideal     Very Good Premium   Ideal     Premium  
## [45955] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [45961] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [45967] Good      Very Good Very Good Very Good Good      Premium  
## [45973] Good      Premium   Very Good Very Good Very Good Premium  
## [45979] Ideal     Premium   Good      Very Good Good      Good     
## [45985] Ideal     Very Good Premium   Good      Good      Very Good
## [45991] Ideal     Fair      Fair      Ideal     Ideal     Very Good
## [45997] Very Good Ideal     Ideal     Premium   Very Good Very Good
## [46003] Ideal     Ideal     Ideal     Very Good Very Good Very Good
## [46009] Very Good Ideal     Ideal     Very Good Ideal     Ideal    
## [46015] Premium   Premium   Premium   Ideal     Ideal     Good     
## [46021] Ideal     Very Good Ideal     Ideal     Ideal     Ideal    
## [46027] Ideal     Ideal     Premium   Ideal     Ideal     Premium  
## [46033] Premium   Premium   Premium   Premium   Ideal     Ideal    
## [46039] Ideal     Premium   Good      Premium   Very Good Ideal    
## [46045] Ideal     Ideal     Premium   Ideal     Ideal     Ideal    
## [46051] Very Good Ideal     Ideal     Ideal     Premium   Premium  
## [46057] Ideal     Ideal     Fair      Very Good Good      Premium  
## [46063] Very Good Premium   Premium   Very Good Very Good Ideal    
## [46069] Good      Ideal     Premium   Premium   Premium   Ideal    
## [46075] Premium   Premium   Ideal     Ideal     Ideal     Ideal    
## [46081] Ideal     Fair      Very Good Premium   Ideal     Good     
## [46087] Ideal     Ideal     Ideal     Ideal     Premium   Fair     
## [46093] Fair      Ideal     Ideal     Premium   Very Good Very Good
## [46099] Ideal     Very Good Ideal     Ideal     Good      Very Good
## [46105] Ideal     Ideal     Ideal     Ideal     Very Good Ideal    
## [46111] Very Good Ideal     Ideal     Ideal     Good      Ideal    
## [46117] Ideal     Premium   Ideal     Ideal     Ideal     Premium  
## [46123] Ideal     Very Good Very Good Ideal     Ideal     Ideal    
## [46129] Ideal     Premium   Good      Premium   Ideal     Very Good
## [46135] Good      Ideal     Ideal     Ideal     Ideal     Ideal    
## [46141] Premium   Premium   Very Good Premium   Ideal     Premium  
## [46147] Premium   Premium   Premium   Premium   Premium   Premium  
## [46153] Premium   Premium   Ideal     Premium   Ideal     Ideal    
## [46159] Ideal     Ideal     Ideal     Premium   Premium   Ideal    
## [46165] Ideal     Ideal     Premium   Premium   Premium   Premium  
## [46171] Premium   Premium   Ideal     Ideal     Premium   Premium  
## [46177] Ideal     Ideal     Premium   Premium   Premium   Ideal    
## [46183] Ideal     Very Good Very Good Very Good Premium   Good     
## [46189] Ideal     Premium   Ideal     Very Good Premium   Ideal    
## [46195] Premium   Ideal     Ideal     Fair      Premium   Good     
## [46201] Premium   Premium   Fair      Very Good Very Good Very Good
## [46207] Ideal     Very Good Ideal     Good      Very Good Very Good
## [46213] Ideal     Ideal     Premium   Ideal     Very Good Ideal    
## [46219] Ideal     Good      Very Good Premium   Premium   Premium  
## [46225] Premium   Ideal     Ideal     Very Good Ideal     Premium  
## [46231] Very Good Very Good Premium   Ideal     Ideal     Ideal    
## [46237] Ideal     Ideal     Ideal     Premium   Premium   Premium  
## [46243] Ideal     Ideal     Premium   Ideal     Premium   Ideal    
## [46249] Premium   Ideal     Premium   Very Good Ideal     Ideal    
## [46255] Very Good Very Good Ideal     Premium   Ideal     Ideal    
## [46261] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [46267] Premium   Ideal     Ideal     Ideal     Ideal     Ideal    
## [46273] Premium   Ideal     Ideal     Ideal     Ideal     Good     
## [46279] Very Good Very Good Very Good Very Good Very Good Premium  
## [46285] Premium   Ideal     Ideal     Ideal     Ideal     Good     
## [46291] Premium   Good      Very Good Ideal     Good      Very Good
## [46297] Premium   Good      Very Good Very Good Ideal     Very Good
## [46303] Good      Very Good Ideal     Good      Premium   Very Good
## [46309] Ideal     Premium   Very Good Premium   Ideal     Ideal    
## [46315] Good      Very Good Good      Good      Very Good Ideal    
## [46321] Ideal     Very Good Very Good Very Good Ideal     Good     
## [46327] Ideal     Very Good Ideal     Good      Premium   Ideal    
## [46333] Good      Premium   Premium   Ideal     Ideal     Ideal    
## [46339] Ideal     Premium   Good      Very Good Very Good Very Good
## [46345] Ideal     Good      Very Good Fair      Ideal     Ideal    
## [46351] Ideal     Ideal     Ideal     Good      Ideal     Very Good
## [46357] Premium   Ideal     Ideal     Good      Fair      Ideal    
## [46363] Ideal     Premium   Ideal     Premium   Very Good Premium  
## [46369] Premium   Premium   Premium   Ideal     Ideal     Ideal    
## [46375] Ideal     Ideal     Very Good Ideal     Fair      Ideal    
## [46381] Good      Very Good Ideal     Ideal     Premium   Very Good
## [46387] Very Good Very Good Good      Ideal     Premium   Ideal    
## [46393] Ideal     Premium   Premium   Premium   Ideal     Ideal    
## [46399] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [46405] Premium   Ideal     Ideal     Very Good Ideal     Ideal    
## [46411] Ideal     Good      Good      Ideal     Ideal     Ideal    
## [46417] Ideal     Very Good Very Good Ideal     Ideal     Very Good
## [46423] Ideal     Ideal     Ideal     Good      Premium   Very Good
## [46429] Ideal     Premium   Fair      Premium   Premium   Very Good
## [46435] Good      Good      Premium   Very Good Ideal     Ideal    
## [46441] Very Good Ideal     Ideal     Ideal     Very Good Very Good
## [46447] Very Good Ideal     Premium   Fair      Premium   Very Good
## [46453] Premium   Ideal     Ideal     Good      Ideal     Ideal    
## [46459] Ideal     Ideal     Ideal     Good      Good      Premium  
## [46465] Premium   Premium   Ideal     Good      Premium   Very Good
## [46471] Ideal     Good      Very Good Ideal     Ideal     Ideal    
## [46477] Fair      Very Good Very Good Very Good Very Good Ideal    
## [46483] Good      Very Good Premium   Premium   Premium   Ideal    
## [46489] Very Good Very Good Very Good Ideal     Ideal     Ideal    
## [46495] Ideal     Ideal     Very Good Very Good Ideal     Ideal    
## [46501] Ideal     Good      Premium   Fair      Ideal     Premium  
## [46507] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [46513] Ideal     Very Good Ideal     Ideal     Ideal     Ideal    
## [46519] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [46525] Ideal     Ideal     Premium   Premium   Ideal     Ideal    
## [46531] Premium   Premium   Ideal     Ideal     Premium   Premium  
## [46537] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [46543] Premium   Premium   Ideal     Ideal     Premium   Ideal    
## [46549] Ideal     Ideal     Ideal     Very Good Ideal     Premium  
## [46555] Premium   Ideal     Ideal     Good      Very Good Premium  
## [46561] Ideal     Ideal     Ideal     Ideal     Premium   Ideal    
## [46567] Ideal     Good      Good      Very Good Very Good Ideal    
## [46573] Ideal     Fair      Ideal     Fair      Very Good Very Good
## [46579] Very Good Premium   Good      Premium   Premium   Ideal    
## [46585] Ideal     Very Good Very Good Very Good Ideal     Good     
## [46591] Premium   Premium   Premium   Ideal     Ideal     Ideal    
## [46597] Ideal     Premium   Ideal     Ideal     Ideal     Ideal    
## [46603] Premium   Premium   Ideal     Premium   Premium   Ideal    
## [46609] Ideal     Very Good Fair      Ideal     Ideal     Very Good
## [46615] Very Good Ideal     Ideal     Ideal     Ideal     Ideal    
## [46621] Premium   Very Good Ideal     Premium   Premium   Premium  
## [46627] Premium   Ideal     Ideal     Very Good Ideal     Very Good
## [46633] Good      Very Good Very Good Good      Ideal     Good     
## [46639] Premium   Good      Very Good Very Good Premium   Very Good
## [46645] Premium   Very Good Very Good Very Good Very Good Good     
## [46651] Premium   Ideal     Ideal     Ideal     Ideal     Ideal    
## [46657] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [46663] Ideal     Ideal     Ideal     Fair      Very Good Very Good
## [46669] Good      Fair      Very Good Ideal     Ideal     Premium  
## [46675] Premium   Ideal     Ideal     Ideal     Ideal     Fair     
## [46681] Very Good Ideal     Good      Good      Ideal     Very Good
## [46687] Ideal     Ideal     Ideal     Ideal     Very Good Ideal    
## [46693] Ideal     Ideal     Ideal     Good      Fair      Ideal    
## [46699] Ideal     Fair      Premium   Ideal     Ideal     Ideal    
## [46705] Fair      Premium   Premium   Very Good Good      Ideal    
## [46711] Ideal     Ideal     Ideal     Good      Good      Good     
## [46717] Good      Premium   Very Good Very Good Very Good Premium  
## [46723] Very Good Ideal     Premium   Very Good Good      Very Good
## [46729] Very Good Ideal     Ideal     Very Good Premium   Ideal    
## [46735] Ideal     Ideal     Ideal     Very Good Very Good Ideal    
## [46741] Very Good Ideal     Fair      Good      Fair      Premium  
## [46747] Premium   Premium   Very Good Good      Good      Good     
## [46753] Ideal     Ideal     Ideal     Ideal     Premium   Ideal    
## [46759] Ideal     Very Good Very Good Ideal     Good      Very Good
## [46765] Ideal     Ideal     Premium   Ideal     Fair      Ideal    
## [46771] Premium   Fair      Ideal     Ideal     Ideal     Ideal    
## [46777] Ideal     Premium   Very Good Ideal     Very Good Very Good
## [46783] Ideal     Ideal     Fair      Very Good Very Good Very Good
## [46789] Good      Good      Good      Very Good Premium   Premium  
## [46795] Premium   Good      Good      Very Good Good      Premium  
## [46801] Premium   Ideal     Ideal     Fair      Premium   Premium  
## [46807] Very Good Very Good Very Good Ideal     Premium   Premium  
## [46813] Premium   Ideal     Premium   Premium   Premium   Premium  
## [46819] Fair      Premium   Ideal     Premium   Fair      Fair     
## [46825] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [46831] Ideal     Ideal     Ideal     Premium   Ideal     Good     
## [46837] Very Good Ideal     Ideal     Ideal     Ideal     Good     
## [46843] Fair      Ideal     Premium   Ideal     Ideal     Ideal    
## [46849] Ideal     Ideal     Premium   Premium   Premium   Good     
## [46855] Very Good Very Good Very Good Ideal     Very Good Ideal    
## [46861] Good      Premium   Ideal     Ideal     Ideal     Premium  
## [46867] Ideal     Ideal     Ideal     Ideal     Premium   Ideal    
## [46873] Ideal     Ideal     Premium   Ideal     Ideal     Ideal    
## [46879] Ideal     Premium   Ideal     Ideal     Ideal     Premium  
## [46885] Ideal     Premium   Premium   Very Good Premium   Ideal    
## [46891] Fair      Ideal     Ideal     Premium   Ideal     Premium  
## [46897] Ideal     Ideal     Ideal     Ideal     Premium   Premium  
## [46903] Ideal     Premium   Premium   Premium   Ideal     Ideal    
## [46909] Very Good Very Good Fair      Premium   Very Good Ideal    
## [46915] Premium   Ideal     Ideal     Ideal     Ideal     Ideal    
## [46921] Very Good Premium   Ideal     Ideal     Premium   Ideal    
## [46927] Ideal     Premium   Premium   Ideal     Ideal     Premium  
## [46933] Premium   Ideal     Ideal     Ideal     Ideal     Premium  
## [46939] Premium   Ideal     Premium   Ideal     Premium   Ideal    
## [46945] Ideal     Ideal     Premium   Ideal     Ideal     Very Good
## [46951] Ideal     Ideal     Very Good Premium   Premium   Very Good
## [46957] Premium   Very Good Ideal     Ideal     Premium   Fair     
## [46963] Good      Ideal     Good      Very Good Ideal     Ideal    
## [46969] Ideal     Ideal     Ideal     Premium   Good      Premium  
## [46975] Very Good Very Good Very Good Very Good Very Good Very Good
## [46981] Premium   Premium   Ideal     Ideal     Ideal     Very Good
## [46987] Good      Ideal     Ideal     Good      Good      Premium  
## [46993] Very Good Very Good Very Good Ideal     Premium   Ideal    
## [46999] Ideal     Very Good Ideal     Ideal     Ideal     Ideal    
## [47005] Ideal     Premium   Good      Good      Ideal     Ideal    
## [47011] Ideal     Very Good Premium   Very Good Ideal     Very Good
## [47017] Ideal     Ideal     Premium   Ideal     Premium   Premium  
## [47023] Very Good Ideal     Ideal     Very Good Premium   Ideal    
## [47029] Ideal     Ideal     Ideal     Ideal     Premium   Ideal    
## [47035] Ideal     Ideal     Ideal     Ideal     Premium   Premium  
## [47041] Premium   Premium   Premium   Premium   Premium   Very Good
## [47047] Ideal     Ideal     Ideal     Ideal     Very Good Very Good
## [47053] Premium   Very Good Ideal     Ideal     Good      Good     
## [47059] Good      Ideal     Good      Very Good Very Good Ideal    
## [47065] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [47071] Ideal     Ideal     Ideal     Ideal     Premium   Very Good
## [47077] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [47083] Premium   Ideal     Premium   Ideal     Good      Good     
## [47089] Premium   Ideal     Ideal     Ideal     Ideal     Ideal    
## [47095] Ideal     Premium   Very Good Premium   Very Good Fair     
## [47101] Ideal     Ideal     Very Good Ideal     Ideal     Premium  
## [47107] Ideal     Premium   Ideal     Fair      Premium   Premium  
## [47113] Ideal     Very Good Very Good Very Good Very Good Ideal    
## [47119] Ideal     Good      Premium   Very Good Very Good Premium  
## [47125] Fair      Fair      Premium   Very Good Very Good Ideal    
## [47131] Ideal     Ideal     Very Good Very Good Ideal     Premium  
## [47137] Ideal     Ideal     Very Good Premium   Premium   Good     
## [47143] Ideal     Ideal     Ideal     Ideal     Good      Ideal    
## [47149] Ideal     Fair      Ideal     Premium   Very Good Very Good
## [47155] Very Good Ideal     Ideal     Ideal     Ideal     Ideal    
## [47161] Premium   Premium   Ideal     Ideal     Ideal     Ideal    
## [47167] Ideal     Ideal     Ideal     Premium   Ideal     Premium  
## [47173] Very Good Ideal     Premium   Ideal     Ideal     Ideal    
## [47179] Ideal     Premium   Very Good Ideal     Premium   Ideal    
## [47185] Ideal     Ideal     Ideal     Ideal     Premium   Ideal    
## [47191] Ideal     Premium   Premium   Very Good Very Good Very Good
## [47197] Very Good Premium   Premium   Ideal     Very Good Ideal    
## [47203] Premium   Ideal     Very Good Very Good Ideal     Ideal    
## [47209] Very Good Ideal     Good      Ideal     Ideal     Ideal    
## [47215] Very Good Premium   Premium   Ideal     Premium   Premium  
## [47221] Premium   Premium   Very Good Ideal     Ideal     Very Good
## [47227] Very Good Ideal     Ideal     Good      Fair      Good     
## [47233] Fair      Very Good Very Good Very Good Very Good Very Good
## [47239] Ideal     Ideal     Good      Premium   Premium   Premium  
## [47245] Ideal     Good      Ideal     Ideal     Ideal     Very Good
## [47251] Ideal     Premium   Premium   Ideal     Ideal     Very Good
## [47257] Ideal     Very Good Very Good Premium   Very Good Premium  
## [47263] Good      Ideal     Premium   Good      Very Good Ideal    
## [47269] Premium   Ideal     Ideal     Ideal     Ideal     Ideal    
## [47275] Good      Premium   Ideal     Ideal     Ideal     Ideal    
## [47281] Very Good Very Good Good      Very Good Premium   Good     
## [47287] Very Good Very Good Very Good Good      Good      Good     
## [47293] Premium   Premium   Good      Good      Good      Very Good
## [47299] Very Good Very Good Very Good Ideal     Ideal     Good     
## [47305] Ideal     Ideal     Very Good Very Good Ideal     Ideal    
## [47311] Very Good Very Good Very Good Very Good Very Good Very Good
## [47317] Very Good Very Good Very Good Very Good Very Good Very Good
## [47323] Very Good Very Good Very Good Very Good Very Good Very Good
## [47329] Very Good Very Good Very Good Very Good Very Good Very Good
## [47335] Very Good Very Good Very Good Very Good Very Good Very Good
## [47341] Good      Ideal     Premium   Premium   Premium   Premium  
## [47347] Ideal     Ideal     Premium   Ideal     Premium   Ideal    
## [47353] Premium   Premium   Ideal     Premium   Ideal     Ideal    
## [47359] Premium   Very Good Premium   Ideal     Ideal     Ideal    
## [47365] Ideal     Good      Premium   Premium   Very Good Very Good
## [47371] Ideal     Ideal     Ideal     Ideal     Premium   Ideal    
## [47377] Ideal     Premium   Ideal     Ideal     Ideal     Ideal    
## [47383] Premium   Premium   Ideal     Premium   Ideal     Ideal    
## [47389] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [47395] Ideal     Good      Premium   Premium   Premium   Ideal    
## [47401] Ideal     Ideal     Fair      Premium   Very Good Very Good
## [47407] Ideal     Fair      Ideal     Good      Ideal     Premium  
## [47413] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [47419] Premium   Ideal     Ideal     Very Good Good      Good     
## [47425] Ideal     Ideal     Very Good Very Good Ideal     Ideal    
## [47431] Ideal     Very Good Premium   Ideal     Ideal     Premium  
## [47437] Ideal     Ideal     Very Good Ideal     Ideal     Very Good
## [47443] Good      Ideal     Ideal     Very Good Premium   Ideal    
## [47449] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [47455] Very Good Ideal     Ideal     Ideal     Premium   Ideal    
## [47461] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [47467] Good      Premium   Premium   Ideal     Ideal     Very Good
## [47473] Ideal     Ideal     Very Good Good      Very Good Very Good
## [47479] Ideal     Ideal     Good      Ideal     Premium   Good     
## [47485] Premium   Ideal     Ideal     Ideal     Ideal     Ideal    
## [47491] Premium   Very Good Ideal     Ideal     Ideal     Ideal    
## [47497] Ideal     Ideal     Ideal     Ideal     Fair      Good     
## [47503] Fair      Ideal     Fair      Premium   Good      Good     
## [47509] Ideal     Very Good Premium   Ideal     Ideal     Very Good
## [47515] Very Good Ideal     Ideal     Very Good Very Good Ideal    
## [47521] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [47527] Premium   Ideal     Very Good Premium   Ideal     Fair     
## [47533] Very Good Ideal     Very Good Very Good Very Good Premium  
## [47539] Ideal     Ideal     Very Good Good      Ideal     Ideal    
## [47545] Ideal     Ideal     Ideal     Premium   Ideal     Ideal    
## [47551] Very Good Premium   Ideal     Ideal     Very Good Ideal    
## [47557] Ideal     Ideal     Ideal     Very Good Very Good Ideal    
## [47563] Premium   Very Good Good      Very Good Ideal     Ideal    
## [47569] Ideal     Very Good Ideal     Ideal     Ideal     Premium  
## [47575] Ideal     Ideal     Premium   Ideal     Ideal     Ideal    
## [47581] Ideal     Premium   Premium   Very Good Ideal     Ideal    
## [47587] Ideal     Ideal     Ideal     Very Good Premium   Very Good
## [47593] Ideal     Ideal     Premium   Ideal     Premium   Very Good
## [47599] Ideal     Ideal     Ideal     Good      Ideal     Very Good
## [47605] Ideal     Ideal     Premium   Premium   Very Good Ideal    
## [47611] Ideal     Ideal     Ideal     Very Good Ideal     Good     
## [47617] Ideal     Ideal     Ideal     Premium   Ideal     Ideal    
## [47623] Premium   Premium   Premium   Fair      Premium   Ideal    
## [47629] Very Good Ideal     Good      Ideal     Ideal     Premium  
## [47635] Ideal     Ideal     Premium   Ideal     Ideal     Ideal    
## [47641] Very Good Very Good Very Good Very Good Ideal     Ideal    
## [47647] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [47653] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [47659] Very Good Very Good Very Good Very Good Very Good Good     
## [47665] Very Good Ideal     Ideal     Ideal     Good      Premium  
## [47671] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [47677] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [47683] Ideal     Premium   Premium   Premium   Premium   Premium  
## [47689] Good      Premium   Very Good Very Good Ideal     Ideal    
## [47695] Ideal     Ideal     Ideal     Very Good Ideal     Premium  
## [47701] Premium   Very Good Premium   Ideal     Ideal     Ideal    
## [47707] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [47713] Very Good Very Good Very Good Good      Ideal     Very Good
## [47719] Ideal     Premium   Very Good Ideal     Ideal     Ideal    
## [47725] Very Good Ideal     Premium   Very Good Ideal     Ideal    
## [47731] Very Good Very Good Very Good Very Good Ideal     Ideal    
## [47737] Ideal     Ideal     Premium   Premium   Fair      Premium  
## [47743] Very Good Ideal     Ideal     Ideal     Ideal     Very Good
## [47749] Ideal     Very Good Ideal     Very Good Premium   Ideal    
## [47755] Premium   Ideal     Ideal     Fair      Very Good Ideal    
## [47761] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [47767] Good      Premium   Ideal     Very Good Very Good Ideal    
## [47773] Good      Very Good Very Good Fair      Very Good Very Good
## [47779] Premium   Premium   Ideal     Ideal     Fair      Ideal    
## [47785] Ideal     Ideal     Ideal     Good      Ideal     Premium  
## [47791] Very Good Very Good Good      Good      Ideal     Ideal    
## [47797] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [47803] Fair      Ideal     Ideal     Premium   Very Good Very Good
## [47809] Very Good Very Good Very Good Ideal     Good      Ideal    
## [47815] Ideal     Fair      Premium   Ideal     Very Good Very Good
## [47821] Very Good Ideal     Ideal     Ideal     Premium   Premium  
## [47827] Ideal     Ideal     Premium   Premium   Very Good Very Good
## [47833] Ideal     Ideal     Ideal     Good      Good      Very Good
## [47839] Very Good Very Good Ideal     Ideal     Ideal     Ideal    
## [47845] Ideal     Ideal     Ideal     Ideal     Premium   Very Good
## [47851] Good      Premium   Good      Premium   Ideal     Ideal    
## [47857] Premium   Very Good Ideal     Ideal     Very Good Premium  
## [47863] Premium   Good      Ideal     Premium   Ideal     Ideal    
## [47869] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [47875] Ideal     Very Good Ideal     Premium   Ideal     Ideal    
## [47881] Ideal     Ideal     Very Good Very Good Ideal     Good     
## [47887] Ideal     Premium   Ideal     Very Good Very Good Premium  
## [47893] Ideal     Premium   Very Good Very Good Very Good Ideal    
## [47899] Ideal     Ideal     Good      Premium   Ideal     Ideal    
## [47905] Ideal     Ideal     Ideal     Premium   Very Good Premium  
## [47911] Premium   Very Good Ideal     Good      Very Good Very Good
## [47917] Ideal     Ideal     Premium   Fair      Premium   Premium  
## [47923] Premium   Ideal     Ideal     Premium   Premium   Ideal    
## [47929] Ideal     Ideal     Ideal     Premium   Premium   Premium  
## [47935] Ideal     Premium   Ideal     Ideal     Premium   Premium  
## [47941] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [47947] Very Good Premium   Ideal     Ideal     Good      Premium  
## [47953] Premium   Fair      Very Good Very Good Very Good Ideal    
## [47959] Premium   Ideal     Ideal     Ideal     Ideal     Premium  
## [47965] Ideal     Premium   Ideal     Ideal     Ideal     Ideal    
## [47971] Very Good Very Good Ideal     Ideal     Ideal     Ideal    
## [47977] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [47983] Good      Very Good Very Good Very Good Very Good Very Good
## [47989] Very Good Very Good Good      Premium   Very Good Ideal    
## [47995] Ideal     Premium   Ideal     Good      Ideal     Premium  
## [48001] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [48007] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [48013] Ideal     Good      Fair      Premium   Premium   Ideal    
## [48019] Premium   Premium   Very Good Good      Ideal     Ideal    
## [48025] Good      Good      Good      Ideal     Very Good Premium  
## [48031] Ideal     Very Good Ideal     Ideal     Ideal     Ideal    
## [48037] Ideal     Ideal     Good      Premium   Premium   Good     
## [48043] Premium   Ideal     Very Good Ideal     Very Good Premium  
## [48049] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [48055] Ideal     Premium   Premium   Premium   Premium   Ideal    
## [48061] Ideal     Ideal     Ideal     Ideal     Premium   Premium  
## [48067] Ideal     Premium   Very Good Ideal     Ideal     Ideal    
## [48073] Ideal     Ideal     Good      Very Good Premium   Ideal    
## [48079] Ideal     Very Good Ideal     Premium   Premium   Very Good
## [48085] Ideal     Ideal     Good      Very Good Ideal     Ideal    
## [48091] Premium   Fair      Fair      Premium   Ideal     Ideal    
## [48097] Ideal     Very Good Ideal     Ideal     Good      Ideal    
## [48103] Premium   Ideal     Premium   Premium   Premium   Ideal    
## [48109] Ideal     Ideal     Premium   Ideal     Ideal     Ideal    
## [48115] Premium   Ideal     Ideal     Ideal     Ideal     Ideal    
## [48121] Very Good Good      Very Good Good      Ideal     Premium  
## [48127] Ideal     Very Good Ideal     Ideal     Fair      Good     
## [48133] Premium   Premium   Very Good Very Good Very Good Very Good
## [48139] Very Good Ideal     Premium   Ideal     Good      Premium  
## [48145] Very Good Ideal     Premium   Premium   Very Good Ideal    
## [48151] Premium   Ideal     Ideal     Ideal     Ideal     Ideal    
## [48157] Very Good Very Good Very Good Very Good Very Good Premium  
## [48163] Ideal     Ideal     Ideal     Good      Premium   Ideal    
## [48169] Ideal     Good      Fair      Very Good Premium   Premium  
## [48175] Premium   Premium   Ideal     Ideal     Ideal     Ideal    
## [48181] Ideal     Premium   Fair      Ideal     Good      Premium  
## [48187] Very Good Good      Ideal     Good      Ideal     Ideal    
## [48193] Premium   Premium   Fair      Ideal     Ideal     Premium  
## [48199] Very Good Very Good Ideal     Ideal     Ideal     Ideal    
## [48205] Ideal     Premium   Ideal     Good      Premium   Very Good
## [48211] Ideal     Ideal     Very Good Ideal     Ideal     Ideal    
## [48217] Fair      Ideal     Ideal     Ideal     Ideal     Ideal    
## [48223] Premium   Ideal     Ideal     Ideal     Ideal     Premium  
## [48229] Premium   Ideal     Ideal     Premium   Premium   Premium  
## [48235] Ideal     Ideal     Ideal     Good      Premium   Ideal    
## [48241] Very Good Fair      Very Good Ideal     Good      Good     
## [48247] Very Good Very Good Very Good Very Good Premium   Ideal    
## [48253] Fair      Premium   Very Good Ideal     Ideal     Premium  
## [48259] Premium   Fair      Very Good Ideal     Good      Ideal    
## [48265] Ideal     Ideal     Ideal     Ideal     Premium   Ideal    
## [48271] Ideal     Premium   Ideal     Ideal     Ideal     Ideal    
## [48277] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [48283] Ideal     Fair      Ideal     Ideal     Ideal     Ideal    
## [48289] Ideal     Ideal     Premium   Ideal     Ideal     Ideal    
## [48295] Ideal     Ideal     Ideal     Premium   Very Good Very Good
## [48301] Ideal     Ideal     Ideal     Good      Ideal     Ideal    
## [48307] Ideal     Ideal     Ideal     Very Good Ideal     Ideal    
## [48313] Ideal     Ideal     Ideal     Ideal     Very Good Very Good
## [48319] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [48325] Ideal     Good      Ideal     Ideal     Premium   Premium  
## [48331] Very Good Very Good Ideal     Good      Ideal     Very Good
## [48337] Premium   Premium   Ideal     Premium   Good      Very Good
## [48343] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [48349] Premium   Ideal     Ideal     Ideal     Fair      Ideal    
## [48355] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [48361] Very Good Ideal     Ideal     Ideal     Ideal     Premium  
## [48367] Very Good Ideal     Ideal     Premium   Ideal     Ideal    
## [48373] Ideal     Ideal     Very Good Very Good Ideal     Good     
## [48379] Ideal     Ideal     Good      Premium   Ideal     Premium  
## [48385] Very Good Ideal     Premium   Ideal     Good      Very Good
## [48391] Ideal     Very Good Ideal     Good      Premium   Premium  
## [48397] Very Good Ideal     Ideal     Ideal     Premium   Good     
## [48403] Very Good Very Good Good      Ideal     Very Good Very Good
## [48409] Very Good Very Good Very Good Very Good Ideal     Premium  
## [48415] Premium   Premium   Very Good Very Good Very Good Premium  
## [48421] Ideal     Good      Very Good Premium   Premium   Premium  
## [48427] Very Good Ideal     Very Good Ideal     Ideal     Ideal    
## [48433] Premium   Ideal     Premium   Ideal     Ideal     Ideal    
## [48439] Ideal     Ideal     Premium   Ideal     Ideal     Ideal    
## [48445] Ideal     Premium   Very Good Premium   Fair      Ideal    
## [48451] Very Good Ideal     Ideal     Premium   Very Good Premium  
## [48457] Premium   Very Good Ideal     Ideal     Good      Ideal    
## [48463] Good      Premium   Very Good Very Good Premium   Premium  
## [48469] Very Good Very Good Very Good Ideal     Ideal     Ideal    
## [48475] Good      Good      Very Good Very Good Very Good Ideal    
## [48481] Ideal     Ideal     Good      Premium   Ideal     Ideal    
## [48487] Ideal     Premium   Fair      Premium   Very Good Ideal    
## [48493] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [48499] Fair      Fair      Very Good Ideal     Ideal     Ideal    
## [48505] Premium   Fair      Premium   Fair      Very Good Very Good
## [48511] Ideal     Ideal     Very Good Ideal     Ideal     Premium  
## [48517] Ideal     Ideal     Ideal     Premium   Ideal     Ideal    
## [48523] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [48529] Very Good Very Good Very Good Very Good Ideal     Very Good
## [48535] Ideal     Ideal     Ideal     Premium   Premium   Premium  
## [48541] Premium   Very Good Very Good Very Good Ideal     Good     
## [48547] Very Good Very Good Good      Ideal     Premium   Premium  
## [48553] Ideal     Very Good Premium   Good      Ideal     Fair     
## [48559] Very Good Ideal     Ideal     Ideal     Ideal     Ideal    
## [48565] Ideal     Good      Premium   Premium   Ideal     Ideal    
## [48571] Very Good Very Good Very Good Very Good Ideal     Ideal    
## [48577] Premium   Premium   Ideal     Premium   Premium   Ideal    
## [48583] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [48589] Premium   Premium   Ideal     Fair      Very Good Good     
## [48595] Fair      Very Good Very Good Fair      Fair      Ideal    
## [48601] Ideal     Premium   Ideal     Premium   Premium   Very Good
## [48607] Fair      Very Good Ideal     Fair      Fair      Ideal    
## [48613] Ideal     Ideal     Premium   Premium   Ideal     Fair     
## [48619] Very Good Very Good Premium   Good      Premium   Very Good
## [48625] Ideal     Good      Fair      Very Good Very Good Very Good
## [48631] Fair      Premium   Good      Very Good Good      Very Good
## [48637] Ideal     Ideal     Premium   Premium   Premium   Premium  
## [48643] Ideal     Ideal     Ideal     Premium   Good      Ideal    
## [48649] Premium   Good      Premium   Premium   Premium   Good     
## [48655] Premium   Very Good Premium   Premium   Good      Good     
## [48661] Very Good Very Good Ideal     Ideal     Ideal     Ideal    
## [48667] Ideal     Premium   Ideal     Fair      Premium   Very Good
## [48673] Very Good Very Good Very Good Very Good Ideal     Ideal    
## [48679] Ideal     Very Good Ideal     Ideal     Ideal     Ideal    
## [48685] Premium   Premium   Fair      Good      Ideal     Ideal    
## [48691] Premium   Ideal     Very Good Very Good Good      Ideal    
## [48697] Very Good Very Good Good      Very Good Premium   Premium  
## [48703] Very Good Very Good Very Good Good      Ideal     Good     
## [48709] Very Good Ideal     Premium   Ideal     Premium   Ideal    
## [48715] Ideal     Premium   Very Good Ideal     Premium   Fair     
## [48721] Premium   Ideal     Ideal     Premium   Ideal     Ideal    
## [48727] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [48733] Ideal     Ideal     Premium   Ideal     Ideal     Very Good
## [48739] Ideal     Premium   Very Good Ideal     Good      Good     
## [48745] Good      Premium   Premium   Premium   Very Good Ideal    
## [48751] Ideal     Ideal     Ideal     Good      Good      Fair     
## [48757] Ideal     Ideal     Ideal     Premium   Ideal     Premium  
## [48763] Premium   Good      Premium   Very Good Ideal     Ideal    
## [48769] Ideal     Ideal     Ideal     Premium   Premium   Very Good
## [48775] Premium   Good      Ideal     Good      Premium   Premium  
## [48781] Premium   Very Good Very Good Very Good Very Good Ideal    
## [48787] Ideal     Very Good Ideal     Ideal     Ideal     Premium  
## [48793] Premium   Premium   Premium   Good      Premium   Ideal    
## [48799] Ideal     Ideal     Very Good Very Good Ideal     Ideal    
## [48805] Ideal     Ideal     Ideal     Ideal     Ideal     Very Good
## [48811] Ideal     Ideal     Ideal     Good      Ideal     Very Good
## [48817] Ideal     Premium   Good      Premium   Very Good Very Good
## [48823] Ideal     Good      Good      Good      Very Good Premium  
## [48829] Good      Ideal     Good      Ideal     Ideal     Premium  
## [48835] Ideal     Ideal     Premium   Ideal     Premium   Ideal    
## [48841] Ideal     Ideal     Ideal     Premium   Very Good Ideal    
## [48847] Ideal     Fair      Very Good Fair      Ideal     Premium  
## [48853] Good      Good      Good      Ideal     Ideal     Ideal    
## [48859] Very Good Very Good Good      Good      Very Good Good     
## [48865] Very Good Ideal     Ideal     Very Good Very Good Good     
## [48871] Fair      Premium   Fair      Very Good Very Good Very Good
## [48877] Premium   Very Good Fair      Very Good Premium   Premium  
## [48883] Premium   Premium   Fair      Good      Very Good Very Good
## [48889] Very Good Premium   Ideal     Ideal     Ideal     Ideal    
## [48895] Ideal     Very Good Very Good Premium   Ideal     Premium  
## [48901] Premium   Ideal     Very Good Very Good Very Good Very Good
## [48907] Very Good Ideal     Ideal     Ideal     Ideal     Ideal    
## [48913] Ideal     Ideal     Good      Ideal     Ideal     Ideal    
## [48919] Premium   Very Good Very Good Very Good Premium   Premium  
## [48925] Very Good Very Good Very Good Ideal     Premium   Premium  
## [48931] Premium   Very Good Premium   Ideal     Ideal     Premium  
## [48937] Premium   Very Good Ideal     Very Good Very Good Premium  
## [48943] Fair      Premium   Very Good Ideal     Ideal     Premium  
## [48949] Ideal     Ideal     Premium   Premium   Very Good Very Good
## [48955] Very Good Very Good Very Good Very Good Good      Ideal    
## [48961] Ideal     Very Good Premium   Premium   Very Good Very Good
## [48967] Ideal     Ideal     Ideal     Very Good Ideal     Ideal    
## [48973] Very Good Ideal     Good      Ideal     Premium   Premium  
## [48979] Ideal     Premium   Ideal     Ideal     Very Good Very Good
## [48985] Very Good Ideal     Ideal     Ideal     Ideal     Ideal    
## [48991] Ideal     Good      Good      Fair      Fair      Very Good
## [48997] Premium   Ideal     Fair      Very Good Fair      Premium  
## [49003] Premium   Premium   Very Good Ideal     Ideal     Ideal    
## [49009] Ideal     Fair      Premium   Very Good Ideal     Very Good
## [49015] Ideal     Premium   Very Good Ideal     Ideal     Premium  
## [49021] Ideal     Good      Ideal     Ideal     Ideal     Premium  
## [49027] Good      Ideal     Ideal     Very Good Very Good Ideal    
## [49033] Ideal     Ideal     Ideal     Ideal     Very Good Ideal    
## [49039] Ideal     Good      Fair      Premium   Very Good Ideal    
## [49045] Premium   Good      Ideal     Ideal     Good      Premium  
## [49051] Premium   Ideal     Very Good Very Good Very Good Ideal    
## [49057] Ideal     Good      Very Good Very Good Very Good Ideal    
## [49063] Premium   Good      Very Good Ideal     Ideal     Good     
## [49069] Premium   Ideal     Premium   Ideal     Ideal     Good     
## [49075] Very Good Very Good Ideal     Premium   Fair      Premium  
## [49081] Ideal     Very Good Ideal     Ideal     Ideal     Ideal    
## [49087] Ideal     Ideal     Premium   Very Good Very Good Good     
## [49093] Good      Ideal     Very Good Very Good Ideal     Ideal    
## [49099] Good      Fair      Very Good Very Good Ideal     Ideal    
## [49105] Ideal     Premium   Ideal     Ideal     Premium   Premium  
## [49111] Ideal     Ideal     Good      Good      Premium   Very Good
## [49117] Very Good Very Good Ideal     Very Good Good      Very Good
## [49123] Good      Very Good Ideal     Ideal     Ideal     Ideal    
## [49129] Ideal     Ideal     Ideal     Ideal     Premium   Ideal    
## [49135] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [49141] Ideal     Good      Ideal     Ideal     Very Good Very Good
## [49147] Ideal     Good      Fair      Ideal     Ideal     Fair     
## [49153] Premium   Very Good Ideal     Ideal     Very Good Premium  
## [49159] Ideal     Premium   Fair      Very Good Premium   Premium  
## [49165] Premium   Very Good Good      Premium   Ideal     Very Good
## [49171] Very Good Very Good Premium   Good      Ideal     Premium  
## [49177] Good      Good      Premium   Premium   Ideal     Premium  
## [49183] Ideal     Ideal     Ideal     Premium   Ideal     Premium  
## [49189] Fair      Ideal     Very Good Very Good Very Good Good     
## [49195] Very Good Ideal     Ideal     Very Good Ideal     Very Good
## [49201] Very Good Ideal     Ideal     Ideal     Ideal     Ideal    
## [49207] Ideal     Fair      Ideal     Ideal     Ideal     Good     
## [49213] Ideal     Ideal     Very Good Ideal     Ideal     Very Good
## [49219] Very Good Premium   Good      Premium   Ideal     Very Good
## [49225] Very Good Very Good Ideal     Very Good Very Good Good     
## [49231] Ideal     Ideal     Ideal     Ideal     Ideal     Fair     
## [49237] Premium   Very Good Ideal     Good      Very Good Very Good
## [49243] Ideal     Ideal     Ideal     Ideal     Ideal     Good     
## [49249] Very Good Fair      Very Good Ideal     Ideal     Ideal    
## [49255] Premium   Premium   Ideal     Good      Very Good Ideal    
## [49261] Very Good Very Good Fair      Good      Fair      Ideal    
## [49267] Premium   Premium   Ideal     Ideal     Ideal     Ideal    
## [49273] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [49279] Premium   Good      Ideal     Ideal     Very Good Very Good
## [49285] Ideal     Ideal     Good      Fair      Very Good Ideal    
## [49291] Very Good Ideal     Ideal     Ideal     Ideal     Ideal    
## [49297] Ideal     Ideal     Good      Premium   Good      Very Good
## [49303] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [49309] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [49315] Good      Ideal     Premium   Premium   Ideal     Very Good
## [49321] Ideal     Premium   Very Good Ideal     Ideal     Very Good
## [49327] Ideal     Good      Fair      Premium   Very Good Very Good
## [49333] Very Good Good      Very Good Ideal     Ideal     Ideal    
## [49339] Very Good Very Good Premium   Good      Good      Good     
## [49345] Very Good Premium   Very Good Premium   Ideal     Ideal    
## [49351] Ideal     Ideal     Ideal     Ideal     Very Good Very Good
## [49357] Very Good Ideal     Ideal     Ideal     Ideal     Good     
## [49363] Good      Premium   Ideal     Ideal     Ideal     Ideal    
## [49369] Very Good Very Good Very Good Ideal     Ideal     Good     
## [49375] Premium   Fair      Premium   Ideal     Very Good Ideal    
## [49381] Good      Very Good Very Good Ideal     Fair      Premium  
## [49387] Premium   Fair      Ideal     Ideal     Ideal     Very Good
## [49393] Very Good Good      Fair      Ideal     Ideal     Premium  
## [49399] Very Good Very Good Very Good Premium   Ideal     Ideal    
## [49405] Premium   Very Good Very Good Very Good Ideal     Very Good
## [49411] Premium   Very Good Very Good Very Good Very Good Ideal    
## [49417] Premium   Ideal     Ideal     Ideal     Ideal     Ideal    
## [49423] Very Good Very Good Very Good Very Good Very Good Premium  
## [49429] Premium   Ideal     Ideal     Ideal     Premium   Ideal    
## [49435] Very Good Very Good Ideal     Ideal     Good      Good     
## [49441] Premium   Ideal     Ideal     Ideal     Ideal     Ideal    
## [49447] Ideal     Ideal     Ideal     Very Good Very Good Ideal    
## [49453] Ideal     Ideal     Good      Ideal     Ideal     Premium  
## [49459] Very Good Premium   Ideal     Fair      Ideal     Ideal    
## [49465] Fair      Premium   Very Good Very Good Ideal     Ideal    
## [49471] Ideal     Very Good Very Good Very Good Very Good Very Good
## [49477] Premium   Very Good Ideal     Good      Good      Ideal    
## [49483] Ideal     Good      Fair      Very Good Good      Ideal    
## [49489] Good      Good      Ideal     Ideal     Ideal     Very Good
## [49495] Fair      Fair      Fair      Ideal     Ideal     Ideal    
## [49501] Ideal     Ideal     Very Good Ideal     Ideal     Good     
## [49507] Good      Fair      Very Good Ideal     Good      Good     
## [49513] Fair      Ideal     Very Good Premium   Ideal     Ideal    
## [49519] Ideal     Ideal     Very Good Very Good Very Good Ideal    
## [49525] Very Good Ideal     Very Good Ideal     Premium   Very Good
## [49531] Good      Premium   Ideal     Very Good Ideal     Ideal    
## [49537] Good      Very Good Fair      Premium   Fair      Very Good
## [49543] Very Good Good      Premium   Very Good Very Good Very Good
## [49549] Very Good Ideal     Ideal     Ideal     Ideal     Premium  
## [49555] Fair      Premium   Good      Good      Very Good Very Good
## [49561] Very Good Very Good Very Good Ideal     Ideal     Ideal    
## [49567] Ideal     Good      Good      Good      Fair      Premium  
## [49573] Premium   Ideal     Very Good Very Good Ideal     Ideal    
## [49579] Ideal     Fair      Ideal     Ideal     Very Good Premium  
## [49585] Fair      Very Good Ideal     Premium   Premium   Ideal    
## [49591] Very Good Very Good Very Good Ideal     Ideal     Very Good
## [49597] Very Good Very Good Very Good Premium   Ideal     Ideal    
## [49603] Ideal     Good      Good      Good      Very Good Very Good
## [49609] Very Good Ideal     Ideal     Ideal     Fair      Good     
## [49615] Ideal     Fair      Ideal     Ideal     Good      Good     
## [49621] Ideal     Very Good Ideal     Very Good Good      Premium  
## [49627] Premium   Premium   Ideal     Premium   Premium   Premium  
## [49633] Premium   Premium   Premium   Very Good Premium   Premium  
## [49639] Very Good Premium   Ideal     Premium   Good      Ideal    
## [49645] Premium   Premium   Premium   Premium   Premium   Premium  
## [49651] Premium   Ideal     Good      Good      Very Good Very Good
## [49657] Very Good Very Good Very Good Very Good Very Good Very Good
## [49663] Ideal     Ideal     Ideal     Very Good Premium   Ideal    
## [49669] Ideal     Good      Ideal     Good      Good      Premium  
## [49675] Ideal     Premium   Ideal     Good      Ideal     Ideal    
## [49681] Ideal     Very Good Good      Fair      Premium   Ideal    
## [49687] Very Good Ideal     Very Good Very Good Ideal     Ideal    
## [49693] Good      Very Good Ideal     Ideal     Very Good Very Good
## [49699] Very Good Very Good Very Good Premium   Very Good Ideal    
## [49705] Ideal     Ideal     Ideal     Ideal     Premium   Ideal    
## [49711] Premium   Premium   Fair      Very Good Ideal     Premium  
## [49717] Premium   Very Good Fair      Ideal     Premium   Premium  
## [49723] Fair      Good      Good      Fair      Fair      Very Good
## [49729] Ideal     Premium   Premium   Ideal     Premium   Ideal    
## [49735] Ideal     Ideal     Ideal     Very Good Very Good Very Good
## [49741] Ideal     Premium   Premium   Premium   Ideal     Good     
## [49747] Ideal     Good      Ideal     Ideal     Very Good Ideal    
## [49753] Ideal     Very Good Ideal     Very Good Premium   Good     
## [49759] Ideal     Ideal     Good      Premium   Very Good Ideal    
## [49765] Ideal     Ideal     Premium   Premium   Premium   Ideal    
## [49771] Ideal     Ideal     Premium   Fair      Very Good Premium  
## [49777] Good      Very Good Ideal     Very Good Very Good Ideal    
## [49783] Very Good Ideal     Ideal     Good      Ideal     Ideal    
## [49789] Ideal     Very Good Premium   Premium   Good      Ideal    
## [49795] Ideal     Ideal     Ideal     Ideal     Ideal     Very Good
## [49801] Very Good Ideal     Ideal     Ideal     Fair      Fair     
## [49807] Good      Ideal     Ideal     Ideal     Ideal     Ideal    
## [49813] Good      Very Good Ideal     Ideal     Fair      Ideal    
## [49819] Good      Ideal     Ideal     Ideal     Ideal     Good     
## [49825] Fair      Fair      Ideal     Fair      Premium   Premium  
## [49831] Premium   Fair      Fair      Very Good Very Good Premium  
## [49837] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [49843] Premium   Ideal     Premium   Very Good Very Good Very Good
## [49849] Premium   Ideal     Ideal     Premium   Very Good Very Good
## [49855] Premium   Premium   Very Good Ideal     Ideal     Premium  
## [49861] Ideal     Premium   Ideal     Premium   Ideal     Good     
## [49867] Good      Very Good Very Good Very Good Ideal     Ideal    
## [49873] Ideal     Ideal     Premium   Premium   Premium   Very Good
## [49879] Good      Fair      Premium   Good      Very Good Ideal    
## [49885] Premium   Premium   Ideal     Ideal     Ideal     Ideal    
## [49891] Ideal     Ideal     Very Good Ideal     Very Good Ideal    
## [49897] Premium   Ideal     Very Good Ideal     Ideal     Ideal    
## [49903] Ideal     Very Good Very Good Very Good Very Good Very Good
## [49909] Ideal     Premium   Premium   Ideal     Ideal     Ideal    
## [49915] Good      Premium   Very Good Ideal     Premium   Very Good
## [49921] Premium   Ideal     Good      Fair      Very Good Very Good
## [49927] Premium   Ideal     Premium   Premium   Premium   Ideal    
## [49933] Premium   Premium   Very Good Very Good Good      Ideal    
## [49939] Good      Good      Very Good Very Good Very Good Very Good
## [49945] Good      Ideal     Good      Very Good Ideal     Ideal    
## [49951] Premium   Premium   Premium   Ideal     Very Good Ideal    
## [49957] Very Good Premium   Ideal     Good      Very Good Premium  
## [49963] Very Good Premium   Premium   Premium   Very Good Premium  
## [49969] Good      Very Good Very Good Very Good Very Good Very Good
## [49975] Very Good Very Good Very Good Very Good Very Good Very Good
## [49981] Good      Ideal     Premium   Ideal     Ideal     Good     
## [49987] Good      Good      Very Good Good      Ideal     Ideal    
## [49993] Ideal     Ideal     Premium   Premium   Fair      Premium  
## [49999] Premium   Ideal     Ideal     Fair      Premium   Fair     
## [50005] Premium   Ideal     Very Good Good      Ideal     Good     
## [50011] Premium   Very Good Premium   Ideal     Premium   Very Good
## [50017] Very Good Premium   Very Good Premium   Very Good Very Good
## [50023] Good      Ideal     Ideal     Very Good Premium   Premium  
## [50029] Ideal     Ideal     Ideal     Very Good Very Good Very Good
## [50035] Premium   Very Good Very Good Fair      Fair      Very Good
## [50041] Very Good Premium   Ideal     Premium   Very Good Very Good
## [50047] Very Good Very Good Very Good Premium   Ideal     Ideal    
## [50053] Very Good Very Good Premium   Ideal     Ideal     Very Good
## [50059] Ideal     Ideal     Ideal     Very Good Premium   Premium  
## [50065] Ideal     Ideal     Ideal     Ideal     Ideal     Good     
## [50071] Premium   Premium   Premium   Premium   Ideal     Fair     
## [50077] Ideal     Ideal     Ideal     Ideal     Premium   Very Good
## [50083] Ideal     Very Good Very Good Very Good Very Good Premium  
## [50089] Premium   Ideal     Ideal     Ideal     Ideal     Ideal    
## [50095] Premium   Premium   Ideal     Ideal     Very Good Premium  
## [50101] Very Good Very Good Good      Fair      Very Good Ideal    
## [50107] Ideal     Ideal     Fair      Premium   Very Good Very Good
## [50113] Ideal     Ideal     Good      Very Good Very Good Ideal    
## [50119] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [50125] Very Good Very Good Fair      Ideal     Ideal     Ideal    
## [50131] Premium   Very Good Premium   Good      Very Good Premium  
## [50137] Very Good Premium   Very Good Good      Very Good Ideal    
## [50143] Premium   Premium   Ideal     Good      Ideal     Ideal    
## [50149] Premium   Good      Good      Premium   Premium   Premium  
## [50155] Premium   Ideal     Premium   Ideal     Ideal     Ideal    
## [50161] Very Good Very Good Very Good Good      Premium   Good     
## [50167] Ideal     Ideal     Ideal     Premium   Premium   Very Good
## [50173] Very Good Very Good Good      Ideal     Ideal     Very Good
## [50179] Good      Very Good Very Good Ideal     Good      Ideal    
## [50185] Premium   Very Good Very Good Very Good Ideal     Ideal    
## [50191] Ideal     Premium   Premium   Very Good Good      Ideal    
## [50197] Ideal     Very Good Very Good Ideal     Good      Good     
## [50203] Very Good Premium   Premium   Ideal     Ideal     Ideal    
## [50209] Very Good Very Good Ideal     Good      Good      Good     
## [50215] Very Good Ideal     Very Good Ideal     Premium   Very Good
## [50221] Very Good Very Good Ideal     Ideal     Ideal     Very Good
## [50227] Ideal     Ideal     Premium   Very Good Ideal     Ideal    
## [50233] Ideal     Ideal     Ideal     Ideal     Premium   Ideal    
## [50239] Ideal     Ideal     Ideal     Very Good Good      Premium  
## [50245] Premium   Ideal     Good      Ideal     Fair      Premium  
## [50251] Premium   Premium   Premium   Premium   Premium   Ideal    
## [50257] Fair      Premium   Very Good Ideal     Ideal     Premium  
## [50263] Ideal     Ideal     Ideal     Very Good Very Good Ideal    
## [50269] Very Good Premium   Ideal     Premium   Very Good Very Good
## [50275] Good      Good      Very Good Very Good Very Good Ideal    
## [50281] Very Good Very Good Very Good Very Good Ideal     Ideal    
## [50287] Ideal     Premium   Very Good Ideal     Ideal     Premium  
## [50293] Ideal     Very Good Ideal     Ideal     Very Good Very Good
## [50299] Very Good Very Good Very Good Very Good Very Good Very Good
## [50305] Very Good Ideal     Ideal     Premium   Ideal     Very Good
## [50311] Good      Premium   Premium   Good      Premium   Ideal    
## [50317] Fair      Very Good Ideal     Ideal     Ideal     Ideal    
## [50323] Ideal     Good      Very Good Very Good Ideal     Fair     
## [50329] Ideal     Good      Ideal     Premium   Very Good Ideal    
## [50335] Ideal     Ideal     Good      Good      Very Good Premium  
## [50341] Ideal     Ideal     Good      Good      Good      Premium  
## [50347] Ideal     Ideal     Very Good Ideal     Premium   Good     
## [50353] Very Good Premium   Fair      Ideal     Very Good Ideal    
## [50359] Premium   Premium   Very Good Premium   Ideal     Ideal    
## [50365] Ideal     Good      Ideal     Ideal     Premium   Ideal    
## [50371] Ideal     Premium   Very Good Ideal     Very Good Ideal    
## [50377] Ideal     Good      Premium   Fair      Very Good Good     
## [50383] Very Good Premium   Ideal     Ideal     Ideal     Good     
## [50389] Premium   Very Good Very Good Ideal     Ideal     Good     
## [50395] Very Good Very Good Good      Ideal     Ideal     Ideal    
## [50401] Ideal     Ideal     Very Good Very Good Ideal     Premium  
## [50407] Ideal     Very Good Ideal     Ideal     Premium   Ideal    
## [50413] Ideal     Good      Good      Good      Ideal     Very Good
## [50419] Very Good Ideal     Ideal     Ideal     Ideal     Very Good
## [50425] Very Good Premium   Ideal     Ideal     Ideal     Very Good
## [50431] Very Good Ideal     Ideal     Premium   Very Good Premium  
## [50437] Good      Premium   Very Good Very Good Very Good Very Good
## [50443] Very Good Ideal     Ideal     Ideal     Ideal     Very Good
## [50449] Very Good Very Good Very Good Ideal     Premium   Ideal    
## [50455] Ideal     Good      Good      Good      Premium   Very Good
## [50461] Premium   Very Good Very Good Premium   Ideal     Ideal    
## [50467] Ideal     Very Good Very Good Very Good Ideal     Ideal    
## [50473] Ideal     Good      Premium   Very Good Ideal     Very Good
## [50479] Ideal     Good      Premium   Ideal     Ideal     Ideal    
## [50485] Good      Premium   Good      Premium   Premium   Premium  
## [50491] Fair      Premium   Premium   Premium   Premium   Ideal    
## [50497] Ideal     Very Good Very Good Ideal     Ideal     Ideal    
## [50503] Good      Ideal     Good      Very Good Ideal     Ideal    
## [50509] Ideal     Good      Good      Premium   Ideal     Ideal    
## [50515] Very Good Very Good Premium   Premium   Premium   Ideal    
## [50521] Ideal     Fair      Very Good Very Good Very Good Very Good
## [50527] Ideal     Very Good Premium   Very Good Good      Fair     
## [50533] Premium   Premium   Ideal     Very Good Very Good Very Good
## [50539] Ideal     Premium   Ideal     Ideal     Good      Good     
## [50545] Premium   Premium   Very Good Very Good Very Good Very Good
## [50551] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [50557] Good      Very Good Good      Premium   Very Good Very Good
## [50563] Very Good Ideal     Premium   Very Good Very Good Very Good
## [50569] Good      Ideal     Ideal     Ideal     Good      Fair     
## [50575] Ideal     Premium   Very Good Premium   Ideal     Ideal    
## [50581] Very Good Very Good Fair      Very Good Very Good Ideal    
## [50587] Good      Very Good Good      Very Good Good      Good     
## [50593] Ideal     Premium   Very Good Good      Premium   Ideal    
## [50599] Ideal     Ideal     Premium   Ideal     Fair      Very Good
## [50605] Premium   Premium   Ideal     Ideal     Ideal     Very Good
## [50611] Ideal     Premium   Very Good Very Good Very Good Good     
## [50617] Good      Ideal     Ideal     Very Good Premium   Very Good
## [50623] Very Good Ideal     Ideal     Ideal     Ideal     Good     
## [50629] Very Good Very Good Very Good Very Good Very Good Very Good
## [50635] Very Good Very Good Very Good Very Good Very Good Very Good
## [50641] Premium   Good      Good      Ideal     Ideal     Very Good
## [50647] Ideal     Good      Good      Premium   Good      Very Good
## [50653] Premium   Ideal     Very Good Good      Good      Very Good
## [50659] Ideal     Ideal     Very Good Good      Ideal     Very Good
## [50665] Very Good Good      Ideal     Premium   Ideal     Very Good
## [50671] Very Good Ideal     Ideal     Ideal     Ideal     Ideal    
## [50677] Ideal     Premium   Fair      Very Good Very Good Ideal    
## [50683] Ideal     Ideal     Good      Ideal     Ideal     Ideal    
## [50689] Very Good Good      Premium   Ideal     Very Good Very Good
## [50695] Fair      Ideal     Ideal     Ideal     Ideal     Good     
## [50701] Premium   Premium   Ideal     Ideal     Ideal     Very Good
## [50707] Premium   Premium   Ideal     Ideal     Fair      Very Good
## [50713] Very Good Ideal     Ideal     Good      Premium   Fair     
## [50719] Very Good Very Good Very Good Premium   Ideal     Ideal    
## [50725] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [50731] Ideal     Ideal     Premium   Ideal     Premium   Very Good
## [50737] Very Good Ideal     Premium   Very Good Very Good Premium  
## [50743] Ideal     Ideal     Ideal     Ideal     Fair      Premium  
## [50749] Very Good Very Good Very Good Ideal     Good      Ideal    
## [50755] Ideal     Ideal     Ideal     Ideal     Premium   Good     
## [50761] Premium   Very Good Very Good Very Good Premium   Premium  
## [50767] Ideal     Ideal     Premium   Fair      Premium   Good     
## [50773] Fair      Fair      Ideal     Ideal     Ideal     Very Good
## [50779] Ideal     Ideal     Very Good Premium   Ideal     Ideal    
## [50785] Premium   Ideal     Premium   Very Good Very Good Very Good
## [50791] Very Good Fair      Ideal     Good      Very Good Good     
## [50797] Premium   Good      Ideal     Premium   Premium   Premium  
## [50803] Ideal     Ideal     Fair      Premium   Ideal     Ideal    
## [50809] Very Good Very Good Premium   Ideal     Ideal     Good     
## [50815] Premium   Premium   Ideal     Ideal     Ideal     Ideal    
## [50821] Ideal     Good      Ideal     Good      Ideal     Very Good
## [50827] Very Good Premium   Ideal     Good      Good      Ideal    
## [50833] Ideal     Very Good Very Good Premium   Very Good Very Good
## [50839] Premium   Ideal     Fair      Good      Good      Very Good
## [50845] Very Good Good      Good      Premium   Premium   Very Good
## [50851] Very Good Very Good Very Good Very Good Very Good Very Good
## [50857] Fair      Ideal     Ideal     Good      Premium   Very Good
## [50863] Very Good Very Good Ideal     Ideal     Ideal     Ideal    
## [50869] Ideal     Good      Ideal     Premium   Ideal     Premium  
## [50875] Very Good Very Good Good      Premium   Premium   Good     
## [50881] Ideal     Very Good Very Good Very Good Good      Premium  
## [50887] Premium   Premium   Premium   Very Good Premium   Premium  
## [50893] Ideal     Premium   Premium   Ideal     Premium   Fair     
## [50899] Ideal     Ideal     Premium   Very Good Very Good Very Good
## [50905] Ideal     Fair      Fair      Good      Fair      Fair     
## [50911] Ideal     Ideal     Premium   Premium   Very Good Ideal    
## [50917] Ideal     Very Good Good      Premium   Very Good Very Good
## [50923] Ideal     Premium   Premium   Premium   Ideal     Ideal    
## [50929] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [50935] Ideal     Ideal     Very Good Very Good Good      Ideal    
## [50941] Ideal     Premium   Very Good Ideal     Very Good Ideal    
## [50947] Ideal     Ideal     Ideal     Premium   Premium   Premium  
## [50953] Good      Fair      Ideal     Ideal     Very Good Ideal    
## [50959] Very Good Premium   Ideal     Premium   Very Good Ideal    
## [50965] Very Good Ideal     Ideal     Very Good Very Good Very Good
## [50971] Very Good Good      Very Good Good      Very Good Premium  
## [50977] Very Good Premium   Premium   Very Good Ideal     Good     
## [50983] Good      Premium   Good      Ideal     Very Good Ideal    
## [50989] Ideal     Ideal     Good      Good      Very Good Very Good
## [50995] Premium   Very Good Premium   Very Good Premium   Good     
## [51001] Very Good Very Good Premium   Ideal     Good      Premium  
## [51007] Ideal     Fair      Very Good Ideal     Ideal     Ideal    
## [51013] Ideal     Good      Premium   Good      Ideal     Very Good
## [51019] Good      Fair      Good      Very Good Premium   Premium  
## [51025] Ideal     Ideal     Fair      Premium   Premium   Ideal    
## [51031] Ideal     Premium   Very Good Very Good Very Good Premium  
## [51037] Premium   Premium   Premium   Premium   Premium   Premium  
## [51043] Good      Good      Premium   Very Good Very Good Ideal    
## [51049] Fair      Premium   Ideal     Good      Premium   Premium  
## [51055] Premium   Premium   Premium   Ideal     Premium   Very Good
## [51061] Premium   Very Good Very Good Very Good Very Good Ideal    
## [51067] Ideal     Ideal     Premium   Ideal     Very Good Very Good
## [51073] Premium   Good      Ideal     Premium   Premium   Ideal    
## [51079] Ideal     Good      Premium   Very Good Very Good Very Good
## [51085] Ideal     Fair      Ideal     Premium   Very Good Very Good
## [51091] Premium   Ideal     Ideal     Ideal     Good      Very Good
## [51097] Very Good Good      Premium   Ideal     Very Good Fair     
## [51103] Premium   Very Good Very Good Very Good Very Good Very Good
## [51109] Ideal     Ideal     Good      Premium   Good      Ideal    
## [51115] Good      Very Good Ideal     Fair      Fair      Ideal    
## [51121] Ideal     Very Good Fair      Premium   Fair      Ideal    
## [51127] Ideal     Premium   Ideal     Ideal     Ideal     Ideal    
## [51133] Premium   Ideal     Premium   Ideal     Ideal     Premium  
## [51139] Premium   Fair      Ideal     Good      Premium   Very Good
## [51145] Very Good Ideal     Ideal     Premium   Very Good Premium  
## [51151] Good      Premium   Good      Fair      Premium   Ideal    
## [51157] Very Good Very Good Very Good Premium   Ideal     Ideal    
## [51163] Ideal     Premium   Good      Premium   Very Good Premium  
## [51169] Ideal     Good      Ideal     Ideal     Premium   Ideal    
## [51175] Ideal     Premium   Premium   Very Good Very Good Ideal    
## [51181] Ideal     Good      Ideal     Very Good Very Good Premium  
## [51187] Very Good Premium   Premium   Premium   Good      Ideal    
## [51193] Very Good Premium   Premium   Premium   Ideal     Very Good
## [51199] Very Good Fair      Ideal     Ideal     Fair      Good     
## [51205] Good      Ideal     Premium   Premium   Premium   Very Good
## [51211] Very Good Premium   Premium   Ideal     Premium   Ideal    
## [51217] Premium   Ideal     Ideal     Ideal     Premium   Good     
## [51223] Fair      Fair      Premium   Fair      Premium   Fair     
## [51229] Good      Premium   Fair      Ideal     Ideal     Premium  
## [51235] Very Good Very Good Ideal     Ideal     Ideal     Good     
## [51241] Very Good Very Good Premium   Ideal     Good      Premium  
## [51247] Very Good Premium   Good      Premium   Ideal     Ideal    
## [51253] Ideal     Ideal     Premium   Very Good Ideal     Ideal    
## [51259] Fair      Ideal     Ideal     Ideal     Premium   Ideal    
## [51265] Good      Ideal     Ideal     Fair      Fair      Very Good
## [51271] Very Good Very Good Ideal     Ideal     Ideal     Ideal    
## [51277] Ideal     Fair      Ideal     Fair      Very Good Very Good
## [51283] Premium   Ideal     Ideal     Ideal     Premium   Ideal    
## [51289] Premium   Ideal     Ideal     Premium   Premium   Good     
## [51295] Ideal     Ideal     Ideal     Premium   Very Good Good     
## [51301] Ideal     Premium   Very Good Good      Premium   Ideal    
## [51307] Premium   Very Good Premium   Good      Good      Ideal    
## [51313] Ideal     Premium   Ideal     Ideal     Good      Ideal    
## [51319] Premium   Good      Ideal     Premium   Good      Very Good
## [51325] Very Good Good      Ideal     Very Good Premium   Very Good
## [51331] Very Good Very Good Very Good Very Good Very Good Premium  
## [51337] Ideal     Ideal     Ideal     Premium   Premium   Good     
## [51343] Fair      Very Good Premium   Premium   Ideal     Good     
## [51349] Fair      Ideal     Good      Premium   Very Good Premium  
## [51355] Premium   Ideal     Premium   Premium   Premium   Premium  
## [51361] Fair      Premium   Very Good Ideal     Ideal     Ideal    
## [51367] Ideal     Ideal     Premium   Very Good Ideal     Ideal    
## [51373] Good      Premium   Good      Very Good Very Good Ideal    
## [51379] Good      Premium   Premium   Good      Very Good Very Good
## [51385] Very Good Ideal     Ideal     Ideal     Ideal     Premium  
## [51391] Premium   Fair      Very Good Premium   Premium   Very Good
## [51397] Very Good Very Good Good      Premium   Premium   Premium  
## [51403] Ideal     Fair      Very Good Premium   Ideal     Good     
## [51409] Ideal     Premium   Premium   Premium   Premium   Ideal    
## [51415] Good      Ideal     Ideal     Ideal     Ideal     Ideal    
## [51421] Very Good Premium   Ideal     Good      Fair      Very Good
## [51427] Good      Good      Ideal     Ideal     Ideal     Premium  
## [51433] Very Good Very Good Very Good Ideal     Ideal     Ideal    
## [51439] Very Good Ideal     Ideal     Ideal     Premium   Premium  
## [51445] Premium   Fair      Ideal     Ideal     Ideal     Premium  
## [51451] Premium   Premium   Fair      Fair      Ideal     Ideal    
## [51457] Ideal     Ideal     Ideal     Premium   Premium   Premium  
## [51463] Fair      Fair      Fair      Premium   Fair      Very Good
## [51469] Premium   Ideal     Ideal     Very Good Very Good Ideal    
## [51475] Ideal     Premium   Ideal     Ideal     Ideal     Premium  
## [51481] Good      Very Good Ideal     Ideal     Ideal     Ideal    
## [51487] Premium   Ideal     Ideal     Good      Premium   Premium  
## [51493] Premium   Premium   Good      Fair      Fair      Fair     
## [51499] Very Good Very Good Ideal     Ideal     Good      Ideal    
## [51505] Very Good Ideal     Premium   Ideal     Ideal     Premium  
## [51511] Ideal     Premium   Very Good Fair      Ideal     Fair     
## [51517] Fair      Premium   Ideal     Very Good Ideal     Ideal    
## [51523] Premium   Very Good Very Good Premium   Good      Very Good
## [51529] Ideal     Ideal     Premium   Premium   Ideal     Premium  
## [51535] Ideal     Ideal     Ideal     Premium   Ideal     Premium  
## [51541] Ideal     Good      Premium   Fair      Premium   Ideal    
## [51547] Ideal     Premium   Premium   Ideal     Ideal     Ideal    
## [51553] Premium   Premium   Premium   Good      Ideal     Ideal    
## [51559] Good      Premium   Very Good Very Good Premium   Ideal    
## [51565] Premium   Very Good Good      Good      Ideal     Ideal    
## [51571] Good      Fair      Premium   Very Good Premium   Premium  
## [51577] Good      Very Good Ideal     Premium   Premium   Very Good
## [51583] Very Good Premium   Ideal     Premium   Premium   Ideal    
## [51589] Premium   Ideal     Premium   Good      Very Good Very Good
## [51595] Good      Premium   Ideal     Good      Good      Good     
## [51601] Good      Premium   Premium   Good      Very Good Premium  
## [51607] Fair      Premium   Premium   Very Good Premium   Ideal    
## [51613] Very Good Very Good Very Good Very Good Ideal     Fair     
## [51619] Very Good Very Good Very Good Good      Premium   Very Good
## [51625] Fair      Premium   Fair      Premium   Fair      Very Good
## [51631] Premium   Good      Very Good Very Good Very Good Ideal    
## [51637] Premium   Good      Ideal     Ideal     Ideal     Good     
## [51643] Premium   Good      Premium   Ideal     Very Good Very Good
## [51649] Very Good Very Good Very Good Ideal     Ideal     Ideal    
## [51655] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [51661] Very Good Ideal     Ideal     Very Good Very Good Ideal    
## [51667] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [51673] Ideal     Ideal     Good      Premium   Premium   Premium  
## [51679] Ideal     Premium   Premium   Good      Premium   Fair     
## [51685] Very Good Very Good Ideal     Ideal     Ideal     Very Good
## [51691] Premium   Fair      Premium   Ideal     Very Good Very Good
## [51697] Very Good Very Good Ideal     Very Good Very Good Very Good
## [51703] Very Good Very Good Very Good Very Good Very Good Ideal    
## [51709] Fair      Ideal     Very Good Very Good Very Good Very Good
## [51715] Very Good Very Good Good      Good      Premium   Good     
## [51721] Good      Ideal     Ideal     Very Good Ideal     Premium  
## [51727] Good      Ideal     Fair      Ideal     Fair      Fair     
## [51733] Premium   Ideal     Premium   Very Good Ideal     Ideal    
## [51739] Very Good Very Good Ideal     Premium   Very Good Very Good
## [51745] Ideal     Good      Ideal     Ideal     Very Good Very Good
## [51751] Premium   Ideal     Good      Premium   Very Good Premium  
## [51757] Ideal     Good      Good      Ideal     Premium   Ideal    
## [51763] Ideal     Good      Very Good Very Good Very Good Ideal    
## [51769] Ideal     Good      Very Good Premium   Premium   Premium  
## [51775] Ideal     Ideal     Ideal     Premium   Very Good Good     
## [51781] Very Good Ideal     Very Good Fair      Ideal     Fair     
## [51787] Premium   Premium   Fair      Ideal     Very Good Very Good
## [51793] Good      Very Good Ideal     Premium   Good      Premium  
## [51799] Premium   Premium   Premium   Very Good Premium   Ideal    
## [51805] Premium   Premium   Very Good Good      Ideal     Ideal    
## [51811] Ideal     Ideal     Ideal     Ideal     Very Good Premium  
## [51817] Ideal     Ideal     Ideal     Premium   Ideal     Good     
## [51823] Very Good Premium   Ideal     Ideal     Very Good Very Good
## [51829] Very Good Ideal     Good      Good      Ideal     Very Good
## [51835] Very Good Ideal     Ideal     Very Good Ideal     Fair     
## [51841] Very Good Very Good Very Good Ideal     Ideal     Good     
## [51847] Premium   Premium   Ideal     Premium   Premium   Very Good
## [51853] Ideal     Premium   Premium   Ideal     Very Good Good     
## [51859] Ideal     Premium   Premium   Very Good Very Good Very Good
## [51865] Ideal     Ideal     Ideal     Ideal     Fair      Very Good
## [51871] Ideal     Ideal     Good      Good      Very Good Good     
## [51877] Premium   Good      Very Good Good      Good      Very Good
## [51883] Very Good Premium   Ideal     Ideal     Ideal     Premium  
## [51889] Very Good Good      Ideal     Ideal     Ideal     Very Good
## [51895] Ideal     Ideal     Ideal     Very Good Very Good Ideal    
## [51901] Very Good Premium   Ideal     Very Good Ideal     Ideal    
## [51907] Ideal     Good      Ideal     Premium   Ideal     Good     
## [51913] Good      Very Good Very Good Ideal     Ideal     Very Good
## [51919] Very Good Very Good Very Good Ideal     Very Good Good     
## [51925] Fair      Fair      Ideal     Good      Fair      Ideal    
## [51931] Very Good Good      Ideal     Ideal     Ideal     Ideal    
## [51937] Ideal     Ideal     Very Good Very Good Very Good Good     
## [51943] Good      Premium   Ideal     Premium   Premium   Premium  
## [51949] Ideal     Very Good Premium   Very Good Very Good Ideal    
## [51955] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [51961] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [51967] Ideal     Ideal     Ideal     Ideal     Good      Premium  
## [51973] Premium   Very Good Very Good Very Good Very Good Very Good
## [51979] Ideal     Ideal     Ideal     Premium   Good      Very Good
## [51985] Very Good Very Good Very Good Very Good Very Good Very Good
## [51991] Ideal     Fair      Very Good Ideal     Ideal     Premium  
## [51997] Good      Premium   Premium   Very Good Very Good Very Good
## [52003] Premium   Premium   Premium   Premium   Ideal     Ideal    
## [52009] Ideal     Ideal     Good      Good      Premium   Premium  
## [52015] Very Good Very Good Ideal     Good      Ideal     Very Good
## [52021] Good      Premium   Ideal     Ideal     Ideal     Premium  
## [52027] Premium   Good      Premium   Very Good Very Good Ideal    
## [52033] Good      Very Good Premium   Very Good Very Good Premium  
## [52039] Ideal     Good      Very Good Premium   Ideal     Very Good
## [52045] Premium   Very Good Ideal     Premium   Very Good Ideal    
## [52051] Good      Good      Ideal     Premium   Ideal     Premium  
## [52057] Very Good Very Good Very Good Very Good Ideal     Good     
## [52063] Good      Very Good Ideal     Ideal     Ideal     Very Good
## [52069] Ideal     Ideal     Premium   Very Good Ideal     Ideal    
## [52075] Very Good Very Good Very Good Ideal     Premium   Ideal    
## [52081] Ideal     Ideal     Ideal     Good      Premium   Premium  
## [52087] Ideal     Premium   Ideal     Ideal     Premium   Very Good
## [52093] Fair      Ideal     Very Good Very Good Ideal     Premium  
## [52099] Premium   Ideal     Ideal     Ideal     Ideal     Good     
## [52105] Very Good Ideal     Ideal     Ideal     Ideal     Good     
## [52111] Premium   Good      Very Good Very Good Very Good Ideal    
## [52117] Ideal     Very Good Very Good Ideal     Premium   Premium  
## [52123] Very Good Very Good Premium   Ideal     Ideal     Ideal    
## [52129] Ideal     Very Good Ideal     Premium   Premium   Ideal    
## [52135] Premium   Ideal     Good      Very Good Very Good Ideal    
## [52141] Premium   Very Good Premium   Ideal     Ideal     Premium  
## [52147] Ideal     Ideal     Good      Very Good Premium   Very Good
## [52153] Very Good Very Good Very Good Very Good Very Good Good     
## [52159] Very Good Very Good Premium   Ideal     Ideal     Very Good
## [52165] Ideal     Good      Good      Very Good Very Good Ideal    
## [52171] Ideal     Ideal     Ideal     Premium   Premium   Premium  
## [52177] Good      Ideal     Very Good Very Good Very Good Ideal    
## [52183] Premium   Very Good Good      Very Good Ideal     Ideal    
## [52189] Ideal     Ideal     Ideal     Fair      Very Good Ideal    
## [52195] Premium   Premium   Premium   Premium   Premium   Premium  
## [52201] Ideal     Good      Very Good Ideal     Ideal     Ideal    
## [52207] Ideal     Very Good Very Good Very Good Very Good Very Good
## [52213] Very Good Very Good Very Good Very Good Ideal     Ideal    
## [52219] Premium   Ideal     Very Good Premium   Very Good Very Good
## [52225] Premium   Premium   Good      Ideal     Premium   Premium  
## [52231] Fair      Premium   Premium   Very Good Very Good Very Good
## [52237] Very Good Ideal     Ideal     Premium   Ideal     Premium  
## [52243] Premium   Very Good Very Good Ideal     Ideal     Ideal    
## [52249] Ideal     Ideal     Ideal     Ideal     Ideal     Very Good
## [52255] Premium   Good      Very Good Ideal     Ideal     Ideal    
## [52261] Premium   Ideal     Ideal     Ideal     Ideal     Very Good
## [52267] Ideal     Premium   Fair      Premium   Premium   Very Good
## [52273] Premium   Premium   Ideal     Ideal     Ideal     Premium  
## [52279] Premium   Very Good Ideal     Ideal     Fair      Very Good
## [52285] Good      Good      Very Good Ideal     Good      Very Good
## [52291] Very Good Very Good Very Good Very Good Very Good Very Good
## [52297] Very Good Very Good Very Good Very Good Ideal     Ideal    
## [52303] Ideal     Ideal     Ideal     Good      Good      Premium  
## [52309] Ideal     Ideal     Very Good Very Good Ideal     Ideal    
## [52315] Premium   Premium   Good      Very Good Premium   Very Good
## [52321] Very Good Very Good Ideal     Premium   Very Good Good     
## [52327] Ideal     Premium   Premium   Premium   Premium   Ideal    
## [52333] Premium   Premium   Good      Ideal     Good      Good     
## [52339] Very Good Very Good Premium   Premium   Ideal     Good     
## [52345] Fair      Premium   Very Good Ideal     Ideal     Premium  
## [52351] Very Good Fair      Very Good Very Good Very Good Ideal    
## [52357] Ideal     Ideal     Ideal     Premium   Very Good Very Good
## [52363] Ideal     Good      Very Good Ideal     Good      Very Good
## [52369] Premium   Ideal     Premium   Ideal     Very Good Very Good
## [52375] Premium   Very Good Ideal     Ideal     Very Good Very Good
## [52381] Very Good Very Good Ideal     Ideal     Ideal     Ideal    
## [52387] Premium   Premium   Ideal     Very Good Very Good Ideal    
## [52393] Ideal     Fair      Very Good Ideal     Ideal     Ideal    
## [52399] Ideal     Premium   Ideal     Premium   Very Good Ideal    
## [52405] Good      Good      Ideal     Ideal     Ideal     Ideal    
## [52411] Ideal     Very Good Premium   Ideal     Premium   Premium  
## [52417] Good      Premium   Premium   Good      Very Good Premium  
## [52423] Fair      Very Good Very Good Very Good Ideal     Very Good
## [52429] Very Good Premium   Very Good Very Good Ideal     Very Good
## [52435] Very Good Premium   Premium   Ideal     Ideal     Ideal    
## [52441] Ideal     Very Good Ideal     Ideal     Premium   Premium  
## [52447] Very Good Very Good Premium   Premium   Ideal     Ideal    
## [52453] Ideal     Ideal     Ideal     Very Good Premium   Premium  
## [52459] Ideal     Fair      Premium   Ideal     Good      Very Good
## [52465] Ideal     Very Good Very Good Very Good Ideal     Good     
## [52471] Fair      Ideal     Premium   Very Good Fair      Premium  
## [52477] Very Good Ideal     Premium   Ideal     Premium   Premium  
## [52483] Premium   Premium   Ideal     Ideal     Ideal     Ideal    
## [52489] Very Good Good      Ideal     Ideal     Good      Ideal    
## [52495] Ideal     Ideal     Premium   Very Good Premium   Ideal    
## [52501] Ideal     Ideal     Ideal     Good      Ideal     Ideal    
## [52507] Ideal     Very Good Premium   Ideal     Ideal     Premium  
## [52513] Premium   Ideal     Premium   Premium   Very Good Premium  
## [52519] Ideal     Ideal     Ideal     Ideal     Ideal     Premium  
## [52525] Ideal     Ideal     Ideal     Very Good Very Good Very Good
## [52531] Ideal     Ideal     Ideal     Premium   Premium   Ideal    
## [52537] Ideal     Ideal     Fair      Very Good Ideal     Premium  
## [52543] Very Good Very Good Premium   Very Good Ideal     Ideal    
## [52549] Premium   Ideal     Good      Premium   Premium   Ideal    
## [52555] Ideal     Very Good Premium   Premium   Ideal     Premium  
## [52561] Ideal     Ideal     Ideal     Premium   Premium   Fair     
## [52567] Premium   Premium   Fair      Ideal     Ideal     Premium  
## [52573] Premium   Fair      Premium   Very Good Very Good Ideal    
## [52579] Ideal     Ideal     Ideal     Very Good Premium   Fair     
## [52585] Very Good Good      Ideal     Ideal     Fair      Fair     
## [52591] Very Good Very Good Good      Fair      Ideal     Good     
## [52597] Very Good Very Good Good      Ideal     Very Good Very Good
## [52603] Premium   Ideal     Very Good Ideal     Premium   Premium  
## [52609] Very Good Very Good Very Good Ideal     Ideal     Ideal    
## [52615] Good      Premium   Good      Premium   Very Good Very Good
## [52621] Very Good Good      Ideal     Very Good Very Good Ideal    
## [52627] Ideal     Very Good Premium   Premium   Very Good Ideal    
## [52633] Very Good Very Good Very Good Very Good Very Good Very Good
## [52639] Very Good Very Good Very Good Very Good Very Good Very Good
## [52645] Very Good Very Good Very Good Very Good Very Good Very Good
## [52651] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [52657] Very Good Very Good Very Good Premium   Good      Good     
## [52663] Very Good Premium   Good      Very Good Very Good Very Good
## [52669] Premium   Ideal     Premium   Ideal     Ideal     Premium  
## [52675] Premium   Fair      Fair      Very Good Very Good Ideal    
## [52681] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [52687] Ideal     Very Good Very Good Premium   Premium   Very Good
## [52693] Very Good Very Good Very Good Ideal     Very Good Very Good
## [52699] Premium   Very Good Ideal     Ideal     Ideal     Fair     
## [52705] Premium   Premium   Fair      Premium   Ideal     Premium  
## [52711] Premium   Premium   Premium   Premium   Very Good Premium  
## [52717] Ideal     Premium   Ideal     Premium   Ideal     Ideal    
## [52723] Very Good Very Good Ideal     Ideal     Ideal     Ideal    
## [52729] Very Good Very Good Very Good Good      Ideal     Ideal    
## [52735] Very Good Very Good Very Good Ideal     Ideal     Good     
## [52741] Ideal     Premium   Ideal     Ideal     Ideal     Ideal    
## [52747] Ideal     Ideal     Good      Premium   Ideal     Fair     
## [52753] Premium   Ideal     Ideal     Very Good Very Good Very Good
## [52759] Very Good Very Good Ideal     Premium   Ideal     Very Good
## [52765] Premium   Very Good Ideal     Ideal     Good      Very Good
## [52771] Very Good Very Good Ideal     Ideal     Ideal     Premium  
## [52777] Very Good Very Good Very Good Very Good Ideal     Ideal    
## [52783] Very Good Very Good Ideal     Ideal     Premium   Very Good
## [52789] Very Good Ideal     Ideal     Very Good Very Good Good     
## [52795] Good      Very Good Very Good Good      Ideal     Very Good
## [52801] Ideal     Ideal     Good      Ideal     Premium   Fair     
## [52807] Premium   Very Good Premium   Good      Premium   Premium  
## [52813] Premium   Premium   Ideal     Premium   Very Good Very Good
## [52819] Very Good Very Good Very Good Very Good Ideal     Premium  
## [52825] Premium   Good      Very Good Ideal     Ideal     Premium  
## [52831] Good      Good      Ideal     Premium   Premium   Very Good
## [52837] Very Good Very Good Premium   Very Good Ideal     Ideal    
## [52843] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [52849] Fair      Premium   Ideal     Ideal     Good      Fair     
## [52855] Premium   Premium   Very Good Very Good Ideal     Ideal    
## [52861] Fair      Fair      Very Good Very Good Premium   Fair     
## [52867] Premium   Ideal     Ideal     Good      Ideal     Premium  
## [52873] Ideal     Ideal     Ideal     Good      Very Good Very Good
## [52879] Very Good Ideal     Ideal     Ideal     Ideal     Ideal    
## [52885] Ideal     Good      Premium   Very Good Very Good Premium  
## [52891] Very Good Very Good Very Good Very Good Ideal     Very Good
## [52897] Ideal     Premium   Very Good Very Good Ideal     Ideal    
## [52903] Ideal     Ideal     Ideal     Premium   Very Good Ideal    
## [52909] Ideal     Ideal     Ideal     Ideal     Very Good Ideal    
## [52915] Premium   Premium   Good      Very Good Very Good Very Good
## [52921] Premium   Very Good Ideal     Ideal     Ideal     Ideal    
## [52927] Ideal     Very Good Very Good Very Good Very Good Ideal    
## [52933] Good      Good      Good      Good      Premium   Very Good
## [52939] Very Good Premium   Premium   Very Good Very Good Very Good
## [52945] Very Good Very Good Very Good Very Good Ideal     Ideal    
## [52951] Very Good Very Good Very Good Very Good Very Good Very Good
## [52957] Very Good Very Good Very Good Very Good Very Good Very Good
## [52963] Very Good Very Good Very Good Ideal     Ideal     Ideal    
## [52969] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [52975] Ideal     Good      Very Good Very Good Premium   Ideal    
## [52981] Premium   Very Good Very Good Ideal     Ideal     Ideal    
## [52987] Ideal     Ideal     Premium   Ideal     Fair      Ideal    
## [52993] Fair      Fair      Premium   Ideal     Ideal     Ideal    
## [52999] Ideal     Fair      Ideal     Premium   Premium   Premium  
## [53005] Premium   Premium   Fair      Premium   Ideal     Premium  
## [53011] Ideal     Premium   Ideal     Very Good Ideal     Ideal    
## [53017] Good      Good      Very Good Very Good Good      Premium  
## [53023] Very Good Very Good Fair      Very Good Ideal     Very Good
## [53029] Very Good Ideal     Ideal     Premium   Premium   Very Good
## [53035] Very Good Very Good Very Good Very Good Ideal     Ideal    
## [53041] Ideal     Good      Ideal     Good      Very Good Very Good
## [53047] Ideal     Ideal     Ideal     Good      Premium   Ideal    
## [53053] Ideal     Premium   Ideal     Ideal     Ideal     Premium  
## [53059] Premium   Premium   Very Good Good      Ideal     Ideal    
## [53065] Fair      Very Good Good      Good      Ideal     Ideal    
## [53071] Ideal     Very Good Ideal     Ideal     Ideal     Premium  
## [53077] Very Good Ideal     Ideal     Ideal     Good      Ideal    
## [53083] Very Good Very Good Ideal     Ideal     Ideal     Ideal    
## [53089] Ideal     Ideal     Premium   Ideal     Very Good Premium  
## [53095] Very Good Ideal     Premium   Ideal     Premium   Ideal    
## [53101] Ideal     Ideal     Ideal     Good      Ideal     Ideal    
## [53107] Ideal     Very Good Premium   Ideal     Premium   Very Good
## [53113] Ideal     Ideal     Ideal     Very Good Very Good Very Good
## [53119] Good      Ideal     Ideal     Premium   Fair      Very Good
## [53125] Ideal     Ideal     Very Good Very Good Very Good Premium  
## [53131] Ideal     Ideal     Ideal     Ideal     Very Good Ideal    
## [53137] Ideal     Good      Premium   Ideal     Ideal     Ideal    
## [53143] Ideal     Ideal     Premium   Premium   Fair      Very Good
## [53149] Very Good Ideal     Ideal     Ideal     Good      Premium  
## [53155] Fair      Very Good Premium   Ideal     Good      Ideal    
## [53161] Premium   Ideal     Ideal     Premium   Ideal     Good     
## [53167] Very Good Ideal     Premium   Premium   Premium   Premium  
## [53173] Premium   Very Good Very Good Very Good Very Good Premium  
## [53179] Ideal     Ideal     Very Good Ideal     Good      Premium  
## [53185] Premium   Premium   Very Good Ideal     Ideal     Ideal    
## [53191] Premium   Very Good Good      Very Good Ideal     Ideal    
## [53197] Very Good Ideal     Premium   Very Good Ideal     Ideal    
## [53203] Ideal     Ideal     Premium   Premium   Very Good Ideal    
## [53209] Ideal     Ideal     Very Good Ideal     Ideal     Ideal    
## [53215] Ideal     Very Good Premium   Good      Ideal     Ideal    
## [53221] Ideal     Very Good Very Good Ideal     Ideal     Fair     
## [53227] Good      Very Good Good      Good      Ideal     Very Good
## [53233] Good      Good      Ideal     Ideal     Ideal     Ideal    
## [53239] Ideal     Very Good Very Good Ideal     Very Good Ideal    
## [53245] Ideal     Premium   Ideal     Premium   Ideal     Premium  
## [53251] Ideal     Very Good Premium   Premium   Premium   Very Good
## [53257] Good      Good      Ideal     Good      Premium   Premium  
## [53263] Premium   Premium   Premium   Premium   Premium   Premium  
## [53269] Premium   Premium   Ideal     Ideal     Fair      Very Good
## [53275] Very Good Very Good Ideal     Good      Good      Ideal    
## [53281] Good      Ideal     Very Good Premium   Premium   Premium  
## [53287] Very Good Ideal     Very Good Good      Premium   Good     
## [53293] Ideal     Good      Very Good Ideal     Good      Ideal    
## [53299] Very Good Ideal     Very Good Ideal     Ideal     Premium  
## [53305] Premium   Ideal     Good      Very Good Premium   Very Good
## [53311] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [53317] Very Good Ideal     Good      Very Good Very Good Very Good
## [53323] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [53329] Ideal     Ideal     Ideal     Very Good Very Good Very Good
## [53335] Ideal     Ideal     Ideal     Premium   Fair      Very Good
## [53341] Very Good Very Good Ideal     Ideal     Ideal     Ideal    
## [53347] Ideal     Premium   Fair      Premium   Good      Premium  
## [53353] Good      Ideal     Ideal     Ideal     Good      Premium  
## [53359] Very Good Premium   Premium   Ideal     Premium   Premium  
## [53365] Premium   Very Good Premium   Premium   Ideal     Ideal    
## [53371] Premium   Ideal     Premium   Ideal     Ideal     Ideal    
## [53377] Ideal     Ideal     Ideal     Ideal     Premium   Very Good
## [53383] Very Good Ideal     Ideal     Ideal     Good      Very Good
## [53389] Very Good Ideal     Ideal     Ideal     Good      Ideal    
## [53395] Very Good Fair      Premium   Ideal     Premium   Ideal    
## [53401] Very Good Very Good Ideal     Premium   Very Good Ideal    
## [53407] Ideal     Ideal     Premium   Ideal     Premium   Premium  
## [53413] Very Good Very Good Ideal     Very Good Ideal     Premium  
## [53419] Premium   Very Good Very Good Very Good Premium   Very Good
## [53425] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [53431] Ideal     Good      Premium   Very Good Fair      Very Good
## [53437] Very Good Premium   Premium   Good      Ideal     Very Good
## [53443] Very Good Good      Premium   Ideal     Ideal     Ideal    
## [53449] Very Good Ideal     Very Good Very Good Good      Ideal    
## [53455] Ideal     Very Good Very Good Ideal     Ideal     Ideal    
## [53461] Good      Ideal     Very Good Premium   Premium   Ideal    
## [53467] Ideal     Ideal     Good      Ideal     Ideal     Ideal    
## [53473] Ideal     Ideal     Good      Very Good Very Good Very Good
## [53479] Good      Ideal     Ideal     Premium   Very Good Very Good
## [53485] Ideal     Ideal     Ideal     Ideal     Ideal     Very Good
## [53491] Premium   Very Good Premium   Ideal     Premium   Fair     
## [53497] Premium   Premium   Premium   Very Good Very Good Premium  
## [53503] Ideal     Good      Fair      Very Good Very Good Ideal    
## [53509] Premium   Premium   Premium   Premium   Ideal     Ideal    
## [53515] Ideal     Fair      Very Good Premium   Very Good Very Good
## [53521] Ideal     Premium   Premium   Ideal     Very Good Very Good
## [53527] Very Good Ideal     Ideal     Ideal     Ideal     Ideal    
## [53533] Ideal     Ideal     Fair      Good      Ideal     Ideal    
## [53539] Ideal     Fair      Fair      Premium   Fair      Ideal    
## [53545] Very Good Ideal     Premium   Very Good Very Good Very Good
## [53551] Good      Ideal     Ideal     Very Good Ideal     Ideal    
## [53557] Ideal     Premium   Ideal     Premium   Premium   Premium  
## [53563] Premium   Very Good Premium   Very Good Premium   Ideal    
## [53569] Good      Premium   Premium   Very Good Very Good Ideal    
## [53575] Ideal     Ideal     Good      Fair      Fair      Ideal    
## [53581] Very Good Premium   Ideal     Very Good Premium   Very Good
## [53587] Ideal     Premium   Ideal     Ideal     Ideal     Ideal    
## [53593] Good      Good      Fair      Premium   Good      Ideal    
## [53599] Ideal     Good      Premium   Fair      Ideal     Ideal    
## [53605] Very Good Ideal     Ideal     Ideal     Good      Premium  
## [53611] Ideal     Very Good Very Good Very Good Premium   Ideal    
## [53617] Very Good Very Good Ideal     Ideal     Good      Premium  
## [53623] Good      Premium   Ideal     Good      Very Good Very Good
## [53629] Premium   Premium   Very Good Ideal     Premium   Ideal    
## [53635] Ideal     Ideal     Ideal     Very Good Ideal     Ideal    
## [53641] Very Good Very Good Ideal     Premium   Premium   Very Good
## [53647] Ideal     Ideal     Ideal     Very Good Good      Premium  
## [53653] Ideal     Ideal     Ideal     Good      Premium   Ideal    
## [53659] Premium   Ideal     Fair      Ideal     Very Good Very Good
## [53665] Good      Premium   Ideal     Very Good Very Good Very Good
## [53671] Ideal     Ideal     Ideal     Fair      Very Good Premium  
## [53677] Premium   Premium   Ideal     Ideal     Ideal     Very Good
## [53683] Very Good Very Good Very Good Very Good Very Good Ideal    
## [53689] Very Good Ideal     Premium   Premium   Good      Ideal    
## [53695] Ideal     Good      Premium   Fair      Fair      Premium  
## [53701] Ideal     Premium   Good      Ideal     Ideal     Ideal    
## [53707] Good      Premium   Ideal     Very Good Good      Ideal    
## [53713] Ideal     Ideal     Ideal     Ideal     Ideal     Ideal    
## [53719] Premium   Ideal     Ideal     Very Good Very Good Good     
## [53725] Ideal     Premium   Premium   Fair      Premium   Ideal    
## [53731] Ideal     Very Good Very Good Premium   Premium   Ideal    
## [53737] Ideal     Ideal     Ideal     Good      Premium   Good     
## [53743] Ideal     Premium   Ideal     Fair      Very Good Very Good
## [53749] Very Good Ideal     Ideal     Ideal     Premium   Premium  
## [53755] Ideal     Good      Fair      Fair      Very Good Premium  
## [53761] Very Good Ideal     Ideal     Very Good Very Good Very Good
## [53767] Ideal     Ideal     Very Good Ideal     Ideal     Ideal    
## [53773] Ideal     Ideal     Good      Ideal     Premium   Premium  
## [53779] Very Good Very Good Ideal     Ideal     Ideal     Ideal    
## [53785] Ideal     Good      Premium   Good      Ideal     Ideal    
## [53791] Ideal     Very Good Ideal     Very Good Premium   Ideal    
## [53797] Ideal     Ideal     Ideal     Ideal     Fair      Ideal    
## [53803] Premium   Good      Premium   Ideal     Premium   Very Good
## [53809] Very Good Ideal     Ideal     Ideal     Very Good Very Good
## [53815] Good      Premium   Premium   Very Good Very Good Premium  
## [53821] Premium   Premium   Premium   Ideal     Very Good Very Good
## [53827] Premium   Premium   Very Good Ideal     Ideal     Ideal    
## [53833] Ideal     Ideal     Premium   Premium   Premium   Ideal    
## [53839] Good      Ideal     Good      Very Good Ideal     Ideal    
## [53845] Very Good Very Good Very Good Ideal     Ideal     Ideal    
## [53851] Very Good Ideal     Ideal     Premium   Ideal     Ideal    
## [53857] Premium   Premium   Very Good Premium   Premium   Premium  
## [53863] Premium   Fair      Very Good Very Good Very Good Premium  
## [53869] Premium   Ideal     Ideal     Premium   Premium   Very Good
## [53875] Very Good Very Good Ideal     Premium   Ideal     Fair     
## [53881] Premium   Ideal     Fair      Very Good Ideal     Very Good
## [53887] Good      Ideal     Premium   Ideal     Good      Ideal    
## [53893] Ideal     Ideal     Ideal     Good      Ideal     Good     
## [53899] Very Good Ideal     Ideal     Premium   Very Good Very Good
## [53905] Premium   Premium   Very Good Very Good Very Good Very Good
## [53911] Premium   Premium   Premium   Good      Good      Ideal    
## [53917] Good      Very Good Premium   Ideal     Very Good Very Good
## [53923] Very Good Ideal     Ideal     Ideal     Ideal     Good     
## [53929] Premium   Ideal     Premium   Premium   Very Good Very Good
## [53935] Premium   Ideal     Good      Very Good Premium   Ideal    
## Levels: Fair < Good < Very Good < Premium < Ideal
```

This is very interesting when looked at side-by-side. Subsetting the tibble "diamonds" with select() gave me the output nicely organized in a column, whereas the subsetting the data frame "diamonds" gave a mass of text (factors).  

4. Practive referring to non-syntactic names in the following data frame by:    
a. Extracting the variable called 1;  
b. Plotting a scatterplot of 1 vs 2;  
c. Creating a new column called 3 which is 2 divided by 1;  
d. Renaming the columns to one, two and three.  

```r
#annoying = tibble(
#  '1' = 1:10,
#  '2' = '1' * 2 + rnorm(length('1'))
#)
#OK the code wouldn't even work. The error message reads: "Error in "1" * 2 : non-numeric argument to binary operator". I think R did not get the '1' part... I don't know what to do. This is truly annoying. 
```

5. What does tibble::enframe() do? When might you use it?   

```r
tibble::enframe(c(1:3))
```

```
## # A tibble: 3 x 2
##    name value
##   <int> <int>
## 1     1     1
## 2     2     2
## 3     3     3
```

I guess it makes a vector into dataframe? I could figure out how the name and value arguments work.  

6. What option controls how many additional column names are printed at the footer of a tibble?  

*I think you use the options() function? But I'm not sure what the question is asking for or how to do it =P. 

