---
    output:
      html_document:
        toc: true
        toc_float: false
        toc_depth: 4
        number_sections: false
  
        code_folding: hide
        code_download: true
  
        fig_width: 9
        fig_height: 4
        fig_align: "center"
        
        highlight: pygments
        theme: cerulean
        
        keep_md: true
        
    title: "Statistical Inference with the GSS data"
    subtitle: "In-depth hypothesis and confidence interval testing"
    author: "by Peter Hontaru"
---

## Background

### Problem Statement:

This document is the report from the final course project for the Inferential Statistics course, as part of the Duke University Statistics with R course in partnership with Coursera. The project consisted of exploring a real-world dataset (a subset of the General Social Survey) and create a report that included statistical inference on a question of interest (ended using three quesitons).

### Summary:

* Although small, there were significant differences between **genders** on whether **sexual education should be taught in schools**. This is especially important for single parents, or parents of the same sex, where one's stance and actions cannot overcompensate for the other's
* We could not prove significant differences in the representation of each **gender** amongst the various **social classes**
* There were significant differences in **total family income** based on the **respondent's family income when 16 years old**. This is important as the earned income should be independent of someone's family background. Further measures should be put into place for helping out those in unfavourable groups

*Note: all of the research questions are focusing on the survey data in the year 2012 to account for year-on-year variances and display the most recent view that our data could provide.*

### Next steps/recommendations:

* analyse changes between years (ie. 1980 vs 2012)
* some variables (ie. social class) were subjective and registered based on the respondent's opinion (more objective methods should be considered)
* further expand on the analysis by looking at other related factors (ie. if there was a difference between total family income based on the respondent's family income when 16 years old, can we find any further variables that can provide a more in-depth view as to why?)

### Dataset

Since 1972, the General Social Survey (GSS) has been monitoring societal change and studying the growing complexity of American society. The GSS aims to gather data on contemporary American society in order to monitor and explain trends and constants in attitudes, behaviors, and attributes; to examine the structure and functioning of society in general as well as the role played by relevant subgroups; to compare the United States to other societies in order to place American society in comparative perspective and develop cross-national models of human society; and to make high-quality data easily accessible to scholars, students, policy makers, and others, with minimal cost and waiting.

Source: GSS project description
https://www.norc.org/Research/Projects/Pages/general-social-survey.aspx





* * *

## Part 1: Data

### generabizability

According to the below link, each survey from 1972 to 2004 was an independently drawn sample of English-speaking persons 18 years of age or over, living in non-institutional arrangements within the United States through probability sampling (page 8).
http://gss.norc.org/documents/codebook/GSS_Codebook_intro.pdf

However, there is a major difference between the individuals prior to 2006 and after, as explained below

* 1972-2004, data includes English speaking individuals older than 18 years from non-institutional arrangements in the US
* 2006 and beyond, data includes English **and** Spanish-speaking individuals

Furthermore, the selection process was ran through systematic selection and controlled selection. Therefore, it can be assumed that the data is representative of the populations mentioned above, based on the year in focus.

### causality 

Due to the observational nature of the study, no random assignment was used (test/control group), and hence causality cannot be inferred.

* * *

## Part 2: Research questions

##### 1 - Is there a difference in the **opinion on sexual education in schools** based on **gender**? 

It would be helpful to know if this perception is dependent on gender, since sex is an important topic and can lead to serious family and health consequences, amongst others. This is especially important for single parents, or parents of the same sex, where one's stance and actions cannot overcompensate for the other's.

##### 2 - Are there significant differences in the representation of each **gender** amongst the various **social classes**?

Ideally, we would have an equal distribution of each gender across classes. This is important as we shouldn't have a significant difference based on someone's gender.

##### 3 - Does the **total family income** depend on the **respondent's family income when 16 years old**?

This is important as the earned income should be independent of someone's family background. If there are differences, further measures should be put into place for helping out those in unfavourable groups.

**Note: all of the research questions are focusing on the survey data in the year 2012 to account for year-on-year variances and display the most recent view that our data could provide.**

* * *

## Part 3: Exploratory data analysis

### 1 - Is there a difference in the **opinion on sexual education in schools** based on **gender**? 

We can see that there is a higher proportion of men that oppose sexual education in schools (11%) than women (8%). The absolute numbers can be seen in the table below.


```r
question_1_SexEduc <- gss%>%
  filter(year == 2012)%>%
  filter(sexeduc != "Depends" & !is.na(sexeduc))%>%
  group_by(sex, sexeduc)%>%
  summarise(count = n())%>%
  mutate(prop = round(count/sum(count),2)*100)

ggplot(question_1_SexEduc, aes(sex, prop, fill = sexeduc))+
  geom_col(col = "black")+
  theme_few()+
  theme(legend.position = "top")+
  geom_text(aes(label = paste(prop, "%", sep = "")), nudge_y = -3)+
  labs(y="Proportion",
       x = NULL,
       fill = "Perception on Sexual Education in schools")
```

<img src="figures/unnamed-chunk-2-1.png" width="100%" />

```r
question_1_SexEduc%>%
  kbl(caption = "Summary statistics")%>%
  kable_paper("hover", full_width = F)
```

<table class=" lightable-paper lightable-hover" style='font-family: "Arial Narrow", arial, helvetica, sans-serif; width: auto !important; margin-left: auto; margin-right: auto;'>
<caption>Summary statistics</caption>
 <thead>
  <tr>
   <th style="text-align:left;"> sex </th>
   <th style="text-align:left;"> sexeduc </th>
   <th style="text-align:right;"> count </th>
   <th style="text-align:right;"> prop </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Male </td>
   <td style="text-align:left;"> Favor </td>
   <td style="text-align:right;"> 519 </td>
   <td style="text-align:right;"> 89 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Male </td>
   <td style="text-align:left;"> Oppose </td>
   <td style="text-align:right;"> 64 </td>
   <td style="text-align:right;"> 11 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Female </td>
   <td style="text-align:left;"> Favor </td>
   <td style="text-align:right;"> 638 </td>
   <td style="text-align:right;"> 92 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Female </td>
   <td style="text-align:left;"> Oppose </td>
   <td style="text-align:right;"> 53 </td>
   <td style="text-align:right;"> 8 </td>
  </tr>
</tbody>
</table>

### 2 - Are there significant differences in the representation of each **gender** amongst the various **social classes**?

We cannot see any significant discrepencies between women and men as it regards their social class in either the graph or summary table.


```r
#create summary table
question_2_gender <- gss%>%
  filter(year == "2012")%>%
  group_by(sex, class)%>%
  summarise(count = n())%>%
  mutate(prop = round(count/sum(count),2)*100)

#create plot table
question_2 <- gss%>%
  filter(year == "2012")%>%
  filter(!is.na(class))%>%
  droplevels()

ggplot(question_2) +
  geom_mosaic(aes(x=product(sex), fill = class), col = "black")+
  theme_few()+
  theme(legend.position = "none")+
  labs(x = "",
       y = "Social class")
```

<img src="figures/unnamed-chunk-3-1.png" width="100%" />

```r
question_2_gender%>%
  kbl(caption = "Summary data")%>%
  kable_paper("hover", full_width = F)
```

<table class=" lightable-paper lightable-hover" style='font-family: "Arial Narrow", arial, helvetica, sans-serif; width: auto !important; margin-left: auto; margin-right: auto;'>
<caption>Summary data</caption>
 <thead>
  <tr>
   <th style="text-align:left;"> sex </th>
   <th style="text-align:left;"> class </th>
   <th style="text-align:right;"> count </th>
   <th style="text-align:right;"> prop </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Male </td>
   <td style="text-align:left;"> Lower Class </td>
   <td style="text-align:right;"> 86 </td>
   <td style="text-align:right;"> 10 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Male </td>
   <td style="text-align:left;"> Working Class </td>
   <td style="text-align:right;"> 385 </td>
   <td style="text-align:right;"> 43 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Male </td>
   <td style="text-align:left;"> Middle Class </td>
   <td style="text-align:right;"> 372 </td>
   <td style="text-align:right;"> 42 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Male </td>
   <td style="text-align:left;"> Upper Class </td>
   <td style="text-align:right;"> 34 </td>
   <td style="text-align:right;"> 4 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Male </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 1 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Female </td>
   <td style="text-align:left;"> Lower Class </td>
   <td style="text-align:right;"> 114 </td>
   <td style="text-align:right;"> 10 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Female </td>
   <td style="text-align:left;"> Working Class </td>
   <td style="text-align:right;"> 468 </td>
   <td style="text-align:right;"> 43 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Female </td>
   <td style="text-align:left;"> Middle Class </td>
   <td style="text-align:right;"> 467 </td>
   <td style="text-align:right;"> 43 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Female </td>
   <td style="text-align:left;"> Upper Class </td>
   <td style="text-align:right;"> 31 </td>
   <td style="text-align:right;"> 3 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Female </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> 8 </td>
   <td style="text-align:right;"> 1 </td>
  </tr>
</tbody>
</table>

### 3 - Does the **total family income** depend on the **respondent's family income when 16 years old**?

We can notice large differences both in median salary and variances across the different classes.


```r
question_3_detail <- gss%>%
  filter(year == "2012")%>%
  filter(!is.na(incom16))%>%
  filter(!is.na(coninc))%>%
  select(incom16, coninc)%>%
  droplevels()

ggplot(question_3_detail, aes(incom16, coninc, fill = incom16))+
  geom_boxplot(col = "black", varwidth = TRUE)+
  labs(x = NULL,
       y = "Total family income (yearly)")+
  scale_y_continuous(labels=scales::dollar_format())+
  coord_flip()+
  theme_few()+
  theme(legend.position = "none")
```

<img src="figures/unnamed-chunk-4-1.png" width="100%" />

```r
question_3_year <- gss%>%
  filter(year == "2012")%>%
  group_by(incom16)%>%
  summarise(count = n(),
            median_income = round(median(coninc, na.rm = TRUE),1))%>%
  mutate(prop = round(count/sum(count)*100,2))

question_3_year%>%
  kbl(caption = "Summary statistics")%>%
  kable_paper("hover", full_width = F)
```

<table class=" lightable-paper lightable-hover" style='font-family: "Arial Narrow", arial, helvetica, sans-serif; width: auto !important; margin-left: auto; margin-right: auto;'>
<caption>Summary statistics</caption>
 <thead>
  <tr>
   <th style="text-align:left;"> incom16 </th>
   <th style="text-align:right;"> count </th>
   <th style="text-align:right;"> median_income </th>
   <th style="text-align:right;"> prop </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Far Below Average </td>
   <td style="text-align:right;"> 181 </td>
   <td style="text-align:right;"> 24895 </td>
   <td style="text-align:right;"> 9.17 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Below Average </td>
   <td style="text-align:right;"> 523 </td>
   <td style="text-align:right;"> 34470 </td>
   <td style="text-align:right;"> 26.49 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Average </td>
   <td style="text-align:right;"> 875 </td>
   <td style="text-align:right;"> 34470 </td>
   <td style="text-align:right;"> 44.33 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Above Average </td>
   <td style="text-align:right;"> 304 </td>
   <td style="text-align:right;"> 51705 </td>
   <td style="text-align:right;"> 15.40 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Far Above Average </td>
   <td style="text-align:right;"> 47 </td>
   <td style="text-align:right;"> 34470 </td>
   <td style="text-align:right;"> 2.38 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> 44 </td>
   <td style="text-align:right;"> 21065 </td>
   <td style="text-align:right;"> 2.23 </td>
  </tr>
</tbody>
</table>

* * *

## Part 4: Inference

### 1 - Is there a difference in the **opinion on sexual education in schools** based on **gender**? 

#### Hypothesis

- **null hypothesis (Ho)** - there is no difference in the proportions of men and women that oppose sex education
- **alternative hypothesis (Ha)** - there is a significant difference in proportions of men and women that oppose sex education

#### Conditions

##### 1) Independence

- sampled observations must be independent of each other
  * random sample / assignment - *true, due to the study design*
  * sample size should be less than 10% of the respective population - *true, since the sample size shown below is smaller than 10% of the population of the USA*
  

```r
question_1 <- gss%>%
  filter(year == "2012")%>%
  select(sex,sexeduc)%>%
  droplevels()

table(question_1$sex, question_1$sexeduc)%>%
  kbl(caption = "Contingency table")%>%
  kable_paper("hover", full_width = F)
```

<table class=" lightable-paper lightable-hover" style='font-family: "Arial Narrow", arial, helvetica, sans-serif; width: auto !important; margin-left: auto; margin-right: auto;'>
<caption>Contingency table</caption>
 <thead>
  <tr>
   <th style="text-align:left;">   </th>
   <th style="text-align:right;"> Favor </th>
   <th style="text-align:right;"> Oppose </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Male </td>
   <td style="text-align:right;"> 519 </td>
   <td style="text-align:right;"> 64 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Female </td>
   <td style="text-align:right;"> 638 </td>
   <td style="text-align:right;"> 53 </td>
  </tr>
</tbody>
</table>

##### 2) Sample size

- success-failure condition: at-least 10 succeses and 10 failures in our sample - *true, as seen from the above table*

#### Methods

In this analysis, we're working with two categorical variables and thus, we will be using an inference based on two proportions. This means that we will be able to report both a **p value** and a **confidence interval**. Given that we met all of the conditions, we can use a theoretical based method rather than a simulation based inference.

##### p value

**Interpretation**:

We observed a p-value lower than 5%, so for this question, we reject the null hypothesis. Thus, there is sufficient evidence to conclude that a larger proportion of males, compared to females, oppose sex education. In the context of our data, a p value of 0.02 means that given a true null hypothesis, (ie. if there were no differences between the groups), there's only a chance of 2% for this difference between males and females to have simply happened by chance.

Hypothesis testing is used to assess the plausibility of a hypothesis by using sample data. The test provides evidence concerning the plausibility of the hypothesis, given the data. Statistical analysts test a hypothesis by measuring and examining a random sample of the population being analyzed


```r
sexeduc_2012 <- gss%>% 
  filter(year == 2012)%>%
  filter(sexeduc %in% c("Favor", "Oppose"))%>%
  select(sex,sexeduc)%>%
  droplevels()
  
inference(x = sex,
          y = sexeduc,
          data = sexeduc_2012, 
          statistic = "proportion", 
          type = "ht", 
          method = "theoretical", 
          alternative = "greater", 
          success = "Oppose", 
          null = 0)
```

```
## Response variable: categorical (2 levels, success: Oppose)
## Explanatory variable: categorical (2 levels) 
## n_Male = 583, p_hat_Male = 0.1098
## n_Female = 691, p_hat_Female = 0.0767
## H0: p_Male =  p_Female
## HA: p_Male > p_Female
## z = 2.0367
## p_value = 0.0208
```

<img src="figures/unnamed-chunk-6-1.png" width="100%" />

##### confidence interval

**Interpretation**:

The 95% confidence interval for the difference between proportions of males and females that oppose sex education is between 0.0009 to 0.0653 (it excludes 0). Though the difference is really small, it still exists. Hence the results for both tests agree, and we can say that a greater proportion of men, than females, oppose sex education, and steps can be taken to better educate the men on the issue.


```r
inference(x = sex,
          y = sexeduc,
          data = sexeduc_2012, 
          statistic = "proportion", 
          type = "ci", 
          method = "theoretical",
          #null = 0,
          #alternative = "greater", 
          success = "Oppose")
```

```
## Response variable: categorical (2 levels, success: Oppose)
## Explanatory variable: categorical (2 levels) 
## n_Male = 583, p_hat_Male = 0.1098
## n_Female = 691, p_hat_Female = 0.0767
## 95% CI (Male - Female): (9e-04 , 0.0653)
```

<img src="figures/unnamed-chunk-7-1.png" width="100%" />

### 2 - Are there significant differences in the representation of each **gender** amongst the various **social classes**?

#### Hypothesis

- **null hypothesis (Ho)** - the respondent's gender and social class are independent variables
- **alternative hypothesis (Ha)** - the respondent's social class and gender are dependent variables

#### Conditions

##### 1) Independence

- sampled observations must be independent of each other
  * random sample / assignment - *true, due to the study design*
  * sample size should be less than 10% of the population - *true, since the sample size shown below is smaller than 10% of the population of the USA*
  * each case only contributes to one cell in the table - *true*


```r
table(question_2$premarsx, question_2$sex)%>%
  kbl(caption = "Contingency table")%>%
  kable_paper("hover", full_width = F)
```

<table class=" lightable-paper lightable-hover" style='font-family: "Arial Narrow", arial, helvetica, sans-serif; width: auto !important; margin-left: auto; margin-right: auto;'>
<caption>Contingency table</caption>
 <thead>
  <tr>
   <th style="text-align:left;">   </th>
   <th style="text-align:right;"> Male </th>
   <th style="text-align:right;"> Female </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Always Wrong </td>
   <td style="text-align:right;"> 104 </td>
   <td style="text-align:right;"> 163 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Almst Always Wrg </td>
   <td style="text-align:right;"> 26 </td>
   <td style="text-align:right;"> 46 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Sometimes Wrong </td>
   <td style="text-align:right;"> 98 </td>
   <td style="text-align:right;"> 107 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Not Wrong At All </td>
   <td style="text-align:right;"> 342 </td>
   <td style="text-align:right;"> 359 </td>
  </tr>
</tbody>
</table>

##### 2) Sample size

- each particular scenario must have at least 5 expected cases - *true, as seen in the above table*

#### Methods

Since the dataset consists of two categorical variables (gender and social class), the adequate test to be used is the chi-square test of independence. This test is to be used when comparing 2 categorical variables where one of the variables has more than 2 levels(social class). This means that we will be using reporting a **p value** but not a **confidence interval**, since the latter isn't defined by a chi-square test of independence.

The Chi-square test of independence works by comparing the distribution that you observe to the distribution that you expect if there is no relationship between the categorical variables. In the Chi-square context, the word ???expected??? is equivalent to what you'd expect if the null hypothesis is true.

##### p value

**Interpretation**:

The low X-squared statistic (1.85) with 3 degrees of freedom leads to a high p-value (0.60). Since the p-value is above alpha (0.05), we can conclude that there is sufficient evidence to fail to reject the null hypothesis (Ho).

In the context of the research question, this result provides evidence that there isn't a significant difference between males and females, as of the data recorded in 2012. In other words, given a true null hypothesis (ie. no differences between the groups), there's a 60% chance that any differences seen have happened by chance.


```r
tidy(chisq.test(question_2$sex,question_2$class))%>%
  kbl(caption = "Chi-square test of independence")%>%
  kable_paper("hover", full_width = F)
```

<table class=" lightable-paper lightable-hover" style='font-family: "Arial Narrow", arial, helvetica, sans-serif; width: auto !important; margin-left: auto; margin-right: auto;'>
<caption>Chi-square test of independence</caption>
 <thead>
  <tr>
   <th style="text-align:right;"> statistic </th>
   <th style="text-align:right;"> p.value </th>
   <th style="text-align:right;"> parameter </th>
   <th style="text-align:left;"> method </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:right;"> 1.854238 </td>
   <td style="text-align:right;"> 0.6032039 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:left;"> Pearson's Chi-squared test </td>
  </tr>
</tbody>
</table>

No other methods were applicable and hence there's nothing to compare the result to.

### 3 - Does the **total family income** depend on the **respondent's family income when 16 years old**?

#### Hypothesis

- **null hypothesis (Ho)** - there are no significant differences between the total family incomes based on their family income when 16 years old
- **alternative hypothesis (Ha)** - at least one of the groups (as defined by family income when 16 years old) is significantly different in their total family income to the others

#### Conditions

##### 1) Independence

- **within groups**: sampled observations must be independent of each other
  * random sample / assignment - *true, due to the study design*
  * each group's sample size should be less than 10% of the respective population - *true, as seen in the below table*
- **between groups**: groups must be independent of each other - *true, we can assume that the groups are independent to each other due to the study design*


```r
question_3_year%>%
  filter(!is.na(incom16))%>%
  kbl(caption = "Summary statistics")%>%
  kable_paper("hover", full_width = F)
```

<table class=" lightable-paper lightable-hover" style='font-family: "Arial Narrow", arial, helvetica, sans-serif; width: auto !important; margin-left: auto; margin-right: auto;'>
<caption>Summary statistics</caption>
 <thead>
  <tr>
   <th style="text-align:left;"> incom16 </th>
   <th style="text-align:right;"> count </th>
   <th style="text-align:right;"> median_income </th>
   <th style="text-align:right;"> prop </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Far Below Average </td>
   <td style="text-align:right;"> 181 </td>
   <td style="text-align:right;"> 24895 </td>
   <td style="text-align:right;"> 9.17 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Below Average </td>
   <td style="text-align:right;"> 523 </td>
   <td style="text-align:right;"> 34470 </td>
   <td style="text-align:right;"> 26.49 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Average </td>
   <td style="text-align:right;"> 875 </td>
   <td style="text-align:right;"> 34470 </td>
   <td style="text-align:right;"> 44.33 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Above Average </td>
   <td style="text-align:right;"> 304 </td>
   <td style="text-align:right;"> 51705 </td>
   <td style="text-align:right;"> 15.40 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Far Above Average </td>
   <td style="text-align:right;"> 47 </td>
   <td style="text-align:right;"> 34470 </td>
   <td style="text-align:right;"> 2.38 </td>
  </tr>
</tbody>
</table>

##### 2) Skewness

- distribution of response variable within each group should be approximately normal (especially important when sample sizes are small) -  we can check this using **normal probability plots (QQ plots)** or the **Shapiro-Wilk normality test**
  * the **QQ plot** does not show a normal distribution of our data
  * given the extremely low p-value, the **Shapiro-Wilk normality test** implies that the distribution of the data is significantly different from a normal distribution and that we cannot assume the normality

However, the sample size is relatively large for each class thus, unlike the independence of data, this assumption is not crucial.


```r
ggplot(question_3_detail, aes(sample=coninc, col = incom16))+
  stat_qq()+
  stat_qq_line()+
  facet_wrap(.~incom16)+
  labs(title = "QQ plot")+
  theme_few()+
  theme(legend.position = "none")
```

<img src="figures/unnamed-chunk-11-1.png" width="100%" />

```r
question_3_detail %>% 
  select(incom16, coninc) %>% 
  group_by(group = as.character(incom16)) %>%
  do(tidy(shapiro.test(.$coninc)))%>%
  #put this into a aesthetically pleasing dataframe
  kbl(caption = "Shapiro-Wilk normality test")%>%
  kable_paper("hover", full_width = F)
```

<table class=" lightable-paper lightable-hover" style='font-family: "Arial Narrow", arial, helvetica, sans-serif; width: auto !important; margin-left: auto; margin-right: auto;'>
<caption>Shapiro-Wilk normality test</caption>
 <thead>
  <tr>
   <th style="text-align:left;"> group </th>
   <th style="text-align:right;"> statistic </th>
   <th style="text-align:right;"> p.value </th>
   <th style="text-align:left;"> method </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Above Average </td>
   <td style="text-align:right;"> 0.8291873 </td>
   <td style="text-align:right;"> 0.0e+00 </td>
   <td style="text-align:left;"> Shapiro-Wilk normality test </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Average </td>
   <td style="text-align:right;"> 0.7930347 </td>
   <td style="text-align:right;"> 0.0e+00 </td>
   <td style="text-align:left;"> Shapiro-Wilk normality test </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Below Average </td>
   <td style="text-align:right;"> 0.7740452 </td>
   <td style="text-align:right;"> 0.0e+00 </td>
   <td style="text-align:left;"> Shapiro-Wilk normality test </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Far Above Average </td>
   <td style="text-align:right;"> 0.7785680 </td>
   <td style="text-align:right;"> 1.3e-06 </td>
   <td style="text-align:left;"> Shapiro-Wilk normality test </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Far Below Average </td>
   <td style="text-align:right;"> 0.6971207 </td>
   <td style="text-align:right;"> 0.0e+00 </td>
   <td style="text-align:left;"> Shapiro-Wilk normality test </td>
  </tr>
</tbody>
</table>

##### 3) Variance

- variability should be consistent across groups (homoscedastic groups). This is especially important when sample sizes differ between groups, as it is in our case and we can check this using:
  * **side by side box plots** (previously shown in the Exploratory Data Analysis)*not met as the variability within the lower class group is much higher than in the other groups*
  * **Residual vs. Fitted plots** - *not true, given the shape of the data and extreme outliers across all groups*
  * **Levene's test** - *from the output below we can see that the p-value is less than the significance level of 0.05. Therefore, we can not assume the homogeneity of variances in the different treatment groups.*

When the homogeneity assumption is violated, as seen above, we could use the Welch one-way test, which does not require for the assumption of equal variances to be met.


```r
res.aov <- aov(coninc ~ incom16, data = question_3_detail)

tidy(leveneTest(question_3_detail$coninc~question_3_detail$incom16))%>%
  kbl(caption = "Levene's test")%>%
  kable_paper("hover", full_width = F)
```

<table class=" lightable-paper lightable-hover" style='font-family: "Arial Narrow", arial, helvetica, sans-serif; width: auto !important; margin-left: auto; margin-right: auto;'>
<caption>Levene's test</caption>
 <thead>
  <tr>
   <th style="text-align:right;"> statistic </th>
   <th style="text-align:right;"> p.value </th>
   <th style="text-align:right;"> df </th>
   <th style="text-align:right;"> df.residual </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:right;"> 12.3667 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 1723 </td>
  </tr>
</tbody>
</table>

```r
ggplot(res.aov, aes(.fitted, .resid, col = incom16))+
  geom_point()+
  stat_smooth(method="loess", show.legend = FALSE)+
  geom_hline(yintercept=0, col="red", linetype="dashed")+
  labs(x = "Fitted values",
       y = "Residuals",
       title = "Residual vs Fitted Plot",
       col = "Class")+
  theme_few()+
  theme(legend.position = "top")
```

<img src="figures/unnamed-chunk-12-1.png" width="100%" />

#### Methods

Given that we have one categorical variable with multiple levels (class) and one numerical variable (tv hours per day), the appropriate method for analysis is ANOVA. At the same time, we violated the homogeneity assumption and a one way test is more appropriate in this case. Furthermore, we will account for the increased risk of type I error with the Tukey test(due to multple comparisons), as well as understand which groups are significant (or not). Lastly, this means that we will be using reporting a **p value** but not a **confidence interval**, since this type of statistical analysis does not support the latter.

ANOVA is used to compare differences of means among more than 2 groups. It does this by looking at variation in the data and where that variation is found (hence its name). Specifically, ANOVA compares the amount of variation between groups with the amount of variation within groups. It can be used for both observational and experimental studies.

##### p value

**Interpretation**:

We can start by conducting a ANOVA test, despite the homogeneity assumption being violated, as a point of comparison with the one way test (below). We can see that the p value is almost 0, which means we can reject the null hypothesis and that there are significant differences between at least two of our groups.


```r
tidy(res.aov)%>%
  kbl(caption = c("ANOVA table - without intercept"), )%>%
  kable_paper("hover", full_width = F)
```

<table class=" lightable-paper lightable-hover" style='font-family: "Arial Narrow", arial, helvetica, sans-serif; width: auto !important; margin-left: auto; margin-right: auto;'>
<caption>ANOVA table - without intercept</caption>
 <thead>
  <tr>
   <th style="text-align:left;"> term </th>
   <th style="text-align:right;"> df </th>
   <th style="text-align:right;"> sumsq </th>
   <th style="text-align:right;"> meansq </th>
   <th style="text-align:right;"> statistic </th>
   <th style="text-align:right;"> p.value </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> incom16 </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 1.248804e+11 </td>
   <td style="text-align:right;"> 31220101666 </td>
   <td style="text-align:right;"> 14.61664 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Residuals </td>
   <td style="text-align:right;"> 1723 </td>
   <td style="text-align:right;"> 3.680206e+12 </td>
   <td style="text-align:right;"> 2135929026 </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> NA </td>
  </tr>
</tbody>
</table>

```r
# tidy(Anova(res.aov, type =3))%>%
#   kbl(caption = c("ANOVA table - with intercept"))%>%
#   kable_paper("hover", full_width = F)

variance_explained <- round(
  summary.lm(res.aov)$r.squared*100, 1
  )
```

In the context of the research question, the ANOVA output also shows us that only around **3.3%** of the variance in total family income is explained by the respondent's income when 16 years old. However, this only tells us that some groups are significantly different to others, not which ones. We will need to run a **Tukey post hoc analysis** to find out more detail.

A similar result is shown by our one way test below. No other methods were applicable and hence there's nothing to compare other than the two described above.


```r
res.aov2 <- oneway.test(coninc ~ incom16, data = question_3_detail)

tidy(res.aov2)%>%
  kbl(caption = c("One way test"), )%>%
  kable_paper("hover", full_width = F)
```

<table class=" lightable-paper lightable-hover" style='font-family: "Arial Narrow", arial, helvetica, sans-serif; width: auto !important; margin-left: auto; margin-right: auto;'>
<caption>One way test</caption>
 <thead>
  <tr>
   <th style="text-align:right;"> num.df </th>
   <th style="text-align:right;"> den.df </th>
   <th style="text-align:right;"> statistic </th>
   <th style="text-align:right;"> p.value </th>
   <th style="text-align:left;"> method </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 246.654 </td>
   <td style="text-align:right;"> 11.30872 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:left;"> One-way analysis of means (not assuming equal variances) </td>
  </tr>
</tbody>
</table>

Once we account for the increased risk of type I error with the Tukey test, we can still see a significant difference between the following groups:
- Above Average and Far Below Average
- Above Average and Below Average
- Above Average and Average


```r
tidy(TukeyHSD(res.aov))%>%
  kbl(caption = c("Tukey post hoc analysis"))%>%
  kable_paper("hover", full_width = F)
```

<table class=" lightable-paper lightable-hover" style='font-family: "Arial Narrow", arial, helvetica, sans-serif; width: auto !important; margin-left: auto; margin-right: auto;'>
<caption>Tukey post hoc analysis</caption>
 <thead>
  <tr>
   <th style="text-align:left;"> term </th>
   <th style="text-align:left;"> contrast </th>
   <th style="text-align:right;"> null.value </th>
   <th style="text-align:right;"> estimate </th>
   <th style="text-align:right;"> conf.low </th>
   <th style="text-align:right;"> conf.high </th>
   <th style="text-align:right;"> adj.p.value </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> incom16 </td>
   <td style="text-align:left;"> Below Average-Far Below Average </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 4398.047 </td>
   <td style="text-align:right;"> -7110.550 </td>
   <td style="text-align:right;"> 15906.64 </td>
   <td style="text-align:right;"> 0.8350256 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> incom16 </td>
   <td style="text-align:left;"> Average-Far Below Average </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 9457.687 </td>
   <td style="text-align:right;"> -1509.506 </td>
   <td style="text-align:right;"> 20424.88 </td>
   <td style="text-align:right;"> 0.1284567 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> incom16 </td>
   <td style="text-align:left;"> Above Average-Far Below Average </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 28072.921 </td>
   <td style="text-align:right;"> 15507.856 </td>
   <td style="text-align:right;"> 40637.99 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> incom16 </td>
   <td style="text-align:left;"> Far Above Average-Far Below Average </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 20273.529 </td>
   <td style="text-align:right;"> -1404.306 </td>
   <td style="text-align:right;"> 41951.36 </td>
   <td style="text-align:right;"> 0.0796375 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> incom16 </td>
   <td style="text-align:left;"> Average-Below Average </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 5059.640 </td>
   <td style="text-align:right;"> -2264.573 </td>
   <td style="text-align:right;"> 12383.85 </td>
   <td style="text-align:right;"> 0.3250145 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> incom16 </td>
   <td style="text-align:left;"> Above Average-Below Average </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 23674.875 </td>
   <td style="text-align:right;"> 14122.616 </td>
   <td style="text-align:right;"> 33227.13 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> incom16 </td>
   <td style="text-align:left;"> Far Above Average-Below Average </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 15875.482 </td>
   <td style="text-align:right;"> -4206.682 </td>
   <td style="text-align:right;"> 35957.65 </td>
   <td style="text-align:right;"> 0.1961012 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> incom16 </td>
   <td style="text-align:left;"> Above Average-Average </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 18615.235 </td>
   <td style="text-align:right;"> 9722.700 </td>
   <td style="text-align:right;"> 27507.77 </td>
   <td style="text-align:right;"> 0.0000001 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> incom16 </td>
   <td style="text-align:left;"> Far Above Average-Average </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 10815.842 </td>
   <td style="text-align:right;"> -8961.034 </td>
   <td style="text-align:right;"> 30592.72 </td>
   <td style="text-align:right;"> 0.5669211 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> incom16 </td>
   <td style="text-align:left;"> Far Above Average-Above Average </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> -7799.392 </td>
   <td style="text-align:right;"> -28505.101 </td>
   <td style="text-align:right;"> 12906.32 </td>
   <td style="text-align:right;"> 0.8421870 </td>
  </tr>
</tbody>
</table>
