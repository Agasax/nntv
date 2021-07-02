NNTV
================
Lars Mølgaard Saxhaug <https://twitter.com/load_dependent>

Last compiled on Friday 02 July, 2021

``` r
base_risk <- 0.001 # risk per  week
sample_size=1e6 # size of each group
vaccinated_risk <- 0.0002 # risk per  week
ve_true <- (base_risk-vaccinated_risk)/base_risk
weeks <- 52 # number of weeks

df <- tibble(week=1:weeks,
            sample_siz=sample_size,
            vaccinated_at_risk=sample_size*(1-vaccinated_risk)^week,
            placebo_at_risk=sample_size*(1-base_risk)^week,
            vaccinated_cases=sample_size-vaccinated_at_risk,
            placebo_cases=sample_size-placebo_at_risk)

df <- df  %>% 
  mutate(eer=(vaccinated_cases)/placebo_at_risk, # exposed event risk
         cer=(placebo_cases)/placebo_at_risk, # placebo event risk
         rr=eer/cer,
         ve=1-rr,
         ar=cer-eer,
         nntv=1/ar
         ) 
```

True vaccine efficacy is 80%
![](README_files/figure-gfm/ve-1.gif)<!-- -->

![](README_files/figure-gfm/nntv-1.gif)<!-- -->
