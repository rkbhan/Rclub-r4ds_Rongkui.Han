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
parse_integer(c("1","231",".","abc","123.45"), na = ".")
```

```
## Warning in rbind(names(probs), probs_f): number of columns of result is not
## a multiple of vector length (arg 1)
```

```
## Warning: 2 parsing failures.
## row # A tibble: 2 x 4 col     row   col               expected actual expected   <int> <int>                  <chr>  <chr> actual 1     4    NA             an integer    abc row 2     5    NA no trailing characters    .45
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


