Exercise 3_2
================

## Kaggle Wellbeing Dataset

For this exercise, we are using the kaggle well-being dataset, which
contains 15,977 survey responses with 24 attributes (Main-Page:
<https://www.kaggle.com/datasets/ydalat/lifestyle-and-wellbeing-data>)

**Kaggle dataset attributes**

| Attribute               | Meaning                                                                               | Format                                            |
|-------------------------|---------------------------------------------------------------------------------------|---------------------------------------------------|
| Timestamp               | Date when survey was completed                                                        | M/D/YY                                            |
| FRUITS_VEGGIES          | HOW MANY FRUITS OR VEGETABLES DO YOU EAT EVERYDAY?                                    | 0-5 (None - Servings per day)                     |
| DAILY_STRESS            | HOW MUCH STRESS DO YOU TYPICALLY EXPERIENCE EVERYDAY?                                 | 0-5 (Not much stress - A lot of stress)           |
| PLACES_VISITED          | HOW MANY NEW PLACES DO YOU VISIT?                                                     | 0-10 (None - New places (or more))                |
| CORE_CIRCLE             | HOW MANY PEOPLE ARE VERY CLOSE TO YOU?                                                | 0-10 (None - People or more)                      |
| SUPPORTING_OTHERS       | HOW MANY PEOPLE DO YOU HELP ACHIEVE A BETTER LIFE?                                    | 0-10 (None - Persons or more)                     |
| SOCIAL_NETWORK          | WITH HOW MANY PEOPLE DO YOU INTERACT WITH DURING A TYPICAL DAY?                       | 0-10 (None - Persons or more)                     |
| ACHIEVEMENT             | HOW MANY REMARKABLE ACHIEVEMENTS ARE YOU PROUD OF?                                    | 0-10 (None - Achievements or more)                |
| DONATION                | HOW MANY TIMES DO YOU DONATE YOUR TIME OR MONEY TO GOOD CAUSES?                       | 0-5 (None - Or more)                              |
| BMI_RANGE               | WHAT IS YOUR BODY MASS INDEX (BMI) RANGE?                                             | 1-2 (Below 25 - Above 25)                         |
| TODO_COMPLETED          | HOW WELL DO YOU COMPLETE YOUR WEEKLY TO-DO LISTS?                                     | 0-10 (Not at all - Very well)                     |
| FLOW                    | IN A TYPICAL DAY, HOW MANY HOURS DO YOU EXPERIENCE “FLOW”?                            | 0-10 (None - Hours per day or more                |
| DAILY_STEPS             | HOW MANY STEPS (IN THOUSANDS) DO YOU TYPICALLY WALK EVERYDAY?                         | 1-10 (Less than 1,000 - Thousand steps            |
| LIVE_VISION             | FOR HOW MANY YEARS AHEAD IS YOUR LIFE VISION VERY CLEAR FOR?                          | 0-10 (I do not have a life vision- Years or more) |
| SLEEP_HOURS             | ABOUT HOW LONG DO YOU TYPICALLY SLEEP?                                                | 1-10 (Hours per night)                            |
| LOST_VACATION           | HOW MANY DAYS OF VACATION DO YOU TYPICALLY LOSE EVERY YEAR ?                          | 0-10 (None-Vacation Days)                         |
| DAILY_SHOUTING          | HOW OFTEN DO YOU SHOUT OR SULK AT SOMEBODY?                                           | 0-10 (None - Times per week, or more)             |
| SUFFICIENT_INCOME       | HOW SUFFICIENT IS YOUR INCOME TO COVER BASIC LIFE EXPENSES?                           | 1-2 (Not or hardly sufficient - Sufficient)       |
| PERSONAL_AWARDS         | HOW MANY RECOGNITIONS HAVE YOU RECEIVED IN YOUR LIFE?                                 | 0-10 (None - Recognitions or more)                |
| TIME_FOR_PASSION        | HOW MANY HOURS DO YOU SPEND EVERYDAY DOING WHAT YOU ARE PASSIONATE ABOUT?             | 0-10 (None - Hours)                               |
| WEEKLY_MEDITATION       | IN A TYPICAL WEEK, HOW MANY TIMES DO YOU HAVE THE OPPORTUNITY TO THINK ABOUT YOURSELF | 0-10                                              |
| AGE                     | AGE GROUPS                                                                            | \-                                                |
| GENDER                  | MALE OR FEMALE                                                                        | \-                                                |
| WORK_LIFE_BALANCE_SCORE | SCORE CALCULATED BY AH.COM ALGORITHM AND REPORTED TO USER IN THE FIRST REPORT         | 480-820                                           |

**Download**

Please download the [data set from our github
repository](https://github.com/tud-ise/Wellbeing_SoSe2023/blob/main/Exercises/Exercise%203/wellbeing.csv).

## Data Preparation

**Open the data set**

    data = read.csv("**Your Path** /wellbeing.csv")
    df = data

**Check the data structure**

    summary(df)
    str(df)

**Format Columns**

The column `Timestamp` is currently loaded as character (`chr`). If we
want to work with a `time series` we need to format the date column into
the `date` format.

    library(lubridate)
    df$Timestamp <- mdy(df$Timestamp)

Most regression libraries will expect a `factor` instead of `character`
as categorical variables. Therefore we transform the data type of `Age`
and `Gender` into `factors`:

    df$AGE = as.factor(df$AGE)
    df$GENDER = as.factor(df$GENDER)

Test if all transformations were successful:

    str(df)

## Data Analysis

### Descriptive Statistics

First, we should use descriptive statistics to describe and summarize
the data. They can be used to get first impressions and correlations
within the data set.

The probably most common used functions are `mean()` (average);
var()(variance); `sd()` (standard deviation) and `cor()` (correlation)

**Examples**

How many fruits or vegetables does the average participant eat every
day?

    mean(df$FRUITS_VEGGIES)

Comparison of male and female.

      mean(df[df$GENDER=="Female", 2]) #2 is the column number of Fruits_Veggies
      mean(df[df$GENDER=="Male", 2]) 
      

or

      aggregate(df$FRUITS_VEGGIES, list(df&GENDER))

What is the standard deviation of `Fruits_Veggies`?

    sd(df$FRUITS_VEGGIES)

What does the result indicate?

Do fruits or vegetables eaten correlate with received stress?

    cor(df$FRUITS_VEGGIES, df$DAILY_STRESS)

Coefficient values can range from +1 to -1, where +1 indicates a perfect
positive relationship, -1 indicates a perfect negative relationship, and
0 indicates no relationship exists.

**Your Turn**

Do younger participants sleep less than older people? Which group
received the most sleep?

Can we see a high correlation between the number of steps done per day
and places visited in our data set?

### Regression

Next, we take a look at inferential statistics to see if we have
statistically significant effects. Our examples will focus on a simple
OLS regression, `lm()`. Just be aware R has a lot of additional
functions to do more extended regressions like `glm()`. If you want to
do predictions for categorical data (group comparison), you typically
use ANOVAs (`aov()`) or t-tests (`t.test()`).

Let’s test if we can find a significant effect of eaten fruits/
vegetables and places visited on received achievements.

    wlb_ols = lm(ACHIEVEMENT~ FRUITS_VEGGIES + PLACES_VISITED, data = df)
    summary(wlb_ols)

We can plot a simple regression:

    df_plot = head(df, 50) #to make the graph clearer for the exercise, don't reduce your data set for your analysis
    wlb_ols = lm(ACHIEVEMENT~ FRUITS_VEGGIES, data = df_plot)
    summary(wlb_ols)

    plot(df_plot$FRUITS_VEGGIES , df_plot$ACHIEVEMENT)
    abline(wlb_ols)

Let’s implement gender as control variable to improve our model:

    wlb_ols = lm( ACHIEVEMENT  ~ FRUITS_VEGGIES + PLACES_VISITED + GENDER   , data = df) #Achievement as dependent variable and FRUITS_VEGGIES and PLACES_VISITED as independent variables.
    summary(wlb_ols)

Let’s add some more independent variables and try to improve our model:

    wlb_ols = lm( df$ACHIEVEMENT  ~ df$FRUITS_VEGGIES + df$PLACES_VISITED + df$GENDER + df$DAILY_STRESS+ df$CORE_CIRCLE + df$SUPPORTING_OTHERS+df$SOCIAL_NETWORK + df$DONATION  , data = df) 
    summary(wlb_ols)

We can not only use regressions to test statistical significance, but we
can also predict future events.

    wlb_ols = lm(  WORK_LIFE_BALANCE_SCORE  ~ FRUITS_VEGGIES + PLACES_VISITED + GENDER + AGE  , data = df) 
    summary(wlb_ols)


    new_data = data.frame(
      FRUITS_VEGGIES = c(5, 0, 3), PLACES_VISITED = c(10,0,5), AGE = as.factor(c("21 to 35","21 to 35","21 to 35")), GENDER = as.factor(c("Male","Male","Male"))
    )

    str(new_data)

    predict(wlb_ols, newdata = new_data)
