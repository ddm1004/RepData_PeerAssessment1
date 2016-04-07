# Reproducible Research: Peer Assessment 1


## Loading and preprocessing the data

```r
if (!file.exists("activity.csv")) unzip("activity.zip")
activity <- read.csv("activity.csv")
activity$interval <- as.factor(activity$interval)
activity$date2 <- as.Date(activity$date)
```
## What is mean total number of steps taken per day?

```r
total.steps.per.day <- with(activity, tapply(steps, date, sum, na.rm=TRUE))
hist(total.steps.per.day)
```

![](PA1_template_files/figure/Code Chunk 2-1.png)

```r
print(paste("Mean: ",mean(total.steps.per.day)))
```

```
## [1] "Mean:  9354.22950819672"
```

```r
print(paste("Median: ",median(total.steps.per.day)))
```

```
## [1] "Median:  10395"
```
## What is the average daily activity pattern?

```r
mean.per.interval <- with(activity, tapply(steps, interval, mean, na.rm=TRUE))
plot(names(mean.per.interval), mean.per.interval, type="l")
```

![](PA1_template_files/figure/Code Chunk 3-1.png)

```r
print(paste("Interval of Most activity: ", names(mean.per.interval[max(mean.per.interval)])))
```

```
## [1] "Interval of Most activity:  1705"
```
## Imputing missing values

```r
print(paste("Number of missing values",sum(is.na(activity$steps))))
```

```
## [1] "Number of missing values 2304"
```
# Replace NAs with mean for that interval

```r
activity2 <- activity
activity2$steps[is.na(activity2$steps)] <- mean.per.interval[activity2$interval[is.na(activity2$steps)]]
total.steps.per.day2 <- with(activity2, tapply(steps, date, sum))
hist(total.steps.per.day2)
```

![](PA1_template_files/figure/Code Chunk 5-1.png)

```r
print(paste("Mean: ", mean(total.steps.per.day)))
```

```
## [1] "Mean:  9354.22950819672"
```

```r
print(paste("Median: ", median(total.steps.per.day)))
```

```
## [1] "Median:  10395"
```

```r
print("Inserting the missing values changed the mean and the median")
```

```
## [1] "Inserting the missing values changed the mean and the median"
```

```r
print("Both the Mean and the Median increased with inserted missing values")
```

```
## [1] "Both the Mean and the Median increased with inserted missing values"
```

## Are there differences in activity patterns between weekdays and weekends?

```r
weekend <- c("Saturday","Sunday")
activity2$weekday <- factor(weekdays(activity2$date2)%in%weekend,
                            levels=c(TRUE,FALSE), 
                            labels=c("weekend","weekday"))
par(mfrow=c(1,2))
```
# Get weeday average per interval

```r
weekday.mean.per.interval <- with(activity2[activity2$weekday=="weekday",],
                                            tapply(steps, 
                                                   interval, 
                                                   mean))
plot(names(weekday.mean.per.interval), 
     weekday.mean.per.interval, 
     type="l",
     xlab="Weekday Mean Per Interval")
```

![](PA1_template_files/figure/Code Chunk 7-1.png)
# Get weekend average per interval

```r
weekend.mean.per.interval <- with(activity2[activity2$weekday=="weekend",], 
                                            tapply(steps, 
                                                   interval, 
                                                   mean))
plot(names(weekend.mean.per.interval), 
     weekend.mean.per.interval, 
     type="l",
     xlab="Weekend Mean Per Interval")
```

![](PA1_template_files/figure/Code Chunk 8-1.png)
