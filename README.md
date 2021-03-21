Statistical inference with the GSS data
================
by Peter Hontaru

<div style="text-align:center">

<img src="_support files/education.jpg" width="350" height="300">

</div>

## Background

### Problem Statement:

This document is the report from the final course project for the
Inferential Statistics course, as part of the Duke University Statistics
with R course in partnership with Coursera. The project consisted of
exploring a real-world dataset (a subset of the General Social Survey)
and create a report that included statistical inference on a question of
interest (ended using three quesitons).

### Summary:

-   Although small, there were significant differences between
    **genders** on whether **sexual education should be taught in
    schools**. This is especially important for single parents, or
    parents of the same sex, where one’s stance and actions cannot
    overcompensate for the other’s.
-   We could not prove significant differences in the representation of
    each **gender** amongst the various **social classes**.
-   There were significant differences in **total family income** based
    on the **respondent’s family income when 16 years old**. This is
    important as the earned income should be independent of someone’s
    family background. Further measures should be put into place for
    helping out those in unfavourable groups.

*Note: all of the research questions are focusing on the survey data in
the year 2012 to account for year-on-year variances and display the most
recent view that our data could provide.*

### Next steps/recommendations:

-   analyse changes within year (ie. were there different
    representations of each gender in 1980 compared to 2012?)
-   some variables (ie. social class) were subjective and registred
    based on the respondent’s opinion
-   further expand on the analysis by looking at other related factors
    (ie. if there was a difference between total family income based on
    the respondent’s family income when 16 years old, can we find any
    further variables that can provide a more indepth view?)

### Dataset

Since 1972, the General Social Survey (GSS) has been monitoring societal
change and studying the growing complexity of American society. The GSS
aims to gather data on contemporary American society in order to monitor
and explain trends and constants in attitudes, behaviors, and
attributes; to examine the structure and functioning of society in
general as well as the role played by relevant subgroups; to compare the
United States to other societies in order to place American society in
comparative perspective and develop cross-national models of human
society; and to make high-quality data easily accessible to scholars,
students, policy makers, and others, with minimal cost and waiting.

Source: GSS project description
<https://www.norc.org/Research/Projects/Pages/general-social-survey.aspx>

------------------------------------------------------------------------

# Introduction

## Problem Statement:

**Can we predict if a candiate was placed in a role after their MBA
studies? If so, which factors helped the most (ie. work experience,
degree, school results, gender, etc)?**

## Who is this project intended for?

-   **students** looking to assess whether they’re likely to receive a
    placement offer or not
-   **hiring managers/recruiters** trying to optimise their process
-   *those generally interested in what improves a candidate’s chance to
    receive a placement role at the MBA level*

## Why this dataset?

As a school governor, I am very interested in the importance of
education in preparing a student for the job market as well as the
current trends in the job market itself. This project would prove as an
excellent way to further improve my data science skills while learning
more about these topics.

# Key insights

## Summary:

-   when combining the stats as lower education (secondary and higher
    secondary) and higher education (university and MBA), it was
    interesting to see that:
    -   **virtually everyone that ranked in the top 25th percentile in
        their lower education were placed, regardless of their higher
        education performance**
    -   almost no one in the bottom 25th percentile across the lower
        education was placed, regardless of their higher education
        peformance

![education](figures/education-1.png)

-   it is important to define what “performance” means when it comes to
    choosing a model (one type of prediction error is costlier than the
    other). For example, incorrectly predicting that someone **would
    be** placed(false positive) is not as bad as incorrectly predicting
    that someone **would not be** placed(false negative). The cost of
    the former is the time spent interviewing, while the cost of the
    latter is losing out on a job that the student would’ve secured
    -   **scenario a) - preferable for hiring managers/recruiters -** if
        we’re trying to maximise accuracy and optimise hiring costs, the
        LDA2 model (**91% test accuracy**) is more appropriate as it
        only has 4 incorrect predictions (3 false positives and 1 false
        negative)
    -   **scenario b) - preferable for students -** if we’re trying to
        minimise costly errors (false negatives), then the ranger model
        (83% test accuracy) is more appropriate (7 false positives and
        **0 false negatives**)

![accuracy](figures/accuracy-1.png)

-   the scores mattered in their cronological order with secondary
    first, followed by higher secondary, undergraduate and then masters;
    this could be due to 2 main factors:
    -   the students who perform better early on are more likely to be
        the type of an ambitious student with a passion for learning
        that makes for a better hire
    -   there is less of a chance to differentiate based on performance
        scores alone towards the end of the formal education since we’ve
        seen that most vary around the median (between 60% and 70%)
        instead of the much wider range early on

![factors](figures/factors-1.png)

## Next steps/recommendations:

-   predictive algorithm to see if we could anticipate placement
    salaries and understand which factors contributed the most
-   creating a dashboard where someone can input their scores and see
    whether the model would predict that they would get the role or not
-   it would be very useful to gather more data, especially that of
    other generations of graduates

## Where did the data come from?

The dataset was downloaded from kaggle (link below). It was made
available to the public by Ben Roshan and it is representative of the
MBA students cohort of an anonymous university.

<https://www.kaggle.com/benroshan/factors-affecting-campus-placement>

# Extended analysis

Full project available

-   at the following
    [link](http://htmlpreview.github.io/?https://github.com/peterhontaru/Job-Offer-Prediction-MBA-Students/blob/master/Placement-Prediction.html),
    in HTML format
-   in the **Placement-Prediction.md** file of this repo (however, I
    recommend previewing it at the link above since it was originally
    designed as a html document)
