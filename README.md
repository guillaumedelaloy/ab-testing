### Overview

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


### Metrics
<br>
**The unit of diversion** is a cookie. Since users click on "start free trial" before being signed in, or even before having an account, it is not possible to rely on the user_ids.
<br>
**The invariant metrics** , that is to say, the metrics that should be evenly distributed between the control and the experiment group : number of unique cookies, number of unique cookies clicking on start free trial.
<br>
**The evaluation metrics** : 
- the gross conversion (the number of user ids completing checkout and enrolled, divided by the number of unique cookies clicking on "start free trial")
- the net conversion (the number of user ids who payed, i.e enrolled more than 14 days, divided by the number of unique cookies clicking on "start free trial")


### Design the experiment

### Analysis
