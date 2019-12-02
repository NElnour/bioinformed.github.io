---
layout: post
title: "Let's Not Rule Out Tabs Yet"
date: 2019-12-01 21:00:00 -0500
author: Nada Elnour
---

## The Case for Spaces

In his 2017 blog, [Developers Who Use Spaces Make More Money Than Those Who Use Tabs](https://stackoverflow.blog/2017/06/15/developers-use-spaces-make-money-use-tabs/), David Robinson explored salary differences between developers who choose to use spaces versus those who choose tabs for code indentation.  David concludes that spaces usage is associated with 8.6% increase in salary when a linear model is fit over the following parameters:
* tab vs. space usage;
* country;
* programming experience in years;
* formal education level;
* open-source project participation; 
* recreational programming; and
* company size. 

David even breaks down the salary differences by country and by developer type to highlight the disparity favouring spaces usage.

The results are definitely interesting, but I argue that we should take a step back and look at what went into the model fitting first.

## Closing the Gap between Tabs and Spaces
I'll be focusing on two important points here. First I'll discuss whether the analysis controlling for the countries is strong. Second, I'll discuss the power of coding habits and redundancies.

### Within-country Disparities
David breaks downs salary disparities between "spaces", "tabs", or "both" users by country. The highlighted countries are the United States (U.S.), India, United Kingdom (U.K.), Germany, and Canada. The argument here is that the countris stand for different GDP-per-capita brackets which could be biasing salaries. I think this is a great addition to the model's factors since David had been comparing the salaries after conersion to US dollars.  

That being said, I think David should go two steps further with this analysis. 

First, while I can see clearly how prominent the disparity is in India, how many of the survey participants are from India? What if it's just 3 respondants? If the sample size per country is small it raises the question of just how many of those disparities are due to randomness? I think David should perform MANOVA to report statistical significance.

Second, even within a country, there are salary disparities depending on the standard of living of its regions. Take for example a metropolitan city like New York, NY with a median household income of [$71,897](https://www.usatoday.com/story/money/economy/2018/05/17/25-richest-cities-in-america/34991163/) versus a town like Selma, AL where the median household income is [$24,223](https://www.usatoday.com/story/money/2019/05/07/poorest-cities-in-every-state-in-the-us/39431283/). By David's argument, it is also conceivable that respondants ine low median household income regions may be more likely to choose one indentation style over the other. Perhaps it's worthwhile to report the median annual salaries per median income household.


### The Power of Coding Habits
David mentions that respondents can report several preferred programming languages. The inclusion of such developers in the dataset, however, is also confounding. For instance, developers who know many languages (i.e. polyglots) might simply be more highly paid that those who know a language or two. It is possible that polyglots also prefer indenting with spaces, thereby biasing the results towards spaces.

If we really want a predictive model (via linear regression), I believe that the model should also be trained with the number of languages as a factor.

## Conclusion

David's post is a reminder to explore the seemingly trivial and unconnected because you might find something unexpected. Nevertheless, I think he should address the confounders I mentioned if he wants to build a predictive model.

Looking forward to seeing the results!
