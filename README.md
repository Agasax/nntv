NNTV
================
Lars Mølgaard Saxhaug <https://twitter.com/load_dependent>

Last compiled on Friday 02 July, 2021

``` r
# packages
pacman::p_load(tidyverse,
               ggthemes,
               gganimate)
```

``` r
base_risk <- 0.001 # risk per  week placebo group
ve_true <- 0.8 # vaccine efficacy, 1-rr
vaccinated_risk <- base_risk -ve_true*base_risk  # risk per  week vaccinated group
sample_size=1e6 # size of each group
weeks <- 52 # number of weeks

df <- tibble(
  week = rep(1:weeks,3),
  
  vaccinated_cases = sample_size * vaccinated_risk * week,
  placebo_cases = sample_size * base_risk * week
) %>%
  mutate(
    eer = (vaccinated_cases) / sample_size, # exposed event risk
    cer = (placebo_cases) / sample_size, # placebo event risk
    rr = eer / cer,
    ve = 1 - rr,
    ar = cer - eer,
    nntv = 1 / ar
  )
```

True vaccine efficacy is 80%
![](README_files/figure-gfm/ve-1.gif)<!-- -->

![](README_files/figure-gfm/nntv-1.gif)<!-- -->
