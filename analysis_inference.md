Analysis for SN Paper 2 - Inference
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

# Parameters
data_file <- here::here("data/stu_admin_all_with_netvars.Rds")
```

Read main data file:

``` r
df <-
  data_file %>%
  read_rds()
```

Student fixed effects:
