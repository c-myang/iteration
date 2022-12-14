Iteration and List Columns
================
2022-11-01

## Lists

In R, vectors are limited to a single data class – all elements are
characters, or all numeric, or all logical. Trying to join the following
vectors will result in coersion, as would creating vectors of mixed
types.

``` r
vec_numeric = 5:8
vec_char = c("My", "name", "is", "Jeff")
vec_logical = c(TRUE, TRUE, TRUE, FALSE)
```

Lists contain indexed elements, and the indexed elements themselves be
scalars, vectors, or other things entirely.

``` r
l = list(
  vec_numeric = 5:8,
  mat         = matrix(1:8, 2, 4),
  vec_logical = c(TRUE, FALSE),
  summary     = summary(rnorm(1000)))
l
```

    ## $vec_numeric
    ## [1] 5 6 7 8
    ## 
    ## $mat
    ##      [,1] [,2] [,3] [,4]
    ## [1,]    1    3    5    7
    ## [2,]    2    4    6    8
    ## 
    ## $vec_logical
    ## [1]  TRUE FALSE
    ## 
    ## $summary
    ##     Min.  1st Qu.   Median     Mean  3rd Qu.     Max. 
    ## -3.00805 -0.69737 -0.03532 -0.01165  0.68843  3.81028

### Accessing list items

Lists can be accessed using names or indices, and the things in lists
can be accessed in the way you would usually access an object of that
type. DO NOT USE DOLLAR SIGNS IN YOUR CODE!

``` r
l$vec_numeric
```

    ## [1] 5 6 7 8

``` r
l[[1]]
```

    ## [1] 5 6 7 8

``` r
l[[1]][1:3]
```

    ## [1] 5 6 7

``` r
l[["mat"]]
```

    ##      [,1] [,2] [,3] [,4]
    ## [1,]    1    3    5    7
    ## [2,]    2    4    6    8

## For loops

Let’s write a for loop to take the mean and SD of four samples from a
normal distribution.

``` r
list_norms = 
  list(
    a = rnorm(20, 5, 4),
    b = rnorm(20, -12, 3),
    c = rnorm(20, 17, .4),
    d = rnorm(20, 100, 1)
  )

is.list(list_norms)
```

    ## [1] TRUE

Here’s my function:

``` r
mean_and_sd = function(x) {
  
  if (!is.numeric(x)) {
    stop("Argument x should be numeric")
  } else if (length(x) == 1) {
    stop("Cannot be computed for length 1 vectors")
  }
  
  mean_x = mean(x)
  sd_x = sd(x)

  tibble(
    mean = mean_x, 
    sd = sd_x
  )
}
```

I can apply that function to each list element. Let’s try to make this
work:

``` r
mean_and_sd(list_norms[[1]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.80  4.48

``` r
mean_and_sd(list_norms[[2]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -11.8  2.45

``` r
mean_and_sd(list_norms[[3]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  17.1 0.381

``` r
mean_and_sd(list_norms[[4]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  99.6  1.18

This is not very concise. Let’s try a `for` loop instead!

We will create a vector of class “list” with a length of 4. We want to
output the mean and sd of each element in `list_norms`, such that we
want `output[[1]] = mean_and_sd(list_norms[[1]])` from 1 to 4.

``` r
output = vector("list", length = 4)

for (i in 1:4) {
  output[[i]] = mean_and_sd(list_norms[[i]])
}

output
```

    ## [[1]]
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.80  4.48
    ## 
    ## [[2]]
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -11.8  2.45
    ## 
    ## [[3]]
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  17.1 0.381
    ## 
    ## [[4]]
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  99.6  1.18

# Let’s try map!

Map applies the `mean_and_sd` function onto `list_norms` –\> same output
as running the for loop. It also applies names to each output.

``` r
map(list_norms, mean_and_sd)
```

    ## $a
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.80  4.48
    ## 
    ## $b
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -11.8  2.45
    ## 
    ## $c
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  17.1 0.381
    ## 
    ## $d
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  99.6  1.18

What if you want a different function…? We can “map” across `list_norms`
any function!

``` r
output = map(list_norms, median)
output
```

    ## $a
    ## [1] 3.485503
    ## 
    ## $b
    ## [1] -11.56734
    ## 
    ## $c
    ## [1] 17.10033
    ## 
    ## $d
    ## [1] 99.47834

``` r
output = map(list_norms, IQR)
output
```

    ## $a
    ## [1] 6.937857
    ## 
    ## $b
    ## [1] 3.594308
    ## 
    ## $c
    ## [1] 0.4435161
    ## 
    ## $d
    ## [1] 1.789004

Map variants… What if we don’t want our output to be a “list”?

``` r
output = map_dbl(list_norms, median)
output
```

    ##          a          b          c          d 
    ##   3.485503 -11.567340  17.100328  99.478335

More complicated… We can’t apply dbl to mean_and_sd because the function
gives two numbers. However, we can output a dataframe!

``` r
output = map_df(list_norms, mean_and_sd, .id = "input")
output
```

    ## # A tibble: 4 × 3
    ##   input   mean    sd
    ##   <chr>  <dbl> <dbl>
    ## 1 a       3.80 4.48 
    ## 2 b     -11.8  2.45 
    ## 3 c      17.1  0.381
    ## 4 d      99.6  1.18

We keep track of the id under the “input” column.

## List columns!

``` r
listcol_df = 
  tibble(
    name = c("a", "b", "c", "d"), 
    samp = list_norms
  )
```

When we print this, we get a weird output. Under “samp”, we get have a
named list, which is a collection of doubles of length 20.

``` r
listcol_df %>% pull(name)
```

    ## [1] "a" "b" "c" "d"

``` r
listcol_df %>% pull(samp)
```

    ## $a
    ##  [1]  9.53986035  9.44772738  1.51688947  5.84292634  5.27758259 -1.65059541
    ##  [7]  8.24335992 -2.64938318  0.01298628  8.99261778  2.83650902  4.13449684
    ## [13] -1.48774917 -0.80385586  6.40363892  4.30181229  2.63428612 -0.33610904
    ## [19]  0.61080600 13.14441444
    ## 
    ## $b
    ##  [1] -12.979469  -9.677984  -9.644981  -9.710262 -11.115574 -15.757068
    ##  [7] -15.028511  -9.745826 -15.925061 -10.417380 -13.600619 -13.195128
    ## [13] -14.368708 -12.690423  -9.368445 -10.638800 -12.697392  -9.389983
    ## [19]  -7.031989 -12.019107
    ## 
    ## $c
    ##  [1] 17.18820 17.11129 16.60884 16.62937 17.76791 17.35251 17.29683 17.05903
    ##  [9] 17.19416 17.06074 17.01680 17.08937 16.59581 17.96049 17.32078 16.89952
    ## [17] 17.48516 16.74910 17.68446 16.84225
    ## 
    ## $d
    ##  [1]  97.67851 101.36412 101.13223  99.22568  98.58963  98.16547  99.73099
    ##  [8]  98.16607  99.18553 100.16357 100.85552  99.18004  99.87640 100.25495
    ## [15] 101.71893  99.04146  98.39569  98.15439 100.55574  99.93988

``` r
listcol_df %>% 
  filter(name == "a")
```

    ## # A tibble: 1 × 2
    ##   name  samp        
    ##   <chr> <named list>
    ## 1 a     <dbl [20]>

listcol_df has everything we have.

Let’s try some operations.

``` r
listcol_df$samp[[1]] # Gives us first element of 'samp' list column
```

    ##  [1]  9.53986035  9.44772738  1.51688947  5.84292634  5.27758259 -1.65059541
    ##  [7]  8.24335992 -2.64938318  0.01298628  8.99261778  2.83650902  4.13449684
    ## [13] -1.48774917 -0.80385586  6.40363892  4.30181229  2.63428612 -0.33610904
    ## [19]  0.61080600 13.14441444

``` r
mean_and_sd(listcol_df$samp[[2]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -11.8  2.45

Can I just…map? We will apply `mean_and_sd` to each element of the
`samp` list column within the `listcol` dataframe.

``` r
map(listcol_df$samp, mean_and_sd)
```

    ## $a
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.80  4.48
    ## 
    ## $b
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -11.8  2.45
    ## 
    ## $c
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  17.1 0.381
    ## 
    ## $d
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  99.6  1.18

So…can I add a list column to our existing `listcol_df`? Use `mutate`!!

``` r
listcol_df = listcol_df %>% 
  mutate(
    summary = map(samp, mean_and_sd),
    medians = map_dbl(samp, median)
  )
```

This is fantastic! In this df, we have the name, sample list we started
with, and the result stored as a list (of tibbles). We don’t lose
information.

## Nested data

Let’s look at weather data

### Weather data

``` r
weather_df = 
  rnoaa::meteo_pull_monitors(
    c("USW00094728", "USC00519397", "USS0023B17S"),
    var = c("PRCP", "TMIN", "TMAX"), 
    date_min = "2017-01-01",
    date_max = "2017-12-31") %>%
  mutate(
    name = recode(
      id, 
      USW00094728 = "CentralPark_NY", 
      USC00519397 = "Waikiki_HA",
      USS0023B17S = "Waterhole_WA"),
    tmin = tmin / 10,
    tmax = tmax / 10) %>%
  select(name, id, everything())
```

Get our list columns. We want to nest anything from `date:tmin` inside
each weather station location.

``` r
weather_nest = 
  weather_df %>% 
  nest(data = date:tmin)
```

``` r
weather_nest %>% pull(name)
```

    ## [1] "CentralPark_NY" "Waikiki_HA"     "Waterhole_WA"

``` r
weather_nest %>% pull(data)
```

    ## [[1]]
    ## # A tibble: 365 × 4
    ##    date        prcp  tmax  tmin
    ##    <date>     <dbl> <dbl> <dbl>
    ##  1 2017-01-01     0   8.9   4.4
    ##  2 2017-01-02    53   5     2.8
    ##  3 2017-01-03   147   6.1   3.9
    ##  4 2017-01-04     0  11.1   1.1
    ##  5 2017-01-05     0   1.1  -2.7
    ##  6 2017-01-06    13   0.6  -3.8
    ##  7 2017-01-07    81  -3.2  -6.6
    ##  8 2017-01-08     0  -3.8  -8.8
    ##  9 2017-01-09     0  -4.9  -9.9
    ## 10 2017-01-10     0   7.8  -6  
    ## # … with 355 more rows
    ## 
    ## [[2]]
    ## # A tibble: 365 × 4
    ##    date        prcp  tmax  tmin
    ##    <date>     <dbl> <dbl> <dbl>
    ##  1 2017-01-01     0  26.7  16.7
    ##  2 2017-01-02     0  27.2  16.7
    ##  3 2017-01-03     0  27.8  17.2
    ##  4 2017-01-04     0  27.2  16.7
    ##  5 2017-01-05     0  27.8  16.7
    ##  6 2017-01-06     0  27.2  16.7
    ##  7 2017-01-07     0  27.2  16.7
    ##  8 2017-01-08     0  25.6  15  
    ##  9 2017-01-09     0  27.2  15.6
    ## 10 2017-01-10     0  28.3  17.2
    ## # … with 355 more rows
    ## 
    ## [[3]]
    ## # A tibble: 365 × 4
    ##    date        prcp  tmax  tmin
    ##    <date>     <dbl> <dbl> <dbl>
    ##  1 2017-01-01   432  -6.8 -10.7
    ##  2 2017-01-02    25 -10.5 -12.4
    ##  3 2017-01-03     0  -8.9 -15.9
    ##  4 2017-01-04     0  -9.9 -15.5
    ##  5 2017-01-05     0  -5.9 -14.2
    ##  6 2017-01-06     0  -4.4 -11.3
    ##  7 2017-01-07    51   0.6 -11.5
    ##  8 2017-01-08    76   2.3  -1.2
    ##  9 2017-01-09    51  -1.2  -7  
    ## 10 2017-01-10     0  -5   -14.2
    ## # … with 355 more rows

``` r
weather_nest$data[[1]] # Gives us a tibble for Central Park
```

    ## # A tibble: 365 × 4
    ##    date        prcp  tmax  tmin
    ##    <date>     <dbl> <dbl> <dbl>
    ##  1 2017-01-01     0   8.9   4.4
    ##  2 2017-01-02    53   5     2.8
    ##  3 2017-01-03   147   6.1   3.9
    ##  4 2017-01-04     0  11.1   1.1
    ##  5 2017-01-05     0   1.1  -2.7
    ##  6 2017-01-06    13   0.6  -3.8
    ##  7 2017-01-07    81  -3.2  -6.6
    ##  8 2017-01-08     0  -3.8  -8.8
    ##  9 2017-01-09     0  -4.9  -9.9
    ## 10 2017-01-10     0   7.8  -6  
    ## # … with 355 more rows

Supposed I want to regress `tmax` on `tmin` for each station. Can I do
it for each of the datasets separately and store its results?

``` r
lm(tmax ~ tmin, weather_nest$data[[1]])
```

    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = weather_nest$data[[1]])
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.209        1.039

``` r
lm(tmax ~ tmin, weather_nest$data[[2]])
```

    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = weather_nest$data[[2]])
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##     20.0966       0.4509

This works…let’s write a function to spit out the `lm` results for any
given dataframe!

What if we wanted
`weather_lm(weather_nest$data[[1]]), weather_lm(weather_nest$data[[2]])`,
etc.?

``` r
weather_lm = function(df) {
  
  lm(tmax ~ tmin, data = df)
}

output = vector("list", 3)

for (i in 1:3) {
  
  output[[i]] = weather_lm(weather_nest$data[[i]])
  
}
```

What about a Map?

``` r
map(weather_nest$data, weather_lm)
```

    ## [[1]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.209        1.039  
    ## 
    ## 
    ## [[2]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##     20.0966       0.4509  
    ## 
    ## 
    ## [[3]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.499        1.221

What about a map in a list column?! (adding results in a column in
`weather_nest` df)

``` r
weather_nest = weather_nest %>% 
  mutate(
    models = map(data, weather_lm)
  )

weather_nest
```

    ## # A tibble: 3 × 4
    ##   name           id          data               models
    ##   <chr>          <chr>       <list>             <list>
    ## 1 CentralPark_NY USW00094728 <tibble [365 × 4]> <lm>  
    ## 2 Waikiki_HA     USC00519397 <tibble [365 × 4]> <lm>  
    ## 3 Waterhole_WA   USS0023B17S <tibble [365 × 4]> <lm>

Now we know that we can fit a separate linear model for each weather
station and save the results in the `models` column!
