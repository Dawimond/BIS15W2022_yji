---
title: "Lab 11 Homework"
author: "Yutong Ji"
date: "2022-02-12"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above. For any included plots, make sure they are clearly labeled. You are free to use any plot type that you feel best communicates the results of your analysis.  

**In this homework, you should make use of the aesthetics you have learned. It's OK to be flashy!**

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the libraries

```r
library(tidyverse)
library(janitor)
library(here)
library(naniar)
```

## Resources
The idea for this assignment came from [Rebecca Barter's](http://www.rebeccabarter.com/blog/2017-11-17-ggplot2_tutorial/) ggplot tutorial so if you get stuck this is a good place to have a look.  

## Gapminder
For this assignment, we are going to use the dataset [gapminder](https://cran.r-project.org/web/packages/gapminder/index.html). Gapminder includes information about economics, population, and life expectancy from countries all over the world. You will need to install it before use. This is the same data that we will use for midterm 2 so this is good practice.

```r
library("gapminder")
```

## Questions
The questions below are open-ended and have many possible solutions. Your approach should, where appropriate, include numerical summaries and visuals. Be creative; assume you are building an analysis that you would ultimately present to an audience of stakeholders. Feel free to try out different `geoms` if they more clearly present your results.  

**1. Use the function(s) of your choice to get an idea of the overall structure of the data frame, including its dimensions, column names, variable classes, etc. As part of this, determine how NA's are treated in the data.**  

```r
gapminder_clean <- clean_names(gapminder)
```


```r
summary(gapminder)
```

```
##         country        continent        year         lifeExp     
##  Afghanistan:  12   Africa  :624   Min.   :1952   Min.   :23.60  
##  Albania    :  12   Americas:300   1st Qu.:1966   1st Qu.:48.20  
##  Algeria    :  12   Asia    :396   Median :1980   Median :60.71  
##  Angola     :  12   Europe  :360   Mean   :1980   Mean   :59.47  
##  Argentina  :  12   Oceania : 24   3rd Qu.:1993   3rd Qu.:70.85  
##  Australia  :  12                  Max.   :2007   Max.   :82.60  
##  (Other)    :1632                                                
##       pop              gdpPercap       
##  Min.   :6.001e+04   Min.   :   241.2  
##  1st Qu.:2.794e+06   1st Qu.:  1202.1  
##  Median :7.024e+06   Median :  3531.8  
##  Mean   :2.960e+07   Mean   :  7215.3  
##  3rd Qu.:1.959e+07   3rd Qu.:  9325.5  
##  Max.   :1.319e+09   Max.   :113523.1  
## 
```

```r
glimpse(gapminder)
```

```
## Rows: 1,704
## Columns: 6
## $ country   <fct> "Afghanistan", "Afghanistan", "Afghanistan", "Afghanistan", ~
## $ continent <fct> Asia, Asia, Asia, Asia, Asia, Asia, Asia, Asia, Asia, Asia, ~
## $ year      <int> 1952, 1957, 1962, 1967, 1972, 1977, 1982, 1987, 1992, 1997, ~
## $ lifeExp   <dbl> 28.801, 30.332, 31.997, 34.020, 36.088, 38.438, 39.854, 40.8~
## $ pop       <int> 8425333, 9240934, 10267083, 11537966, 13079460, 14880372, 12~
## $ gdpPercap <dbl> 779.4453, 820.8530, 853.1007, 836.1971, 739.9811, 786.1134, ~
```

```r
options(scipen=999)
```

**2. Among the interesting variables in gapminder is life expectancy. How has global life expectancy changed between 1952 and 2007?**

```r
gapminder_clean %>% 
  filter(between(year, 1952, 2007)) %>%
  group_by(year)%>%
  summarise(mean_life_exp=mean(life_exp))%>%
  ggplot(aes(x=year, y=mean_life_exp))+
  scale_y_log10()+
  geom_line()+
  geom_point(shape=10)+
  theme(axis.text.x = element_text(angle = 60, hjust = 1))+
  labs(title = "Global Life Expectancy per Year",
       x = "Year",
       y = "Life Expectancy")
```

![](lab11_hw_files/figure-html/unnamed-chunk-7-1.png)<!-- -->

**3. How do the distributions of life expectancy compare for the years 1952 and 2007?**

```r
gapminder_clean %>% 
  filter(between(year, 1952, 2007)) %>%
  group_by(year)%>%
  ggplot(aes(x= life_exp)) +
  geom_histogram(alpha = 0.4, color = "black", fill = "thistle3", bins=80)+
  labs(title = "Distribution of Life Expectancy", 
       x = "Life Expextancy", 
       y = "n")
```

![](lab11_hw_files/figure-html/unnamed-chunk-8-1.png)<!-- -->

**4. Your answer above doesn't tell the whole story since life expectancy varies by region. Make a summary that shows the min, mean, and max life expectancy by continent for all years represented in the data.**

```r
gapminder_clean %>%
  group_by(continent) %>%
  summarise(min_life_expectancy=min(life_exp),
            max_life_expectancy=max(life_exp), 
            mean_life_expectancy=mean(life_exp))
```

```
## # A tibble: 5 x 4
##   continent min_life_expectancy max_life_expectancy mean_life_expectancy
##   <fct>                   <dbl>               <dbl>                <dbl>
## 1 Africa                   23.6                76.4                 48.9
## 2 Americas                 37.6                80.7                 64.7
## 3 Asia                     28.8                82.6                 60.1
## 4 Europe                   43.6                81.8                 71.9
## 5 Oceania                  69.1                81.2                 74.3
```

**5. How has life expectancy changed between 1952-2007 for each continent?**

```r
gapminder_clean %>% 
  filter(between(year, 1952, 2007)) %>%
  group_by(year, continent)%>%
  summarise(mean_life_exp=mean(life_exp))%>%
  ggplot(aes(x=year, y=mean_life_exp, group=continent, fill=continent, color=continent))+
  geom_line()+
  geom_point(shape=10)+
  labs(title = "Life Expectancy per Year by Continent",
       x = "Year",
       y = "Life Expectancy")
```

```
## `summarise()` has grouped output by 'year'. You can override using the `.groups` argument.
```

![](lab11_hw_files/figure-html/unnamed-chunk-10-1.png)<!-- -->

**6. We are interested in the relationship between per capita GDP and life expectancy; i.e. does having more money help you live longer?**

```r
gapminder_clean %>%
  ggplot(aes(x= gdp_percap, y=life_exp))+
  geom_point(size=0.5)+
  theme(axis.text.x = element_text(hjust = 1))+
  scale_y_log10()+
  labs(title = "Per Capita by Life Expectancy",
       x = "Per Capita GDP",
       y = "Life Expectancy")
```

![](lab11_hw_files/figure-html/unnamed-chunk-11-1.png)<!-- -->

**7. Which countries have had the largest population growth since 1952?**

```r
gapminder_clean %>%
  filter(year=="1952"|year=="2007")%>%
  group_by(country) %>%
  summarise(pop_growth= max(pop)-min(pop))%>%
  arrange(desc(pop_growth))
```

```
## # A tibble: 142 x 2
##    country       pop_growth
##    <fct>              <int>
##  1 China          762419569
##  2 India          738396331
##  3 United States  143586947
##  4 Indonesia      141495000
##  5 Brazil         133408087
##  6 Pakistan       127924057
##  7 Bangladesh     103561480
##  8 Nigeria        101912068
##  9 Mexico          78556574
## 10 Philippines     68638596
## # ... with 132 more rows
```

**8. Use your results from the question above to plot population growth for the top five countries since 1952.**

```r
gapminder_clean %>%
  filter(year=="1952"|year=="2007")%>%
  group_by(country) %>%
  mutate(pop_growth= max(pop)-min(pop))%>%
  arrange(desc(pop_growth))%>%
  head(pop_growth, n=10)%>%
  ggplot(aes(x=year, y=pop, group=country, color=country))+
  geom_line()+
  geom_point(size=1)+
  scale_y_log10()+
  theme(axis.text.x = element_text(hjust = 1))+
  labs(title = "Top Five Country Population Growth",
       x = "Year",
       y = "Population Growth")
```

![](lab11_hw_files/figure-html/unnamed-chunk-13-1.png)<!-- -->

**9. How does per-capita GDP growth compare between these same five countries?**

```r
gapminder_clean %>%
  filter(year=="1952"|year=="2007")%>%
  group_by(country) %>%
  mutate(pop_growth= max(pop)-min(pop))%>%
  arrange(desc(pop_growth))%>%
  head(pop_growth, n=10)%>%
  ggplot(aes(x=year, y=gdp_percap, group=country, color=country))+
  geom_line()+
  geom_point(size=1)+
  scale_y_log10()+
  theme(axis.text.x = element_text(hjust = 1))+
  labs(title = "Top Five Country GDP Growth",
       x = "Year",
       y = "GDP Growth")
```

![](lab11_hw_files/figure-html/unnamed-chunk-14-1.png)<!-- -->

**10. Make one plot of your choice that uses faceting!**

```r
p <- gapminder_clean %>% 
  ggplot(aes(x = life_exp)) +
  geom_histogram(bins = 60)
p + facet_wrap(~continent)
```

![](lab11_hw_files/figure-html/unnamed-chunk-15-1.png)<!-- -->

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences. 
