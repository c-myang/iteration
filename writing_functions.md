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

## Fixing bad stuff

In reading data from the web, we wrote code that allowed us to scrape
information in Amazon reviews. That code is below.

``` r
url = "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviewerType=avp_only_reviews&sortBy=recent&pageNumber=1"

dynamite_html = read_html(url)

review_titles = 
  dynamite_html %>%
  html_nodes(".a-text-bold span") %>%
  html_text()

review_stars = 
  dynamite_html %>%
  html_nodes("#cm_cr-review_list .review-rating") %>%
  html_text() %>%
  str_extract("^\\d") %>%
  as.numeric()

review_text = 
  dynamite_html %>%
  html_nodes(".review-text-content span") %>%
  html_text() %>% 
  str_replace_all("\n", "") %>% 
  str_trim()

reviews = tibble(
  title = review_titles,
  stars = review_stars,
  text = review_text
)
```

Let’s write a quick function to scrape review information for any URL to
an Amazon review page.

``` r
read_page_reviews = function(url) {
  
  html = read_html(url)
  
  review_titles = 
    html %>%
    html_nodes(".a-text-bold span") %>%
    html_text()
  
  review_stars = 
    html %>%
    html_nodes("#cm_cr-review_list .review-rating") %>%
    html_text() %>%
    str_extract("^\\d") %>%
    as.numeric()
  
  review_text = 
    html %>%
    html_nodes(".review-text-content span") %>%
    html_text() %>% 
    str_replace_all("\n", "") %>% 
    str_trim() %>% 
    str_subset("The media could not be loaded.", negate = TRUE) %>% 
    str_subset("^$", negate = TRUE)
  
  tibble(
    title = review_titles,
    stars = review_stars,
    text = review_text
  )
}
```

Let’s try with a URL.

``` r
url = "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviewerType=avp_only_reviews&sortBy=recent&pageNumber=4"

read_page_reviews(url)
```

    ## # A tibble: 10 × 3
    ##    title                                    stars text                          
    ##    <chr>                                    <dbl> <chr>                         
    ##  1 Gosh!                                        5 "Ever wonder if chickens have…
    ##  2 An Acquired Taste                            1 "This is one of those \"I get…
    ##  3 What is this ?                               4 "Nice movie for family night …
    ##  4 Napoleon Dynamite                            2 "I was not impressed by this …
    ##  5 Great movie                                  5 "Great movie"                 
    ##  6 Good movie                                   5 "Good movie"                  
    ##  7 Came as Described                            5 "Came as Described"           
    ##  8 Oddly on my list of keepers.                 5 "Good movie. Underrated but q…
    ##  9 Low budget fun                               5 "Oddball characters doing qui…
    ## 10 On a scale of 1 to 10 this rates a minus     1 "This movie is hands down the…

What good does this do? We’ll use this to read in reviews from a few
pages and combine the results.

``` r
url_base = "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviewerType=avp_only_reviews&sortBy=recent&pageNumber="

vec_urls = str_c(url_base, 1:5)

dynamite_reviews = bind_rows(
  read_page_reviews(vec_urls[1]),
  read_page_reviews(vec_urls[2]),
  read_page_reviews(vec_urls[3]),
  read_page_reviews(vec_urls[4]),
  read_page_reviews(vec_urls[5])
)

dynamite_reviews
```

    ## # A tibble: 50 × 3
    ##    title                                stars text                              
    ##    <chr>                                <dbl> <chr>                             
    ##  1 70’s and 80’s Schtick Comedy             5 …especially funny if you have eve…
    ##  2 Amazon Censorship                        5 I hope Amazon does not censor my …
    ##  3 Watch to say you did                     3 I know it's supposed to be a cult…
    ##  4 Best Movie Ever!                         5 We just love this movie and even …
    ##  5 Quirky                                   5 Good family film                  
    ##  6 Funny movie - can't play it !            1 Sony 4k player won't even recogni…
    ##  7 A brilliant story about teenage life     5 Napoleon Dynamite delivers dry hu…
    ##  8 HUHYAH                                   5 Spicy                             
    ##  9 Cult Classic                             4 Takes a time or two to fully appr…
    ## 10 Sweet                                    5 Timeless Movie. My Grandkids are …
    ## # … with 40 more rows

## Loading LoTR data

In tidy data, we broke the “only copy code twice” rule when we used the
code below to process the LoTR words data:

``` r
fellowship_ring = readxl::read_excel("./data/LotR_Words.xlsx", range = "B3:D6") %>%
  mutate(movie = "fellowship_ring")

two_towers = readxl::read_excel("./data/LotR_Words.xlsx", range = "F3:H6") %>%
  mutate(movie = "two_towers")

return_king = readxl::read_excel("./data/LotR_Words.xlsx", range = "J3:L6") %>%
  mutate(movie = "return_king")

lotr_tidy = bind_rows(fellowship_ring, two_towers, return_king) %>%
  janitor::clean_names() %>%
  gather(key = sex, value = words, female:male) %>%
  mutate(race = str_to_lower(race)) %>% 
  select(movie, everything()) 
```

The function below will read in and clean LoTR data – it differs from
the previous code by including some data tidying steps in the function
rather than after data have been combined, but produces the same result.

``` r
lotr_load_and_tidy = function(path, range, movie_name) {
  
  df = readxl::read_excel(path, range = range) %>%
    janitor::clean_names() %>%
    gather(key = sex, value = words, female:male) %>%
    mutate(race = str_to_lower(race),
           movie = movie_name)
  
  df
  
}

lotr_tidy = 
  bind_rows(
    lotr_load_and_tidy("./data/LotR_Words.xlsx", "B3:D6", "fellowship_ring"),
    lotr_load_and_tidy("./data/LotR_Words.xlsx", "F3:H6", "two_towers"),
    lotr_load_and_tidy("./data/LotR_Words.xlsx", "J3:L6", "return_king")) %>%
  select(movie, everything()) 
```

## Functions as arguments

One powerful tool is the ability to pass functions as arguments into
functions. This might seem like a weird thing to do, but it has a lot of
handy applications – we’ll see just how far it goes in the next modules
in this topic.

Do NOT use variable names you created in your function outside your
function!

``` r
x_vec = rnorm(25, 0, 1)

my_summary = function(x, summ_func) {
  summ_func(x)
}

my_summary(x_vec, sd)
```

    ## [1] 1.089694

``` r
my_summary(x_vec, IQR)
```

    ## [1] 1.455819

``` r
my_summary(x_vec, var)
```

    ## [1] 1.187433
