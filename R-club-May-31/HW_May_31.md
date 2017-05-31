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

