# Reproducible Research: Peer Assessment 1


## Loading and preprocessing the data
steps_data_raw <- read.csv("activity.csv")
steps_data_raw$date <- as.Date(steps_data_raw$date)
class(steps_data_raw$date)

aggregate(steps~date, steps_data_raw, median)


## What is mean total number of steps taken per day?



## What is the average daily activity pattern?



## Imputing missing values



## Are there differences in activity patterns between weekdays and weekends?
