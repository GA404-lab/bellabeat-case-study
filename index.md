<h2 align="center">Bellabeat Data Analysis Case Study</h2>

*Gervon Alcide*

<p align="center"><img width="1696" height="754" alt="Relationship between Activity, Calories and Sleep" src="https://github.com/user-attachments/assets/0cde30e6-fa2d-4b9b-96ae-4580c9adb903" /></p>

<p align="center"><em>Tableau Dashboard</em></p>

## Introduction

Bellabeat, founded in 2013, is a smart-device, health-focused manufacturer for women. By 2016, Bellabeat has grown rapidly positioning itself as a tech-driven wellness company operating around the world. Whilst traditional media have been a point of operation in Bellabeat's marketing, digital means such as Instagram, Google Search, YouTube, Twitter and Facebook have been their bulk of marketing investments and attention.

This case study analyzes smart device usage data to support marketing decisions for the Bellabeat App. The smart device data are not from Bellabeat products.

The Bellabeat App provides users with health data related to their activity, sleep, stress, menstrual cycle, and mindfulness habits. The Bellabeat app connects to their line of smart 
wellness products.

## Ask
Business Task:
Analyze non-Bellabeat smart device data on use cases for the Bellabeat App to provide recommendations to the Bellabeat marketing team.


The business questions are:
1. What patterns appear frequently in non-Bellabeat smart device data?
2. What do these patterns tell us about the use cases and behavior of consumers?
3. How can these insights be used in marketing for a Bellabeat product?

## Prepare
This analysis utilizes this dataset from Kaggle
FitBit Fitness Tracker Data: [here](https://www.kaggle.com/datasets/arashnic/fitbit)

The dataset consists of results from a survey via Amazon Mechanical Turk between March 12th, 2016 - May 12th, 2016. Thirty Fitbit users consented to the submission of personal tracker data. The data consists of minute-level output for physical activity, heart rate, and sleep monitoring. The dataset was most recently updated in 2024.
License: [CC0: Public Domain](https://creativecommons.org/publicdomain/zero/1.0/)

Limitations include:

- Small sample consisting of 30 participants.
- Old data recorded in 2016.
- Potential bias as it includes users who consented which may not be representative of the entire population.

The data is organized into the folder "FitbitFitnessTrackerData_Mobius" which is stored inside another folder "Project1_non-BellabeatData".
The files and folders are stored locally on my computer.

The dataset consists of multiple CSV files totaling ~588 MB with some tables consisting of over 1 million rows. For these reasons, Rstudio is the program of my choice for analysis.

## Process

In this section, selecting relevant data and cleaning was the main focus.
As my analysis is centered on use cases concerning the Bellabeat app, the CSV files "dailyActivity_merged" and "sleepDay_merged" were selected. They contain information surrounding numerous metrics about sleep, activity, intensity of activity, and calories. For these reasons, these files were the subject of analysis.


#### Cleaning:

Cleaning in R started with loading my packages and the files.

```r
library(tidyverse)
library(lubridate)
library(skimr)
library(janitor)


Activity <- read.csv("dailyActivity_merged.csv")
Sleep <- read.csv("sleepDay_merged.csv")
```

Viewed the contents and formats of this dataset. Checked for missing values, duplicates, improper data types and labelling, value ranges, and business logic.
Some days contained three sleep records. These rows were investigated and determined to be valid anomalies rather than erroneous entries.

Changed data type of the date columns of each table from "str" to "date" data type.
```r
Activity <- Activity %>% 
  mutate(ActivityDate = as.Date(ActivityDate, format = "%m/%d/%Y"))
Sleep <- Sleep %>% 
  mutate(SleepDay = mdy_hms(SleepDay))
```

Removed 3 duplicate records in the Sleep table.
```r
Sleep <- Sleep %>% 
  distinct()
```

Notables:

- The date columns in both tables were of the "string" data type, not the "date" data type.
- Removed 3 duplicates in the Sleep table

Data is cleaned and ready for analysis.


## Analyze
Figure 1 shows the distribution of daily activity by intensity.
<p align="center"><img width="1216" height="734" alt="Distribution of Daily Activity by Intensity (4)" src="https://github.com/user-attachments/assets/be8d2e00-35cb-404e-bcc0-8327df877225" /></p>
<p align="center"><em>Figure 1: Distribution of Daily Activity by Intensity</em></p>

Participants of the survey spent 81% of their time sedentary, 16% lightly active, 2% very active, and 1% spent fairly active. 

Inactive(sedentary) time was excluded in figure 2.
<p align="center">
  <img width="500" alt="Distribution of Non-Sedentary Daily Activity"
       src="https://github.com/user-attachments/assets/aaee87a2-0e4f-4cd5-838a-0c081c1c7432" />
</p>

<p align="center"><em>Figure 2: Distribution of Non-Sedentary Daily Activity</em></p>

The majority (85%) of activity is light activity, followed by very active (9%) and lastly, fairly active (6%). This indicates sedentary behavior dominates daily time, with light activity dominating non-sedentary activity.


Average awake time in bed was 39.3 minutes. This statistic is inline with other reports from different datasets, reaffirming validity and confidence in the quality of our data.<sup>[[1](https://www.whoop.com/us/en/thelocker/average-sleep-stages-time/)]</sup> Additionally, external research states the quality of sleep is negatively correlated with awake time in bed.<sup>[[2](https://www.semanticscholar.org/paper/Is-insomnia-disorder-associated-with-time-in-bed-Birling-Li/b9c25b4e035faedf4c018897ffd8203f7207a505)]</sup> This suggests higher awake-in-bed time is associated with poorer sleep quality, indicating an opportunity for Bellabeat to focus on sleep efficiency rather than duration.


Inactive(Sedentary) time is negatively correlated with total sleep time as seen in figure 3.
<p align="center"><img width="700" height="734" alt="Sedentary Time vs  Total Sleep" src="https://github.com/user-attachments/assets/2dcb7d75-eaf9-458f-b781-6f83d9208313" /></p>

<p align="center"><em>Figure 3: Sedentary Time vs. Total Sleep</em></p>

The strength of the correlation is strong. With 1 being perfect positive correlation, -1 being perfect negative correlation and 0 being no correlation, the strength of this correlation is -0.6. This indicates that on any given day, being sedentary will yield less sleep. Furthermore, sedentary behavior appears more important than non-sedentary activity for total time asleep.



Figure 4 displays non-sedentary activity against calories burnt.
<p align="center"><img width="700" height="734" alt="Non-Sedentary Activity vs  Calories Burnt" src="https://github.com/user-attachments/assets/d8d4c3ad-dec6-4e5a-a34a-6729b292c898" /></p>
<p align="center"><em>Figure 4: Non-Sedentary Activity vs. Calories Burnt</em></p>

Non-sedentary activity is associated with more calories burnt, additionally, the intensity of activity differs in efficiency concerning calorie burning. Very active intensity had a 0.6 correlation, fairly active intensity had a 0.3 correlation and lightly active intensity had a 0.29 correlation. Light activity is almost just as efficient at calorie burning as fairly active, however, light activity dominates the total distribution of daily non-sedentary activity. This suggests consistent light activity is the main driver of calories burnt.

## Share
Tableau was utilized to create and export the visualization for this case study.
These are the most relevant insights from this dataset:

1. Sedentary behavior is negatively associated with total time asleep.
2. Consistent light activity accounted for the majority of calories burnt.
3. People spend most time in their day being sedentary.

These insights were communicated using the following visuals:

- Distribution of Daily Activity by Intensity: 
Showcases sedentary time being the bulk of daily activity.

- Distribution of Non-Sedentary Daily Activity: 
Showcases light activity being the majority of non-sedentary activity.

- Sedentary time vs. Total Sleep:
Showcases the negative correlation between daily sedentary time and total sleep.

Non-Sedentary Activity vs. Calories Burnt:
Showcases the positive correlation between activity and calories burnt
<p align="center"><img width="1696" height="754" alt="Relationship between Activity, Calories and Sleep" src="https://github.com/user-attachments/assets/12aa8089-330f-4e56-8b4a-49e27bc71ec3" /></p>
<p align="center">Tableau Dashboard</em></p>



## Act
Health outcomes are more strongly associated with reducing sedentary time and maintaining consistent light activity than with high-intensity exercise.

1. Because sedentary time is associated with less sleep, Bellabeat's marketing team should focus on the effects of a sedentary lifestyle and propose the Bellabeat app as a way of monitoring activity so that users associate better sleep with active, low-intensity, movement.

2. Due to light activity making the bulk of calories burnt, as well as light activity being almost as efficient at burning calories compared to stronger intensities, Bellabeat's marketing team should stress the effectiveness of consistent low-effort activity so that users feel calorie burn is attainable without high-intensity exercise.

3. Because external research shows awake time in bed is associated with worse sleep, Bellabeat's marketing team should focus on the ability of the app to measure time in bed separate for sleep time, allowing users to find how long their awake time in bed is, enabling users to monitor their sleeping quality rather than their total time asleep.


## Appendix: code
This appendix holds the R code that produced this analysis. Unnecessary code was removed for clarity and viewing purposes. Exploratory visualizations were initially created in R using ggplot2 to validate patterns prior to final visualization in Tableau.

#### Library and Loading
```r
library(tidyverse)
library(lubridate)
library(skimr)
library(janitor)

Activity <- read.csv("dailyActivity_merged.csv")
Sleep <- read.csv("sleepDay_merged.csv")
```

#### Data Cleaning
```r
# Identify duplicate records
Activity %>% 
  get_dupes(Id, ActivityDate)

Sleep %>% 
  get_dupes(Id, SleepDay)

# Convert date columns to "date" data type
Activity <- Activity %>% 
  mutate(ActivityDate = as.Date(ActivityDate, format = "%m/%d/%Y"))
Sleep <- Sleep %>% 
  mutate(SleepDay = mdy_hms(SleepDay))

# Removed duplicates in the sleep column
Sleep <- Sleep %>% 
  distinct()
```

#### Activity Intensity Distribution
```r
Activity %>% 
  summarise(
    VeryActiveMinutes = sum(VeryActiveMinutes),
    SedentaryMinutes = sum(SedentaryMinutes),
    LightlyActiveMinutes = sum(LightlyActiveMinutes),
    FairlyActiveMinutes = sum(FairlyActiveMinutes),
  )
TotalMinutes =
  sum(Activity$VeryActiveMinutes) + 
  sum(Activity$SedentaryMinutes) + 
  sum(Activity$LightlyActiveMinutes) +
  sum(Activity$FairlyActiveMinutes)

Activity %>% 
  summarise(
    VeryActive = (sum(VeryActiveMinutes) / TotalMinutes) * 100,
    Sedentary = (sum(SedentaryMinutes) / TotalMinutes) * 100,
    LightlyActive = (sum(LightlyActiveMinutes) / TotalMinutes) * 100,
    FairlyActive = (sum(FairlyActiveMinutes) / TotalMinutes) * 100
  )

IntensityTotals <- Activity %>% 
  summarise(
    VeryActive = sum(VeryActiveMinutes),
    Sedentary = sum(SedentaryMinutes),
    LightlyActive = sum(LightlyActiveMinutes),
    FairlyActive = sum(FairlyActiveMinutes)
  ) %>% 
  pivot_longer(
    cols = everything(),
    names_to = "Intensity",
    values_to = "Minutes"
  ) %>% 
  mutate(Percent = Minutes / sum(Minutes) * 100)

# Exploratory Visuals
ggplot(IntensityTotals, aes(x = Intensity, y = Percent)) +
  geom_col() + 
  labs(title = "Intensity by time %")
```

#### Non-Sedentary Activity Distribution
```r
IntensityNonSedentary <- Activity %>% 
  summarise(
    VeryActive = sum(VeryActiveMinutes),
    LightlyActive = sum(LightlyActiveMinutes),
    FairlyActive = sum(FairlyActiveMinutes)
  ) %>% 
  pivot_longer(
    cols = everything(),
    names_to = "Intensity",
    values_to = "Minutes"
  ) %>% 
  mutate(Percent = Minutes / sum(Minutes) * 100)


# Exploratory Visuals
ggplot(IntensityNonSedentary, aes(x = Intensity, y = Percent)) +
  geom_col() + 
  labs(title = "Intensity by time %")
```

#### Awake Time in Bed vs. Total Time Asleep
```r
Sleep <- Sleep %>% 
  mutate(AwakeTimeInBed = TotalTimeInBed - TotalMinutesAsleep)

# Exploratory Visuals
ggplot(Sleep, aes(x = AwakeTimeInBed, y = TotalMinutesAsleep)) +
  geom_point() +
  geom_smooth(method = "lm", se = FALSE)

# No statistical correlation is observed
cor(Sleep$AwakeTimeInBed, Sleep$TotalMinutesAsleep)
```

#### Total Sleep and Total Activity Trends over Time
```r
# Change in total sleep over time
# Exploratory Visuals
ggplot(Sleep, aes(x = SleepDay, y = TotalMinutesAsleep, group = Id)) + 
  geom_line()
Sleep_avg <- Sleep %>% 
  group_by(SleepDay) %>% 
  summarize(DailyAvgSleep = mean(TotalMinutesAsleep))
ggplot(Sleep_avg, aes(x = SleepDay, y = DailyAvgSleep)) + 
  geom_line()

# Change in total activity over time
Activity <- Activity %>% 
  mutate(TotalMinutesActive = VeryActiveMinutes + FairlyActiveMinutes + LightlyActiveMinutes)

Active_avg <- Activity %>% 
  group_by(ActivityDate) %>% 
  summarize(AverageMinutesActive = mean(TotalMinutesActive))

ggplot(Active_avg, aes(x = ActivityDate, y = AverageMinutesActive)) +
  geom_line()

# No correlation was observed is neither total sleep over time nor total activity over time
```

#### Activity and Sleep Relationship
```r
Activity_join <- Activity %>% 
  select(Id, ActivityDate,TotalMinutesActive, VeryActiveMinutes, SedentaryMinutes, FairlyActiveMinutes)

Sleep_join <- Sleep %>% 
  select(Id, SleepDay, TotalMinutesAsleep, TotalTimeInBed, AwakeTimeInBed)

ActivitySleep <- inner_join(
  Activity_join, Sleep_join, by = c("Id" = "Id", "ActivityDate" = "SleepDay")
  )

# Exploratory Visuals
ggplot(ActivitySleep, aes(x = TotalMinutesActive, y = TotalMinutesAsleep)) + 
  geom_point(alpha = 0.5) +
  geom_smooth(method = "lm", se = FALSE)

ggplot(ActivitySleep, aes(x = SedentaryMinutes, y = TotalMinutesAsleep)) + 
  geom_point(alpha = 0.5) +
  geom_smooth(method = "lm", se = FALSE)

# There is a strong negative correlation between sedentary activity and total minutes asleep
cor(ActivitySleep$SedentaryMinutes, ActivitySleep$TotalMinutesAsleep)
cor(ActivitySleep$TotalMinutesActive, ActivitySleep$TotalMinutesAsleep)
```

#### Activity Intensity and Calories Relationship
```r
# Exploratory Visuals
ggplot(Activity, aes(x = Calories, y = VeryActiveMinutes)) +
  geom_point() +
  geom_smooth(method = "lm", se = FALSE)

ggplot(Activity, aes(x = Calories, y = FairlyActiveMinutes)) +
  geom_point() +
  geom_smooth(method = "lm", se = FALSE)

ggplot(Activity, aes(x = Calories, y = LightlyActiveMinutes)) +
  geom_point() +
  geom_smooth(method = "lm", se = FALSE)

# Finding the correlation between activity intensity and calories burnt
cor(Activity$VeryActiveMinutes, Activity$Calories) # ~0.6
cor(Activity$FairlyActiveMinutes, Activity$Calories) # ~0.3
cor(Activity$LightlyActiveMinutes, Activity$Calories) # ~ 0.29

# Slight negative correlation between calories burnt and sedentary activity
# Exploratory Visuals
ggplot(Activity, aes(x = Calories, y = SedentaryMinutes)) +
  geom_point() +
  geom_smooth(method = "lm", se = FALSE)
cor(Activity$SedentaryMinutes, Activity$Calories)
```

#### Exporting Data for Tableau Visualizations
```r
#Sedentary time is negatively correlated with total sleep
SedentarySleep <- ActivitySleep %>%
  select(SedentaryMinutes, TotalMinutesAsleep)

write.csv(SedentarySleep, "SedentarySleep.csv", row.names = FALSE)

#Percentage of non sedentary activity
write.csv(IntensityNonSedentary, "NonSedentaryActivity.csv", row.names = FALSE)

#Percentage of all activity
write.csv(IntensityTotals, "PercentActivity.csv", row.names = FALSE)

#Total Activity is positively correlated with calories burnt
ActivityCalories <- Activity %>% 
  select(TotalMinutesActive, Calories)

write.csv(ActivityCalories, "ActivityCalories.csv", row.names = FALSE)
```

---
#### Footnotes
[1] Source: https://www.whoop.com/us/en/thelocker/average-sleep-stages-time/

[2] Source: https://www.semanticscholar.org/paper/Is-insomnia-disorder-associated-with-time-in-bed-Birling-Li/b9c25b4e035faedf4c018897ffd8203f7207a505
