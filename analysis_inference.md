Analysis for SN Paper 2
================
Saurabh Khanna
2020-03-28

``` r
# Libraries
library(tidyverse)
library(haven)
library(stargazer)
library(plm)
library(sandwich)
library(lmtest)
library(AER)

# Parameters
data_file <- here::here("data/stu_admin_all_with_netvars.Rds")
```

Read main data file:

``` r
df <-
  data_file %>%
  read_rds()
```

Reservation students:

``` r
df %>%
  filter(stu_merge == 3) %>% 
  mutate(
    age = 2017 - lubridate::year(b_birthdate)
  ) %>% 
  filter(reservation == "Reservation") %>%
  # lm(
  #   e_seg_studymate ~ 
  #     ea_seats_reserved_students_sc + ea_seats_reserved_students_st + ea_seats_reserved_students_obc,
  #   data = .
  # ) %>% 
  ivreg(
    e_ql_score_perc ~ e_seg_studymate + b_ql_score_perc + gender + age |
    ea_seats_reserved_students_sc + ea_seats_reserved_students_st + ea_seats_reserved_students_obc + b_ql_score_perc + gender + age,
    data = .
  ) %>%
  coeftest(vcov = vcovHC, type = "HC1")
```

    ## 
    ## t test of coefficients:
    ## 
    ##                   Estimate Std. Error t value  Pr(>|t|)    
    ## (Intercept)      39.520948   7.808289  5.0614  4.72e-07 ***
    ## e_seg_studymate -11.975770   5.684995 -2.1066 0.0353353 *  
    ## b_ql_score_perc   0.748241   0.026026 28.7498 < 2.2e-16 ***
    ## gender           -2.497827   0.803528 -3.1086 0.0019182 ** 
    ## age              -1.002297   0.292696 -3.4244 0.0006342 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
