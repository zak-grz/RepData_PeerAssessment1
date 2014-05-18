# Reproducible Research : Peer Assesment 1
========================================================

Report from Reproducible Research for Coursera Data Science specialization.

## Loading and preprocessing the data

```r
data <- read.csv("activity.csv")
data_correction <- data
nb_of_rows <- nrow(data)
tf <- complete.cases(data)
data <- data[tf, ]
nb_of_rows_complete <- nrow(data)
sum_by_day <- by(data$steps, data$date, sum)
```



```r
hist(sum_by_day, 61, xlab = "total number of steps", main = "Total number of steps")
```

![plot of chunk unnamed-chunk-2](figure/unnamed-chunk-2.png) 

```r
a <- as.numeric(sum_by_day)
complete_cases_sumbyday <- a[complete.cases(a)]

```

Median is equal:

```r
median(complete_cases_sumbyday)
```

```
## [1] 10765
```

## What is mean total number of steps taken per day?



Mean is equal

```r
mean(complete_cases_sumbyday)
```

```
## [1] 10766
```

```r

avg_of_steps <- by(data$steps, data$interval, mean)
intervals <- unique(data[, 3])
```

## What is the average daily activity pattern?

```r
plot(intervals, avg_of_steps, type = "l")
```

![plot of chunk unnamed-chunk-5](figure/unnamed-chunk-5.png) 

Maximum number of avg steps is in interval

```r
intervals[which.max(as.numeric(avg_of_steps))]
```

```
## [1] 835
```

Meaning from 8:35 - 8:40

Following gives number of incomplete inputs:

```r
nb_of_rows - nb_of_rows_complete
```

```
## [1] 2304
```






## Imputing missing values


```r
for (i in 1:288) {
    
    
    tf <- (is.na(data_correction[, 1]) & data_correction[, 3] == intervals[i])
    data_correction[tf, 1] <- as.numeric(avg_of_steps[i])
}


sum_by_day_corrected <- by(data_correction$steps, data_correction$date, sum)
hist(sum_by_day, 61, xlab = "total number of steps [corrected data]", main = "Total number of steps [corrected data]")
```

![plot of chunk unnamed-chunk-8](figure/unnamed-chunk-8.png) 


Little change was done when missing values was input:

```r
median(sum_by_day_corrected)
```

```
## 2012-11-04 
##      10766
```

```r
mean(sum_by_day_corrected)
```

```
## [1] 10766
```

```r


data_correction["week_factor"] <- "NA"
tf <- (weekdays(as.Date(data_correction[, 2])) == "poniedzia³ek" | weekdays(as.Date(data_correction[, 
    2])) == "wtorek" | weekdays(as.Date(data_correction[, 2])) == "œroda" | 
    weekdays(as.Date(data_correction[, 2])) == "czwartek" | weekdays(as.Date(data_correction[, 
    2])) == "pi¹tek")

data_correction[tf, 4] <- "weekday"

tf <- (weekdays(as.Date(data_correction[, 2])) == "sobota" | weekdays(as.Date(data_correction[, 
    2])) == "niedziela")
data_correction[tf, 4] <- "weekend"


avg <- by(data_correction$steps, data_correction[, 3:4], mean)
```

## Are there differences in activity patterns between weekdays and weekends?

```r
split.screen(c(2, 1))
```

```
## [1] 1 2
```

```r
screen(1)
plot(intervals, avg[, 1], type = "l", xlab = "interval", ylab = "number of steps", 
    main = "weekday")
screen(2)
plot(intervals, avg[, 2], type = "l", xlab = "interval", ylab = "number of steps", 
    main = "weekend")
```

![plot of chunk unnamed-chunk-10](figure/unnamed-chunk-10.png) 



Grzegorz ¯ak
