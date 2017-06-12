
```r
library(reshape2)
library(ggplot2)
library(dplyr)
library(tidyr)
```

## 1.Code for reading in the dataset and/or processing the data

```r
activity.data <- read.csv("activity.csv")
```
## 2.Histogram of the total number of steps taken each day

```r
daily.sum <- aggregate(steps~date, data = activity.data, sum)
hist(daily.sum$steps)
```

![plot of chunk daysum](figure/daysum-1.png)

## 3.Mean and median number of steps taken each day

```r
daily.mean <- aggregate(steps~date, data = activity.data, mean)
median(daily.sum$steps)
```

```
## [1] 10765
```

```r
mean(daily.sum$steps)
```

```
## [1] 10766
```

## 4.Time series plot of the average number of steps taken


```r
plot(daily.mean, type = "l", main = "Average number of steps taken per day")
lines(daily.mean)
```

![plot of chunk dayseries](figure/dayseries-1.png)

## 5.The 5-minute interval that, on average, contains the maximum number of steps
interval.mean <- aggregate(steps~interval, data = activity.data, mean)

```r
interval.mean[which.max(interval.mean$steps), "interval"]
```

```
## [1] 835
```

## 6.Code to describe and show a strategy for imputing missing data

```r
activity.complete <- activity.data[complete.cases(activity.data$steps),]
head(activity.complete)
```

```
##     steps       date interval
## 289     0 2012-10-02        0
## 290     0 2012-10-02        5
## 291     0 2012-10-02       10
## 292     0 2012-10-02       15
## 293     0 2012-10-02       20
## 294     0 2012-10-02       25
```

## 7.Histogram of the total number of steps taken each day after missing values are imputed

```r
daily.sum.complete <- aggregate(steps~date, data = activity.complete, sum)
hist(daily.sum.complete$steps, main = "Total number of steps taken each day", xlab = "Steps per day")
```

![plot of chunk fullstep](figure/fullstep-1.png)

## 8.Panel plot comparing the average number of steps taken per 5-minute interval across weekdays and weekends


```r
weekday.activity <- group_by(activity.complete, weekdays(as.Date(activity.complete$date)))
names(weekday.activity) <- c("steps", "date", "interval", "weekday")
within.week <- weekday.activity[-grep("sábado|domingo",weekday.activity$weekday),]
weekend <- weekday.activity[grep("sábado|domingo",weekday.activity$weekday),]

week.interval.mean <- aggregate(steps~interval, data = within.week, mean)
weekend.interval.mean <- aggregate(steps~interval, data = weekend, mean)

par(mfrow = c(2,1))
plot(week.interval.mean$steps~week.interval.mean$interval, type = "l", main = "Weekday avarage", xlab = "Interval", ylab = "Avarage steps")
plot(weekend.interval.mean$steps~weekend.interval.mean$interval, type = "l",main = "Weekend avarage", xlab = "Interval", ylab = "Avarage steps")
```

![plot of chunk panel](figure/panel-1.png)



