# Reproducible Research: Peer Assessment 1
## Loading and preprocessing the data

```r
data <- read.csv("activity.csv")
```
Select only complete cases

```r
cdata <- data[complete.cases(data),]
```
## What is mean total number of steps taken per day?
Create aggregate table

```r
sbd <- aggregate(cdata$steps, by=list(cdata$date), FUN=sum)
colnames(sbd) <- c("date","steps")
```
... and plot histogram:

```r
with(sbd, 
     hist(steps, main=paste('Total number of steps taken each day'), 
          xlab='Steps per day'))
```

![plot of chunk unnamed-chunk-4](./PA1_template_files/figure-html/unnamed-chunk-4.png) 

The mean number of steps taken per day is 

```r
mean(sbd$steps) 
```

```
## [1] 10766
```
and median 

```r
median(sbd$steps)
```

```
## [1] 10765
```

## What is the average daily activity pattern?
Generate aggregate table:

```r
sbi <- aggregate(cdata$steps, by=list(cdata$interval), FUN=mean)
colnames(sbi) <- c("interval", "mean steps")
```
... and plot:

```r
plot(sbi, type="l")
```

![plot of chunk unnamed-chunk-8](./PA1_template_files/figure-html/unnamed-chunk-8.png) 

The 5-min interval having the maximal number of steps: 

```r
sbi[which.max(sbi$"mean steps"),]
```

```
##     interval mean steps
## 104      835      206.2
```

## Imputing missing values

The total number of missing values:


```r
nrow(data[!complete.cases(data),])
```

```
## [1] 2304
```

Imputing mean values for each interval:


```r
data$interval <- sapply(data$interval, function(x) as.factor(x))
comp <- data
comp[!complete.cases(data$steps), "steps"] <- sapply(data[!complete.cases(data$steps), "interval"], function(x) sbi[x, "mean steps"])
```

Create aggregate table

```r
sbdc <- aggregate(comp$steps, by=list(comp$date), FUN=sum)
colnames(sbdc) <- c("date","steps")
```
... and plot histogram:

```r
with(sbdc, 
     hist(steps, main=paste('Total number of steps taken each day after imputing missing values'), 
          xlab='Steps per day'))
```

![plot of chunk unnamed-chunk-13](./PA1_template_files/figure-html/unnamed-chunk-13.png) 

The mean number of steps taken per day is 

```r
mean(sbdc$steps) 
```

```
## [1] 10766
```
and median 

```r
median(sbdc$steps)
```

```
## [1] 10766
```

## Are there differences in activity patterns between weekdays and weekends?
