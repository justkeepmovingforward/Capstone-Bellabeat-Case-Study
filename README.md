# Bellabeat Case Study

Capstone Project for Google Data Analytics Certificate 

![image](https://github.com/user-attachments/assets/d585d2e7-10ca-4c58-9ea3-bc5403ce6a1d)


## Table of Contents

- [Business Task](#Business-Task)
- [Stakeholders](#Stakeholders)
- [Featured Product](#Featured-Product)
- [About the Company](#About-the-Company)
- [Ask](#Ask)
- [Prepare](#Prepare)
    - [Fitbit Fitness Tracker Dataset](#Fitbit-Fitness-Tracker-Dataset)
    - [Gym Members Exercise Dataset](#Gym-Members-Exercise-Dataset)
- [Process](#Process)
- [Analyze](#Analyze)
    - [Correlation](#Correlation)
    - [Analysis Summary](#Analysis-Summary)
- [Share](#Share)
- [Act](#Act)

## Business Task

Focus on one of Bellabeat’s products and analyze usage data from smart devices to understand how users are currently engaging with health and wellness technology. Based on these insights, provide high-level recommendations to help inform Bellabeat’s marketing strategy.

## Stakeholders

* Urška Sršen: Bellabeat’s cofounder and Chief Creative Officer
* Sando Mur: Mathematician and Bellabeat’s cofounder; key member of the Bellabeat executive team
* Bellabeat marketing analytics team

## Featured Product

* Spring: A smart water bottle that monitors daily water intake using integrated technology to help users maintain proper hydration. It syncs with the Bellabeat app to log and analyze hydration levels.

## About the Company

Bellabeat is a wellness technology company that creates health-focused smart products designed to inform and inspire women worldwide. By collecting data on activity, sleep, stress, and reproductive health, Bellabeat empowers women with personalized insights into their wellness habits.

By 2016, Bellabeat had expanded globally, opening offices around the world and launching multiple products. Its products became available through a growing number of online retailers, in addition to its own e-commerce platform. While the company has invested in traditional advertising channels—such as radio, billboards, print, and television—it places a strong emphasis on digital marketing.

## Ask

1. Are smart device users achieving an active lifestyle by averaging at least 7,500 steps per day and limiting sedentary time to under 8 hours, as recommended by the World Health Organization, over a 60-day period?
2. Are users demonstrating signs of good health by sleeping at least 7 hours per night and maintaining a resting heart rate between 60–100 bpm during the 60-day data collection period?
3. How frequently do users log data on their devices throughout the 60-day span?
4. On which days and at what times are users most active?
5. How can the identified behavioral trends and health engagement metrics inform Bellabeat’s marketing strategy for the Spring smart water bottle?

## Prepare

#### Dataset Overview

This project uses two datasets to analyze user behavior and health engagement patterns related to smart devices. These datasets help address the business questions about lifestyle, physical activity, sleep quality, and user engagement—factors critical to developing marketing insights for Bellabeat's Spring smart water bottle.

## Fitbit Fitness Tracker Dataset

The Fitbit Fitness Tracker Dataset (CC0: Public Domain) was sourced from Kaggle via the Mobius platform https://www.kaggle.com/datasets/arashnic/fitbit . It contains personal fitness data from 35 consenting Fitbit users, collected over a two-month period. The dataset offers minute-level and daily-level information on:

* Physical activity
* Heart rate
* Sleep patterns
* Weight logging
* These features provide behavioral insights that align closely with our business objectives.

#### Structure

The dataset is split into two groups, each covering a different month:

* Group 1: March 12, 2016 – April 11, 2016 (11 CSV files)
* Group 2: April 12, 2016 – May 12, 2016 (18 CSV files)

Most files use Id as a unique user identifier, paired with time-based columns such as ActivityDate, Date, or Datetime for tracking user behavior over time.

#### Data Integrity, Licensing, and Limitations

* Integrity: Data was checked for duplicates, missing values, and user ID consistency.

* Licensing: CC0 (Public Domain) license—freely usable without privacy concerns.

* Limitations:

  * Small sample size (n=35), not representative of the broader population.
  * Dated (from 2016), which may affect current relevance.
  * No demographic fields (e.g., age, gender, occupation)
  * Lacks water intake data, which limits direct insight into Spring water bottle usage. A secondary dataset is explored to supplement this.

#### Suitability for Analysis

Despite its limitations, the dataset effectively supports analysis of:

* Physical activity (e.g., step count, sedentary time)
* Sleep quality
* Heart rate trends
* Caloric expenditure
* User engagement frequency
  
These metrics directly support the project's objectives and provide input for marketing strategy development.

## Gym Members Exercise Dataset

This dataset contains 973 records of gym workout sessions. Each row represents a unique session, with data on demographics, physiological metrics, and workout behaviors. The dataset can be downloaded here https://www.kaggle.com/datasets/valakhorasani/gym-members-exercise-dataset

Key Structure and Features

* Demographics: Age, Gender, Experience_Level (1–3)
* Physical Attributes: Weight, Height, BMI, Fat_Percentage
* Health Metrics: Max_BPM, Avg_BPM, Resting_BPM, Calories_Burned
* Behavioral Patterns: Workout_Type, Workout_Frequency, Session_Duration, Water_Intake

No date or timestamp fields—this dataset is cross-sectional rather than time-series.

#### Data Integrity, Licensing, and Limitations

* Integrity: No missing values or duplicates.

* Licensing: Apache 2.0 License—modification and redistribution allowed with attribution.

* Limitations:
  
 * No temporal granularity (no date/time per session).
 * No unique user ID—cannot track progress across sessions.
 * Modest size (n=973); may not generalize widely.
 * No lifestyle context (e.g., diet, sleep, occupation).

#### Suitability for Analysis

Despite constraints, this dataset enables:

* Comparison of workout intensity by gender or experience level
* Correlation studies (e.g., active minutes vs. calories burned)
* Segmentation by workout type or fat percentage
* Exploratory modeling of potential health trends and performance indicators

It complements the Fitbit data by offering richer demographic and physiological detail, making it useful for identifying trends that could support audience targeting and personalization in Bellabeat’s marketing.

## Process

#### 1. Tools Used

I conducted the analysis in RStudio Cloud for its accessibility and seamless integration with tidyverse. Key R libraries used:

    install.packages("tidyverse")
    install.packages("lubridate")
    
    library(tidyverse)
    library(lubridate)

Additional tools:

* Google BigQuery: For merging large datasets, specifically heartrate_seconds_merged and sleepDay_merged.

* Canva: For visualization and presentation of insights.


#### 2.  Folder Organization

To manage files by time period, I renamed the dataset folders to:

* Marchdata (March 12 – April 11)

* Aprildata (April 12 – May 12)

#### 3. Dataset Selection

I identified common files across both months using list.files() and intersect() in R. Based on business relevance, I selected the following datasets:

* dailyActivity_merged.csv

* minuteMETsNarrow_merged.csv

* weightLogInfo_merged.csv
  
* minuteStepsNarrow_merged.csv

* sleepDay_merged.csv (April only)
  
* heartrate_seconds_merged.csv (April only)

These files include key behavioral indicators such as step count, calories burned, heart rate, sleep, and weight metrics.

#### 4. File Renaming

To avoid import conflicts, I batch-renamed files with month-specific prefixes using file.rename():

    March_data <- list.files(March_working_directory)
    #placing "March" filename prefix in a variable
    file_prefix <- "March"
    #using for loop and file.rename() to rename files by batch
    #using paste to concatenate strings
    for (x in March_data) {file.rename(x, paste(file_prefix,x,sep="_"))}

#### 5. Data Quality Checks

* Duplicates: Checked using duplicated():

      sum(duplicated(March_dailyActivity_merged))

      
* Missing Values: Checked with sum(is.na()) across datasets:

      sum(is.na(March_dailyActivity_merged))


#### 6. User ID Consistency 

I verified unique user IDs using distinct(Id) to confirm overlap across datasets. Since I computed averages at the user level, I retained all participants to preserve behavioral variability.

![image](https://github.com/user-attachments/assets/8bdcba5f-6178-492b-a807-ee1f435fe14a)

#### 7. Merging Datasets 

For datasets present in both months, I combined them using bind_rows() or union() depending on structure. I verified row counts before and after merging to ensure data integrity.

#### 8. Gym dataset has no null values and duplicates. 
columns were cleaned using R's janitor library.

	gym <- janitor::clean_names(gym)

#### 9. Assumptions and Limitations

* Assumed general behavioral consistency (e.g., sleep, steps, heart rate) between March and April.
* Data may include manual entries (e.g., weight logs) and auto-tracked data (e.g., steps), affecting reliability.
* Not all users had complete data across all categories—some comparisons rely on partial records.


## Analyze

### 1a. Are smart device users achieving an active lifestyle by taking at least 7500 steps per day and limiting sedentary time to under 8 hours?

Dataset used: March_dailyActivity_merged and April_dailyActivity_merged
* no duplicates on each file and no null values
* 35 user Ids

Getting the average:

        library(dplyr)
    
        cleaned_Daily_Activities <- Daily_Activities %>%
        select(Id, TotalSteps, VeryActiveMinutes, FairlyActiveMinutes, LightlyActiveMinutes, SedentaryMinutes, Calories)
    
        Steps <- cleaned_Daily_Activities %>%
        group_by(Id) %>%
        summarize(
            steps_average = mean(TotalSteps),
            sedentary_minutes_average = mean(SedentaryMinutes),
            active_minutes_average = mean(VeryActiveMinutes + FairlyActiveMinutes + LightlyActiveMinutes),
            calories_average = mean(Calories)
        )
I classified users using the Tudor-Locke classification system: 
source https://www.researchgate.net/figure/Classification-of-activity-according-to-Tudor-Locke-et-al_tbl1_375576827

* <5,000 steps/day – Sedentary

* 5,000–7,499 steps/day – Low active

* 7,500–9,999 steps/day – Somewhat active

* 10,000–12,499 steps/day – Active

* 12,500+ steps/day – Highly active
  
#### Summary:

* Average total steps: 6,982

* Average sedentary time: 1,004 minutes (≈ 16 hours and 40 minutes)

* Average active time: 216 minutes (≈ 3 hours and 36 minutes)

* Average calories burned: 2,249

* Only 15 out of 35 (42.86%) were active or somewhat active.

### 1b.Are users meeting physical activity guidelines based on METs?

Dataset used: March_minuteMETsNarrow_merged and April_minuteMETsNarrow_merged
* 35 user Ids

I calculated the average MET (Metabolic Equivalent of Task) per user.

    minuteMETs <- union(March_minuteMETsNarrow_merged, April_minuteMETsNarrow_merged)

    MET_average <- minuteMETs %>%
    group_by(Id) %>%
    summarize(MET_average = mean(METs))

#### Summary:

* Average MET = 14.42, with a range from 10.21 to 19.81, reflects mostly light to moderate activity — consistent with the step and active minutes data.
* MET showed a positive correlation with steps, indicating that higher step counts were generally associated with increased energy expenditure. This suggests that most physical activity recorded was step-based, such as walking or running.


### 2a. Are users demonstrating signs of good health based on sleeping at least 7 hours per night?

Dataset used: "sleepDay_merged.csv" (April 12 to May 12)
* No null values. Removed 3 duplicates. Resulting unique sleep records of 410 entries
* 24 user Ids

        library(lubridate)
        Sleep <- sleepDay_merged
        Sleep$date <- mdy_hms(Sleep$date)
    
        Sleep_count <- Sleep %>%
        distinct(Id,SleepDay) %>%
        count()
    
        Sleep_average <- Sleep %>%
        group_by(Id) %>%
        summarize(sleep_average=round(mean(TotalMinutesAsleep)/60))

#### Summary:

* 14 out of 24 users (58.33%) averaged ≥7 hours of sleep per night.
* 41.67% fell below the healthy sleep threshold.

This suggests that just over half of the users meet the recommended sleep duration for good health (CDC & WHO guidelines).

However, a significant 41.67% are not getting adequate rest, which may increase their risk for fatigue, stress, and poor health outcomes.


### 2b. Do users maintain a healthy resting heart rate within the recommended range of 60–100 bpm?

Dataset used: April_heartrate_seconds_merged and cleaned SleepDay_merged
* no null values on both files. removed 3 duplicates from SleepDay_merged
* I used BigQuery in combining these files due to its large size.
* 10 user Ids

1. Determining the start and beginning of sleep using R 

        sleepDay <- sleepDay %>%
          mutate(
            sleep_start = SleepDay,
            sleep_end = SleepDay + minutes(TotalMinutesAsleep)
          )

2. BIGQUERY- extract entries from the heartrate table where the timestamps fall between the start_time and end_time of sleep sessions in the sleepduration table. 

       SELECT
          hr.*,sleep.TotalMinutesAsleep
        FROM
          fitbit.heartrate AS hr
        JOIN
          fitbit.sleepduration AS sleep
        ON
          hour.Id = sleep.Id
        WHERE
          hour.Time BETWEEN sleep.sleep_start AND sleep.sleep_end

5. Get average resting heartrate per user

        SELECT
          Id,
          AVG(Value) AS ave_heartrate,AVG(TotalMinutesAsleep)/60 AS ave_sleep
        FROM fitbit.heartrateXsleep_timestamps
        GROUP BY Id
  
Resting heartrate compared with sleep

    heartrateCORsleep_ave <- heartrateXsleep_AVERAGE
    cor(heartrateCORsleep_ave$ave_heartrate,heartrateCORsleep_ave$ave_sleep)
    [1] -0.1194499 

#### Summary: 

Average of resting heart rate: 67.6

The analysis of average resting heart rate showed a weak, negative correlation (-0.1194) with sleep duration, meaning there’s no significant relationship. Most users had a healthy resting heart rate (60–77 bpm), but with only 10 users, the results can’t be generalized. More data is needed for stronger conclusions.

### 3. How frequently do users log data  on their devices?

Dataset used: Weight_logs
* 2 duplicates were removed
* 13 user Ids

    Weight_combined <- bind_rows(March_weightLogInfo_merged,April_weightLogInfo_merged)

    weight_distinct <- Weight_combined %>%
    distinct()

#### Summary: 

* Logging frequency was very low overall, with 10 users logging just 1–4 times.

* Only 2 users (IDs 6962181067 and 8877689391) logged consistently (44 and 33 times respectively).

* Average weight: 80.0 kg
* Minimum: 52.8 kg
* Maximum: 131.6 kg

#### 4. What time and days are the users most active?

Dataset used: March_minuteStepsNarrow.csv and April_minuteStepsNarrow.csv


        minuteSteps$ActivityMinute <- mdy_hms(minuteSteps$ActivityMinute)
        minuteSteps <- minuteSteps %>%
             mutate(
                weekday = wday(ActivityMinute, label = TRUE, abbr = TRUE),
                 hour = hour(ActivityMinute)
             )

Average:

    avg_steps_by_day <- minuteSteps %>%
         group_by(weekday) %>%
         summarise(avg_steps = mean(Steps, na.rm = TRUE))

    avg_steps_by_hour <- minuteSteps %>%
        group_by(hour) %>%
        summarise(avg_steps = mean(Steps, na.rm = TRUE))

#### Summary

* Most active day: Saturday

* Most active hours: 12-2pm and 5-7pm


#### 5. Gym Dataset Analysis
Dataset: Cleaned 15 columns using janitor library
* 0 nulls/duplicates

##### Water Intake

* Average: 2.63 L/day
* Range: 1.5 – 3.7 L/day

##### Summary:
* 511 Male records, 462 Female records

## Correlation

A. Fitbit dataset
	
1. Steps and Active Minutes show a strong positive correlation (r = 0.731), indicating that higher step counts are closely associated with more active minutes. This suggests that steps are a good proxy for physical activity intensity and duration.
2. Steps and Calories Burned have a moderate positive correlation (r = 0.426), implying that as people take more steps, they generally burn more calories — though other factors like workout intensity or metabolism may also influence calorie burn.
3. When examining summed user-level data, physical activity appears to be associated with sleep duration:
	* Sedentary Minutes and Sleep have a strong negative correlation (r = -0.771), suggesting that users who are more sedentary overall tend to sleep significantly less.
	* Active Minutes and Sleep show a moderate positive correlation (r = 0.426), indicating that users who are more active may likely enjoy longer sleep duration.
	* Together, these findings support the idea that higher physical activity and less sedentary time are associated with better sleep outcomes.

			1. Steps vs. Active Minutes
			cor(Steps$steps_average, Steps$active_minutes_average)
			 ➣ 0.7313401
			2. Steps vs. Calories
			cor(Steps$steps_average, Steps$calories_average)
			 ➣ 0.4261802
			3a. Sedentary Minutes vs. Sleep (Summed Data)
			cor(daily_sleepSUM$sedentaryMin, daily_sleepSUM$asleep)
			 ➣ -0.7706977
			3b. Active Minutes vs. Sleep (Summed Data)
			cor(daily_sleepSUM$activemin, daily_sleepSUM$asleep)
			 ➣ 0.4264465

B. Gym dataset

Combined data for Male and Female:

1. Individuals with lower body fat tend to drink more water. This might reflect better fitness habits or overall discipline in health behaviors.
Strongest Negative Correlation: Fat Percentage (r = -0.59)

2. Taller and heavier individuals tend to drink more water.
Moderate Positive Correlations: Weight (r = 0.39) and Height (r = 0.39)

3. Longer and more intense workouts are linked to higher water intake.
Calories Burned (r = 0.36) and Session Duration (r = 0.28)

4. As session duration increases, calories burned also increases.
Strong and consistent. In short: longer workouts burn more calories—as expected.

5. More experienced and consistent gym-goers hydrate more.
Experience Level (r = 0.30) and Workout Frequency (r = 0.24)

6. Water intake is not significantly influenced by age, heart rate, or BMI.
No Meaningful Correlation: Age (r = 0.04), Max BPM (r = 0.03), Average BPM, Resting BPM, and BMI

Summary: 

* Water intake is mostly influenced by physical size, workout intensity, and fitness experience. It’s not associated with age or general heart rate metrics. The strong negative correlation with fat percentage could suggest that more fit individuals are also more mindful of hydration.
* This supports the idea that water intake is part of broader healthy habits among experienced or active members.

		1. > cor(gym$water_intake_liters,gym$fat_percentage)
		 ➣ -0.5886828
  		2. > cor(gym$water_intake_liters,gym$weight_kg)
		 ➣ 0.3942757
		> cor(gym$water_intake_liters,gym$height_m)
		 ➣ 0.3935329
  		3. > cor(gym$water_intake_liters,gym$calories_burned)
		 ➣ 0.3569307
		  > cor(gym$water_intake_liters,gym$session_duration_hours)
		 ➣ 0.283411
  		4. > cor(gym$session_duration_hours,gym$calories_burned)
		 ➣ 0.9081404
  		5. > cor(gym$water_intake_liters,gym$experience_level)
		 ➣ 0.3041035
  		> cor(gym$water_intake_liters,gym$workout_frequency_days_week)
		 ➣ 0.2385626

#### How Combined Data Differs from Gender-Based Analysis:

* Body Size: In the combined data, weight and height show a moderate positive correlation with water intake (r = 0.39), but this disappears when separated by gender.

* Fat Percentage: The negative correlation with fat percentage is strongest in the combined dataset (r = -0.59), with females showing a stronger link (r = -0.53) than males (r = -0.43).

* Physical Activity: Females show a stronger correlation between water intake and workout duration/frequency than males.

* Experience Level: Only the combined data shows a moderate positive correlation with experience level (r = 0.30), which wasn’t captured in gender-based analysis.

* Heart Rate & BMI: No significant correlations with water intake in any dataset.

## Analysis Summary

🔎 Majority of users are not meeting physical activity recommendations, spending excessive time sedentary.

* Avg. steps/day: 6,982 — in the “low active” category.
* Avg. sedentary time: 1,004 mins/day (~16 hrs) — far exceeds the recommended max of 8 hrs/day.
* Only 42.86% are somewhat active-active. 20% reached 10,000 steps/day and 22.86% were “somewhat active”. 
	
🔎 Activity intensity is mostly light to moderate, which matches the step and active minutes data.

* Avg. MET: 14.42, ranging from 10.21 to 19.81.

🔎 Just over half get healthy sleep; rest may be at risk for fatigue, stress, or health issues.

* 58.33% average ≥7 hours sleep/night (CDC/WHO recommendation).
* 41.67% fall short.

🔎 No strong link between sleep and resting heart rate. Sample size (10 users) is too small to generalize.

* Avg. resting HR during sleep: 60–77 bpm (within healthy range).
* Weak negative correlation with sleep duration (r = -0.12).

🔎Device usage for logging is minimal, indicating low user engagement with tracking features.

* Most users log weight infrequently (1–4 times).
* Only 2 users log weight consistently.
* Weight range: 52.8 – 131.6 kg; Avg: 80.0 kg.

🔎 Users are more active during weekends and midday to early evening, possibly due to free time or routine workouts.

* Most active day: Saturday
* Most active time slots: 12–2 PM and 5–7 PM

🔎 People who drink more water tend to be more physically active and have less body fat.

* They work out longer, more often, and burn more calories.
* This pattern is stronger in women than in men.
* Women who drink more water are more likely to have lower body fat compared to men.

🔎 Water intake doesn’t seem connected to things like age, body size, or heart rate.

 * It has little to no link with age, weight, height, BMI, or heart rate for both men and women.

🔎 Fat percentage has the strongest link with water intake in the combined Male & Female data.

* Strong negative correlation with fat percentage (r = -0.59), suggesting that individuals with lower body fat tend to drink more water.
* For gender-based analysis, women show a slightly stronger link between lower fat percentage and water intake (r = -0.53) than men (r = -0.43).
 
🔎 Physical activity correlates with water intake.

* Longer and more intense workouts are associated with higher water intake.
* For females, the link between water intake and workout duration/frequency is stronger than for males.
* Experience level also influences water intake, but this correlation is only visible in the combined dataset (r = 0.30), not in the gender-based analysis.

## Share


![image](https://github.com/user-attachments/assets/67c3f71f-2f33-42fd-907b-2d8ad55814ed)

![image](https://github.com/user-attachments/assets/a8958a76-e481-457e-ad16-8c07614ce8d4)

![image](https://github.com/user-attachments/assets/affa2ac5-c621-453c-ae87-31a14d5d74a8)

![image](https://github.com/user-attachments/assets/c83a53ea-6c31-44ed-b308-a84db835fff4)

![image](https://github.com/user-attachments/assets/9fbd062f-d79d-4380-ac45-dd017539d688)

![image](https://github.com/user-attachments/assets/7aad36a8-1168-493e-9523-2896b1fbbb59)

![image](https://github.com/user-attachments/assets/abccfc25-92bf-4cc2-a5ac-6ec433477d2e)

![image](https://github.com/user-attachments/assets/47ae2e48-5795-481b-9329-11764d7b6257)

![image](https://github.com/user-attachments/assets/178a5eec-18df-4d8a-86e3-57be7d7ab5e4)

![image](https://github.com/user-attachments/assets/2d9eca11-fbe5-4ee0-92f5-2e4755b23095)

![image](https://github.com/user-attachments/assets/3b6fc2c9-3100-49eb-9dc5-d925f4a0a256)

![image](https://github.com/user-attachments/assets/1b803772-d9c6-41fa-bef5-52dcb10d48a3)



## Act (Recommendation)

1. Make progress visible to boost motivation
	* Users aren’t engaging much with the app. To fix this, Bellabeat can add a visual element — like an avatar or plant that changes based on hydration and activity. If users don’t drink enough water, the avatar looks tired. As they stay active and hydrated, it looks better and healthier. This helps users see their progress, which makes them more likely to stick to healthy habits.
	📚 Backed by research: Voigt & Kauffeld, 2024

2. Promote walking to improve sleep and hydration
	* The data shows that people who are more active sleep better and drink more water. We can encourage walking at least 7,500 steps a day as a simple goal to avoid insomnia and stay hydrated.

3. Focus on women as the main users
	* Women in the data showed higher active minutes but also higher body fat and lower water intake compared to men. This might mean they’re not moving intensely enough or not drinking enough water. We can tailor the app content, messages, and features to support and educate women better.

4. Reach users when they’re most active
	* Most users are active during weekdays, likely around lunch or after work. They might be working professionals walking to eat out or meet friends. We can target these time windows through:

6. In-person promotions near restaurants
	* Partnering with workplaces
	* Wellness events like fun runs

7. Help reduce long sitting time
	* Use reminders to nudge users to move when they’ve been inactive for too long — just short walks or stretches. We can even link this to the visual avatar so they see the effect right away.

