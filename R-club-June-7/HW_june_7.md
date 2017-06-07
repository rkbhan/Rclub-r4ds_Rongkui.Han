# HW_June_7
Rongkui Han  
June 7, 2017  
##11 Data Import
####11.1.1 Prerequisites

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

###11.2 Getting started

```r
read_csv("a,b,c
         1,2,3
         4,5,6")
```

```
## # A tibble: 2 x 3
##       a     b     c
##   <int> <int> <int>
## 1     1     2     3
## 2     4     5     6
```

This is a rather interesting (not sure how useful) function. Notice that it is an "_" between "read" and "csv", not the usual ".". Use *enter* to denote change of row (like you would intuitively).   


```r
read_csv("The first line of metadata
         The second line of metadata
         x,y,z
         1,2,3", skip = 2)
```

```
## # A tibble: 1 x 3
##       x     y     z
##   <int> <int> <int>
## 1     1     2     3
```

```r
read_csv("# the line I want to skip
         x,y,z
         1,2,3", comment= "#")
```

```
## # A tibble: 1 x 3
##       x     y     z
##   <int> <int> <int>
## 1     1     2     3
```

Magic!  

The data don't necessarily have colume names. Or the colume names don't have to be specified in the parentheses.   

```r
read_csv("1,2,3\n4,5,6", col_names = FALSE)
```

```
## # A tibble: 2 x 3
##      X1    X2    X3
##   <int> <int> <int>
## 1     1     2     3
## 2     4     5     6
```

```r
read_csv("1,2,3\n4,5,6", col_names = c("x","y","z"))
```

```
## # A tibble: 2 x 3
##       x     y     z
##   <int> <int> <int>
## 1     1     2     3
## 2     4     5     6
```

You can use *backslash n* in place of *enter*.  

You can even create a table with na in it, using the "na =" option.   

```r
read_csv("a,b,c\n1,2,blahblah", na = "blahblah")
```

```
## # A tibble: 1 x 3
##       a     b     c
##   <int> <int> <chr>
## 1     1     2  <NA>
```

I am more ammused by "na = "blahblah"" than I am supposed to.   

####11.2.1 Compared to base R  

+ read_csv() can be 10x faster than read.csv() *surprise!*  
+ They produce tibbles  
+ They are more reproducible. Base R functions inherit some behaviors from your operation system and environment variables, so importing codes that works on your computer might not work on someone else's.  

This is actually a really convincing list of reasons.  

####11.2.2 Exercises  
1. Water function would you use to read a file where fields where separated with "|"? *(psv? -- nope)*  


```r
read_delim("1|2|3\n4|5|6", col_names = c("x","y","z"), delim = "|")
```

```
## # A tibble: 2 x 3
##       x     y     z
##   <int> <int> <int>
## 1     1     2     3
## 2     4     5     6
```

2. Apart from file, skip, and comment, what other arguments do read_csv() and read_tsv() have in common?  

*Actually all of their options are the same.*  
read_tsv(file, col_names = TRUE, col_types = NULL,  
  locale = default_locale(), na = c("", "NA"), quoted_na = TRUE,  
  quote = "\"", comment = "", trim_ws = TRUE, skip = 0, n_max = Inf,  
  guess_max = min(1000, n_max), progress = show_progress())  
  
3. What are the most important arguments ro read_fwf()?  

*I think the most important arguments are the ones that don't have decult values, like "file =" and "col_positions".   

4. Sometimes strings in a CSV file contain commas. To prevent them from causing problems they need to be surrounded by a quoting character, like " or '. By convention, read_csv() assumes that the quoting character will be ", and if you want to change it you’ll need to use read_delim() instead. What arguments do you need to specify to read the following text into a data frame?   

*You will need a "quote =" argument.*   

```r
read_delim("x,y\n1,'a,b'", delim = ",", quote = "''" )
```

```
## # A tibble: 1 x 2
##       x     y
##   <int> <chr>
## 1     1   a,b
```

5. Identify what is wrong with each of the following inline CSV files. What happens when you run the code?  


```r
read_csv("a,b\n1,2,3\n4,5,6") 
```

```
## Warning: 2 parsing failures.
## row # A tibble: 2 x 5 col     row   col  expected    actual         file expected   <int> <chr>     <chr>     <chr>        <chr> actual 1     1  <NA> 2 columns 3 columns literal data file 2     2  <NA> 2 columns 3 columns literal data
```

```
## # A tibble: 2 x 2
##       a     b
##   <int> <int>
## 1     1     2
## 2     4     5
```

```r
#the third column gets dropped because the first row has only two items.
read_csv("a,b,c\n1,2\n1,2,3,4") 
```

```
## Warning: 2 parsing failures.
## row # A tibble: 2 x 5 col     row   col  expected    actual         file expected   <int> <chr>     <chr>     <chr>        <chr> actual 1     1  <NA> 3 columns 2 columns literal data file 2     2  <NA> 3 columns 4 columns literal data
```

```
## # A tibble: 2 x 3
##       a     b     c
##   <int> <int> <int>
## 1     1     2    NA
## 2     1     2     3
```

```r
#The first row has an NA and the fourth item on the second row gets dropped. The problem is that the rows are not of the same length.  
read_csv("a,b\n\"1")
```

```
## Warning: 2 parsing failures.
## row # A tibble: 2 x 5 col     row   col                     expected    actual         file expected   <int> <chr>                        <chr>     <chr>        <chr> actual 1     1     a closing quote at end of file           literal data file 2     1  <NA>                    2 columns 1 columns literal data
```

```
## # A tibble: 1 x 2
##       a     b
##   <int> <chr>
## 1     1  <NA>
```

```r
#second item (column b) missing (NA) in the fist row.  
read_csv("a,b\n1,2\na,b")
```

```
## # A tibble: 2 x 2
##       a     b
##   <chr> <chr>
## 1     1     2
## 2     a     b
```

```r
#idk... second row has the same content at the column names?
read_csv("a;b\n1;3")
```

```
## # A tibble: 1 x 1
##   `a;b`
##   <chr>
## 1   1;3
```

```r
#I think this code is trying the use ";" as a delimiter. The correct code for taht purpose would be:
read_delim("a;b\n1;3", delim = ";")
```

```
## # A tibble: 1 x 2
##       a     b
##   <int> <int>
## 1     1     3
```

###11.3 Parsing a vector  

```r
str(parse_logical(c("TRUE","FALSE","NA")))
```

```
##  logi [1:3] TRUE FALSE NA
```

```r
str(parse_integer(c("1","2","3")))
```

```
##  int [1:3] 1 2 3
```

```r
str(parse_date(c("2010-01-01","1979-10-14")))
```

```
##  Date[1:2], format: "2010-01-01" "1979-10-14"
```

```r
parse_integer(c("1","231",".","456"), na = ".")
```

```
## [1]   1 231  NA 456
```

```r
#a parsing failure example:
x = parse_integer(c("1","231",".","abc","123.45"), na = ".")
```

```
## Warning in rbind(names(probs), probs_f): number of columns of result is not
## a multiple of vector length (arg 1)
```

```
## Warning: 2 parsing failures.
## row # A tibble: 2 x 4 col     row   col               expected actual expected   <int> <int>                  <chr>  <chr> actual 1     4    NA             an integer    abc row 2     5    NA no trailing characters    .45
```

```r
x
```

```
## [1]   1 231  NA  NA  NA
## attr(,"problems")
## # A tibble: 2 x 4
##     row   col               expected actual
##   <int> <int>                  <chr>  <chr>
## 1     4    NA             an integer    abc
## 2     5    NA no trailing characters    .45
```

```r
problems(x) #returns a tibble
```

```
## # A tibble: 2 x 4
##     row   col               expected actual
##   <int> <int>                  <chr>  <chr>
## 1     4    NA             an integer    abc
## 2     5    NA no trailing characters    .45
```

####11.3.1 Numbers  

```r
#you can specify decimal marks via parse_double() 
parse_double("1.23")
```

```
## [1] 1.23
```

```r
parse_double("1,23", locale = locale(decimal_mark = ","))
```

```
## [1] 1.23
```

```r
#parse_number() ignores all non-numeric symbols.
parse_number("$100")
```

```
## [1] 100
```

```r
parse_number("20%")
```

```
## [1] 20
```

```r
#the combination of parse_number() and parse_double deals with grouping mark.
parse_number("$123,456,789")
```

```
## [1] 123456789
```

```r
parse_number("123.456.789", locale = locale(grouping_mark = "."))
```

```
## [1] 123456789
```

```r
parse_number("123'456'789", locale = locale(grouping_mark = "'"))
```

```
## [1] 123456789
```

####11.3.2 Strings  

```r
charToRaw("Hadley")
```

```
## [1] 48 61 64 6c 65 79
```

```r
x1 <- "El Ni\xf1o was particularly bad this year"
x2 <- "\x82\xb1\x82\xf1\x82\xc9\x82\xbf\x82\xcd"
parse_character(x1, locale = locale(encoding = "Latin1"))
```

```
## [1] "El Niño was particularly bad this year"
```

```r
parse_character(x2, locale = locale(encoding = "Shift-JIS"))
```

```
## [1] "こんにちは"
```

```r
guess_encoding(charToRaw(x1))
```

```
## # A tibble: 2 x 2
##     encoding confidence
##        <chr>      <dbl>
## 1 ISO-8859-1       0.46
## 2 ISO-8859-9       0.23
```

```r
guess_encoding(charToRaw(x2))
```

```
## # A tibble: 1 x 2
##   encoding confidence
##      <chr>      <dbl>
## 1   KOI8-R       0.42
```

####11.3.3 Factors  

```r
fruit = c("apple","banana")
parse_factor(c("apple","banana","bananana"), levels = fruit)
```

```
## Warning: 1 parsing failure.
## row # A tibble: 1 x 4 col     row   col           expected   actual expected   <int> <int>              <chr>    <chr> actual 1     3    NA value in level set bananana
```

```
## [1] apple  banana <NA>  
## attr(,"problems")
## # A tibble: 1 x 4
##     row   col           expected   actual
##   <int> <int>              <chr>    <chr>
## 1     3    NA value in level set bananana
## Levels: apple banana
```

####11.3.4 Dates, date-times, and times  

```r
parse_datetime("2010-10-01T2010")
```

```
## [1] "2010-10-01 20:10:00 UTC"
```

```r
parse_datetime("20101010")
```

```
## [1] "2010-10-10 UTC"
```

```r
parse_date("2010-10-01")
```

```
## [1] "2010-10-01"
```

```r
parse_time("01:10 am")
```

```
## 01:10:00
```

```r
parse_time("20:10:01")
```

```
## 20:10:01
```

```r
parse_date("01/02/15","%m/%d/%y")
```

```
## [1] "2015-01-02"
```

```r
parse_date("01/02/15","%d/%m/%y")
```

```
## [1] "2015-02-01"
```

```r
parse_date("01/02/15","%y/%m/%d")
```

```
## [1] "2001-02-15"
```

```r
parse_date("1 janvier 2015", "%d %B %Y", locale = locale("fr"))
```

```
## [1] "2015-01-01"
```

####11.3.5 Exercises
1. What are the most important arguments to locale()?  

```r
locale(date_names = "en", date_format = "%AD", time_format = "%AT",
  decimal_mark = ".", grouping_mark = ",", tz = "UTC",
  encoding = "UTF-8", asciify = FALSE)
```

All of them look pretty important... it's hard for me to decide which ones are the most important...   

2.What happens if you try and set decimal_mark and grouping_mark to the same character? What happens to the default value of grouping_mark when you set decimal_mark to “,”? What happens to the default value of decimal_mark when you set the grouping_mark to “.”?   

```r
parse_double("123,456,789", locale = locale(decimal_mark = ",", grouping_mark = ","))
```

There was an error message telling me that decimal_mark and grouping_mark must be different.   

```r
parse_double("123456,789", locale = locale(decimal_mark = ","))
```

```
## [1] 123456.8
```

I guess the default changes?  


```r
parse_double("123.456.789", locale = locale(grouping_mark = "."))
```

```
## Warning: 1 parsing failure.
## row # A tibble: 1 x 4 col     row   col               expected   actual expected   <int> <int>                  <chr>    <chr> actual 1     1    NA no trailing characters .456.789
```

```
## [1] NA
## attr(,"problems")
## # A tibble: 1 x 4
##     row   col               expected   actual
##   <int> <int>                  <chr>    <chr>
## 1     1    NA no trailing characters .456.789
```

Changing grouping_mark to "."" did not work for me.   

3. What do date_format and time_formate options to locale do?  
 *I couldn't figure this out.*  
 
 4. If you live outside the US, create a new locale object tjat encapsulates the settings for te types of file you read most commonly. 
 
 *Couldn't figure out how to do this either = =*  
 
 5. What's the difference between read_csv() and read_csv2()?
 *read_csv2() uses ";" instead of comma to delimit.*  
 
 6. What are the most common encodings used in Europe? What are teh most common encodings used in Asia? Do some googling to find out.  
 *IEC 8859 is used for most European languages, and GB 18030 is used in China. EUC is used in Japan.*  
 
7. Generate the correct formate strong to parse peach of the following dates and times:  

```r
d1 <- "January 1, 2010"
parse_date(d1, "%B %d, %Y")
```

```
## [1] "2010-01-01"
```

```r
d2 <- "2015-Mar-07"
parse_date(d2, "%Y-%b-%d")
```

```
## [1] "2015-03-07"
```

```r
d3 <- "06-Jun-2017"
parse_date(d3, "%d-%b-%Y")
```

```
## [1] "2017-06-06"
```

```r
d4 <- c("August 19 (2015)", "July 1 (2015)")
parse_date(d4, "%B %d (%Y)")
```

```
## [1] "2015-08-19" "2015-07-01"
```

```r
d5 <- "12/30/14" # Dec 30, 2014
parse_date(d5, "%m/%d/%y")
```

```
## [1] "2014-12-30"
```

```r
t1 <- "1705"
parse_time(t1, "%H%M")
```

```
## 17:05:00
```

```r
t2 <- "11:15:10.12 PM"
parse_time(t2, "%I:%M:%OS %p")
```

```
## 23:15:10.12
```

###Parsing a file  
####11.4.1 Strategy  

```r
guess_parser("2010-10-01")
```

```
## [1] "date"
```

```r
guess_parser("15:01")
```

```
## [1] "time"
```

```r
guess_parser(c("TRUE","FALSE"))
```

```
## [1] "logical"
```

```r
guess_parser(c("1","5","9"))
```

```
## [1] "integer"
```

```r
guess_parser(c("12","352","561"))
```

```
## [1] "integer"
```

```r
str(guess_parser ("2010-10-10"))
```

```
##  chr "date"
```

####11.4.2 Problems  

```r
challenge = read_csv(readr_example("challenge.csv"))
```

```
## Parsed with column specification:
## cols(
##   x = col_integer(),
##   y = col_character()
## )
```

```
## Warning in rbind(names(probs), probs_f): number of columns of result is not
## a multiple of vector length (arg 1)
```

```
## Warning: 1000 parsing failures.
## row # A tibble: 5 x 5 col     row   col               expected             actual expected   <int> <chr>                  <chr>              <chr> actual 1  1001     x no trailing characters .23837975086644292 file 2  1002     x no trailing characters .41167997173033655 row 3  1003     x no trailing characters  .7460716762579978 col 4  1004     x no trailing characters   .723450553836301 expected 5  1005     x no trailing characters   .614524137461558 actual # ... with 1 more variables: file <chr>
## ... ................. ... ....................................................... ........ ....................................................... ...... ....................................................... .... ....................................................... ... ....................................................... ... ....................................................... ........ ....................................................... ...... .......................................
## See problems(...) for more details.
```

```r
problems(challenge)
```

```
## # A tibble: 1,000 x 5
##      row   col               expected             actual
##    <int> <chr>                  <chr>              <chr>
##  1  1001     x no trailing characters .23837975086644292
##  2  1002     x no trailing characters .41167997173033655
##  3  1003     x no trailing characters  .7460716762579978
##  4  1004     x no trailing characters   .723450553836301
##  5  1005     x no trailing characters   .614524137461558
##  6  1006     x no trailing characters   .473980569280684
##  7  1007     x no trailing characters  .5784610391128808
##  8  1008     x no trailing characters  .2415937229525298
##  9  1009     x no trailing characters .11437866208143532
## 10  1010     x no trailing characters  .2983446326106787
## # ... with 990 more rows, and 1 more variables: file <chr>
```

```r
challenge <- read_csv(
  readr_example("challenge.csv"), 
  col_types = cols(
    x = col_integer(),
    y = col_character()
  )
)
```

```
## Warning in rbind(names(probs), probs_f): number of columns of result is not a multiple of vector length (arg 1)

## Warning in rbind(names(probs), probs_f): 1000 parsing failures.
## row # A tibble: 5 x 5 col     row   col               expected             actual expected   <int> <chr>                  <chr>              <chr> actual 1  1001     x no trailing characters .23837975086644292 file 2  1002     x no trailing characters .41167997173033655 row 3  1003     x no trailing characters  .7460716762579978 col 4  1004     x no trailing characters   .723450553836301 expected 5  1005     x no trailing characters   .614524137461558 actual # ... with 1 more variables: file <chr>
## ... ................. ... ....................................................... ........ ....................................................... ...... ....................................................... .... ....................................................... ... ....................................................... ... ....................................................... ........ ....................................................... ...... .......................................
## See problems(...) for more details.
```

```r
challenge <- read_csv(
  readr_example("challenge.csv"), 
  col_types = cols(
    x = col_double(),
    y = col_character()
  )
)
tail(challenge)
```

```
## # A tibble: 6 x 2
##           x          y
##       <dbl>      <chr>
## 1 0.8052743 2019-11-21
## 2 0.1635163 2018-03-29
## 3 0.4719390 2014-08-04
## 4 0.7183186 2015-08-16
## 5 0.2698786 2020-02-04
## 6 0.6082372 2019-01-06
```

```r
challenge <- read_csv(
  readr_example("challenge.csv"), 
  col_types = cols(
    x = col_double(),
    y = col_date()
  )
)
tail(challenge)
```

```
## # A tibble: 6 x 2
##           x          y
##       <dbl>     <date>
## 1 0.8052743 2019-11-21
## 2 0.1635163 2018-03-29
## 3 0.4719390 2014-08-04
## 4 0.7183186 2015-08-16
## 5 0.2698786 2020-02-04
## 6 0.6082372 2019-01-06
```

####11.4.3 Other strategies  

```r
challenge2 = read_csv(readr_example("challenge.csv"), guess_max = 1001)
```

```
## Parsed with column specification:
## cols(
##   x = col_double(),
##   y = col_date(format = "")
## )
```

```r
challenge2
```

```
## # A tibble: 2,000 x 2
##        x      y
##    <dbl> <date>
##  1   404     NA
##  2  4172     NA
##  3  3004     NA
##  4   787     NA
##  5    37     NA
##  6  2332     NA
##  7  2489     NA
##  8  1449     NA
##  9  3665     NA
## 10  3863     NA
## # ... with 1,990 more rows
```

```r
challenge2 <- read_csv(readr_example("challenge.csv"), 
  col_types = cols(.default = col_character())
)
df <- tribble(
  ~x,  ~y,
  "1", "1.21",
  "2", "2.32",
  "3", "4.56"
)
df
```

```
## # A tibble: 3 x 2
##       x     y
##   <chr> <chr>
## 1     1  1.21
## 2     2  2.32
## 3     3  4.56
```

```r
type_convert(df)
```

```
## Parsed with column specification:
## cols(
##   x = col_integer(),
##   y = col_double()
## )
```

```
## # A tibble: 3 x 2
##       x     y
##   <int> <dbl>
## 1     1  1.21
## 2     2  2.32
## 3     3  4.56
```

###11.5 Writing to a file  

```r
write_csv(challenge, "challenge.csv")
challenge
```

```
## # A tibble: 2,000 x 2
##        x      y
##    <dbl> <date>
##  1   404     NA
##  2  4172     NA
##  3  3004     NA
##  4   787     NA
##  5    37     NA
##  6  2332     NA
##  7  2489     NA
##  8  1449     NA
##  9  3665     NA
## 10  3863     NA
## # ... with 1,990 more rows
```

```r
read_csv("challenge.csv")
```

```
## Parsed with column specification:
## cols(
##   x = col_integer(),
##   y = col_character()
## )
```

```
## Warning in rbind(names(probs), probs_f): number of columns of result is not
## a multiple of vector length (arg 1)
```

```
## Warning: 1000 parsing failures.
## row # A tibble: 5 x 5 col     row   col               expected             actual            file expected   <int> <chr>                  <chr>              <chr>           <chr> actual 1  1001     x no trailing characters .23837975086644292 'challenge.csv' file 2  1002     x no trailing characters .41167997173033655 'challenge.csv' row 3  1003     x no trailing characters  .7460716762579978 'challenge.csv' col 4  1004     x no trailing characters   .723450553836301 'challenge.csv' expected 5  1005     x no trailing characters   .614524137461558 'challenge.csv'
## ... ................. ... ....................................................................... ........ ....................................................................... ...... ....................................................................... .... ....................................................................... ... ....................................................................... ... ....................................................................... ........ .......................................................................
## See problems(...) for more details.
```

```
## # A tibble: 2,000 x 2
##        x     y
##    <int> <chr>
##  1   404  <NA>
##  2  4172  <NA>
##  3  3004  <NA>
##  4   787  <NA>
##  5    37  <NA>
##  6  2332  <NA>
##  7  2489  <NA>
##  8  1449  <NA>
##  9  3665  <NA>
## 10  3863  <NA>
## # ... with 1,990 more rows
```

```r
write_rds(challenge, "challenge.rds")
read_rds("challenge.rds")
```

```
## # A tibble: 2,000 x 2
##        x      y
##    <dbl> <date>
##  1   404     NA
##  2  4172     NA
##  3  3004     NA
##  4   787     NA
##  5    37     NA
##  6  2332     NA
##  7  2489     NA
##  8  1449     NA
##  9  3665     NA
## 10  3863     NA
## # ... with 1,990 more rows
```


