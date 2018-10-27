## Overview

The objective of this A/B test experiment is to make a recommendation on Udacity's course overview page.
<br>
<br>
**Before the experiment**, you had two options on the course overview page: "start free trial" and "access course materials".
If the student clicks **"start free trial"**, they will be asked to enter their credit card information, and then they will be enrolled in a free trial for the paid version of the course. After 14 days, they will automatically be charged unless they cancel first. If the student clicks **"access course materials"**, they will have access to the course content but will not receive any feedbacks.
<br>
<br>
**The experiment** consists in asking the following question if the students clicks "start free trial": How much time will you spend on this course per week? If the student writes 5 hours or more, then the student will access the checkout process as before. If the student indicates less than 5 hours, then he will receive a message telling him that this course requires usually at least 5 hours/week. At this point, the student would have the option to continue enrolling in the free trial, or access the course materials for free instead. Here is a screenshot explaining the experiment:
<br>
<br>
<p align="center">
  <img src= "https://github.com/guillaumedelaloy/ab-testing/blob/master/image/Final Project_ Experiment Screenshot.png?raw=true">
</p>
<br>
<br>
**The objective** of the experiment is to guide the students towards the offer matching their needs. For example, before the experiment, some students were getting frustrated because they couldn't spend enough time on the course and then had to pay while they couldn't really discover the content.
<br>
<br>
**Hypothesis**: 
The experiment should filter students with higher commitment in the "free trial". As a consequence, we should have a higher rate of students who started the free trial and completed it.
The experiment could also negatively affect the number of students choosing the free trial.


## Metrics
<br>
**The unit of diversion** is a cookie. Since users click on "start free trial" before being signed in, or even before having an account, it is not possible to rely on the user_ids.
<br>
**The invariant metrics** , that is to say, the metrics that should be evenly distributed between the control and the experiment group : number of unique cookies, number of unique cookies clicking on start free trial.
<br>
**The evaluation metrics** : 
- the gross conversion, GC, (the number of user ids completing checkout and enrolled, divided by the number of unique cookies clicking on "start free trial")
- the net conversion (the number of user ids who payed, i.e enrolled more than 14 days, divided by the number of unique cookies clicking on "start free trial")


## Design the experiment
<br>
First we need to determine how much data we need in order to implement an experiment with a good level of confidence.
We choose:
```
Significance level α = 0.5

Statistical power 1−β = 0.8

Minimum pratical difference (GC) = 1%

Minimum pratical difference (NC) = 0.75%
```
The minimum practical difference here is a relative change that is considered to have a meaningful impact on business.

I used the following [sample size calculator](http://www.evanmiller.org/ab-testing/sample-size.html) in order to compute the number of pageviews we need to run the experiment. The baseline conversion rate for the net conversion is 10.93%. According to the calculator, we need 27411 clicks for each group. Since the ratio click/view is 8%, the number of page views we need is:
```
27441 * 2 * (1/0.08) = 685275
```

**number of page views = 685275 page views**

The daily traffic is 40K pageviews, so **we will need 18 days to perform the test** (assuming that all the traffic is involved in the experiment).


## Analysis

#### sanity checks

We first briefly check that the invariant metrics fall within the expected range during the experiment.
In order to have the 95% interval confidence we calculate : ``` expected_prob +/- 1.96 * SD ```
Both number of cookies and number of clicks fall within this range.

#### recommendations

We can now focus on the results of the experiment. 
We want to test if the proportion of students paying and students enrolled is different between the control group and the experiment group.
In order to do this, we usually run a **t test**.

**assumptions**
- independance : the data was randomly divided between the control/experiment groups - > good
- variance: we check for the homogeneity of variance between the two groups for the evaluation metrics. The Levene test confirms we can accept the null hypothesis for both metrics (p_value(GC) = 0.44, p_value(NC)=0.66).
- normality: we check the normality of distributions for the metrics for each group. The Shapiro tests reveal that we can't verify this assumption.
** We cannot run a t test**.
As the normality assumption does not hold here, we use a non parametric test called **Mann-Whitney U** which performs well in those [cases](https://digitalcommons.wayne.edu/cgi/viewcontent.cgi?referer=https://en.wikipedia.org/&httpsredir=1&article=1011&context=coe_tbf)

**results**

| evaluation metric  | p value | null hyp | alternative hyp |
| ------------------ | ------- |
|  gross conversion  | 0.0194  |
|  net conversion    | 0.6039  |

