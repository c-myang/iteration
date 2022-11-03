Simulations
================
2022-11-03

## Simulations!!

Here’s our function from before.

``` r
sim_mean_sd = function(n_obs, mu = 7, sigma = 4) {
  
  x = rnorm(n = n_obs, mean = mu, sd = sigma)

tibble(
  mu_hat = mean(x), 
  sigma_hat = sd(x)
)
}
```

How did we use this before?

``` r
sim_mean_sd(n_obs = 30)
```

    ## # A tibble: 1 × 2
    ##   mu_hat sigma_hat
    ##    <dbl>     <dbl>
    ## 1   7.33      3.70

Every time we run this function, we get different estimates. Can we do
this 100 times? 1000 times?

How can we use this now…a for loop perhaps? We can create a vector with
100 empty spots, and fill these columns with `sim_mean_sd`.

``` r
output = vector("list", length = 100)

for (i in 1:100) {
  output[[i]] = sim_mean_sd(n_obs = 30)
  
}

bind_rows(output)
```

    ## # A tibble: 100 × 2
    ##    mu_hat sigma_hat
    ##     <dbl>     <dbl>
    ##  1   7.53      3.18
    ##  2   7.44      3.84
    ##  3   7.45      3.53
    ##  4   5.68      3.69
    ##  5   7.95      4.22
    ##  6   7.27      4.34
    ##  7   6.05      4.05
    ##  8   7.10      3.72
    ##  9   7.55      4.11
    ## 10   7.87      3.79
    ## # … with 90 more rows

Let’s use list columns instead! `expand_grid` gives us all possible
combinations of sample size = 30 and iteration from 1 to 100. Then, we
use `map` to input the results of `sim_mean_sd` into our df.

``` r
sim_results_df = 
  expand_grid(
    sample_size = 30,
    iteration = 1:100
  ) %>% 
  mutate(
    estimate_df = map(sample_size, sim_mean_sd)
  ) %>% 
  unnest(estimate_df)

sim_results_df
```

    ## # A tibble: 100 × 4
    ##    sample_size iteration mu_hat sigma_hat
    ##          <dbl>     <int>  <dbl>     <dbl>
    ##  1          30         1   7.63      4.15
    ##  2          30         2   8.86      4.29
    ##  3          30         3   7.32      3.96
    ##  4          30         4   7.82      4.97
    ##  5          30         5   6.16      4.08
    ##  6          30         6   8.67      3.55
    ##  7          30         7   7.08      4.12
    ##  8          30         8   7.36      3.99
    ##  9          30         9   7.62      4.76
    ## 10          30        10   7.13      4.35
    ## # … with 90 more rows

Let’s plot some summaries for our simulation results.

``` r
sim_results_df %>% 
  ggplot(aes(x = mu_hat)) + 
  geom_density()
```

![](simulation_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->
