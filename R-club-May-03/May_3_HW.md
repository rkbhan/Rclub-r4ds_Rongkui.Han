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
  geom_smooth(mapping = aes(x = displ, y = hwy, linetype = drv, color = drv)) #this is really a funny looking plot. 
```

```
## `geom_smooth()` using method = 'loess'
```

![](May_3_HW_files/figure-html/unnamed-chunk-1-3.png)<!-- -->

```r
ggplot(data = mpg) +
  geom_smooth(mapping = aes(x = displ, y = hwy, group = drv)) #"group" aesthetics just group the data according to a category and overlay each group on top of each other. it does not add a separate visual feature (e.g. colors/shape/legend etc.)
```

```
## `geom_smooth()` using method = 'loess'
```

![](May_3_HW_files/figure-html/unnamed-chunk-1-4.png)<!-- -->

To display multiple geoms in the same plot:

```r
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy)) +
  geom_smooth(mapping = aes(x = displ, y = hwy)) #wow
```

```
## `geom_smooth()` using method = 'loess'
```

![](May_3_HW_files/figure-html/unnamed-chunk-2-1.png)<!-- -->

```r
#Or do this to eliminate duplication in code:
ggplot(data = mpg, mapping = aes(x = displ, y = hwy)) + #first line (ggplot()) for shared aesthetics
  geom_point(mapping = aes(color= drv)) + #and then each geom can have its unique aesthetics too
  geom_smooth(data = filter(mpg, class == "subcompact"), se = FALSE) # or even its own dataset. 
```

```
## `geom_smooth()` using method = 'loess'
```

![](May_3_HW_files/figure-html/unnamed-chunk-2-2.png)<!-- -->

####3.6.1 Exercise  
#####1.   
line chart: geom_line(); boxplot: geom_boxplot(); histogram: geom_histogram(); area plot (I don't even know what this is): geom_area()   
#####2.   
I think this plot will look like a plot of engine displacement against mileage on highway, with different colors corresponding to different drives. There will be dots overlaid on top of a smooth line.  

```r
ggplot(data = mpg, mapping = aes(x = displ, y = hwy, color = drv)) + 
  geom_point() + 
  geom_smooth(se = FALSE)
```

```
## `geom_smooth()` using method = 'loess'
```

![](May_3_HW_files/figure-html/unnamed-chunk-3-1.png)<!-- -->
#####3.    

```r
ggplot(data = mpg, mapping = aes(x = displ, y = hwy, color = drv), show.legend = FALSE) + 
  geom_point() + 
  geom_smooth(se = FALSE)
```

```
## `geom_smooth()` using method = 'loess'
```

![](May_3_HW_files/figure-html/unnamed-chunk-4-1.png)<!-- -->

Hmm honestly I don't see where it's been used early in the chapter...  

#####4.   

```r
ggplot(data = mpg, mapping = aes(x = displ, y = hwy, color = drv), show.legend = FALSE) + 
  geom_point() + 
  geom_smooth(se = TRUE)
```

```
## `geom_smooth()` using method = 'loess'
```

![](May_3_HW_files/figure-html/unnamed-chunk-5-1.png)<!-- -->

The se option add confidence interval zones around the smooth lines.  

#####5.   
I don't think they should look different. But let's take a look:  

```r
ggplot(data = mpg, mapping = aes(x = displ, y = hwy)) + 
  geom_point() + 
  geom_smooth()
```

```
## `geom_smooth()` using method = 'loess'
```

![](May_3_HW_files/figure-html/unnamed-chunk-6-1.png)<!-- -->

```r
ggplot() + 
  geom_point(data = mpg, mapping = aes(x = displ, y = hwy)) + 
  geom_smooth(data = mpg, mapping = aes(x = displ, y = hwy))
```

```
## `geom_smooth()` using method = 'loess'
```

![](May_3_HW_files/figure-html/unnamed-chunk-6-2.png)<!-- -->

Indeed they don't look like they are different.  

#####6.  

```r
ggplot(data = mpg, mapping = aes(x = displ, y = hwy)) + 
  geom_point() + 
  geom_smooth(se = FALSE)
```

```
## `geom_smooth()` using method = 'loess'
```

![](May_3_HW_files/figure-html/unnamed-chunk-7-1.png)<!-- -->

```r
ggplot(data = mpg, mapping = aes(x = displ, y = hwy)) +
  geom_point() +
  geom_smooth(mapping = aes(group = drv), se = FALSE)
```

```
## `geom_smooth()` using method = 'loess'
```

![](May_3_HW_files/figure-html/unnamed-chunk-7-2.png)<!-- -->

```r
ggplot(data = mpg, mapping = aes(x = displ, y = hwy, color = drv)) +
  geom_point() +
  geom_smooth(mapping = aes(group = drv), se = FALSE)
```

```
## `geom_smooth()` using method = 'loess'
```

![](May_3_HW_files/figure-html/unnamed-chunk-7-3.png)<!-- -->

```r
ggplot(data = mpg, mapping = aes(x = displ, y = hwy)) +
  geom_point(mapping = aes(color = drv)) +
  geom_smooth(se = FALSE)
```

```
## `geom_smooth()` using method = 'loess'
```

![](May_3_HW_files/figure-html/unnamed-chunk-7-4.png)<!-- -->

```r
ggplot(data = mpg, mapping = aes(x = displ, y = hwy)) +
  geom_point(mapping = aes(color = drv)) +
  geom_smooth(mapping = aes(linetype = drv),se = FALSE)
```

```
## `geom_smooth()` using method = 'loess'
```

![](May_3_HW_files/figure-html/unnamed-chunk-7-5.png)<!-- -->

```r
ggplot(data = mpg) +
  geom_point(mapping = aes(x = displ, y = hwy, color = drv), size = 5) ##I'm not sure if I'm getting at what this question is asking for. How do you get the white "halo" around each dot??
```

![](May_3_HW_files/figure-html/unnamed-chunk-7-6.png)<!-- -->

####3.7 Statistical Transformations

```r
ggplot(data = diamonds) + 
  geom_bar(mapping = aes(x = cut))
```

![](May_3_HW_files/figure-html/unnamed-chunk-8-1.png)<!-- -->

```r
ggplot(data = diamonds) + 
  stat_count(mapping = aes(x = cut)) #wut! You can use geom and stats interchangeably! Because every geom has a default stat, and every stat has a default geom. 
```

![](May_3_HW_files/figure-html/unnamed-chunk-8-2.png)<!-- -->

```r
#And you can override the stat:
ggplot(data = diamonds) + 
  geom_bar(mapping = aes(x = cut, y = ..prop.., group = 1)) # like specifying y = ..prop.. changes count to proportion. the dot part is kind of weird. What is group = 1?
```

![](May_3_HW_files/figure-html/unnamed-chunk-8-3.png)<!-- -->

```r
ggplot(data = diamonds) + 
  stat_summary(
    mapping = aes(x = cut, y = depth),
    fun.ymin = min,
    fun.ymax = max,
    fun.y = median
  )
```

![](May_3_HW_files/figure-html/unnamed-chunk-8-4.png)<!-- -->

####3.7.1 Exercise  
#####1.  
I'm thinking geom_linerange() or geom_pointrange(), but I'm not sure.  
#####2.  

```r
ggplot(data = diamonds) + 
  geom_col(mapping = aes(x = cut, y = price, color = clarity))
```

![](May_3_HW_files/figure-html/unnamed-chunk-9-1.png)<!-- -->

```r
ggplot(data = diamonds) + 
  geom_bar(mapping = aes(x = cut, color = clarity))
```

![](May_3_HW_files/figure-html/unnamed-chunk-9-2.png)<!-- -->

It looks like that geom_col has to take a y argument. It's default stat is not to count.   

#####3.  
I don't know if there is a go-to documentation for this.. but I found these pairs from [this webpage](http://sape.inf.usi.ch/quick-reference/ggplot2/geom):  
(geom-stat)  
abline-abline  
hline-hline  
vline-vline    
bar-bin  
boxplot-boxplot  
bin2d-bin2d  
hex-binhex  
density-density  
density2d-density2d  
contour-contour  
freqpoly-bin  
quantile-quantile   
smooth-smooth  
A lot of them have the same syntax.  

#####4.  

```r
?stat_smooth
```

it calculates:  
y: predicted value given x;  
ymin: lower pointwise confidence interval around the mean;  
ymax: upper pointwise confidence interval around the mean;  
se: standard error.  

Parameters:  
method: what linear regression model to use;  
formula: if you want to transform one of the input column (e.g. y ~ log(x));  
na.rm: if remove missing values;  
n: number of points to evaluate smoother at;  
level: level of confidence interval to use.  

#####5.   

```r
ggplot(data = diamonds) + 
  geom_bar(mapping = aes(x = cut, y = ..prop..))
```

![](May_3_HW_files/figure-html/unnamed-chunk-11-1.png)<!-- -->

```r
ggplot(data = diamonds) + 
  geom_bar(mapping = aes(x = cut, fill = color, y = ..prop..))
```

![](May_3_HW_files/figure-html/unnamed-chunk-11-2.png)<!-- -->

I think we need to set group = 1 because we want to find of the proportion of each x category in relation to the total population, not in relation to itself. Or the proportion is always gonna be 1. 

####3.8 Position Adjustments

```r
ggplot(data = diamonds) + 
  geom_bar(mapping = aes(x = cut, colour = cut))
```

![](May_3_HW_files/figure-html/unnamed-chunk-12-1.png)<!-- -->

```r
ggplot(data = diamonds) + 
  geom_bar(mapping = aes(x = cut, fill = cut))
```

![](May_3_HW_files/figure-html/unnamed-chunk-12-2.png)<!-- -->

```r
ggplot(data = diamonds) + 
  geom_bar(mapping = aes(x = cut, fill = clarity)) #automatic stacking of a second variable.
```

![](May_3_HW_files/figure-html/unnamed-chunk-12-3.png)<!-- -->

```r
ggplot(data = diamonds, mapping = aes(x = cut, fill = clarity)) + 
  geom_bar(alpha = 1/5, position = "identity") # un-stacked. 
```

![](May_3_HW_files/figure-html/unnamed-chunk-12-4.png)<!-- -->

```r
ggplot(data = diamonds, mapping = aes(x = cut, colour = clarity)) + 
  geom_bar(fill = NA, position = "identity") #un-stacked. Rather difficult to read. 
```

![](May_3_HW_files/figure-html/unnamed-chunk-12-5.png)<!-- -->

```r
ggplot(data = diamonds) + 
  geom_bar(mapping = aes(x = cut, fill = clarity), position = "fill") # for comparing composition across different groups. 
```

![](May_3_HW_files/figure-html/unnamed-chunk-12-6.png)<!-- -->

```r
ggplot(data = diamonds) + 
  geom_bar(mapping = aes(x = cut, fill = clarity), position = "dodge") # this is pretty
```

![](May_3_HW_files/figure-html/unnamed-chunk-12-7.png)<!-- -->

```r
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy), position = "jitter") # the jitter function adds a little random noise to each data point so they dont overlap.
```

![](May_3_HW_files/figure-html/unnamed-chunk-12-8.png)<!-- -->

####3.8.1 Exercise  
#####1.  

```r
ggplot(data = mpg, mapping = aes(x = cty, y = hwy)) + 
  geom_point()
```

![](May_3_HW_files/figure-html/unnamed-chunk-13-1.png)<!-- -->

The problem with this plot is overplotting, as in there are more datapoints than there are shown in the plot. Tp fix it we need to:  

```r
ggplot(data = mpg, mapping = aes(x = cty, y = hwy)) + 
  geom_point(position = "jitter")
```

![](May_3_HW_files/figure-html/unnamed-chunk-14-1.png)<!-- -->

#####2.  
width and height arguments control the amount of jittering in the grom_jitter() function.   

#####3.   

```r
ggplot(data = mpg, mapping = aes(x = cty, y = hwy)) + 
  geom_jitter()
```

![](May_3_HW_files/figure-html/unnamed-chunk-15-1.png)<!-- -->

```r
ggplot(data = mpg, mapping = aes(x = cty, y = hwy)) + 
  geom_count()
```

![](May_3_HW_files/figure-html/unnamed-chunk-15-2.png)<!-- -->

geom_count() uses size of the dot to represent how many dots are located on the exct same spot on the plot, while geom_jitter() moves each plot slightly so they don't mask over each other. I think I like the geom_count() function a little better, because it doesn't alter the data presented.  

#####4.  
The default position adjustment for geom_boxplot() is "dodge". Let us take a look.  

```r
ggplot(data = mpg, mapping = aes(x = drv, y = hwy, fill = class)) + 
  geom_boxplot()
```

![](May_3_HW_files/figure-html/unnamed-chunk-16-1.png)<!-- -->

```r
ggplot(data = mpg, mapping = aes(x = drv, y = hwy, fill = class)) + 
  geom_boxplot(position = "identity") #this made a big difference
```

![](May_3_HW_files/figure-html/unnamed-chunk-16-2.png)<!-- -->

####3.9  

```r
ggplot(data = mpg, mapping = aes(x = class, y = hwy)) + 
  geom_boxplot()
```

![](May_3_HW_files/figure-html/unnamed-chunk-17-1.png)<!-- -->

```r
ggplot(data = mpg, mapping = aes(x = class, y = hwy)) + 
  geom_boxplot() +
  coord_flip()
```

![](May_3_HW_files/figure-html/unnamed-chunk-17-2.png)<!-- -->

```r
#######
library(maps)
```

```
## 
## Attaching package: 'maps'
```

```
## The following object is masked from 'package:purrr':
## 
##     map
```

```r
nz <- map_data("nz")
ggplot(nz, aes(long, lat, group = group)) +
  geom_polygon(fill = "white", colour = "black")
```

![](May_3_HW_files/figure-html/unnamed-chunk-17-3.png)<!-- -->

```r
ggplot(nz, aes(long, lat, group = group)) +
  geom_polygon(fill = "white", colour = "black") +
  coord_quickmap()
```

![](May_3_HW_files/figure-html/unnamed-chunk-17-4.png)<!-- -->

```r
#######
bar <- ggplot(data = diamonds) + 
  geom_bar(
    mapping = aes(x = cut, fill = cut), 
    show.legend = FALSE,
    width = 1
  ) + 
  theme(aspect.ratio = 1) +
  labs(x = NULL, y = NULL)

bar + coord_flip()
```

![](May_3_HW_files/figure-html/unnamed-chunk-17-5.png)<!-- -->

```r
bar + coord_polar()
```

![](May_3_HW_files/figure-html/unnamed-chunk-17-6.png)<!-- -->

####3.9.1 Exercise  
#####1.   

```r
bar2 = ggplot(data = diamonds) + 
  geom_bar(mapping = aes(x = cut, fill = clarity))
bar2 + coord_polar()
```

![](May_3_HW_files/figure-html/unnamed-chunk-18-1.png)<!-- -->

#####2.  
labs() changes plot title/caption and labeling of the axis.   

#####3.  

```r
nz.map1 = ggplot(nz, aes(long, lat, group = group)) +
  geom_polygon(fill = "white", colour = "black")
nz.map1 + coord_quickmap()
```

![](May_3_HW_files/figure-html/unnamed-chunk-19-1.png)<!-- -->

```r
library(mapproj)
nz.map1 + coord_map()
```

![](May_3_HW_files/figure-html/unnamed-chunk-19-2.png)<!-- -->

I noticed that I needed to install package "mapproj" in order to run the coord_map() function. there is no latitude 46, 42 or 38 ticks on the y axis on the map generated from coord_map().

#####4.  


```r
ggplot(data = mpg, mapping = aes(x = cty, y = hwy)) +
  geom_point() + 
  geom_abline() +
  coord_fixed()
```

![](May_3_HW_files/figure-html/unnamed-chunk-20-1.png)<!-- -->

```r
ggplot(data = mpg, mapping = aes(x = cty, y = hwy)) +
  geom_point() + 
  geom_abline()
```

![](May_3_HW_files/figure-html/unnamed-chunk-20-2.png)<!-- -->

Coord_fixed() made srue that the x- and y-axis had the same length for each unit increment. This is important when we are comparing two variables of the same unit. The geom_abline() added a reference line for clarification. Any vehicle above the line has higher highway mileage than city mileage, which is shown to be all the vehicles in this dataset.   

####3.10 The layered grammar of graphics
ggplot code template:

ggplot(data = <DATA>) + 
  <GEOM_FUNCTION>(
     mapping = aes(<MAPPINGS>),
     stat = <STAT>, 
     position = <POSITION>
  ) +
  <COORDINATE_FUNCTION> +
  <FACET_FUNCTION>

