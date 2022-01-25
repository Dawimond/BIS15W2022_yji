---
title: "Midterm 1"
author: "Yutong Ji"
date: "2022-01-25"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your code should be organized, clean, and run free from errors. Remember, you must remove the `#` for any included code chunks to run. Be sure to add your name to the author header above. You may use any resources to answer these questions (including each other), but you may not post questions to Open Stacks or external help sites. There are 15 total questions, each is worth 2 points.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

This exam is due by 12:00p on Thursday, January 27.  

## Load the tidyverse
If you plan to use any other libraries to complete this assignment then you should load them here.

```r
library(tidyverse)
```

```
## -- Attaching packages --------------------------------------- tidyverse 1.3.1 --
```

```
## v ggplot2 3.3.5     v purrr   0.3.4
## v tibble  3.1.6     v dplyr   1.0.7
## v tidyr   1.1.4     v stringr 1.4.0
## v readr   2.1.1     v forcats 0.5.1
```

```
## -- Conflicts ------------------------------------------ tidyverse_conflicts() --
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```

```r
library(janitor)
```

```
## 
## Attaching package: 'janitor'
```

```
## The following objects are masked from 'package:stats':
## 
##     chisq.test, fisher.test
```

## Questions  
Wikipedia's definition of [data science](https://en.wikipedia.org/wiki/Data_science): "Data science is an interdisciplinary field that uses scientific methods, processes, algorithms and systems to extract knowledge and insights from noisy, structured and unstructured data, and apply knowledge and actionable insights from data across a broad range of application domains."  

1. (2 points) Consider the definition of data science above. Although we are only part-way through the quarter, what specific elements of data science do you feel we have practiced? Provide at least one specific example.  
**We have learned how to "extract knowledge and insights from noisy, structured and unstructured data". Commands like "summary", "str", "skimr" all provide different types of information insights on the contents and structures, such as mean, medium and mode, of the data sets we are working with. There are also functions like "na_if" and "clean_names" to quickly modify the data sets by setting readable NAs and changing variable names and make them ready to be read and analyzed by R correctly.**

2. (2 points) What is the most helpful or interesting thing you have learned so far in BIS 15L? What is something that you think needs more work or practice?  
**The most interesting thing I learned in general is how R language is very organized ,if not strict, in terms of the codes and coding sequences than I initially imagined. Sometimes commands must be put in a specific order and connected in a specific way in order to get the results intended.**

In the midterm 1 folder there is a second folder called `data`. Inside the `data` folder, there is a .csv file called `ElephantsMF`. These data are from Phyllis Lee, Stirling University, and are related to Lee, P., et al. (2013), "Enduring consequences of early experiences: 40-year effects on survival and success among African elephants (Loxodonta africana)," Biology Letters, 9: 20130011. [kaggle](https://www.kaggle.com/mostafaelseidy/elephantsmf).  

3. (2 points) Please load these data as a new object called `elephants`. Use the function(s) of your choice to get an idea of the structure of the data. Be sure to show the class of each variable.

```r
elephants <- readr::read_csv(file = "data/ElephantsMF.csv")
```

```
## Rows: 288 Columns: 3
```

```
## -- Column specification --------------------------------------------------------
## Delimiter: ","
## chr (1): Sex
## dbl (2): Age, Height
```

```
## 
## i Use `spec()` to retrieve the full column specification for this data.
## i Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

4. (2 points) Change the names of the variables to lower case and change the class of the variable `sex` to a factor.

```r
elephants_clean <- clean_names(elephants)
elephants_clean$sex <- as.factor(elephants_clean$sex)
```

5. (2 points) How many male and female elephants are represented in the data?

```r
elephants_clean %>%
  count(sex)
```

```
## # A tibble: 2 x 2
##   sex       n
##   <fct> <int>
## 1 F       150
## 2 M       138
```

6. (2 points) What is the average age all elephants in the data?

```r
mean(elephants_clean$age)
```

```
## [1] 10.97132
```

7. (2 points) How does the average age and height of elephants compare by sex?

```r
m_elephants <- elephants_clean %>%
  filter(sex=="M")
mean(m_elephants$age)
```

```
## [1] 8.945145
```

```r
mean(m_elephants$height)
```

```
## [1] 185.1312
```

```r
f_elephants <- elephants_clean %>%
  filter(sex=="F")
mean(f_elephants$age)
```

```
## [1] 12.8354
```

```r
mean(f_elephants$height)
```

```
## [1] 190.0307
```

8. (2 points) How does the average height of elephants compare by sex for individuals over 20 years old. Include the min and max height as well as the number of individuals in the sample as part of your analysis.  

```r
elephants_clean %>%
  filter(age>20) %>%
  group_by(sex) %>%
  summarize(mean_height=mean(height), min_height=min(height), max_height=max(height), n=length(sex))
```

```
## # A tibble: 2 x 5
##   sex   mean_height min_height max_height     n
##   <fct>       <dbl>      <dbl>      <dbl> <int>
## 1 F            232.       193.       278.    37
## 2 M            270.       229.       304.    13
```

For the next series of questions, we will use data from a study on vertebrate community composition and impacts from defaunation in [Gabon, Africa](https://en.wikipedia.org/wiki/Gabon). One thing to notice is that the data include 24 separate transects. Each transect represents a path through different forest management areas.  

Reference: Koerner SE, Poulsen JR, Blanchard EJ, Okouyi J, Clark CJ. Vertebrate community composition and diversity declines along a defaunation gradient radiating from rural villages in Gabon. _Journal of Applied Ecology_. 2016. This paper, along with a description of the variables is included inside the midterm 1 folder.  

9. (2 points) Load `IvindoData_DryadVersion.csv` and use the function(s) of your choice to get an idea of the overall structure. Change the variables `HuntCat` and `LandUse` to factors.

```r
invindodata <- readr::read_csv("data/IvindoData_DryadVersion.csv")
```

```
## Rows: 24 Columns: 26
```

```
## -- Column specification --------------------------------------------------------
## Delimiter: ","
## chr  (2): HuntCat, LandUse
## dbl (24): TransectID, Distance, NumHouseholds, Veg_Rich, Veg_Stems, Veg_lian...
```

```
## 
## i Use `spec()` to retrieve the full column specification for this data.
## i Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
invindodata_clean <- clean_names(invindodata)
invindodata_clean$hunt_cat <- as.factor(invindodata_clean$hunt_cat)
invindodata_clean$land_use <- as.factor(invindodata_clean$land_use)
```

10. (4 points) For the transects with high and moderate hunting intensity, how does the average diversity of birds and mammals compare?

```r
invindodata_clean %>%
  filter(hunt_cat=="High" | hunt_cat=="Moderate") %>%
  group_by(hunt_cat) %>%
  summarize(mean_div_birds=mean(diversity_bird_species), mean_div_mammals=mean(diversity_mammal_species))
```

```
## # A tibble: 2 x 3
##   hunt_cat mean_div_birds mean_div_mammals
##   <fct>             <dbl>            <dbl>
## 1 High               1.66             1.74
## 2 Moderate           1.62             1.68
```

11. (4 points) One of the conclusions in the study is that the relative abundance of animals drops off the closer you get to a village. Let's try to reconstruct this (without the statistics). How does the relative abundance (RA) of apes, birds, elephants, monkeys, rodents, and ungulates compare between sites that are less than 3km from a village to sites that are greater than 25km from a village? The variable `Distance` measures the distance of the transect from the nearest village. Hint: try using the `across` operator.  

```r
invindodata_clean %>%
  filter(distance<=3 | distance>=25) %>%
  select(transect_id, distance, ra_apes:ra_ungulate) %>%
  arrange(distance)
```

```
## # A tibble: 3 x 8
##   transect_id distance ra_apes ra_birds ra_elephant ra_monkeys ra_rodent
##         <dbl>    <dbl>   <dbl>    <dbl>       <dbl>      <dbl>     <dbl>
## 1          15     2.7     0        85.0        0.29       9.09      3.74
## 2          27     2.92    0.24     68.2        0         25.6       4.05
## 3          24    26.8     4.91     31.6        0         54.1       1.29
## # ... with 1 more variable: ra_ungulate <dbl>
```

12. (4 points) Based on your interest, do one exploratory analysis on the `gabon` data of your choice. This analysis needs to include a minimum of two functions in `dplyr.`

**I chose to analyze the min, max, and average diversity of all species for transects that are used for logging **

```r
invindodata_clean %>%
  filter(land_use=="Logging") %>%
  group_by(transect_id) %>%
  summarize(min_div_all=min(diversity_all_species), max_div_all=max(diversity_all_species), mean_div_all=mean(diversity_all_species))
```

```
## # A tibble: 13 x 4
##    transect_id min_div_all max_div_all mean_div_all
##          <dbl>       <dbl>       <dbl>        <dbl>
##  1           3        2.01        2.01         2.01
##  2           7        2.23        2.23         2.23
##  3           9        1.97        1.97         1.97
##  4          13        2.27        2.27         2.27
##  5          14        2.16        2.16         2.16
##  6          16        2.13        2.13         2.13
##  7          18        2.36        2.36         2.36
##  8          19        2.19        2.19         2.19
##  9          20        2.27        2.27         2.27
## 10          22        2.37        2.37         2.37
## 11          25        2.32        2.32         2.32
## 12          26        2.37        2.37         2.37
## 13          27        2.38        2.38         2.38
```
