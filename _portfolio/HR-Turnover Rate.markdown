---
title: "Data Science Project: Predicting Turnover Rate"
output: html_notebook
---

*** 
 <img  src="https://s-media-cache-ak0.pinimg.com/originals/32/ef/23/32ef2383a36df04a065b909ee0ac8688.gif"/>
 
# Contents:

[1. Introduction](#intro)
<Br>
[2. Litelature Review](#lite)
<Br>
[3. Data Description](#data)
<Br>
[4. Model Analysis](#model)
<Br>
[5. R Code Analysis](#rcode)
<Br>
    * [DataSet Details](#ls)
  <Br>
    * [Describe DataSet](#et)
  <Br>
    * [Create a Contigency Table for each variable in dataset](#et2)
  <Br>
    * [Create two-way contingency tables for the categorical variables](#es)
  <Br>
    * [Boxplot Creation](#on)
  <Br>
    * [Histogram for the variables](#es2)
  <Br>
    * [Visualize your correlation matrix using corrgram](#am)
  <Br> 
    * [Create a scatter plot matrix](#ix)
  <Br>
    * [Run a suitable test to check your hypothesis for your suitable assumptions](#ns)
  <Br>
    * [T- Test Hypothesis](#is)
<Br>
[6. Result](#result)
<Br>
[7. References](#refe)

#########################
#########################
<Br>
<Br>
<Br>
<Br>



 
#1. Introduction {#intro}
* High employee retention rate (quit job) is big problem for companies.
* High employee retention rate (turnover or quit) costed a lot, fom Job postings, hiring processes, paperwork to new hire training
* For customer facing business such as E-commerce or consulting company, customers are often prefer to interact with familiar people. Errors and issues are more often likely if the company have new workers.


# 2. Litelature Review:{#lite}
## **The Top 5 reasons for job change:**

1) Quality of Work

2) Performance Pressure

3) Friends

4) Location

5) Money

# 3. Data Description:{#data}
## Why are they best and most experienced employees leaving?
This dataset come from kaggle HR Analytics

### Data Definition

1. Satisfaction Level : Employee Satisfaction (can be interpreted as a %)

2. Last evaluation : Employee Evaluation (can be interpreted as a %)

3. Projects : Number of Projects (per year)

4. Average monthly hours : Average monthly hours

5. Time spent at company : Time spent at company

6. Accident : Whether they have had a work accident

7. Promotion Last 5 yrs : Whether they have had a promotion in the last 5 years

8. Department : Type of Job Position

9. Salary : Salary level (1= low, 2= medium, 3= high)

10. Left : Whether the employee has left (0= remains employed, 1= left)


# 4. Model Analysis {#model}
## What factors increase job satisfaction?
In this case we used regression model (linear) to know the significant value of parameters

## Full Model
* 9 indepedent variables in the dataset predicted the 1 dependent variable (satisfaction level)

```{r}
head(hr)
```
```{r}
fullmodel <- lm(satisfaction_level ~ salary + average_montly_hours + number_project + time_spend_company +  promotion_last_5years + last_evaluation + Work_accident + left,data=hr )

summary(fullmodel)
```

Average monthly hours, number project, time spent at company, last evaluation, and whether or not the employee left have significant p-values.

R-squared and adjusted R-squared are both at about 0.19. This is not typically a good value, but is rather common in any data analysis of human behavior. For example, in psychology studies this R-squared level would not eliminate the model's validity, especially if p-values indicate significance

More abour summary function in linear model Regression:https://feliperego.github.io/blog/2015/10/23/Interpreting-Model-Output-In-R

## Revised Model
```{r}
revisedmodel<- lm(satisfaction_level ~ average_montly_hours + number_project + time_spend_company + last_evaluation,data=hr)
summary(revisedmodel)
```

This model has even lower R-squared and adjusted R-squared values of about 0.05, which again can be attributed to the fact that human behavior is very difficult to predict. The variables remained significant, except for average monthly hours. Based on these p-values, we concluded the model and remove the insignificant variable for the next model.

# 5. R Code Analysis {#rcode}

## DataSet Details {#ls}
```{r}
dim(hr)
```
## Describe DataSet {#et}
* Descriptive statistics (min, max, median etc) of each variable
```{r}
#install.package("psych")
#library(psych)
# if needed: mnormt package
describe(hr)
```

## Create a Contigency Table for each variable in dataset {#et2}
```{r}
table_salary<-with(hr,table(salary))
table_salary
```

```{r}
table_satisfication<-with(hr,table(satisfaction_level))
table_satisfication
```

```{r}
table_lastevaluation<-with(hr,table(last_evaluation))
table_lastevaluation

```
```{r}
table_numberproject<-with(hr,table(number_project))
table_numberproject
```
```{r}
table_avgmontlyhours<-with(hr,table(average_montly_hours))
table_avgmontlyhours
```
```{r}
table_timespend<-with(hr,table(time_spend_company))
table_timespend
```
```{r}
table_workaccident<-with(hr,table(Work_accident))
table_workaccident
```
```{r}
table_left<-with(hr,table(left))
table_left
```
```{r}
table_promotion<-with(hr,table(promotion_last_5years))
table_promotion
```
```{r}
table_sales<-with(hr,table(Department))
table_sales
```
```{r}
table_salary<-with(hr,table(salary))
table_salary
```


## Create two-way contingency tables for the categorical variables {#es}

```{r}
table_project_spend<-xtabs(~number_project+time_spend_company,data=hr)
head(table_project_spend,5)
```
```{r}
table_satisfication_salary<-xtabs(~satisfaction_level+salary,data=hr)
head(table_satisfication_salary,5)

```

```{r}
table_Department_salary<-xtabs(~Department+salary,data=hr)
head(table_Department_salary,5)
```
```{r}
table_avgmontlyhours_salary<-xtabs(~average_montly_hours+salary,data=hr)
head(table_avgmontlyhours_salary,5)
```
```{r}
table_accident_salary<-xtabs(~Work_accident+salary,data=hr)
table_accident_salary
```

```{r}
table_promotion_salary<-xtabs(~promotion_last_5years+salary,data=hr)
table_promotion_salary
```
```{r}
table_project_timespend<-xtabs(~number_project+time_spend_company,data=hr)
table_project_timespend
```

## Boxplot Creation {#on}

```{r}
boxplot(satisfaction_level ~salary,data=hr, horizontal=TRUE,
           ylab="Salary Level", xlab="Satisfaction level", las=1,
           main="Analysis of Salary of Employee on the basis of their satisfaction level",
           col=c("red","blue","green")
           )
```
```{r}
boxplot(satisfaction_level ~left, data=hr, horizontal=TRUE,
           ylab="Left", xlab="Satisfaction level", las=1,
           main="Analysis of of Employee Left on the basis of their satisfaction level",
           col=c("Yellow","Orange")
           )
```
```{r}
boxplot(number_project~left,data=hr, horizontal=TRUE,
           ylab="Left", xlab="No of Projects", las=1,
           main="Analysis of of Employee Left on the basis of their Number of Projects",
           col=c("Red","Magenta")
           )

```
```{r}
boxplot(average_montly_hours ~left, data=hr,horizontal=TRUE,
           ylab="Left", xlab="Average Monthly Hours", las=1,
           main="Analysis of of Employee Left on the basis of their Average Monthly Hours",
           col=c("Yellow","Orange")
           )

```
```{r}
boxplot(Work_accident~left,data=hr, horizontal=TRUE,
           ylab="Left", xlab="Work Accident", las=1,
           main="Analysis of of Employee Left on the basis of their Work Accident",
           col=c("Yellow","Orange")
           )
```

```{r}
boxplot(last_evaluation ~left,data=hr, horizontal=TRUE,
           ylab="Left", xlab="Last Evaluation", las=1,
           main="Analysis of of Employee Left on the basis of their Last Evaluation",
           col=c("Yellow","Orange")
           )
```



## Histogram for the variables {#es2}

```{r}
hist(hr$satisfaction_level, main=" Variation in Satisfaction Level ", xlab="Satisfaction Level",breaks=10,ylab="Frequency", col="green")
```
```{r}
hist(hr$last_evaluation, main=" Variation in Last Evaluation ", xlab="Last Evaluation",breaks=10,ylab="Frequency", col="blue")
```

```{r}
hist(hr$satisfaction_level, main=" Variation in Time Spent in the Company ", xlab="Time Spent in the Company",breaks=10,ylab="Frequency", col="yellow")
```

```{r}
hist(hr$average_montly_hours, main=" Variation in Average Monthly Hours ", xlab="Average Monthly Hours",breaks=10,ylab="Frequency", col="red")
```
```{r}
plot(y=hr$salary, x=hr$Department,
     col="red",
     main="Relationship Btw salary and sales",
     ylab="Salary", xlab="Sales")
```
```{r}
plot(y=hr$average_montly_hours, x=hr$Department,
     col="green",
     main="Relationship Btw Average Monthly Hours and sales",
     ylab="Average Monthly Hours", xlab="Sales")
```
```{r}
library(corrplot)
```
```{r}
correlationMatrix <- cor(hr[,c(1:8)])
corrplot(correlationMatrix, method="circle")
```

```{r}
cor(hr[ ,c(1,2,3,4,5,6,7,8)])
```

## Visualize your correlation matrix using corrgram {#am}

```{r}
library(corrgram)
```

```{r}
corrgram(hr, lower.panel = panel.shade, upper.panel = panel.pie, text.panel = panel.txt, main = "Corrgram of all  variables")
```

## Create a scatter plot matrix {#ix}

```{r}
library(car)
```


```{r}
#The following object is masked from 'package:psych':
scatterplotMatrix(formula = ~left + satisfaction_level + time_spend_company + Work_accident +average_montly_hours , data = hr,smooth= TRUE)
```


## Run a suitable test to check your hypothesis for your suitable assumptions {#ns}

```{r}
cor.test(hr$left,hr$satisfaction_level)
```

```{r}
cor.test(hr$left,hr$time_spend_company)
```

```{r}
cor.test(hr$left,hr$last_evaluation)
```

```{r}
cor.test(hr$left,hr$number_project)
```

## T- Test Hypothesis {#is}

```{r}
t.test(hr$satisfaction_level~hr$left)
```

```{r}
t.test(hr$time_spend_company~hr$left)
```

```{r}
t.test(hr$average_montly_hours~hr$left)
```

```{r}
t.test(hr$last_evaluation~hr$left)
```

# 6. Result:

## 1. Why are our best and most experienced employees leaving prematurely?

* High salaried employees show a different pattern for leaving the company
<Br>
* The parameters to quit out job are
    * Satisfaction level < 0.5
    * Average monthly hours > 200
    * After spending average 4 years of time in the company


## 2. Which employee will leave next?

Emplpoyee who has low salary, with satisfaction level < 0.5 and is putting in average monthly hours > 200. The probability of the employee leaving is 70% (3 parameters from total 10 parameters)


## 3. Interesting Insights

* Number of projects, time spent at company, and last evaluation are significant predictors of job satisfaction.
* The well-balanced worker who has recently been promoted is the happiest.
* Employees who are over-worked or under-work are relatively dissatisfied
* Employees at the lowest salary level (level 1) are actually the most satisfied
* The happiest employees work 3 or 4 projects each year
* Employeea with the most extreme hours are typically very disatisfied
* Employees with poor or excellent last evaluations are the most satisfied
* Employees who have left the company were typically on the higher end of average monthly hours.
* Employees who left worked are under 3 projects, recieved poor or excellent performance rating, were not promoted in the last 5 years, and most did not have a work accident. 
* We see that the last evaluations of employees who left have a bimodal distribution with great quantities at each extreme.
* In terms of time spent at the company, there is a huge spike in satisfaction levels for employees who have remained with the company for 2.5 years or more.
* Satisfaction levels decrease past the 2.5 year satisfaction peak.
* Loyal employees who have remained employed with the company for more than 6 years tend to be happier in their positions.

# 7. References {#refe}

https://smallbusiness.chron.com/meaning-attrition-used-hr-61183.html

https://www.linkedin.com/pulse/top-5-reasons-employee-attrition-how-deal-mahidhar-reddy/








