# Rmarkdown_HW_May3
Rongkui Han  
May 3, 2017  
####3.6 Geometric Objects

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
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy))
```

![](May_3_HW_files/figure-html/unnamed-chunk-1-1.png)<!-- -->

```r
ggplot(data = mpg) + 
  geom_smooth(mapping = aes(x = displ, y = hwy))
```

```
## `geom_smooth()` using method = 'loess'
```

![](May_3_HW_files/figure-html/unnamed-chunk-1-2.png)<!-- -->

```r
ggplot(data = mpg) + 
  geom_smooth(mapping = aes(x = displ, y = hwy, linetype = drv)) #this is really a funny looking plot. 
```

```
## `geom_smooth()` using method = 'loess'
```

![](May_3_HW_files/figure-html/unnamed-chunk-1-3.png)<!-- -->

