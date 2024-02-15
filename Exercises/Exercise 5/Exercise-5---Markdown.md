Exercise 5
================

## Wellbeing - Data Set

In this exercise, we will focus on the final data set you can use for
the report, which includes your mobile and PC Screen time data as well
as the morning, evening, base, and weekly survey. We will offer two
kinds of data sets. A minimized version (wellbing_data.csv), which
contains already summarized and labeled columns, and the full version
(wellbing_data_total), so that you have the possibility to analyze
whatever you want. In this exercise, we will focus on the minimized
version of the data set.

Please be aware that this exercise gives you some examples of how to
transform and handle the data set. You can use this for your analysis,
but you should analyze the data with additional approaches using the
skills you have already learned in the exercises before.

The data set contains around 14,500 entries with 123 attributes:

**Data set Attributes**

| Attribute                   | Meaning                                         | Format     |
|-----------------------------|-------------------------------------------------|------------|
| Primarykey                  | Unique row id (generated via ID and Date)       |            |
| ID                          | User id                                         |            |
| Date                        | Timestamp                                       | YYYY-MM-DD |
| week                        | calender week                                   |            |
| BS_week                     | Number of base survey                           | 1-3        |
| WK_week                     | Number of weekly survey                         | 1-7        |
|                             |                                                 |            |
| Communication.ICT           | Mobile Screen Time - Communication technologies | Seconds    |
| Entertainment               | Mobile Screen Time - Entertainment Media        | Seconds    |
| Other                       | Mobile Screen Time - Uncategorized              | Seconds    |
| Study…Work.ICT              | Mobile Screen Time - Study Work Related Media   | Seconds    |
| Social.Media                | Mobile Screen Time - Social Media               | Seconds    |
|                             |                                                 |            |
| Communication.ICT_frequency | Mobile Frequencies - Communication technologies | Number     |
| Entertainment_frequency     | Mobile Frequencies - Entertainment Media        | Number     |
| Other_frequency             | Mobile Frequencies - Uncategorized              | Number     |
| Study…Work.ICT_frequency    | Mobile Frequencies - Study Work Related Media   | Number     |
| Social.Media_frequency      | Mobile Frequencies - Social Media               | Number     |
|                             |                                                 |            |
| ST_Communication_PC         | Screen Time - Communication technologie         | Seconds    |
| ST_Enterteinment_PC         | Screen Time - Entertainment Media               | Seconds    |
| ST_Other_PC                 | Screen Time - Uncategorized                     | Seconds    |
| ST_study_work_PC            | Screen Time - Study Work Related Media          | Seconds    |
| ST_social_media_PC          | Screen Time - Social Media                      | Seconds    |
|                             |                                                 |            |
| MO\_\*                      | Morning survey items                            |            |
| EV\_\*                      | Evening survey items                            |            |
| BS\_\*                      | Base survey items                               |            |
| WS\_\*                      | Weekly survey items                             |            |

. . . Please see excel file “legende_v2.xlsx” for more information

**Download**

Please download the [data set from our github
repository](https://github.com/tud-ise/Wellbeing_SoSe2023/blob/main/Exercises/Exercise%205/wellbeing_data.csv).

## Data Preparation

**Open the data set**

data = read.csv2(“/*Your Path*/wellbeing_data.csv” ) 
df_input = data

**Check the data structure**

summary(df_input) str(df_input)

**Format the data**

First we have to transform some columns into the correct format.

Transform the timestamp column into a Date format. Please Note: The date
format has to be YYYY-MM-DD. If you open the file in Excel, Excel will
change it to a German format. If that’s the case, change the format in
Excel using “Format cells-> Date -> Pick YYYY-MM-DD.” That’s also a
common problem if you receive any error if you try to change the format
into a date.

##Transform date

    df_input$date = as.Date(df_input$date)

##Transform numbers Use a for loop to transform all numbers into the
numeric format:

    for(i in 7:23){ #7 is the first column with numbers
      
      df_input[,i] = as.numeric(df_input[,i]) 
      
    }

24-27 are Bed Times, which has to stay as character.

    for(i in 28:123){ #123 is the last column with numbers
      
      df_input[,i] = as.numeric(df_input[,i]) 
      
    }

##Transform factors

    df_input$BS_week = as.factor(df_input$BS_week)
    df_input$WS_week = as.factor(df_input$WS_week)
    df_input$MO_type_of_Activity_planned_Morning = as.factor(df_input$MO_type_of_Activity_planned_Morning)
    df_input$MO_Activity_planned_todo_Morning = as.factor(df_input$MO_Activity_planned_todo_Morning)

Check data:

    str(df_input)

**Clean data**

      df_input$MO_Time_to_get_asleep[is.na(df_input$MO_Time_to_get_asleep) | df_input$MO_Time_to_get_asleep >= 300] = NA #Replace all entries higher than 5 hours with NA
      
      summary(df_input$MO_Time_to_get_asleep)

**Prepare data frames**

We now have one main data frame with all data in the correct format. We
can split our analysis into different interesting parts to work with.

      df_input = df_input #The main Dataset including all data
      
      df_weekly = aggregate(x = df_input[,91:123],na.action=na.omit, na.rm = TRUE, FUN = mean, by = list(Group.activity = df_input$ID, WS_week = df_input$WS_week)) #Weekly Survey data for each week 
      
      df_base = aggregate(x = df_input[,63:90],na.action=na.omit, na.rm = TRUE, FUN = mean, by = list(Group.activity = df_input$ID, BS_week = df_input$BS_week)) #Base survey data for basiseline, break and endline
      
      df_single = df_input[df_input$ID == "0tv2k8fm", ] #Here you should use your own ID. This ID was randomly picked for demonstration purposes
      
      df_overall = aggregate(x = df_input[,7:120],na.action=na.omit, na.rm = TRUE, FUN = mean, by = list(Group.date = df_input$date)) #representation of the complete course overall without differentation between IDs      
      

Variations are possible

      df_base = aggregate(x = df_input[,c("Entertainment", "BS_Cognitive_Well_Being")],na.action=na.omit, na.rm = TRUE, FUN = mean, by = list(Group.activity = df_input$ID, BS_week = df_input$BS_week))
      

**Trend**

Plots are a create tool to see if there is a trend in the data, which
can be a first hint of a correlation or effect.

ggplot2 is the most used library for plots in r. It makes it easy to
handle time-series data, and you will find a lot of documentation on the
web about ways to use ggplot2.

Install ggplot2

      install.packages("ggplot2") 
      
      install.packages("dplyr") 

Load ggplot2

      library(ggplot2)
      
      library(dplyr)
      

Plot time-series Note: Group.date is the date column in df_overall. Your
can rename it, if you want to use another name.

Examples:

    p = ggplot(df_overall, aes(x=Group.date, y=EV_Cognitive_Well_Being)) +
      geom_line() + 
      xlab("")
    p # show plot

That was an example of the overall course, but we had different groups.
Let’s pick only students with fasting in week 2:

    ids = df_input[df_input$date == "2023-05-30" & df_input$MO_type_of_Activity_planned_Morning == 2, "ID" ]
    df = df_input[df_input$ID %in% ids, ]

    df = aggregate(x = df[,7:123],na.action=na.omit, na.rm = TRUE, FUN = mean, by = list(Group.date = df$date)) #Format it into an timeseries

    p = ggplot(df, aes(x=Group.date, y= EV_Cognitive_Well_Being)) +
      geom_line() + 
      xlab("")
    p #show plot

Let’s plot just for one ID:

    p = ggplot(df_single, aes(x=date, y=EV_Cognitive_Well_Being)) +
      geom_line() + 
      xlab("")
    p #show plot

#Descriptive statistics You can use descriptive statistics like the
average and standard derivations to receive some interesting information
about yourself compared with others.

Example: How high was my average cognitive wellbeing compared to the
whole course?

    mean(df_single$EV_Cognitive_Well_Being, na.rm = TRUE) #na.rm = TRUE is necessary to ignore NA values

    mean(df_overall$EV_Cognitive_Well_Being, na.rm = TRUE)

How long does the average student take to fall asleep compared to you?

    mean(df_overall$MO_Time_to_get_asleep, na.rm = TRUE)

    mean(df_single$MO_Time_to_get_asleep, na.rm = TRUE)

You can also test how the average cognitive wellbeing of the course was
at the beginning of the course compared to the end:

        aggregate(x = df_base$BS_Cognitive_Well_Being,na.action=na.omit, na.rm = TRUE, FUN = mean, by = list(BS_Week = df_base$BS_week))

Or on an weekly base:

        aggregate(x = df_weekly$WS_Cognitive_Well_Being,na.action=na.omit, na.rm = TRUE, FUN = mean, by = list(WS_Week = df_weekly$WS_week))

To do it just for students who had fasting in week 1:

        ids = df_input[df_input$date == "2023-05-30" & df_input$MO_type_of_Activity_planned_Morning == 2, "ID" ]
        
        df = df_base[df_base$Group.activity %in% ids, ] #for Base survey(Group.activity = IDs)
        aggregate(x = df$BS_Cognitive_Well_Being,na.action=na.omit, na.rm = TRUE, FUN = mean, by = list(BS_Week = df$BS_week))
        
        
        df = df_weekly[df_weekly$Group.activity %in% ids, ] #for weekly survey(Group.activity = IDs)
        aggregate(x = df$WS_Cognitive_Well_Being,na.action=na.omit, na.rm = TRUE, FUN = mean, by = list(WS_Week = df$WS_week))
        

Please take a look into Exercise 3 and 4 for more information.

### inferential statistics

Next, we take a look on inferential statistics to see if we have
statistical significant effects.

**OLS Regression**

We can use a simple OLS regression (Exercise 3) to analyze the data of
one specific group, such as “your own” data (df_single).

Let’s test if we can find a significant effect of the activity and
Entertainment screen time on cognitive wellbeing:

    wlb_ols = lm(df_single$EV_Cognitive_Well_Being ~  df_single$Entertainment + df_single$EV_Activity_Check)

    summary(wlb_ols)

Overall

    wlb_ols = lm(df_overall$EV_Cognitive_Well_Being ~  df_overall$Social.Media)

    summary(wlb_ols)

**Analysis of Variance (ANOVA)**

Recap: How is the average Entertainment screen time for each type of
activity?

    aggregate(x = df_input$Entertainment, na.action=na.omit, na.rm = TRUE, FUN = mean, by = list(Activity = df_input$MO_type_of_Activity_planned_Morning))

Is there a statistical difference in cognitive wellbeing dependent on
the exercise they do?

    model = aov(aov(df_input$EV_Cognitive_Well_Being ~ df_input$MO_type_of_Activity_planned_Morning))
    summary(model)

There is a significant difference, but we still do not know the exact
comparisons between those groups. To get more information, we perform a
posthoc test named TukeyHSD:

    TukeyHSD(model)

    aggregate(x = df_input$EV_Cognitive_Well_Being, na.action=na.omit, na.rm = TRUE, FUN = mean, by = list(Activity = df_input$MO_type_of_Activity_planned_Morning))

Is there a statistical difference in sleep duration dependent on the
exercise they do?

    model = aov(aov(df_input$MO_sleep_time ~ df_input$MO_type_of_Activity_planned_Morning))
    summary(model)
    TukeyHSD(model)
    aggregate(x = df_input$MO_sleep_time, na.action=na.omit, na.rm = TRUE, FUN = mean, by = list(Activity = df_input$MO_type_of_Activity_planned_Morning))

Is there a statistical difference in cognitive wellbeing from Baseline
to Endline?

    model = aov(aov(df_base$BS_Cognitive_Well_Being ~ df_base$BS_week))
    summary(model)
    TukeyHSD(model)
    aggregate(x = df_base$BS_Cognitive_Well_Being, na.action=na.omit, na.rm = TRUE, FUN = mean, by = list(Activity = df_base$BS_week))

**Panel Regression**

To analyze the whole data set, we have to pick the correct model which
fits the data structure. As we already learned, the data set is based on
panel data, which we can analyze with a panel regression. In this
exercise, we will use the ‘plm()’ function from the ‘plm’ package to run
our panel regression.

*Install library*

    install.packages("plm")

    library("plm")

*Prepare data*

    E = pdata.frame(df_input, index=c("ID","date"), drop.index=TRUE, row.names=TRUE)

    head(E)

    head(attr(E, "index"))

*Run regression*

    panel_reg = plm(df_input$MO_Cognitive_Well_Being ~ df_input$MO_sleep_time, data = E, model = "random")

    panel_reg = plm(df_input$MO_Cognitive_Well_Being ~ df_input$Social.Media, data = E, model = "random")
     

    summary(panel_reg)
