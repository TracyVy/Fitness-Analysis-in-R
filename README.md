# About The Company

Founded by Urska Srsen and Sando Mur, Bellabeat is a high-tech company
that manufactures health-focused smart products. Its goal is to inform
and inspire women around the world to live healthier lives.

### Analyze Questions

Analyze smart device usage data to gain insights into how consumers use
non-Bellabeat smart devices.

1.  What are some trends in smart device usage?
2.  How could these trends apply to Bellabeat customers?
3.  How could these trends help influence Bellabeat marketing strategy?

### Dataset

This [FitBit dataset](https://www.kaggle.com/arashnic/fitbit) was used
to for analysis. This dataset includes a sample set of eligible and
consented users logging in their activities between April 12th to May
12th of 2016.

I will be conducting analysis in R and using the ggplot for
visualization. (Please look forward to my SQL and Tableau version of
this coming soon to a GitHub near you. :-)

To get started, open RStudio and download the aforementioned dataset in [Kaggle](https://www.kaggle.com/arashnic/fitbit).

## Table of Contents
1. [Install and Load Pacakges](https://github.com/TracyVy/Fitness-Analysis-in-R#Install-and-Load-Packages)
2. [Load CSV file](https://github.com/TracyVy/Fitness-Analysis-in-R#Load-CSV-file)
3. [Exploring data](https://github.com/TracyVy/Fitness-Analysis-in-R#Exploring-Data)
4. [Cleaning and Processing Data](https://github.com/TracyVy/Fitness-Analysis-in-R#Cleaning-and-Processing-Data)
5. [Summary of Analysis](https://github.com/TracyVy/Fitness-Analysis-in-R#Summary-of-Analysis)

## Install and Load Packages

``` r
# Install packages
install.packages (c("tidyverse", "lubridate", "scales"), repos = "http://cran.us.r-project.org")
```

    ## 
    ## The downloaded binary packages are in
    ##  /var/folders/vy/q5vptsyx73q7364_by3bgs1h0000gn/T//RtmplYEml6/downloaded_packages

``` r
# Load package, tidyverse
library(tidyverse)
```

    ## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.1 ──

    ## ✓ ggplot2 3.3.5     ✓ purrr   0.3.4
    ## ✓ tibble  3.1.6     ✓ dplyr   1.0.7
    ## ✓ tidyr   1.1.4     ✓ stringr 1.4.0
    ## ✓ readr   2.1.1     ✓ forcats 0.5.1

    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
# Load package, lubridate
library(lubridate)
```

    ## 
    ## Attaching package: 'lubridate'

    ## The following objects are masked from 'package:base':
    ## 
    ##     date, intersect, setdiff, union

``` r
# Load package, scales
library(scales)
```

    ## 
    ## Attaching package: 'scales'

    ## The following object is masked from 'package:purrr':
    ## 
    ##     discard

    ## The following object is masked from 'package:readr':
    ## 
    ##     col_factor

## Load CSV file

Note: Make sure the files needed are in the same working directory.

``` r
# Load csv file (first)
dayActivity <- read_csv("dailyActivity.csv")
```

    ## Rows: 940 Columns: 15

    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (1): ActivityDate
    ## dbl (14): Id, TotalSteps, TotalDistance, TrackerDistance, LoggedActivitiesDi...

    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
# Load csv file (second)
daySleep <- read_csv("sleepDay.csv")
```

    ## Rows: 413 Columns: 5

    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (1): SleepDay
    ## dbl (4): Id, TotalSleepRecords, TotalMinutesAsleep, TotalTimeInBed

    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

## Exploring Data

Let’s check out what kind of data these tables show.

``` r
# Find column names in dayActivity csv file
colnames(dayActivity)
```

    ##  [1] "Id"                       "ActivityDate"            
    ##  [3] "TotalSteps"               "TotalDistance"           
    ##  [5] "TrackerDistance"          "LoggedActivitiesDistance"
    ##  [7] "VeryActiveDistance"       "ModeratelyActiveDistance"
    ##  [9] "LightActiveDistance"      "SedentaryActiveDistance" 
    ## [11] "VeryActiveMinutes"        "FairlyActiveMinutes"     
    ## [13] "LightlyActiveMinutes"     "SedentaryMinutes"        
    ## [15] "Calories"

``` r
# Find column names in daySleep csv file
colnames(daySleep)
```

    ## [1] "Id"                 "SleepDay"           "TotalSleepRecords" 
    ## [4] "TotalMinutesAsleep" "TotalTimeInBed"

``` r
# Explore data structure in dayActivity csv file
str(dayActivity)
```

    ## spec_tbl_df [940 × 15] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
    ##  $ Id                      : num [1:940] 1.5e+09 1.5e+09 1.5e+09 1.5e+09 1.5e+09 ...
    ##  $ ActivityDate            : chr [1:940] "4/12/2016" "4/13/2016" "4/14/2016" "4/15/2016" ...
    ##  $ TotalSteps              : num [1:940] 13162 10735 10460 9762 12669 ...
    ##  $ TotalDistance           : num [1:940] 8.5 6.97 6.74 6.28 8.16 ...
    ##  $ TrackerDistance         : num [1:940] 8.5 6.97 6.74 6.28 8.16 ...
    ##  $ LoggedActivitiesDistance: num [1:940] 0 0 0 0 0 0 0 0 0 0 ...
    ##  $ VeryActiveDistance      : num [1:940] 1.88 1.57 2.44 2.14 2.71 ...
    ##  $ ModeratelyActiveDistance: num [1:940] 0.55 0.69 0.4 1.26 0.41 ...
    ##  $ LightActiveDistance     : num [1:940] 6.06 4.71 3.91 2.83 5.04 ...
    ##  $ SedentaryActiveDistance : num [1:940] 0 0 0 0 0 0 0 0 0 0 ...
    ##  $ VeryActiveMinutes       : num [1:940] 25 21 30 29 36 38 42 50 28 19 ...
    ##  $ FairlyActiveMinutes     : num [1:940] 13 19 11 34 10 20 16 31 12 8 ...
    ##  $ LightlyActiveMinutes    : num [1:940] 328 217 181 209 221 164 233 264 205 211 ...
    ##  $ SedentaryMinutes        : num [1:940] 728 776 1218 726 773 ...
    ##  $ Calories                : num [1:940] 1985 1797 1776 1745 1863 ...
    ##  - attr(*, "spec")=
    ##   .. cols(
    ##   ..   Id = col_double(),
    ##   ..   ActivityDate = col_character(),
    ##   ..   TotalSteps = col_double(),
    ##   ..   TotalDistance = col_double(),
    ##   ..   TrackerDistance = col_double(),
    ##   ..   LoggedActivitiesDistance = col_double(),
    ##   ..   VeryActiveDistance = col_double(),
    ##   ..   ModeratelyActiveDistance = col_double(),
    ##   ..   LightActiveDistance = col_double(),
    ##   ..   SedentaryActiveDistance = col_double(),
    ##   ..   VeryActiveMinutes = col_double(),
    ##   ..   FairlyActiveMinutes = col_double(),
    ##   ..   LightlyActiveMinutes = col_double(),
    ##   ..   SedentaryMinutes = col_double(),
    ##   ..   Calories = col_double()
    ##   .. )
    ##  - attr(*, "problems")=<externalptr>

``` r
# Explore data structure in daySleep csv file
str(daySleep)
```

    ## spec_tbl_df [413 × 5] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
    ##  $ Id                : num [1:413] 1.5e+09 1.5e+09 1.5e+09 1.5e+09 1.5e+09 ...
    ##  $ SleepDay          : chr [1:413] "4/12/2016 12:00:00 AM" "4/13/2016 12:00:00 AM" "4/15/2016 12:00:00 AM" "4/16/2016 12:00:00 AM" ...
    ##  $ TotalSleepRecords : num [1:413] 1 2 1 2 1 1 1 1 1 1 ...
    ##  $ TotalMinutesAsleep: num [1:413] 327 384 412 340 700 304 360 325 361 430 ...
    ##  $ TotalTimeInBed    : num [1:413] 346 407 442 367 712 320 377 364 384 449 ...
    ##  - attr(*, "spec")=
    ##   .. cols(
    ##   ..   Id = col_double(),
    ##   ..   SleepDay = col_character(),
    ##   ..   TotalSleepRecords = col_double(),
    ##   ..   TotalMinutesAsleep = col_double(),
    ##   ..   TotalTimeInBed = col_double()
    ##   .. )
    ##  - attr(*, "problems")=<externalptr>

``` r
# Explore a tibble of dayActivity
head(dayActivity)
```

    ## # A tibble: 6 × 15
    ##        Id ActivityDate TotalSteps TotalDistance TrackerDistance LoggedActivitie…
    ##     <dbl> <chr>             <dbl>         <dbl>           <dbl>            <dbl>
    ## 1  1.50e9 4/12/2016         13162          8.5             8.5                 0
    ## 2  1.50e9 4/13/2016         10735          6.97            6.97                0
    ## 3  1.50e9 4/14/2016         10460          6.74            6.74                0
    ## 4  1.50e9 4/15/2016          9762          6.28            6.28                0
    ## 5  1.50e9 4/16/2016         12669          8.16            8.16                0
    ## 6  1.50e9 4/17/2016          9705          6.48            6.48                0
    ## # … with 9 more variables: VeryActiveDistance <dbl>,
    ## #   ModeratelyActiveDistance <dbl>, LightActiveDistance <dbl>,
    ## #   SedentaryActiveDistance <dbl>, VeryActiveMinutes <dbl>,
    ## #   FairlyActiveMinutes <dbl>, LightlyActiveMinutes <dbl>,
    ## #   SedentaryMinutes <dbl>, Calories <dbl>

``` r
# Explore a tibble of daySleep 
head(daySleep)
```

    ## # A tibble: 6 × 5
    ##           Id SleepDay          TotalSleepRecor… TotalMinutesAsle… TotalTimeInBed
    ##        <dbl> <chr>                        <dbl>             <dbl>          <dbl>
    ## 1 1503960366 4/12/2016 12:00:…                1               327            346
    ## 2 1503960366 4/13/2016 12:00:…                2               384            407
    ## 3 1503960366 4/15/2016 12:00:…                1               412            442
    ## 4 1503960366 4/16/2016 12:00:…                2               340            367
    ## 5 1503960366 4/17/2016 12:00:…                1               700            712
    ## 6 1503960366 4/19/2016 12:00:…                1               304            320

``` r
# Find how many unique users there are in dayActivity, based on Id
n_distinct(dayActivity$Id)
```

    ## [1] 33

``` r
# Find how many unique users there are in daySleep, based on Id
n_distinct(daySleep$Id)
```

    ## [1] 24

Based on this, we can see that there are 33 distinct users who log their
activities, and 24 distinct users log their sleep patterns between April
12th - May 12th, 2016.

## Cleaning and Processing Data

1. All date and time observations include the time of 12:00:00 AM. This
    data does not help with analysis; rather, it creates noise and makes
    it more difficult to read. Therefore, we will parse out the
    redundant time stamp of 12:00:00AM.

``` r
# Uniform format date to YYYY-MM-DD
daySleep$SleepDay <- mdy_hms(daySleep$SleepDay)

dayActivity$ActivityDate <- mdy(dayActivity$ActivityDate)
```

2. There are a few observations with sedentary minutes to be 1440,
        which means the users were idle for the entire 24 hours of the
        day. This has a very low possibility of occurrence, unless the
        user is gravely ill. Keeping the data will yield inaccurate
        insights.

I’m using sound judgment to filter out all observations with
SedentaryMinutes of more than 1435. I’ve set the cap at 1435, allowing
the possibility of the user taking 5 minutes to take care of personal
things, even on an inactive day.

``` r
# Filter out observations with SedentaryMinutes of more than 1435 minutes.
dayActivity_new <- dayActivity %>%
  filter(SedentaryMinutes<=1435) 
```

3. With that same judgment, I will also filter out all observations
        with 0 (zero) TotalActiveMins. Again, taking steps to take care
        of personal things would trigger the active minutes counter,
        hence an observation of 0 (zero) active minutes has a very low
        possibility of occurence.

``` r
# Filter out observations with zero active minutes.
dayActivity_new <- dayActivity_new %>%
  filter((VeryActiveMinutes + FairlyActiveMinutes + LightlyActiveMinutes)>1)
```

4. Create new column “TotalActiveMins” and verify if the sum of
        “TotalActiveMins” and “SedentaryMins” is 1440. 1440 is the total
        minutes in a 24 hour period.

``` r
# Create new column, TotalActiveMins, that reflects the sum of all active minutes.
dayActivity_new$TotalActiveMins <- dayActivity_new$VeryActiveMinutes + dayActivity_new$FairlyActiveMinutes + dayActivity_new$LightlyActiveMinutes

# Create new column, TotalActive_plus_Sedentary, that reflects the sum of total active minutes and sedentary minutes. Observe data.
dayActivity_new$TotalActive_plus_Sedentary <- dayActivity_new$TotalActiveMins + dayActivity_new$SedentaryMinutes
```

5. After sorting TotalActive_plus_Sedentary in descending order, we find that majority
        of the observations did not include an entire day’s worth of
        activities. Find out how many observations did not include an
        entire day of activities.

``` r
# Use count() to find out how many observations did not equal to 1440, total minutes in a day.
dayActivity_new %>%
  count(dayActivity_new$TotalActive_plus_Sedentary == 1440)
```

    ## # A tibble: 2 × 2
    ##   `dayActivity_new$TotalActive_plus_Sedentary == 1440`     n
    ##   <lgl>                                                <int>
    ## 1 FALSE                                                  458
    ## 2 TRUE                                                   391

Based on the data, 391 of 849, or 46%, observations include an entire day’s worth of
activity log. 458 observations, or 54%, did not.

6. Filter out observations that has TotalActive_plus_Sedentary < 1200 minutes
    or 20 hours of logged activities. Our team decided that 20 hours of logged activities warrant a day's worth of activities.

``` r
# Filter out observations with TotalActive_plus_Sedentary less than 1200 minutes.
dayActivity_new <- dayActivity_new %>%
  filter(TotalActive_plus_Sedentary>=1200)

# Create a new data frame with Id, TotalActiveMins, SedentaryMinutes, TotalActive_plus_Sedentary, grouped by Id.
pivot_Loggers <- dayActivity_new %>%
  group_by(Id) %>%
  select(Id, TotalActiveMins, SedentaryMinutes, TotalActive_plus_Sedentary)

# Find out how many unique observations there are now.
n_distinct(pivot_Loggers$Id)
```

    ## [1] 30

We are down to 30 distinct users. Adequate amount of data to use for analysis.

7. Find the average of total sedentary minutes, SedentaryMinutes, and
    total active minutes, TotalActiveMins, and group by User.

``` r
# Create new data frame, pivot_AVG_active_sedentary, with two new columns: AvgSedentaryMins and AvgActiveMins.

pivot_AVG_active_sedentary <- pivot_Loggers %>%
  group_by(Id) %>%
  summarize (AvgSedentaryMins = mean(SedentaryMinutes), AvgActiveMins = mean(TotalActiveMins))

# Add new column, Total_AVG_active_sedentary, that includes the sum of AvgSedentaryMins and AvgActiveMins.
pivot_AVG_active_sedentary$Total_AVG_active_sedentary <- pivot_AVG_active_sedentary$AvgActiveMins + pivot_AVG_active_sedentary$AvgSedentaryMins

# Create new data frame, pivot_AVG_ActSed_SELECT, that includes selected columns: Id, AvgSedentaryMins, and AvgActiveMins.
pivot_AVG_ActSed_SELECT <- pivot_AVG_active_sedentary %>%
  group_by(Id) %>%
  select(Id, AvgSedentaryMins, AvgActiveMins)
```
8. Find the average total minutes each user spends asleep and idle, in
    bed but not asleep.
Note: Minutes in TotalTimeInBed (daySleep) is included, or a part of, SedentaryMinutes (dayActivity).
``` r
# Calculate total minutes in bed NOT asleep.
daySleep$TotalIdleInBed <- daySleep$TotalTimeInBed - daySleep$TotalMinutesAsleep

# Create new data frame, pivot_AVG_Bed, with columns AvgTotalMinsInBed, AvgTotalMinsAsleep, AvgTotalMinsIdleInBed, group by Id.
pivot_AVG_Bed <- daySleep %>%
   group_by(Id) %>%
   summarize (AvgTotalMinsInBed = mean(TotalTimeInBed), AvgTotalMinsAsleep = mean(TotalMinutesAsleep), AvgTotalMinsIdleInBed = mean(TotalIdleInBed)) 

#Create new data fram, pivot_AVG_Bed_SELECT, with selected columns: Id, AvgTotalMinsAsleep, AvgTotalMinsIdleInBed.
pivot_AVG_Bed_SELECT <- pivot_AVG_Bed %>%
  group_by(Id) %>%
  select(Id, AvgTotalMinsAsleep, AvgTotalMinsIdleInBed)
```

    ## Adding missing grouping variables: `Id`

User 1844505072 is idle in bed a lot longer than the other users. It
raises a red flag. After checking the observations in data frame
daySleep, it appears to be correct and no other action is needed.

9. Create a new data frame that merge the average minutes active,
        asleep, and idle in bed, join by “Id”.

``` r
# Create new data frame, final_data, inner join by Id.
final_data <- 
pivot_AVG_ActSed_SELECT %>%
inner_join(pivot_AVG_Bed_SELECT)
```

    ## Joining, by = "Id"

10. Include a new column, SedentaryDifference, to express the
        difference between sedentary minutes and total minutes asleep
        and idle in bed.

``` r
# Create new column, AvgSedentaryDifference, to find the difference of sedentary time and time in bed.
final_data$AvgSedentaryDifference <- final_data$AvgSedentaryMins - final_data$AvgTotalMinsAsleep - final_data$AvgTotalMinsIdleInBed
```

11. Select data to graph.

``` r
# Create new data fram with selected data to graph.
final_data_graph <- final_data %>%
  group_by(Id) %>%
  select(Id, AvgActiveMins, AvgTotalMinsAsleep, AvgTotalMinsIdleInBed, AvgSedentaryDifference)
```

12. Then group top third of active users in category “Gold”, middle
    third as “Silver”, and least active as “Bronze”. Create new column
    for Category - Gold/Silver/Bronze.

``` r
# Arrange data with AvgActiveMins in descending order.
final_data_graph <- final_data_graph %>% 
  arrange(desc(AvgActiveMins))

# Divide unique users into 3 to evenly distribute to 3 categories.
n_distinct(final_data_graph$Id)/3
```

    ## [1] 7

``` r
# Create new column, category, and set Gold to first 7, second 7 to Silver, and last 7 to Bronze.
final_data_graph$category <- factor(rep(c("Gold","Silver","Bronze"),each=7),levels=c("Gold","Silver","Bronze"))
```

13. Get the average total sleep, idle minutes in bed, active minutes,
    and sedentary minutes. Group by Category.

``` r
# Create date frame, pie_data, with average data grouped by category.
pie_data <- final_data_graph %>% 
  group_by(category) %>% 
  summarize(`Active Mins`=mean(AvgActiveMins),
            `Sleep Mins`=mean(AvgTotalMinsAsleep),
            `Sedentary Mins`=mean(AvgSedentaryDifference),
            `Idle Mins` = mean(AvgTotalMinsIdleInBed))
```

14. Create pie charts.

``` r
# Create 3 pie charts, grouped by category.
pie_long <- pie_data %>% 
  gather("type","value",-category)

#Fitness Activity for Gold category
ggplot(pie_long %>% 
         filter(category=="Gold"), aes(x="",y=value, fill=type))+
  geom_bar(width = 1, stat = "identity")+
  coord_polar("y",start = 0)+
  theme_void()+
    geom_text(aes(label = paste0(round((value),0),"m")),
            position = position_stack(vjust = 0.5)) +
  labs(title="Average Active Mins For Gold category",
       fill="Type")
```

![](Fitness_Analysis_With_R_files/figure-markdown_github/pie%20charts,%20group%20by%203%20categories:%20Gold,%20Silver,%20Bronze-1.png)

``` r
#Fitness Activity for Silver category
ggplot(pie_long %>% 
         filter(category=="Silver"), aes(x="",y=value, fill=type))+
  geom_bar(width = 1, stat = "identity")+
  coord_polar("y",start = 0)+
  theme_void()+
    geom_text(aes(label = paste0(round((value),0),"m")),
            position = position_stack(vjust = 0.5)) +
  labs(title="Average Active Mins For Silver category",
       fill="Type")
```

![](Fitness_Analysis_With_R_files/figure-markdown_github/pie%20charts,%20group%20by%203%20categories:%20Gold,%20Silver,%20Bronze-2.png)

``` r
#Fitness Activity for Bronze category
ggplot(pie_long %>% 
         filter(category=="Bronze"), aes(x="",y=value, fill=type))+
  geom_bar(width = 1, stat = "identity")+
  coord_polar("y",start = 0)+
  theme_void()+
    geom_text(aes(label = paste0(round((value),0),"m")),
            position = position_stack(vjust = 0.5)) +
  labs(title="Average Active Mins For Bronze category",
       fill="Type")
```

![](Fitness_Analysis_With_R_files/figure-markdown_github/pie%20charts,%20group%20by%203%20categories:%20Gold,%20Silver,%20Bronze-3.png)

## Summary of Analysis

#### A Day’s Glance -

*GOLD* - 5 hours 13 mins - active, 5 hours 11 mins - asleep, 44 mins -
idle in bed, 12 hours 19 mins - sedentary.

*SILVER* - 3 hours 43 mins - active, 6 hours 14 mins - asleep, 65 mins -
idle in bed, 12 hours 7 mins - sedentary.

*BRONZE* - 2 hours 6 mins - active, 6 hours 56 mins - asleep, 22 mins -
idle in bed, 13 hours 57 mins - sedentary.

Based on the data, the amount of active minutes is conversely related to
the amount of sleep minutes. For example, the most active users, Gold
category, logged in the least amount of sleep at 5 hours 11 mins. The
least active users, Bronze category, logged in the most amount of sleep
at close to 7 hours of sleep.

Although the Gold users’ active minutes are impressive, their sleep
patterns are short of what the [CDC
suggests.](https://www.cdc.gov/sleep/about_sleep/how_much_sleep.html)
While the sleep patterns of the bronze users are aligned with CDC
suggestion, their sedentary time is alarming.

Regardless of which category the users fall in, there are ways to
improve their lifestyle. One commonality that all users have is the
amount of idle time the users spend in bed - between 22-65 minutes. Some
users may have trouble going to sleep or getting out of bed when they
wake up and will check their smartphones.

#### Strategy suggestions for Stakeholders -

With the assumption that majority of the users check the smartphone
during their idle time in bed, Bellabeat should send out health tips
notifications to users so they can plan the day ahead. The gold users
should receive tips on how to relax and sleep longer, while the bronze
users should get motivational tips to get the body moving.

However, it is advised to collect additional data to strategize a better marketing campaign. For instance, collecting data to see if users do in fact check their smartphones in bed will be helpful. Also, the actual time when users go to sleep and
idle time would yield a more successful outcome as well. There are users who prefer to
check their smartphone before they go to sleep and plan ahead for the next
day, while some users check the smartphone the morning of and plan for
the day ahead. It is never a bad choice to be considerate.
