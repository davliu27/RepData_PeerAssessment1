#RepData_PeerAssessment1
---
title: "PA1_template"
author: "David Liu"
date: "November 15, 2015"
output: 
  html_document:
    keep_md:yes
---

Loading and preprocessing the data

```r
setwd("C:/Users/David/Documents/Coursera_Data_Science/5_Reproducable_Research/project1/RepData_PeerAssessment1")
data<-read.csv("activity.csv")
data$interval<-as.numeric(data$interval)
data$date<-as.Date(data$date)
```
What is mean total number of steps taken per day?
1.Calculate the total number of steps taken per day

```r
#Calculate the total number of steps taken per day
good<-!is.na(data$steps)
data2<-data[good,]
library(plyr)
steps_per_day<-ddply(data2,.(date),summarize,steps=sum(steps))
```
2.Make a histogram of the total number of steps taken each day

```r
hist(steps_per_day$steps)
```

![plot of chunk histogram](figure/histogram-1.png) 

3.Calculate and report the mean and median of the total number of steps taken per day

```r
mean(steps_per_day$steps)
```

```
## [1] 10766.19
```

```r
median(steps_per_day$steps)
```

```
## [1] 10765
```

What is the average daily activity pattern?
1.Make a time series plot

```r
interval<-ddply(data2,.(interval),summarize,steps=mean(steps))
plot(interval$interval,interval$steps)
lines(interval$interval,interval$steps)
```

![plot of chunk timeseries_plot](figure/timeseries_plot-1.png) 

2.Which 5-minute interval, on average across all the days in the dataset, contains the maximum number of steps?

```r
interval$interval[which.max(interval$steps)]
```

```
## [1] 835
```

Imputing missing values

1.Calculate and report the total number of missing values in the dataset

```r
missing_values<-is.na(data$steps)
length(missing_values[missing_values==TRUE])
```

```
## [1] 2304
```

2.Devise a strategy for filling in all of the missing values in the dataset.

```r
#filling missing values with the mean of that day
steps_per_day_mean2<-ddply(data,.(date),summarize,steps=mean(steps))
for(i in 1:length(steps_per_day$date)){
  data3[is.na(data3[data3$date==steps_per_day_mean$date[i],1]),1]<-steps_per_day_mean[i,2]
  }
```
