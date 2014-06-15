# Reproducible Research: Peer Assessment 1


## Loading and preprocessing the data
We first load the necessary packages:

```r
library(plyr)
```

We then begin the data processing by unzipping and loading the data.


```r
data <- read.csv(unz("activity.zip", "activity.csv"))
```

## What is mean total number of steps taken per day?
To calculate the total number of steps taken each day, we use the plyr package. We first create a new data frame that sums the steps per day.

```r
dailySum <- ddply(data, .(date), summarize, daily.sum=sum(steps, na.rm=TRUE))
```
We then plot a hostogram of the above:

```r
hist(dailySum$daily.sum, xlab='Steps per Day', col='red', 
     main='Histogram of Steps per Day')
```

![plot of chunk unnamed-chunk-4](figure/unnamed-chunk-4.png) 
Finally we calculate the mean and the median of the total steps per day:

```r
meanDailySteps <- mean(dailySum$daily.sum, na.rm=TRUE)
medianDailySteps <- median(dailySum$daily.sum, na.rm=TRUE)

meanDailySteps
```

```
## [1] 9354
```

```r
medianDailySteps
```

```
## [1] 10395
```

## What is the average daily activity pattern?
Again use plyr to creat a summary data table, this time summarizing with respect to activity period:

```r
intervalSum <- ddply(data, .(interval), summarize, 
                     interval.mean=mean(steps, na.rm=TRUE))
```
Plotting this as time series data gives us a view of how activity chanegs during the day:

```r
plot(intervalSum$interval, intervalSum$interval.mean,type='l', xlab='Interval',
     ylab='Mean number of steps', main='Daily Mean Steps')
```

![plot of chunk unnamed-chunk-7](figure/unnamed-chunk-7.png) 

The interval with the maximum number of steps is given below.

```r
maxIntevalSteps <- max(intervalSum$interval.mean)
intervalSum[which(intervalSum$interval.mean==maxIntevalSteps), 'interval']
```

```
## [1] 835
```
## Imputing missing values
How many rows with missing data are there?

```r
numberOfNARows <- sum(is.na(data$steps))
numberOfNARows
```

```
## [1] 2304
```


## Are there differences in activity patterns between weekdays and weekends?
