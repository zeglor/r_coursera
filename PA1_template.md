# Reproducible Research: Peer Assessment 1


## Loading and preprocessing the data

```r
steps_data_raw <- read.csv("activity.csv")
steps_data_raw$date <- as.Date(steps_data_raw$date)
steps_data_daily <- aggregate(steps ~ date, steps_data_raw, sum)
steps_data_interval <- aggregate(steps ~ interval, steps_data_raw, mean)
```


## What is mean total number of steps taken per day?

```r
hist(aggregate(steps ~ date, steps_data_raw, sum)$steps, col = "green")
```

![plot of chunk unnamed-chunk-2](figure/unnamed-chunk-2.png) 



```r
steps_mean <- mean(steps_data_daily$steps)
steps_median <- median(steps_data_daily$steps)
```


The average of steps taken each day is *1.0766 &times; 10<sup>4</sup>*  
The median of steps taken each day is *10765*

## What is the average daily activity pattern?

```r
plot(x = steps_data_interval$interval, y = steps_data_interval$steps, type = "l")
```

![plot of chunk unnamed-chunk-4](figure/unnamed-chunk-4.png) 



## Imputing missing values



## Are there differences in activity patterns between weekdays and weekends?
