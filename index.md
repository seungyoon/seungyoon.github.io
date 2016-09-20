---
title       : New York Air Quality Measurements
subtitle    : coursera data product course project
author      : Seungyoon Lee
job         : 
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : []            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
runtime     : shiny 
---

## basic concept

- the built-in [R] airquality data set has Ozone data with Solar, Wind and Temperatere From May 1, 1973 to Sep 30, 1973.
- the Shiny app will load that data set and plot the Ozone data per Wind or Temperature, and provide the trend line of the corrected data by the selection.
- Selecting the variable from left drop box will pick the x-axis to draw Ozone value per the selection.
 - selecting Wind from the left drop box will give plot of Ozone ~ Wind
 - seleting Temperate from the left drop box will plot of Ozone ~ Temp
- Checking the box for trend will draw trend line on top of above plot

--- .class #id 

## airqualiry data set

Daily air quality measurements in New York, May to September 1973. A built-in data frame with 154 observations on 6 variables. Here are the few of the data.


```r
head(airquality)
```

```
##   Ozone Solar.R Wind Temp Month Day
## 1    41     190  7.4   67     5   1
## 2    36     118  8.0   72     5   2
## 3    12     149 12.6   74     5   3
## 4    18     313 11.5   62     5   4
## 5    NA      NA 14.3   56     5   5
## 6    28      NA 14.9   66     5   6
```


---

## Ui.R

- Ui.R has four elements, title, plotly output which calculated and updated from Server.R, two user inputs - drop box for x-axis and check box for trend line and text output for brief descriptio of the app.

--- 

##  Server.R

- Server.R will perform the data set calculation based on user selection, for example, below cleaned up data set for Temperature case will be generated for its plotting.

```r
temp <- airquality[, c("Temp", "Ozone")]
completeDf <- temp[complete.cases(temp),] ## get data set with removal of NA
meanDf <- aggregate(completeDf[,2], list(completeDf$Temp), mean) ## get mean by Temp
freqDf <- as.data.frame(table(completeDf$Temp)) ## get frequency of Temp
cdf <- merge(meanDf, freqDf, by.x="Group.1", by.y="Var1") ## merge
colnames(cdf) <- c("Temp", "Ozone", "Freq")
head(cdf, n=5) 
```

```
##   Temp    Ozone Freq
## 1   57  6.00000    1
## 2   58 18.00000    1
## 3   59 10.00000    2
## 4   61 14.66667    3
## 5   62 14.50000    2
```

