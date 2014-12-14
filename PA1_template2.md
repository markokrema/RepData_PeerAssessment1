# Reproducible Research: Peer Assessment 1
```{r
library(dplyr)
library(ggplot2)
library(lubridate)
```
## Loading and preprocessing the data
```{r
activity <- read.csv("activity.csv", stringsAsFactors=FALSE)
mutate (activity, date=ymd(date))
```
## What is mean total number of steps taken per day?
```{r
stepsperday <- activity %>%
  group_by(date) %>%
  summarise(steps = sum(steps, na.rm = TRUE))
qplot(steps, data=stepsperday, geom="histogram")

summarise(stepsperday, mean(steps), median(steps))
```
## What is the average daily activity pattern?
```{r
stepsperinterval <- activity %>%
  group_by(interval) %>%
  summarise(steps = mean(steps, na.rm = TRUE))
 p <- ggplot(stepsperinterval, aes(x=interval, y=steps,))
p + geom_line()
```

## Imputing missing values
```{r
sum(is.na(activity$steps))
activityjoined <- activity %>%
left_join(stepsperinterval, by = "interval") %>%
mutate(steps = ifelse(is.na(steps.x), steps.y, steps.x))
qplot(steps, data=activityjoined, geom="histogram")
```

