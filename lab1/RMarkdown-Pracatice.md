---
title: "RMarkdown Practice"
author: "Yutong Ji"
date: "1/6/2022"
output: 
  html_document: 
    keep_md: yes
---




```r
#install.packages("tidyverse")
library("tidyverse")
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
ggplot(mtcars, aes(x = factor(cyl))) +
    geom_bar()
```

![](RMarkdown-Pracatice_files/figure-html/unnamed-chunk-2-1.png)<!-- -->
