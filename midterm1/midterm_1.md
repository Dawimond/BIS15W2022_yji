---
title: "Midterm 1"
author: "Yutong Ji"
date: "2022-01-27"
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

```r
str(elephants)
```

```
## spec_tbl_df [288 x 3] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
##  $ Age   : num [1:288] 1.4 17.5 12.8 11.2 12.7 ...
##  $ Height: num [1:288] 120 227 235 210 220 ...
##  $ Sex   : chr [1:288] "M" "M" "M" "M" ...
##  - attr(*, "spec")=
##   .. cols(
##   ..   Age = col_double(),
##   ..   Height = col_double(),
##   ..   Sex = col_character()
##   .. )
##  - attr(*, "problems")=<externalptr>
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
elephants_clean %>%
  group_by(sex) %>%
  summarize(mean_age=mean(age), mean_height=mean(height))
```

```
## # A tibble: 2 x 3
##   sex   mean_age mean_height
##   <fct>    <dbl>       <dbl>
## 1 F        12.8         190.
## 2 M         8.95        185.
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
str(invindodata_clean)
```

```
## spec_tbl_df [24 x 26] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
##  $ transect_id             : num [1:24] 1 2 2 3 4 5 6 7 8 9 ...
##  $ distance                : num [1:24] 7.14 17.31 18.32 20.85 15.95 ...
##  $ hunt_cat                : chr [1:24] "Moderate" "None" "None" "None" ...
##  $ num_households          : num [1:24] 54 54 29 29 29 29 29 54 25 73 ...
##  $ land_use                : chr [1:24] "Park" "Park" "Park" "Logging" ...
##  $ veg_rich                : num [1:24] 16.7 15.8 16.9 12.4 17.1 ...
##  $ veg_stems               : num [1:24] 31.2 37.4 32.3 29.4 36 ...
##  $ veg_liana               : num [1:24] 5.78 13.25 4.75 9.78 13.25 ...
##  $ veg_dbh                 : num [1:24] 49.6 34.6 42.8 36.6 41.5 ...
##  $ veg_canopy              : num [1:24] 3.78 3.75 3.43 3.75 3.88 2.5 4 4 3 3.25 ...
##  $ veg_understory          : num [1:24] 2.89 3.88 3 2.75 3.25 3 2.38 2.71 3.25 3.13 ...
##  $ ra_apes                 : num [1:24] 1.87 0 4.49 12.93 0 ...
##  $ ra_birds                : num [1:24] 52.7 52.2 37.4 59.3 52.6 ...
##  $ ra_elephant             : num [1:24] 0 0.86 1.33 0.56 1 0 1.11 0.43 2.2 0 ...
##  $ ra_monkeys              : num [1:24] 38.6 28.5 41.8 19.9 41.3 ...
##  $ ra_rodent               : num [1:24] 4.22 6.04 1.06 3.66 2.52 1.83 3.1 1.26 4.37 6.31 ...
##  $ ra_ungulate             : num [1:24] 2.66 12.41 13.86 3.71 2.53 ...
##  $ rich_all_species        : num [1:24] 22 20 22 19 20 22 23 19 19 19 ...
##  $ evenness_all_species    : num [1:24] 0.793 0.773 0.74 0.681 0.811 0.786 0.818 0.757 0.773 0.668 ...
##  $ diversity_all_species   : num [1:24] 2.45 2.31 2.29 2.01 2.43 ...
##  $ rich_bird_species       : num [1:24] 11 10 11 8 8 10 11 11 11 9 ...
##  $ evenness_bird_species   : num [1:24] 0.732 0.704 0.688 0.559 0.799 0.771 0.801 0.687 0.784 0.573 ...
##  $ diversity_bird_species  : num [1:24] 1.76 1.62 1.65 1.16 1.66 ...
##  $ rich_mammal_species     : num [1:24] 11 10 11 11 12 12 12 8 8 10 ...
##  $ evenness_mammal_species : num [1:24] 0.736 0.705 0.65 0.619 0.736 0.694 0.776 0.79 0.821 0.783 ...
##  $ diversity_mammal_species: num [1:24] 1.76 1.62 1.56 1.48 1.83 ...
##  - attr(*, "spec")=
##   .. cols(
##   ..   TransectID = col_double(),
##   ..   Distance = col_double(),
##   ..   HuntCat = col_character(),
##   ..   NumHouseholds = col_double(),
##   ..   LandUse = col_character(),
##   ..   Veg_Rich = col_double(),
##   ..   Veg_Stems = col_double(),
##   ..   Veg_liana = col_double(),
##   ..   Veg_DBH = col_double(),
##   ..   Veg_Canopy = col_double(),
##   ..   Veg_Understory = col_double(),
##   ..   RA_Apes = col_double(),
##   ..   RA_Birds = col_double(),
##   ..   RA_Elephant = col_double(),
##   ..   RA_Monkeys = col_double(),
##   ..   RA_Rodent = col_double(),
##   ..   RA_Ungulate = col_double(),
##   ..   Rich_AllSpecies = col_double(),
##   ..   Evenness_AllSpecies = col_double(),
##   ..   Diversity_AllSpecies = col_double(),
##   ..   Rich_BirdSpecies = col_double(),
##   ..   Evenness_BirdSpecies = col_double(),
##   ..   Diversity_BirdSpecies = col_double(),
##   ..   Rich_MammalSpecies = col_double(),
##   ..   Evenness_MammalSpecies = col_double(),
##   ..   Diversity_MammalSpecies = col_double()
##   .. )
##  - attr(*, "problems")=<externalptr>
```

```r
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
