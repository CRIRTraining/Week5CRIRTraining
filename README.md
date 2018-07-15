---
title: "Week 5"
output: html_document
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```
Before beginning any statistical tests, I have one more R tool I want to share.  Let us say we want to conduct percentage changes on multiple measures and we want to write out the formula each time.  We can write our function and then run the data through it each time.  First, generate some random data.  Then use the function function with the arguments.  Because we want to look at the differences between pre and post, we will need two arguments.  Then we use the brackets to tell R what we want it to do with these arguments.  Then since we saved the function as an argument, we can use the simulated data as the arguments and output the results. 
```{r}
set.seed(1234)
prePHQ9 = rnorm(100, mean = 1.7, sd = 1)
postPHQ9 = rnorm(100, m = 2, sd = 1)
pChange = function(pre,post){
  round(mean((post-pre)/pre),2)
}
pChange(prePHQ9, postPHQ9)
```
Understanding the standard deviation is probably the first step in conducting statistical tests. What is the standard deviation?  The standard deviation is the average amount of distance each value is from the mean.  In the example below, if we were to randomly select several data points from the variable we just generated on average those values would be one value away from the mean of zero.

Large standard deviations means that we are unsure about where the mean is.  This means that if we were to run the program or experiment again, we might get a much different mean.  This is usually bad because we want to be sure about values such as the mean. 
$$\sigma = \sqrt{\Sigma(x-\bar{x})^2/(n-1)}$$
Histograms are created for continuous variables and understanding whether a variable is normally distributed.  What does a normally distributed variable look like?  We can generate data from a normally distributed variable using the rnorm function, telling it how many data points we want, what the mean of the variable will be and what is the standard deviation.

To generate a histogram you can use the hist function for the continuous variable.
```{r}
normVar = rnorm(10000, mean = 0, sd = 1)
hist(normVar)
```
What does a non-normal distribution look like?  It is important to know before conducting any statistical tests.  Here is an example. 
```{r}
non_normVar = rpois(10000, 3)
hist(non_normVar)
```
Often times a goal in statistics is to evaluate if something is statistically significant, but what does that mean?  

Let us try out a simple example.  Let us say we have a difference score between post and pre PHQ-9's and we want to see if this difference is statistically significantly different from zero.  
The research question is whether the difference between the mean of the difference score is different from zero.  Because we want PHQ-9s to be smaller, assuming we provided some intervention, that the PHQ-9 scores would be lower, therefore we hope the mean of the difference scores is lower than zero. 

But how do we tell if the difference score is actually different from zero or if you just happen to get a different score that is really large.  For example, if we ran the experiment 100 times, is our experiment's mean difference score different enough from zero that we can feel confident enough that we did not just randomly get a high difference score during this experiment and the actual mean difference score is different from zero. 

I am going to show the long version to answer this question and using a statistical test that you won't use that often; however, it is easier to learn this version and I will show you the short version and the other version (the t-test instead of a z-test) afterward.

Basically, we need to standardize the difference between zero and the difference score into a format that allows us to use a mathmatical probability distribution to evaluate if the differences are statistically significantly different.  We do this by generating a test statistic which is turning the difference into z-score.  Put it on a scale where we can use the normal distribution to look up p-values.  This is why it is important when using z and t-tests that our data is normally distributed because we are using the normal distribution to evaluate whether there is a statistically significant difference. 
$$ {(0-\bar{x})/\sigma}$$
So for example in R, let us assume we have an average difference score of -10 and this difference score has a standard deviation of 3.  
```{r}
z = (0--10)/3
z
```
Now that we have the test statistic in a format that the normal distribution understands, we can get the p-value.  The correct interpretation of a p-value is, if we were to conduct an experiment an infinite number of times, what is the probability of seeing a test statistic this large (or small) or larger (or smaller).  For example, a p-value of .05 means that 5% of the times we theoretically rerun the experiment, we would get a test statistic of this value or larger (smaller).  We use the threshold of .05 because we have decided as a society that we are willing to tolerate that much error.

To look up a p-value for a particular test statistic we can use the pnorm function in R...

However, the p-norm function gives us the probability of seeing the test statistic of at least that value, therefore, we need to take 1-pnorm(z) to get the probability of seeing a test statistic of that value or greater.

Additionally, if we are unsure if the test statistic is greater than or less than 0 then you may want to conduct a two-tailed test, which means we need to double the p-value.
```{r}
pnorm(z)
1-pnorm(z)
2*(1-pnorm(z))
```
Next week, I will show you how to construct confidence intervals and their interpretation, conduct z and t tests, their assumptions, briefly cover degrees of freedom, and how to conduct paired t-tests.  The following week we will review the non-  version of the t-test, correlation, then bivariate regression, and multivarite regression.   
