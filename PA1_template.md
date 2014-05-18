# Reproducible Research: Peer Assessment 1


## Loading and preprocessing the data
First we read given .csv file and convert column

```r
steps_data_raw <- read.csv("activity.csv")
steps_data_raw$date <- as.Date(steps_data_raw$date)
steps_data_daily <- aggregate(steps ~ date, steps_data_raw, sum)
steps_data_mean_interval <- aggregate(steps ~ interval, steps_data_raw, mean)
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
plot(x = steps_data_mean_interval$interval, y = steps_data_mean_interval$steps, 
    type = "l")
```

![plot of chunk unnamed-chunk-4](figure/unnamed-chunk-4.png) 


To find 5-minute interval containing maximum average number of steps, let's first find out the maximum average value of steps. Then we can find value of `interval` variable corresponding to max steps value in data set.

```r
max_steps_value <- max(steps_data_mean_interval$steps)
max_steps_interval <- steps_data_mean_interval[steps_data_mean_interval$steps == 
    max_steps_value, ]$interval
```


Index of 5-minute interval, which contains maximum number of steps average across whole dataset, is *835*

## Imputing missing values

```r
num_missing_values <- sum(!complete.cases(steps_data_raw))
```

Total number of rows containing missing values is *2304*


```r
# steps_data_clean is the frame containing approximated na values
steps_data_clean <- steps_data_raw
steps_data_clean$date <- as.Date(steps_data_clean$date)

steps_data_daily_mean <- aggregate(steps ~ date, steps_data_raw, mean)

for (date in unique(steps_data_raw$date)) {
    steps_data_raw_date <- steps_data_raw[steps_data_raw$date == date, ]
    # if we have na values on this date
    if (sum(is.na(steps_data_raw_date$steps)) > 0) {
        # if we don't have mean value for this date, replace na's with 0
        if (nrow(steps_data_daily_mean[steps_data_daily_mean$date == date, ]) == 
            0) {
            steps_data_clean[steps_data_raw$date == date & is.na(steps_data_raw$steps), 
                ]$steps <- 0
        } else {
            # if we do have a mean steps value for this date, replace all NAs with this
            # mean steps value
            steps_data_clean[steps_data_raw$date == date & is.na(steps_data_raw$steps), 
                ]$steps <- steps_data_daily_mean[steps_data_daily_mean$date == 
                date, "steps"]
        }
    }
}

steps_data_daily_clean <- aggregate(steps ~ date, steps_data_clean, sum)
```



```r
hist(aggregate(steps ~ date, steps_data_clean, sum)$steps, col = "green")
```

![plot of chunk unnamed-chunk-8](figure/unnamed-chunk-8.png) 



```r
steps_mean_clean <- mean(steps_data_daily_clean$steps)
steps_median_clean <- median(steps_data_daily_clean$steps)
```


The average of steps taken each day in clean frame is *9354.2295*  
The median of steps taken each day in clean frame is *1.0395 &times; 10<sup>4</sup>*

As we can see, mean and median are shifted to 0. Also, histogram shows that there are more 0 values than in frame containing NAs. This indicates that most of NAs were replaced by 0 values.

## Are there differences in activity patterns between weekdays and weekends?

```r
steps_data_clean$weekend <- ifelse(weekdays(steps_data_clean$date) %in% c("воскресенье", 
    "суббота"), "weekend", "weekday")
```



```r
par(mfrow = c(2, 1))
steps_data_weekday <- aggregate(steps ~ interval, subset(steps_data_clean, weekend == 
    "weekday"), mean)
steps_data_weekend <- aggregate(steps ~ interval, subset(steps_data_clean, weekend == 
    "weekend"), mean)
with(steps_data_weekday, plot(interval, steps, main = "Weekday", type = "l"))
with(steps_data_weekend, plot(interval, steps, main = "Weekend", type = "l"))
```

![plot of chunk unnamed-chunk-11](figure/unnamed-chunk-11.png) 

