---
title: "lab4_warmup"
author: "Jacob Yousif"
date: "2023-01-19"
output: 
  html_document: 
    keep_md: yes
---



1. Load new package "palmerpenguins":

```r
library("tidyverse")
```

```
## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.2 ──
## ✔ ggplot2 3.4.0      ✔ purrr   1.0.0 
## ✔ tibble  3.1.8      ✔ dplyr   1.0.10
## ✔ tidyr   1.2.1      ✔ stringr 1.5.0 
## ✔ readr   2.1.3      ✔ forcats 0.5.2 
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## ✖ dplyr::filter() masks stats::filter()
## ✖ dplyr::lag()    masks stats::lag()
```

```r
#install.packages("palmerpenguins") #First-time install new package
library("palmerpenguins")
```

2. Dimensions:

```r
dim(penguins)
```

```
## [1] 344   8
```
344 rows, 8 columns

3. Variable names:

```r
colnames(penguins)
```

```
## [1] "species"           "island"            "bill_length_mm"   
## [4] "bill_depth_mm"     "flipper_length_mm" "body_mass_g"      
## [7] "sex"               "year"
```

4. Individuals sampled on each island:

```r
table(penguins$island)
```

```
## 
##    Biscoe     Dream Torgersen 
##       168       124        52
```
168 individuals on Biscoe Island, 124 individuals on Dream Island, and 52 individuals on Torgersen Island

5. Mean body mass for all penguins:

```r
mean(penguins$body_mass_g, na.rm = T)
```

```
## [1] 4201.754
```
About 4202g
