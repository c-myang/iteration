Writing Functions
================
2022-10-27

## My first function

Let’s compute the Z-score version of a list of numbers!

``` r
x_vec = rnorm(25, mean = 7, sd = 4)
(x_vec - mean(x_vec)) / sd(x_vec)
```

    ##  [1] -0.83687228  0.01576465 -1.05703126  1.50152998  0.16928872 -1.04107494
    ##  [7]  0.33550276  0.59957343  0.42849461 -0.49894708  1.41364561  0.23279252
    ## [13] -0.83138529 -2.50852027  1.00648110 -0.22481531 -0.19456260  0.81587675
    ## [19]  0.68682298  0.44756609  0.78971253  0.64568566 -0.09904161 -2.27133861
    ## [25]  0.47485186

Suppose you want to do this often.

``` r
z_scores = function(x) {
  if (!is.numeric(x)) {
    stop("Z scores only work for numbers")
  }
  
  if (length(x) < 3) {
    stop("Z scores really only work if you have three or more numbers")
  }
  
  z = (x - mean(x)) / sd(x)
  z #or return(z)
}
```

Now we have a `z_scores` function!

``` r
z_scores(x_vec)
```

    ##  [1] -0.83687228  0.01576465 -1.05703126  1.50152998  0.16928872 -1.04107494
    ##  [7]  0.33550276  0.59957343  0.42849461 -0.49894708  1.41364561  0.23279252
    ## [13] -0.83138529 -2.50852027  1.00648110 -0.22481531 -0.19456260  0.81587675
    ## [19]  0.68682298  0.44756609  0.78971253  0.64568566 -0.09904161 -2.27133861
    ## [25]  0.47485186

``` r
z_scores(x = 1:10)
```

    ##  [1] -1.4863011 -1.1560120 -0.8257228 -0.4954337 -0.1651446  0.1651446
    ##  [7]  0.4954337  0.8257228  1.1560120  1.4863011

``` r
z_scores(x = rbinom(100, 1, 0.6))
```

    ##   [1]  0.7789571 -1.2709300  0.7789571  0.7789571  0.7789571  0.7789571
    ##   [7]  0.7789571  0.7789571 -1.2709300  0.7789571 -1.2709300  0.7789571
    ##  [13]  0.7789571  0.7789571 -1.2709300  0.7789571  0.7789571 -1.2709300
    ##  [19]  0.7789571 -1.2709300  0.7789571 -1.2709300  0.7789571  0.7789571
    ##  [25]  0.7789571 -1.2709300 -1.2709300  0.7789571 -1.2709300 -1.2709300
    ##  [31]  0.7789571 -1.2709300  0.7789571  0.7789571 -1.2709300  0.7789571
    ##  [37] -1.2709300  0.7789571  0.7789571  0.7789571  0.7789571  0.7789571
    ##  [43] -1.2709300 -1.2709300 -1.2709300 -1.2709300  0.7789571  0.7789571
    ##  [49] -1.2709300 -1.2709300 -1.2709300  0.7789571  0.7789571 -1.2709300
    ##  [55] -1.2709300  0.7789571  0.7789571  0.7789571 -1.2709300  0.7789571
    ##  [61] -1.2709300 -1.2709300  0.7789571  0.7789571  0.7789571  0.7789571
    ##  [67] -1.2709300  0.7789571  0.7789571 -1.2709300 -1.2709300  0.7789571
    ##  [73]  0.7789571  0.7789571 -1.2709300  0.7789571  0.7789571  0.7789571
    ##  [79]  0.7789571  0.7789571  0.7789571  0.7789571  0.7789571 -1.2709300
    ##  [85] -1.2709300  0.7789571  0.7789571  0.7789571 -1.2709300  0.7789571
    ##  [91] -1.2709300 -1.2709300  0.7789571  0.7789571 -1.2709300  0.7789571
    ##  [97]  0.7789571 -1.2709300  0.7789571 -1.2709300

``` r
#z_scores(x = 3) #Can't take the sd() of 3, so returns NA
#z_scores(x = "My name is Cathy") #Can't take the sd() or mean() of characters
```

## Let’s have multiple outputs

Let’s just get the mean and sd from the vector input.

``` r
mean_and_sd = function(x) {
  
  if (!is.numeric(x)) {
    stop("Argument x should be numeric")
  } else if (length(x) == 1) {
    stop("Cannot be computed for length 1 vectors")
  }
  
  mean_x = mean(x)
  sd_x = sd(x)
  
  tibble(mean = mean_x, 
       sd = sd_x)
}
```

``` r
mean_and_sd(x_vec)
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  7.67  3.80

``` r
mean_and_sd(x_vec)
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  7.67  3.80

``` r
mean_and_sd(x = 1:10)
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1   5.5  3.03

``` r
mean_and_sd(x = rbinom(100, 1, 0.6))
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  0.63 0.485

``` r
#mean_and_sd(x = 3) 
#mean_and_sd(x = "My name is Cathy") 
```

## Simulations

When we compute the mean and sd each time, we are getting different
values each time.

``` r
x_vec = rnorm(n = 25000, mean = 7, sd = 4)

tibble(
  mean = mean(x_vec), 
  sd = sd(x_vec)
)
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  7.00  4.00

Can I do this using a function? YUP

``` r
sim_mean_sd = function(n_obs, true_mean = 7, true_sd = 4) {
  
  x = rnorm(n = n_obs, mean = true_mean, sd = true_sd)

tibble(
  mean = mean(x), 
  sd = sd(x)
)
}
```

Does it work??

``` r
sim_mean_sd(n_obs = 25)
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  6.61  4.34

We can see when running the function, we are getting a tibble that
outputs the true mean and sd each time. When we don’t set `true_mean`
and `true_sd`, it defaults to the values we set of 7 and 4. If you don’t
name the arguments of the function, it will assume you are putting in
order of the arguments of the function.
