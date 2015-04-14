# Reproducible Research: Peer Assessment 1


## Loading and preprocessing the data
Unzip and read the file

```r
#Turn off warnings to clean up output
options(warn = -1)
data <- read.csv(unz("activity.zip", "activity.csv"))
```
Convert factor to date using lubridate

```r
library(lubridate)
data$date <- ymd(data$date)
```

## What is mean total number of steps taken per day?
Summarize data using dplyr

```r
#Turn off messages to clean up output
suppressMessages(library(dplyr))
a <- summarize(group_by(data, date), sum(steps))
names(a) <- c("date", "steps")
```
Create histogram using ggplot2

```r
library(ggplot2)
ggplot(a, aes(steps)) + geom_histogram(binwidth = 1000) 
```

![](PA1_template_files/figure-html/unnamed-chunk-4-1.png) 

Calculate the mean and median

```r
mean(a$steps, na.rm = TRUE)
```

```
## [1] 10766.19
```

```r
median(a$steps, na.rm = TRUE)
```

```
## [1] 10765
```

## What is the average daily activity pattern?
Summarize data using dplyr

```r
b <- summarize(group_by(data, interval), mean(steps, na.rm = TRUE))
names(b) <- c("interval", "steps")
```
Create time series using ggplot2

```r
ggplot(b, aes(interval, steps)) + geom_line()
```

![](PA1_template_files/figure-html/unnamed-chunk-7-1.png) 

Find interval with maximum steps

```r
b[which(b$steps == max(b$steps)),1]
```

```
## Source: local data frame [1 x 1]
## 
##   interval
## 1      835
```

## Imputing missing values
Calculate number of missing values

```r
sum(!complete.cases(data))
```

```
## [1] 2304
```

## Are there differences in activity patterns between weekdays and weekends?
