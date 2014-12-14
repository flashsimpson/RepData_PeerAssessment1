# Reproducible Research: Peer Assessment 1

```
## 
## Attaching package: 'dplyr'
## 
## The following object is masked from 'package:stats':
## 
##     filter
## 
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
## 
## -------------------------------------------------------------------------
## You have loaded plyr after dplyr - this is likely to cause problems.
## If you need functions from both plyr and dplyr, please load plyr first, then dplyr:
## library(plyr); library(dplyr)
## -------------------------------------------------------------------------
## 
## Attaching package: 'plyr'
## 
## The following objects are masked from 'package:dplyr':
## 
##     arrange, count, desc, failwith, id, mutate, rename, summarise,
##     summarize
```
## Loading and preprocessing the data

```r
data<- read.csv("activity.csv")
dates<- group_by(data, date)
```
## What is mean total number of steps taken per day?

```r
hist(data$steps, breaks= 50, main="Frequency of Steps Histogram", xlab="Number of Daily Steps")
```

![plot of chunk unnamed-chunk-3](./PA1_template_files/figure-html/unnamed-chunk-3.png) 

```r
stmean<- mean(data$steps, na.rm=T)
stmean
```

```
## [1] 37.38
```

```r
stmedian<- median(data$steps, na.rm=T)
stmedian
```

```
## [1] 0
```
## What is the average daily activity pattern?

```r
dates<- group_by(data, interval)
intsum<- summarise(dates, mean=mean(steps, na.rm=T), median=median(steps, na.rm=T))
```
##```{r}
qplot(data=intsum, x=interval, y=mean,  geom="line",main="Mean Steps by Daily Interval", xlab="Daily Interval", ylab="Mean Number of Steps")
abline(v=835, col="red")
intsum$interval[which.max(intsum$mean)]
##```

## Imputing missing values
### Number of missing values

```r
Nas<-sum(is.na(data$steps))
Nas
```

```
## [1] 2304
```
### Impute NA value based on mean value for that interval

```r
impute.mean <- function(x) replace(x, is.na(x), mean(x, na.rm = TRUE))
imp1 <- ddply(data, ~ interval, transform, steps = impute.mean(steps))

hist(imp1$steps, breaks= 50, main="Frequency of Steps Histogram", xlab="Number of Daily Steps")
```

![plot of chunk unnamed-chunk-6](./PA1_template_files/figure-html/unnamed-chunk-6.png) 

```r
stmean1<- mean(imp1$steps, na.rm=T)
stmean1
```

```
## [1] 37.38
```

```r
stmedian1<- median(imp1$steps, na.rm=T)
stmedian1
```

```
## [1] 0
```
## Are there differences in activity patterns between weekdays and weekends?

