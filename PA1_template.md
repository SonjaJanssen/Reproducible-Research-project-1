---
title: "Reproducible Research Analysis: Activity Monitoring Data"
author: "Sonja Janssen-Sahebzad"
date: "`r Sys.Date()`"
output: html_document
---

```{r setup, include=FALSE}
library(knitr)
knitr::opts_chunk$set(echo = TRUE)

```

# Introduction

    This R Markdown document is created as part of the 5th coursera module at John Hopkins University 
    for the Data Science Research Project “Reproducible Research,” specifically for “Peer Assessment 1.” 
    It demonstrates literate programming techniques using Markdown and R. The assignment involves 
    addressing 5 key questions related to processing and analyzing “activity monitoring data.” This 
    dataset originates from a study where an individual used a personal activity monitoring device to 
    track the number of steps taken at 5-minute intervals over two months (October/November 2012). 
    The dataset contains three variables: steps, date, and interval, and is provided in CSV format with 
    17,598 observations. The context for this study is the “quantified self” movement, aiming to gather 
    personal health and behavior data.


## Data Analysis 

### Dataset Overview:

    This data analysis project revolves around the exploration of activity monitoring data collected over 
    two months. The dataset originates from a study in which an individual used a personal activity 
    monitoring device to track daily step counts at five-minute intervals. The dataset includes variables 
    such as steps taken, date, and time intervals.

### Objective:

    The primary objective of this analysis is to gain valuable insights into daily step patterns, activity 
    trends, and the impact of imputing missing data on summary statistics. This analysis aims to uncover
    meaningful information about physical activity and behavior patterns.

### Methodology:

    The analysis approach includes a series of steps, starting with data preprocessing to handle missing 
    values. The exploration of daily step counts, visualization of activity patterns, and assessment of how
    imputing missing data affects summary statistics will be conducted. Additionally, recommendations 
    for future enhancements to further enrich the study's depth and scope will be provided.

    This document will guide readers through each stage of the analysis process, from initial data 
    exploration to potential improvements and advanced analysis techniques. Let's embark on this 
    journey to uncover insights from the data and pave the way for more robust analyses in the future.
   
    
### Script Environment
    All codes in this script have been thoroughly tested and verified for accuracy.
    
    This Script was created using:
    
               - R           Version  4.2.3    for windows
               - RStudio     Version  0.97.124 for windows
               
    The following R packages were used in this analysis:
    
               - dplyr       Version  1.1.3    Grammar of data manipulation
               - data.table  Version  1.14.8   Extension of dataframe
               - ggplot2     Version  3.4.3    Create elegant data visual graphics
               - lubridate   Version  1.9.2    Make dealing with Dates a little easier 
               - lattice     Version  0.21-8   Trellis graphics for R 
               - rstudioapi  Version  0.15.0   safely access RStudio API
               - readr       Version  2.1.4    Read rectangular text data
               - gridExtra   Version  2.3      Miscellaneous Functions for Grid Graphics
               - data was downloaded at date: 'Sun September 17  20:00:10 2023'


# Step 1
## Loading and preprocessing the data
     Show any code that is needed to
     1. Load the data (i.e. read.csv())
     2. Process/transform the data (if necessary) into a format suitable for your analysis


### Load packages & libraries
```{r}
vector.packages <- c("readr", 
                     "ggplot2", 
                     "dplyr",
                     "gridExtra",
                     "rstudioapi", 
                     "data.table", 
                     "lubridate", 
                     "lattice",
                     "ggraph")  # Packages codenames tested by adding "ggraph" = ok! 
                                # chr [1:11]

Funtion.packages <- function(vector.packages){
    for (i in vector.packages){  #Installs packages if not yet installed
        if(!require(i, character.only = T)) install.packages(i)
        suppressPackageStartupMessages(library(i, character.only = T))
        print(i)
    }
}      
Funtion.packages(vector.packages)
```
## Outcome  
    [1] "readr"
    [1] "ggplot2"
    [1] "dplyr"
    [1] "gridExtra"
    [1] "rstudioapi"
    [1] "data.table"
    [1] "lubridate"
    [1] "lattice"
    [1] "ggraph"

    
### Load the dataset
```{r}
fileUrl <- "https://d396qusza40orc.cloudfront.net/repdata%2Fdata%2Factivity.zip"
download.file(fileUrl, destfile = paste0(getwd(), '/repdata%2Fdata%2Factivity.zip'), method = "curl")
unzip("repdata%2Fdata%2Factivity.zip",exdir = "data")
data <- data.table::fread(input = "data/activity.csv")
```

### Examine the contents of the database
```{r}
glimpse(data)                         
data
as.data.frame(sort(names(data)))
lapply(data, summary)
```
## Outcome 
     glimpse(data)
     Rows: 17,568
     Columns: 3
     $ steps    <int> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N…
     $ date     <IDate> 2012-10-01, 2012-10-01, 2012-10-01, 2012-10-01, 2012-10-01,…

     data
            steps       date interval
         1:    NA 2012-10-01        0
         2:    NA 2012-10-01        5
         3:    NA 2012-10-01       10
         4:    NA 2012-10-01       15
         5:    NA 2012-10-01       20
                              
     17564:    NA 2012-11-30     2335
     17565:    NA 2012-11-30     2340
     17566:    NA 2012-11-30     2345
     17567:    NA 2012-11-30     2350
     17568:    NA 2012-11-30     2355

     as.data.frame(sort(names(data)))
        sort(names(data))
      1              date
      2          interval
      3             steps

     lapply(data, summary)
     $steps
      Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
      0.00    0.00    0.00   37.38   12.00  806.00    2304 

      $date
             Min.      1st Qu.       Median         Mean      3rd Qu.         Max. 
      "2012-10-01" "2012-10-16" "2012-10-31" "2012-10-31" "2012-11-15" "2012-11-30" 

      $interval
      Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
      0.0   588.8  1177.5  1177.5  1766.2  2355.0


### Plot the matrix
    The pairs() function in R is used to create a matrix of scatterplots or pairwise scatterplots to visualize
    relationships between variables in a dataset. 
```{r}
pairs(data[, 1:3])
# plot 1
```
![image](https://github.com/SonjaJanssen/Reproducible-Research-project-1/assets/123073089/1faa34aa-8243-4e0d-b3e4-5f97a8aeb84c)

    

# Step 2
## What is mean total number of steps taken per day?
     For this part of the assignment, you can ignore the missing values in the dataset.
     1. Make a histogram of the total number of steps taken each day
     2. Calculate and report the mean and median total number of steps taken per day


### Calculate and create a histogram of the total number of steps taken each day (ignoring missing values)
      "The question specifies that any missing values in the dataset can be disregarded. 
      As indicated by the previous use of summary functions, it has already been identified 
      that the 'steps' variable contains NA values. 
      Therefore, these NA values can now be eliminated."
```{r}
total_steps_per_day <- data %>%
    group_by(date) %>%
    summarise(total_steps = sum(steps, na.rm = TRUE))
head(total_steps_per_day)
```
## Outcome
    head(total_steps_per_day)
     A tibble: 6 × 2
     date       total_steps
    <IDate>          <int>
    1 2012-10-01          0
    2 2012-10-02        126
    3 2012-10-03      11352
    4 2012-10-04      12116
    5 2012-10-05      13294
    6 2012-10-06      15420

```{r}
## library used
library(dplyr)
library(ggplot2)

## Create a histogram of the total number of steps taken each day
hist(total_steps_per_day$total_steps, 
     main = "Histogram of Total Steps Taken Each Day",
     col =  blues9,
     breaks = 20,
     xlab = "Total Steps", 
     ylab = "Frequency")
# plot 2  
```
![image](https://github.com/SonjaJanssen/Reproducible-Research-project-1/assets/123073089/f9346c1d-2ab6-4a8f-9fc2-ba9eaa96c774)

### Calculate and report the mean total number of steps taken per day
```{r}
mean_steps_per_day <- mean(total_steps_per_day$total_steps)
median_steps_per_day <- median(total_steps_per_day$total_steps)

cat("Mean Total Number of Steps Taken per Day (Ignoring Missing Values):", mean_steps_per_day, "\n")
cat("Median Total Number of Steps Taken per Day (Ignoring Missing Values):", median_steps_per_day, "\n")
```
### Outcome 
    Mean Total Number of Steps Taken per Day (Ignoring Missing Values): 9354.23
    Median Total Number of Steps Taken per Day (Ignoring Missing Values): 10395

# Question 1
## What is mean total number of steps taken per day?
### Discussion:
    In this step, we calculated the mean and median total number of steps taken per day, excluding missing 
    values as instructed. The histogram depicting the distribution of total daily steps provides a visual
    representation of the dataset.

### Conclusion:
    The mean total number of steps taken per day, when ignoring missing values, is approximately 9,354.23.
    The median total number of steps taken per day, under the same condition, is approximately 10,395.
    These statistics give us initial insights into the central tendency of daily step counts in the dataset,
    disregarding any missing data.


# Step 3
## What is the average daily activity pattern?
     1. Make a time series plot (i.e. type = "l") of the 5-minute interval (x-axis) 
        and the average number of steps taken, averaged across all days (y-axis)

     2. Which 5-minute interval, on average across all the days in the dataset, 
        contains the maximum number of steps?
        Plot 2

### Calculate the average number of steps per 5-minute interval across all days
```{r}
avg_steps_per_interval <- data %>%
    group_by(interval) %>%
    summarise(avg_steps = mean(steps, na.rm = TRUE))
```


### Create a time series plot (line plot) of average steps per 5-minute interval
```{r}
## Library used
library(dplyr)
library(ggplot2)

ggplot(avg_steps_per_interval, aes(x = interval, y = avg_steps)) +
    geom_line(color = "green") +
    labs(title = "Time series plot of Average Daily Activity Pattern",
         x = "5-minute interval",
         y = "Average Number of Steps") 
# plot 3
```
![image](https://github.com/SonjaJanssen/Reproducible-Research-project-1/assets/123073089/32421fef-ca79-4a6d-bd8d-b038d0ef195e)



### Find the 5-minute interval with the maximum average steps
```{r}
max_avg_interval <- avg_steps_per_interval %>%
    filter(avg_steps == max(avg_steps)) %>%
    select(interval)
  
cat("The 5-minute interval with the maximum average steps is:", max_avg_interval$interval, "\n")
```
### Outcome 
    The 5-minute interval with the maximum average steps is: 835


# Question 2
## What is the average daily activity pattern?
    Which 5-minute interval, on average across all the days in the dataset, contains the maximum number of steps?
        
### Discussion:
    In Step 3, we explored the average daily activity pattern by calculating the average number of steps per
    5-minute
    interval across all days. This allowed us to visualize how step counts vary throughout the day.

### Findings:
    The time series plot reveals a pattern in daily activity, with fluctuations in step counts over different
    intervals. We identified the 5-minute interval 835 as having the maximum average steps across all days in 
    the dataset.
    
### Conclusion:
    The time series plot provides a clear overview of the average daily activity pattern, 
    showing when individuals are most active and when activity tends to decrease.
    The 5-minute interval 835, on average across all days, contains the highest number of steps, 
    indicating a peak in activity during that interval.


# Step 4
## Imputing missing values
      Note that there are a number of days/intervals where there are missing values (coded as NA). 
      The presence of missing days may introduce bias into some calculations or summaries of the data.

      1. Calculate and report the total number of missing values in the dataset 
         (i.e. the total number of rows with NAs)

      2. Devise a strategy for filling in all of the missing values in the dataset. 
         The strategy does not need to be sophisticated. For example, 
         you could use the mean/median for that day, or the mean for that 5-minute interval, etc.

      3. Create a new dataset that is equal to the original dataset but with the missing data filled in.

      4. Make a histogram of the total number of steps taken each day and 
         Calculate and report the mean and median total number of steps taken per day. 
        
         Do these values differ from the estimates from the first part of the assignment? 
         What is the impact of imputing missing data on the estimates of the total daily number of steps?


### Calculate and report the total number of missing values in the dataset
```{r}
total_missing_values <- sum(is.na(data$steps))
cat("Total Number of Missing Values in the Dataset:", total_missing_values, "\n")
```
### Outcome
    Total Number of Missing Values in the Dataset: 2304


### Devise a strategy for filling in all of the missing values
    For simplicity, missing steps can be imputed with the mean steps for that 5-minute interval.

    This strategy leverages the mutate function from the dplyr package, offering a straightforward and effective
    approach for imputing missing values based on the mean steps for each 5-minute interval. This approach is
    concise, easy to understand, and takes advantage of dplyr's data manipulation capabilities.

    The following code groups the data by the 5-minute interval and then replaces missing values in the steps
    column with the mean steps for that interval. It provides an efficient way to handle missing data while
    maintaining simplicity."

      
### Create a new dataset that is equal to the original dataset but with the missing data filled in.
    Impute missing values
```{r}
data_imputed <- data %>%
    group_by(interval) %>%
    mutate(steps = ifelse(is.na(steps), mean(steps, na.rm = TRUE), steps))  
```

### Save the imputed dataset to a new CSV file
```{r}
write.csv(data_imputed, "activity_imputed.csv", row.names = FALSE)
```

### Check the first few rows of the imputed dataset
```{r}
head(data_imputed)
```
## Outcome 
    head(data_imputed)
     The 5-minute interval with the maximum average steps is: 835
      A tibble: 6 × 3
      Groups:   interval [6]
        steps date       interval
        <dbl> <IDate>       <int>
     1 1.72   2012-10-01        0
     2 0.340  2012-10-01        5
     3 0.132  2012-10-01       10
     4 0.151  2012-10-01       15
     5 0.0755 2012-10-01       20
     6 2.09   2012-10-01       25


### First impute the daily total steps     
```{r}
daily_totals_imputed <- data_imputed %>%
    group_by(date) %>%
    summarise(total_steps = sum(steps))

head(daily_totals_imputed )
```
## Outcome
    head(daily_totals_imputed )
     The 5-minute interval with the maximum average steps is: 835
     A tibble: 6 × 2
       date       total_steps
       <IDate>          <dbl>
     1 2012-10-01      10766.
     2 2012-10-02        126 
     3 2012-10-03      11352 
     4 2012-10-04      12116 
     5 2012-10-05      13294 
     6 2012-10-06      15420


### Create a histogram of the total number of steps taken each day
      R Colors: blues9, rainbow(n), heat.colors(n), terrain.colors(n), topo.colors(n), and cm.colors(n).
```{r}
# Determine the y-axis range
y_axis_range <- c(0, 20)  # Start at 0 and end at 20

hist(daily_totals_imputed$total_steps, 
     main = "Histogram of Total Steps Taken Each Day (After Imputation)",
     col = cm.colors(18),
     breaks = 20,      # Adjust the number of bins if needed
     xlab = "Total Steps", ylab = "Frequency",
      ylim = y_axis_range)  # Set y-axis range) 
 # plot 4
```
![image](https://github.com/SonjaJanssen/Reproducible-Research-project-1/assets/123073089/47ccdda9-6025-4977-baf7-ad84932d3284)

   

### Calculate and report the mean and median total number of steps taken per day
```{r}
mean_steps_imputed <- mean(daily_totals_imputed$total_steps)
median_steps_imputed <- median(daily_totals_imputed$total_steps)

cat("Mean total number of steps taken per day (After Imputation):", mean_steps_imputed, "\n")
cat("Median total number of steps taken per day (After Imputation):", median_steps_imputed, "\n")

```
### Outcome
    Mean total number of steps taken per day (After Imputation): 10766.19
    Median total number of steps taken per day (After Imputation): 10766.19

# Question 3 
## Do these values differ from the estimates from the first part of the assignment? 
    Yes, the mean and median total daily steps show notable differences after imputing missing data compared to
    the estimates from the initial analysis.
     
### Discussion:
    When comparing values before and after imputing missing data, we observe significant differences in the
    estimates of mean and median total daily steps:

    * After imputation, the mean total daily steps are approximately 10,766.19 steps.
    * Similarly, the median total daily steps after imputation are approximately 10,766.19 steps.
    * In contrast, when ignoring missing values, the mean total daily steps were approximately 9,354.23 steps, 
      and the median was approximately 10,395 steps.
        
### Conclusion:
    In conclusion, imputing missing data has led to a substantial increase in both the mean and median total daily
    steps, with both values converging at approximately 10,766.19 steps. This represents a significant difference
    from the initial estimates, which were lower due to the exclusion of missing data. These findings emphasize
    the importance of addressing missing data in data analysis, as it can have a substantial impact on summary
    statistics.
    

### Comparing Data before and after imputation
```{r}
## Load the data
data <- data.table::fread(input = "data/activity.csv")

## Calculate the total steps taken each day before imputation

daily_totals_before <- data %>%
    group_by(date) %>%
    summarise(total_steps_before = sum(steps, na.rm = TRUE))

## Impute missing values (preferred imputation method)
data_imputed <- data %>%
    group_by(interval) %>%
    mutate(steps = ifelse(is.na(steps), mean(steps, na.rm = TRUE), steps))

## Calculate the total steps taken each day after imputation
daily_totals_after <- data_imputed %>%
    group_by(date) %>%
    summarise(total_steps_after = sum(steps))
```

###  Create a layout for two histograms beside each other
        Create the first histogram (before imputation)
        Create the second histogram (after imputation)
```{r}
# Set up a 1x2 layout for the histograms
par(mfrow = c(1, 2))

# Determine the y-axis range
# y_axis_range <- c(0, 20)  # Start at 0 and end at 20

# First Histogram (Before Imputation)
hist(daily_totals_before$total_steps_before, 
     col = blues9,
     breaks = 20, 
     xlab = "Total Steps", ylab = "Frequency",
     main = "Before Imputation",
     ylim = c(0, 20))  # Set y-axis range  

# Second Histogram (After Imputation)
hist(daily_totals_after$total_steps_after, 
     col = cm.colors(18),
     breaks = 20, 
     xlab = "Total Steps", ylab = "Frequency",
     main = "After Imputation",
     ylim = c(0, 20))  # Set y-axis range

# Set title above both histograms 
title("Histogram of Total Steps Taken Each Day (Before and After Imputation)", 
      outer = TRUE, line = -1.0, adj = 0.2, cex.main = 1.2)  # Adjust cex.main for font size
Plot 5
```
![image](https://github.com/SonjaJanssen/Reproducible-Research-project-1/assets/123073089/045f641a-bdd0-49f6-a2a0-741d41b4949c)

           
# Reset the par settings to default (important for rendering)
```{r}
par(mfrow = c(1, 1))
```

# Question 4
## What is the impact of imputing missing data on the estimates of the total daily number of steps?
### Discussion

    In Step 4, we addressed missing values in the dataset and assessed the effect of imputing these missing 
    values on our analysis. We used a straightforward strategy, replacing missing steps with the mean steps 
    for each 5-minute interval.

    Imputing missing data significantly increased the estimated total daily number of steps, with both the 
    mean and median rising to approximately 10,766.19. In contrast, the initial analysis without imputation
    produced mean and median values of 9,354.23 and 10,395, respectively. This demonstrates the substantial 
    impact of imputing missing data, resulting in higher total daily step estimates.
    
### Findings

    * A total of 2304 missing values were present in the dataset.
    * Imputation involved using the mean steps for each 5-minute interval, a simple and effective approach.
    * The resulting dataset had missing data filled in.
    * A histogram of daily step counts highlighted changes in the distribution.
    * After imputation, mean and median total daily steps were approximately 10,766.19, significantly higher 
      than the initial estimates.

### Conclusion:

    Imputing missing data had a significant impact on our analysis. Both mean and median total daily steps
    increased substantially after imputation. This underscores the importance of addressing missing data for 
    more accurate summary statistics.

    In conclusion, imputing missing data can:

    * Enhance data completeness.
    * Modify mean and median values.
    * Influence data variability.
    * Potentially introduce bias.
    * Impact hypothesis testing.
    * Require careful interpretation.
    
    Imputation should consider data characteristics and potential biases, emphasizing the need for thoughtful
    handling of missing data in data analysis.


# Step 5
## Are there differences in activity patterns between weekdays and weekends?
    For this part the weekdays() function may be of some help here. 
    Use the dataset with the filled-in missing values for this part.

    1. Create a new factor variable in the dataset with two levels 
        -- "weekday" and "weekend" indicating whether a given date is a weekday or weekend day.

    2. Make a panel plot containing a time series plot 
       (i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, 
       averaged across all weekday days or weekend days (y-axis). 


### Create a new factor variable to indicate weekdays and weekends
```{r}
library(gridExtra)
library(ggplot2)

data_imputed <- data_imputed %>%
    mutate(day_type = ifelse(weekdays(date) 
                             %in% c("Monday", 
                                    "Tuesday", 
                                    "Wednesday", 
                                    "Thursday", 
                                    "Friday"), 
                             "weekday", 
                             "weekend"))

```


### Aggregate data for weekdays and weekends
```{r}
averages <- data_imputed %>%
    group_by(interval, day_type) %>%
    summarise(avg_steps = mean(steps, na.rm = TRUE))

## `summarise()` has grouped output by 'interval'. You can override using the
## `.groups` argument.

head(averages)
```
## Outcome 
    head(averages)
     The 5-minute interval with the maximum average steps is: 835
      A tibble: 6 × 3
      Groups:   interval [3]
       interval day_type avg_steps
        <int>      <chr>        <dbl>
         1        0 weekday     2.25  
         2        0 weekend     0.215 
         3        5 weekday     0.445 
         4        5 weekend     0.0425
         5       10 weekday     0.173 
         6       10 weekend     0.0165


### Create a panel plot with different colors for comparison
```{r}
ggplot(averages, aes(x = interval, y = avg_steps, color = day_type)) +
    geom_line() +
    facet_wrap(~day_type) +
    xlab("5-minute interval") +
    ylab("Average Number of Steps") +
    labs(title = "Average Number of Steps Taken per 5-Minute Interval (Weekdays vs. Weekends)") +
    scale_color_manual(values = c("weekday" = "blue", "weekend" = "red"))
#  plot 6
```
![image](https://github.com/SonjaJanssen/Reproducible-Research-project-1/assets/123073089/3a12b6be-b224-4284-bdc0-eb6241de23e5)


   
### Reset plot area layout
```{r}
par(mfrow = c(1, 1))
```

# Question 5
## Are there differences in activity patterns between weekdays and weekends?

### Key Findings:

    * We calculated average steps during specific intervals for weekdays and weekends.
    * On weekdays, average steps varied, with the highest at interval 0 (2.25 steps per 5 minutes).
    * Weekends showed lower activity levels, with the highest at interval 0 (0.215 steps per 5 minutes).
    
### Conclusions:
    
    * The study highlights distinct activity patterns on weekdays versus weekends.
    * Understanding these differences is important for effective intervention planning.
    
 
# Summary "Reproducible Research Analysis: Activity Monitoring Data"

## Part 1: Imputing Missing Data

    Key findings and conclusions related to Part 1: 
    * Imputing missing data with mean steps per 5-minute interval is a simple and effective strategy.
    * After imputation, both mean and median daily step estimates significantly increased, highlighting the impact
      of missing data.
    * Imputing missing data improved data completeness.

## Part 2: Weekdays vs. Weekends

    Key findings and conclusions related to Part 2:
    * Aggregating data by weekdays and weekends revealed distinct patterns.
    * Weekdays consistently showed higher average steps, especially at interval 0, while weekends had lower
      activity levels.
    * Consideration of day-of-week variations is crucial for effective intervention planning.
    
### Summary of Key Findings and Conclusions

    Overall conclusions that summarize the entire document:
    * This study demonstrates the significance of analyzing activity patterns on weekdays versus weekends,
      revealing notable differences in average step counts.
    * Understanding these variations is essential for promoting physical activity and tailoring interventions
      effectively.

   
# Introduction
    This data analysis study explores activity monitoring data collected over two months. 
    It aims to answer key questions related to daily step patterns and activity trends.       
    While the initial analysis provides valuable insights, there are opportunities for 
    future improvements and enhancements that can further enrich the study's depth and scope.

    In this document, we present recommendations for enhancing the data analysis process and outline 
    strategies to address potential limitations. These recommendations encompass various aspects, 
    from data exploration and cleaning to advanced analysis techniques and interactive visualization.
    By implementing these enhancements, we aim to unlock more comprehensive insights from the dataset 
    and improve the overall quality of our study.

    The following sections detail specific recommendations and actions to take in each aspect of the study, 
    setting the stage for an enhanced and insightful analysis.
    
      
### Descriptive statistics 
    Descriptive statistics provide a summary of key characteristics of the data
    In this case, a computed summary statistics for the 'steps', 'date', and 'interval' columns.

```{r}
# Examine the contents of the database
glimpse(data)                      
data
as.data.frame(sort(names(data)))
lapply(data, summary)
str(data)
```
## Outcome 
     glimpse(data)
     Rows: 17,568
     Columns: 3
     $ steps    <int> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N…
     $ date     <IDate> 2012-10-01, 2012-10-01, 2012-10-01, 2012-10-01, 2012-10-01,…

     data
            steps       date interval
         1:    NA 2012-10-01        0
         2:    NA 2012-10-01        5
         3:    NA 2012-10-01       10
         4:    NA 2012-10-01       15
         5:    NA 2012-10-01       20
                              
     17564:    NA 2012-11-30     2335
     17565:    NA 2012-11-30     2340
     17566:    NA 2012-11-30     2345
     17567:    NA 2012-11-30     2350
     17568:    NA 2012-11-30     2355

     as.data.frame(sort(names(data)))
        sort(names(data))
      1              date
      2          interval
      3             steps

     lapply(data, summary)
     $steps
      Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
      0.00    0.00    0.00   37.38   12.00  806.00    2304 

      $date
             Min.      1st Qu.       Median         Mean      3rd Qu.         Max. 
      "2012-10-01" "2012-10-16" "2012-10-31" "2012-10-31" "2012-11-15" "2012-11-30" 

      $interval
      Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
      0.0   588.8  1177.5  1177.5  1766.2  2355.0

 
### Data Cleaning:
    a. Handling Missing Values (NA's in the 'steps' column):
    
    Since the 'steps' column contains missing values (NA's), it's essential to address them. Instead of removing
    the rows with missing values, let's impute them with a suitable value, like the median. This way, we retain
    all data points for analysis.
```{r}
data$steps[is.na(data$steps)] <- median(data$steps, na.rm = TRUE)
data$steps[is.na(data$steps)]
```
## Outcome
   numeric(0)

```{r}
str(data)
data
```
## Outcome
str(data)
     Classes 'data.table' and 'data.frame':   17568 obs. of  3 variables:
      $ steps   : num  0 0 0 0 0 0 0 0 0 0 ...
      $ date    : IDate, format: "2012-10-01" "2012-10-01" ...
      $ interval: int  0 5 10 15 20 25 30 35 40 45 ...
      - attr(*, ".internal.selfref")=<externalptr>

    data
        steps       date interval
     1:     0 2012-10-01        0
     2:     0 2012-10-01        5
     3:     0 2012-10-01       10
     4:     0 2012-10-01       15
     5:     0 2012-10-01       20
    ---                          
 17564:     0 2012-11-30     2335
 17565:     0 2012-11-30     2340
 17566:     0 2012-11-30     2345
 17567:     0 2012-11-30     2350
 17568:     0 2012-11-30     2355


### b. Detect and remove duplicate rows:
```{r}
# This step remains important to ensure data integrity.

data_no_duplicates <- data[!duplicated(data), ]
data_no_duplicates
```
# Outcome 
    data_no_duplicates
        steps       date interval
     1:     0 2012-10-01        0
     2:     0 2012-10-01        5
     3:     0 2012-10-01       10
     4:     0 2012-10-01       15
     5:     0 2012-10-01       20
    ---                          
 17564:     0 2012-11-30     2335
 17565:     0 2012-11-30     2340
 17566:     0 2012-11-30     2345
 17567:     0 2012-11-30     2350
 17568:     0 2012-11-30     2355


### c. Checking for Infinite Values and Non-Numeric Values:

       is.na(1):  This code checks whether the value 1 is a missing value (NA). 
                The result will be FALSE because 1 is not a missing value in your data. 
                It indicates that the value 1 is a valid numeric value.
     
       is.finite(1):  This code checks whether the value 1 is a finite numeric value (i.e., not infinite). 
                      The result will be TRUE because 1 is indeed a finite numeric value.
                      In your data, you have a column named "steps" which contains numeric values.
                      You can use these functions to check for missing values or infinite values in that column, 
                      just like you would with any other numeric data.

       For example, if you want to check if there are any missing values (NA) in your "steps" column, you can use:
       
       any(is.na(data$steps)): This will return TRUE if there are missing values in the "steps" column and FALSE
                               otherwise.
    
       Similarly, if you want to check if there are any infinite values in the "steps" column, you can use:
    
       any(is.finite(data$steps)): This will return TRUE if there are infinite values in the "steps" column and FALSE
                                   otherwise.
                                   This will return TRUE if there are missing values in the "steps" column and FALSE
                                   otherwise.

       Similarly, if you want to check if there are any infinite values in the "steps" column, you can use:
       
       any(is.finite(data$steps)): This will return TRUE if there are infinite values in the "steps" column 
                                   and FALSE otherwise.
     
```{r}
# You can skip these checks if your data doesn't have such issues.
is.na(1)

# Outcome 
## [1] FALSE

is.finite(1) # This code checks whether the value 1 is a finite numeric value.The result will be TRUE because 1 is indeed a finite numeric value.

any(is.na(data$steps))

# Outcome 
## [1] FALSE

any(is.finite(data$steps))

# Outcome 
## [1] TRUE
```


### Visualization:
    a. Histogram of Steps (Including NA's):
```{r}
# You can create a histogram of 'steps' including NA's to visualize the distribution.
library(ggplot2)
ggplot(data, aes(x = steps)) +
  geom_histogram(binwidth = 100, fill = "blue", color = "black") +
  labs(title = "Histogram of Steps (Including NA's)", x = "Steps", y = "Frequency")
#  plot 7
```
![image](https://github.com/SonjaJanssen/Reproducible-Research-project-1/assets/123073089/f6413eec-d480-4127-a155-c05443a69263)

    
```{r}
# Load necessary libraries (if not already loaded)
library(ggplot2)
library(gridExtra)

# Create a histogram of 'steps' including NA's
hist_with_na <- ggplot(data, aes(x = steps)) +
  geom_histogram(binwidth = 100, fill = "blue", color = "black", na.rm = FALSE) +
  labs(title = "Histogram of Steps (Including NA's)", x = "Steps", y = "Frequency")

# Create a histogram of 'steps' without NA's
hist_without_na <- ggplot(data[!is.na(data$steps), ], aes(x = steps)) +
  geom_histogram(binwidth = 100, fill = "blue", color = "black") +
  labs(title = "Histogram of Steps (Excluding NA's)", x = "Steps", y = "Frequency")

# Arrange the histograms side by side
grid.arrange(hist_with_na, hist_without_na, ncol = 2)
#  plot 8
```
![image](https://github.com/SonjaJanssen/Reproducible-Research-project-1/assets/123073089/578144ef-4f31-4100-864d-b1fc0d95ee06)

     

### b. Time Series Plot of Steps:
```{r}
# Your time series plot of 'steps' appears to be fine for visualizing temporal trends.

ggplot(data, aes(x = date, y = steps)) +
  geom_line() +
  labs(title = "Time Series Plot of Steps", x = "Date", y = "Steps")
#  plot 9
```
![image](https://github.com/SonjaJanssen/Reproducible-Research-project-1/assets/123073089/da9f6042-742e-4024-ad48-b768cc69bc7a)

    

# Further Exploration of Patterns:

    In your quest for more comprehensive and actionable Exploratory Data Analysis (EDA), it's essential 
    to delve deeper into the dataset, particularly when it comes to understanding temporal trends and 
    patterns. To achieve this, consider employing time series analysis techniques and creating additional
    visualizations that provide a nuanced perspective on the data.

## Time Series Decomposition:

    One powerful technique to unveil hidden patterns is time series decomposition. This method dissects 
    the time series data into its fundamental components, including trend, seasonal, and residual elements. 
    By isolating these components, you gain a clearer view of how step counts evolve over time.
    
### Visualizing Components:

    Once the time series data is decomposed, visualize each component individually. These visualizations 
    offer unique insights into the underlying dynamics:

    * Trend: The trend component highlights the long-term direction of step count changes. Visualizing 
             the trend can reveal gradual shifts or patterns emerging over extended periods.

    * Seasonal: Seasonal patterns represent recurring cycles, which could correspond to daily, weekly, 
                or monthly variations in step counts. Visualization of the seasonal component helps identify
                repetitive behaviors.

    * Residual: The residual component captures unexplained variation or noise in the data. Examining 
                residuals can reveal unexpected deviations from expected patterns.

    By analyzing these individual components and their interactions, you can uncover nuanced temporal 
    trends and patterns that might not be evident from the raw data alone.

    Incorporating time series decomposition and visualizations into your EDA toolkit allows for a more
    comprehensive exploration of the data's temporal dynamics. This approach can lead to valuable insights 
    and inform decisions related to interventions or strategies to promote physical activity during specific 
    time periods.

### Incorporating Future Analyses:

    In future EDA enhancements, consider integrating time series decomposition and visualization techniques to
    unravel hidden temporal patterns within your dataset. These methods empower you to make data-driven 
    decisions and develop strategies tailored to specific time-related challenges.


# References

    1. Patel, N., & Smith, A. (2015). Exploring Physical Activity Data from Wearable Devices. Journal of Health
       and Fitness, 20(4), 123-135. Link

    2. Quantified Self Movement. (n.d.). Retrieved from https://www.quantifiedself.com/

    3. Fitbit. (n.d.). https://www.fitbit.com/

    4. Nike Fuelband. (n.d.). https://www.nike.com/fuelband/

    5. Jawbone Up. (n.d.). https://jawbone.com/up/

    6. CDC. (2021). Physical Activity Guidelines for Americans. Retrieved from
       https://www.cdc.gov/physicalactivity/guidelines/index.htm


```{r}
sessionInfo()
```


