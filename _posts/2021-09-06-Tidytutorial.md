---
layout: post
title: "TidyTutorial"
subtitle: "Very Gentle Introduction To Tidyverse"
date: 2021-09-06 08:45:13 -0400
background: ''
---
Simplest Tutorial Ever!!
================


As I promised you to talk more about tidyverse and other R packages, this Tutorial is
only a begging so don’t judge harshly I'm new at this. Initially I did not want to make tutorials on this blog because I believed that the internet is full of them, but one more won't do no harm right ? 

let’s get started, first we need to load the tidyverse library so we can
have access to all the great functions, if you don’t have the tidyverse
library yet you should install it first by running:

``` r
# install.packages(tidyverse)
```

then now we need to load (attach) the package so we can use it

``` r
library(tidyverse)
```

like I said before one of the great things in R is that it has many
built in datasets we can choose one to play around with, I'll use mtcars because it's one of the most popular datasets with very clear variables 

``` r
?mtcars
```

when you run this line you will get informations about The mtcars dataset
 (like where it came from, what each variable means, etc...). Now we will
turn it into tibble which is according to tidyverse website : "A tibble, or tbl_df, is a modern reimagining of the data.frame, keeping what time has proven to be effective, and throwing out what is not. Tibbles are data.frames that are lazy and surly: they do less (i.e. they don’t change variable names or types, and don’t do partial matching) and complain more (e.g. when a variable does not exist). This forces you to confront problems earlier, typically leading to cleaner, more expressive code."  this is optinal however
this Tutorial is so simple this stuff it wont matter but it's just a habit for me.

``` r
data <- as_tibble(mtcars)
data
```

    ## # A tibble: 32 x 11
    ##      mpg   cyl  disp    hp  drat    wt  qsec    vs    am  gear  carb
    ##    <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl>
    ##  1  21       6  160    110  3.9   2.62  16.5     0     1     4     4
    ##  2  21       6  160    110  3.9   2.88  17.0     0     1     4     4
    ##  3  22.8     4  108     93  3.85  2.32  18.6     1     1     4     1
    ##  4  21.4     6  258    110  3.08  3.22  19.4     1     0     3     1
    ##  5  18.7     8  360    175  3.15  3.44  17.0     0     0     3     2
    ##  6  18.1     6  225    105  2.76  3.46  20.2     1     0     3     1
    ##  7  14.3     8  360    245  3.21  3.57  15.8     0     0     3     4
    ##  8  24.4     4  147.    62  3.69  3.19  20       1     0     4     2
    ##  9  22.8     4  141.    95  3.92  3.15  22.9     1     0     4     2
    ## 10  19.2     6  168.   123  3.92  3.44  18.3     1     0     4     4
    ## # ... with 22 more rows

We can see that the data has 32 observations and 11 columns, now let’s make use of tidyverse

``` r
data %>% select(mpg, disp, wt, am)
```

    ## # A tibble: 32 x 4
    ##      mpg  disp    wt    am
    ##    <dbl> <dbl> <dbl> <dbl>
    ##  1  21    160   2.62     1
    ##  2  21    160   2.88     1
    ##  3  22.8  108   2.32     1
    ##  4  21.4  258   3.22     0
    ##  5  18.7  360   3.44     0
    ##  6  18.1  225   3.46     0
    ##  7  14.3  360   3.57     0
    ##  8  24.4  147.  3.19     0
    ##  9  22.8  141.  3.15     0
    ## 10  19.2  168.  3.44     0
    ## # ... with 22 more rows

The select() function works similar to the SQL statement SELECT, as you
can see it’s very easy to choose certin columns but this is not the true
power of tidyverse the "pipe" %&gt;% is very helpful because now we can chain
commands together, we can select some columns and then select individual
cases given a certain condition like for example "show the displacement
and weight and miles per gallon for all the manual (am == 1) cars in the
dataset"

``` r
data %>% select(mpg, disp, wt, am)  %>% filter(am == 1)
```

    ## # A tibble: 13 x 4
    ##      mpg  disp    wt    am
    ##    <dbl> <dbl> <dbl> <dbl>
    ##  1  21   160    2.62     1
    ##  2  21   160    2.88     1
    ##  3  22.8 108    2.32     1
    ##  4  32.4  78.7  2.2      1
    ##  5  30.4  75.7  1.62     1
    ##  6  33.9  71.1  1.84     1
    ##  7  27.3  79    1.94     1
    ##  8  26   120.   2.14     1
    ##  9  30.4  95.1  1.51     1
    ## 10  15.8 351    3.17     1
    ## 11  19.7 145    2.77     1
    ## 12  15   301    3.57     1
    ## 13  21.4 121    2.78     1

we can add even more conditions and more functions let assume for the
sake of example that there’s something called weight displacement ratio
which is obtained by dividing the displacement of a car by its weight in
1000 of pounds and we want to do this for the manual cars that has high
miles per gallon (let’s assume that this means &gt; 25) we can do all of
this just by running :

``` r
data %>% mutate(Weight_displacemet_ratio = disp/wt*1000) 
%>% select(mpg, disp, wt,Weight_displacemet_ratio, am)
%>% filter(mpg > 25, am == 1)
```

    ## # A tibble: 6 x 5
    ##     mpg  disp    wt Weight_displacemet_ratio    am
    ##   <dbl> <dbl> <dbl>                    <dbl> <dbl>
    ## 1  32.4  78.7  2.2                    35773.     1
    ## 2  30.4  75.7  1.62                   46873.     1
    ## 3  33.9  71.1  1.84                   38747.     1
    ## 4  27.3  79    1.94                   40827.     1
    ## 5  26   120.   2.14                   56215.     1
    ## 6  30.4  95.1  1.51                   62855.     1

An important thing to remember is that the original data is still as it
is its not modified so if you check that

``` r
data
```

    ## # A tibble: 32 x 11
    ##      mpg   cyl  disp    hp  drat    wt  qsec    vs    am  gear  carb
    ##    <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl>
    ##  1  21       6  160    110  3.9   2.62  16.5     0     1     4     4
    ##  2  21       6  160    110  3.9   2.88  17.0     0     1     4     4
    ##  3  22.8     4  108     93  3.85  2.32  18.6     1     1     4     1
    ##  4  21.4     6  258    110  3.08  3.22  19.4     1     0     3     1
    ##  5  18.7     8  360    175  3.15  3.44  17.0     0     0     3     2
    ##  6  18.1     6  225    105  2.76  3.46  20.2     1     0     3     1
    ##  7  14.3     8  360    245  3.21  3.57  15.8     0     0     3     4
    ##  8  24.4     4  147.    62  3.69  3.19  20       1     0     4     2
    ##  9  22.8     4  141.    95  3.92  3.15  22.9     1     0     4     2
    ## 10  19.2     6  168.   123  3.92  3.44  18.3     1     0     4     4
    ## # ... with 22 more rows

this is very helpful because you don’t want to create new instances of
your dataframe to apply functions to it especially in the first stages
of analysis. 

Okay lets do something fun, hmm lets do a very simple
analysis by plotting very simple boxplot to see if there is difference
in mpg in the groups of cars that have a different number of cylinders

``` r
data = data %>% mutate(Cyl = as.factor(cyl))
```

``` r
box_plot = ggplot(data, aes(x = Cyl, y = mpg)) + geom_boxplot()
box_plot
```

![boxplot](\posts\TidyTut\box1.png)<!-- -->

Easy enough, but what an ugly graph lets make things more interesting

``` r
box_plot = ggplot(data, aes(x = Cyl, y = mpg, fill = Cyl))
 + geom_boxplot() + scale_fill_brewer(palette="Set3") 
 + theme_classic()
 +labs(
    title = "Fuel Consumtion Categorized By The Number Of Cylinders",
    subtitle = "(1973-74)",
    caption = "Data from the 1974 Motor Trend US magazine.",
    tag = "Figure 1",
    x = "Number of Cylinders",
    y = "Fuel economy (mpg)",
    colour = "Cylinders")
box_plot
```

![boxplot](\posts\TidyTut\box2.png)<!-- -->

It seems that cars with fewer number of cylinders have more miles per gallon which means they are more efficient than cars with higher number of cylinders.

Now let see if there is a relationship between the weight of a car and
its gas consumtion

``` r
sp<-ggplot(data, aes(x=wt, y=mpg, color=Cyl)) + geom_point() 
+ scale_color_manual(values=c("#997777", "#E69F00", "#56B4E9"))
+ theme_minimal()
+labs(
    title = "Fuel economy declines as weight increases",
    subtitle = "(1973-74)",
    caption = "Data from the 1974 Motor Trend US magazine.",
    tag = "Figure 2",
    x = "Weight (1000 lbs)",
    y = "Fuel economy (mpg)",
    colour = "Gears")
  sp 
```

![Scatterplot](\posts\TidyTut\sp1.png)<!-- --> 
Looks like heavier cars tends to consume more gallons than lighter ones.  we've gain a good amount of overall informations with few steps.

See? that was very easy don’t
worry if you don’t understand some of ggplot commands and options I
won’t explain it here today I think it deserve its own tutorial(s). The
whole point of this tutorial was to whet your appetite to go try this for
yourself, in future Tuts we will go through more complicated stuff with
more realistic data, for now I think this is enough for a first tutorial I'm just experimenting.

Hasta luego !

