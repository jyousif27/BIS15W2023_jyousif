---
title: "BIS 15L Midterm 2"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your code should be organized, clean, and run free from errors. Remember, you must remove the `#` for any included code chunks to run. Be sure to add your name to the author header above.  

After the first 50 minutes, please upload your code (5 points). During the second 50 minutes, you may get help from each other- but no copy/paste. Upload the last version at the end of this time, but be sure to indicate it as final. If you finish early, you are free to leave.

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean! Use the tidyverse and pipes unless otherwise indicated. To receive full credit, all plots must have clearly labeled axes, a title, and consistent aesthetics. This exam is worth a total of 35 points. 

Please load the following libraries.

```r
library("tidyverse")
library("janitor")
library("naniar")
```

## Data
These data are from a study on surgical residents. The study was originally published by Sessier et al. “Operation Timing and 30-Day Mortality After Elective General Surgery”. Anesth Analg 2011; 113: 1423-8. The data were cleaned for instructional use by Amy S. Nowacki, “Surgery Timing Dataset”, TSHS Resources Portal (2016). Available at https://www.causeweb.org/tshs/surgery-timing/.

Descriptions of the variables and the study are included as pdf's in the data folder.  

Please run the following chunk to import the data.

```r
surgery <- read_csv("data/surgery.csv")
```

1. (2 points) Use the summary function(s) of your choice to explore the data and get an idea of its structure. Please also check for NA's.

```r
surgery %>% 
  glimpse() %>% 
  miss_var_summary()
```

```
## Rows: 32,001
## Columns: 25
## $ ahrq_ccs            <chr> "<Other>", "<Other>", "<Other>", "<Other>", "<Othe…
## $ age                 <dbl> 67.8, 39.5, 56.5, 71.0, 56.3, 57.7, 56.6, 64.2, 66…
## $ gender              <chr> "M", "F", "F", "M", "M", "F", "M", "F", "M", "F", …
## $ race                <chr> "Caucasian", "Caucasian", "Caucasian", "Caucasian"…
## $ asa_status          <chr> "I-II", "I-II", "I-II", "III", "I-II", "I-II", "IV…
## $ bmi                 <dbl> 28.04, 37.85, 19.56, 32.22, 24.32, 40.30, 64.57, 4…
## $ baseline_cancer     <chr> "No", "No", "No", "No", "Yes", "No", "No", "No", "…
## $ baseline_cvd        <chr> "Yes", "Yes", "No", "Yes", "No", "Yes", "Yes", "Ye…
## $ baseline_dementia   <chr> "No", "No", "No", "No", "No", "No", "No", "No", "N…
## $ baseline_diabetes   <chr> "No", "No", "No", "No", "No", "No", "Yes", "No", "…
## $ baseline_digestive  <chr> "Yes", "No", "No", "No", "No", "No", "No", "No", "…
## $ baseline_osteoart   <chr> "No", "No", "No", "No", "No", "No", "No", "No", "N…
## $ baseline_psych      <chr> "No", "No", "No", "No", "No", "Yes", "No", "No", "…
## $ baseline_pulmonary  <chr> "No", "No", "No", "No", "No", "No", "No", "No", "N…
## $ baseline_charlson   <dbl> 0, 0, 0, 0, 0, 0, 2, 0, 1, 2, 0, 1, 0, 0, 0, 0, 0,…
## $ mortality_rsi       <dbl> -0.63, -0.63, -0.49, -1.38, 0.00, -0.77, -0.36, -0…
## $ complication_rsi    <dbl> -0.26, -0.26, 0.00, -1.15, 0.00, -0.84, -1.34, 0.0…
## $ ccsmort30rate       <dbl> 0.0042508, 0.0042508, 0.0042508, 0.0042508, 0.0042…
## $ ccscomplicationrate <dbl> 0.07226355, 0.07226355, 0.07226355, 0.07226355, 0.…
## $ hour                <dbl> 9.03, 18.48, 7.88, 8.80, 12.20, 7.67, 9.53, 7.52, …
## $ dow                 <chr> "Mon", "Wed", "Fri", "Wed", "Thu", "Thu", "Tue", "…
## $ month               <chr> "Nov", "Sep", "Aug", "Jun", "Aug", "Dec", "Apr", "…
## $ moonphase           <chr> "Full Moon", "New Moon", "Full Moon", "Last Quarte…
## $ mort30              <chr> "No", "No", "No", "No", "No", "No", "No", "No", "N…
## $ complication        <chr> "No", "No", "No", "No", "No", "No", "No", "Yes", "…
```

```
## # A tibble: 25 × 3
##    variable          n_miss pct_miss
##    <chr>              <int>    <dbl>
##  1 bmi                 3290 10.3    
##  2 race                 480  1.50   
##  3 asa_status             8  0.0250 
##  4 gender                 3  0.00937
##  5 age                    2  0.00625
##  6 ahrq_ccs               0  0      
##  7 baseline_cancer        0  0      
##  8 baseline_cvd           0  0      
##  9 baseline_dementia      0  0      
## 10 baseline_diabetes      0  0      
## # … with 15 more rows
```

2. (3 points) Let's explore the participants in the study. Show a count of participants by race AND make a plot that visually represents your output.

```r
surgery %>% 
  count(race)
```

```
## # A tibble: 4 × 2
##   race                 n
##   <chr>            <int>
## 1 African American  3790
## 2 Caucasian        26488
## 3 Other             1243
## 4 <NA>               480
```

```r
surgery %>% 
  ggplot(aes(x=race, fill=race)) +
  geom_bar(color = "black") +
  labs(title = "Number of Participants by Race") +
  scale_fill_brewer(palette = "Oranges")
```

![](midterm_2_final_files/figure-html/unnamed-chunk-5-1.png)<!-- -->

3. (2 points) What is the mean age of participants by gender? (hint: please provide a number for each) Since only three participants do not have gender indicated, remove these participants from the data.

```r
surgery %>% 
  filter(is.na(gender)==F) %>% 
  group_by(gender) %>% 
  summarize(mean_age = mean(age, na.rm=T))
```

```
## # A tibble: 2 × 2
##   gender mean_age
##   <chr>     <dbl>
## 1 F          56.7
## 2 M          58.8
```

4. (3 points) Make a plot that shows the range of age associated with gender.

```r
surgery %>% 
  filter(is.na(gender)==F) %>% 
  ggplot(aes(x = gender, y = age, fill = gender)) +
  geom_boxplot(na.rm=T) +
  scale_fill_brewer(palette = "Oranges") +
  labs(title="Age Range by Gender")
```

![](midterm_2_final_files/figure-html/unnamed-chunk-7-1.png)<!-- -->

5. (2 points) How healthy are the participants? The variable `asa_status` is an evaluation of patient physical status prior to surgery. Lower numbers indicate fewer comorbidities (presence of two or more diseases or medical conditions in a patient). Make a plot that compares the number of `asa_status` I-II, III, and IV-V.

```r
surgery %>%
  filter(is.na(asa_status)==F) %>% 
  ggplot(aes(x=asa_status, fill=asa_status)) +
  geom_bar(color="black") +
  scale_fill_brewer(palette = "Oranges") +
  labs(title = "Pre-existing Comorbidities in Patients")
```

![](midterm_2_final_files/figure-html/unnamed-chunk-8-1.png)<!-- -->

6. (3 points) Create a plot that displays the distribution of body mass index for each `asa_status` as a probability distribution- not a histogram. (hint: use faceting!)

```r
surgery %>% 
  filter(is.na(asa_status)==F) %>%
  ggplot(aes(x=bmi, fill=asa_status)) +
  geom_density(na.rm=T) +
  facet_wrap(~asa_status) +
  scale_fill_brewer(palette = "Oranges") +
  labs(title = "BMI vs. Comorbidity", y = "comorbidity")
```

![](midterm_2_final_files/figure-html/unnamed-chunk-9-1.png)<!-- -->

The variable `ccsmort30rate` is a measure of the overall 30-day mortality rate associated with each type of operation. The variable `ccscomplicationrate` is a measure of the 30-day in-hospital complication rate. The variable `ahrq_ccs` lists each type of operation.  

7. (4 points) What are the 5 procedures associated with highest risk of 30-day mortality AND how do they compare with the 5 procedures with highest risk of complication? (hint: no need for a plot here)

```r
surgery %>% 
  group_by(ahrq_ccs) %>% 
  summarize(avg_mort = mean(ccsmort30rate), avg_complication = mean(ccscomplicationrate)) %>% 
  slice_max(avg_mort, n=5)
```

```
## # A tibble: 5 × 3
##   ahrq_ccs                                             avg_mort avg_complication
##   <chr>                                                   <dbl>            <dbl>
## 1 Colorectal resection                                  0.0167            0.312 
## 2 Small bowel resection                                 0.0129            0.466 
## 3 Gastrectomy; partial and total                        0.0127            0.190 
## 4 Endoscopy and endoscopic biopsy of the urinary tract  0.00811           0.0270
## 5 Spinal fusion                                         0.00742           0.183
```

```r
surgery %>% 
  group_by(ahrq_ccs) %>% 
  summarize(avg_mort = mean(ccsmort30rate), avg_complication = mean(ccscomplicationrate)) %>% 
  slice_max(avg_complication, n=5)
```

```
## # A tibble: 5 × 3
##   ahrq_ccs                         avg_mort avg_complication
##   <chr>                               <dbl>            <dbl>
## 1 Small bowel resection             0.0129             0.466
## 2 Colorectal resection              0.0167             0.312
## 3 Nephrectomy; partial or complete  0.00276            0.197
## 4 Gastrectomy; partial and total    0.0127             0.190
## 5 Spinal fusion                     0.00742            0.183
```

8. (3 points) Make a plot that compares the `ccsmort30rate` for all listed `ahrq_ccs` procedures.

```r
surgery %>% 
  ggplot(aes(x=ahrq_ccs, y=ccsmort30rate)) +
  geom_col() +
  #scale_fill_brewer(palette = "Oranges") +
  labs(title="30-Day Mortality Rate by Procedure") +
  coord_flip()
```

![](midterm_2_final_files/figure-html/unnamed-chunk-12-1.png)<!-- -->

9. (4 points) When is the best month to have surgery? Make a chart that shows the 30-day mortality and complications for the patients by month. `mort30` is the variable that shows whether or not a patient survived 30 days post-operation.

```r
best_month_mort <- surgery %>% 
  select(month, mort30) %>% 
  group_by(month) %>% 
  count(mort30) %>% 
  pivot_wider(names_from = mort30, values_from = n) %>% 
  mutate(death_probability = (Yes / (No + Yes))) %>% 
  arrange(desc(death_probability))
best_month_mort
```

```
## # A tibble: 12 × 4
## # Groups:   month [12]
##    month    No   Yes death_probability
##    <chr> <int> <int>             <dbl>
##  1 Jan    2651    19           0.00712
##  2 Feb    2489    17           0.00678
##  3 Jul    2313    12           0.00516
##  4 Sep    3192    16           0.00499
##  5 Jun    2980    14           0.00468
##  6 Mar    2685    12           0.00445
##  7 Apr    2686    12           0.00445
##  8 May    2644    10           0.00377
##  9 Oct    2681     8           0.00298
## 10 Aug    3168     9           0.00283
## 11 Dec    1835     4           0.00218
## 12 Nov    2539     5           0.00197
```

```r
best_month_comp <- surgery %>% 
  select(month, complication) %>% 
  group_by(month) %>% 
  count(complication) %>% 
  pivot_wider(names_from = complication, values_from = n) %>% 
  mutate(comp_probability = (Yes / (No + Yes))) %>% 
  arrange(desc(comp_probability))
best_month_comp
```

```
## # A tibble: 12 × 4
## # Groups:   month [12]
##    month    No   Yes comp_probability
##    <chr> <int> <int>            <dbl>
##  1 Jan    2263   407            0.152
##  2 Aug    2715   462            0.145
##  3 Oct    2312   377            0.140
##  4 Jun    2584   410            0.137
##  5 Feb    2163   343            0.137
##  6 Sep    2784   424            0.132
##  7 Jul    2024   301            0.129
##  8 Dec    1602   237            0.129
##  9 Nov    2219   325            0.128
## 10 May    2321   333            0.125
## 11 Mar    2373   324            0.120
## 12 Apr    2377   321            0.119
```


10. (4 points) Make a plot that visualizes the chart from question #9. Make sure that the months are on the x-axis. Do a search online and figure out how to order the months Jan-Dec.

```r
best_month_mort %>% 
  ggplot(aes(x = month, y = death_probability, fill = death_probability)) +
  geom_col() +
  scale_x_discrete(limits=month.abb) +
  labs(title = "Probability of Death by Surgery")
```

![](midterm_2_final_files/figure-html/unnamed-chunk-15-1.png)<!-- -->

```r
best_month_comp %>% 
  ggplot(aes(x = month, y = comp_probability, fill = comp_probability)) +
  geom_col() +
  scale_x_discrete(limits=month.abb) +
  labs(title = "Probability of Surgery Complications")
```

![](midterm_2_final_files/figure-html/unnamed-chunk-16-1.png)<!-- -->


Please provide the names of the students you have worked with with during the exam:

Collaborators: Emily and Tim

Please be 100% sure your exam is saved, knitted, and pushed to your github repository. No need to submit a link on canvas, we will find your exam in your repository.
