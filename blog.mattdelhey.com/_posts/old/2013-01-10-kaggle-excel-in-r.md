---
layout: post
title: 'Kaggle: Getting Started with Excel -- In R'
category: old
date: 2013-01-10
excerpt: "If you're reading this blog post then you might have used Kaggle, a flashy resource in the data science world that works both as a platform for competition and for knowledge. The focus of the site is machine learning, something that is rather new to me. Being the awesome people that they are, Kaggle has put together a short series of guides for getting started, the first of which involves using Excel. But Excel is kinda boring, so I thought it would be fun to run through the guide using R and ggplot2 visualizations."

---

If you're reading this blog post then you might have used Kaggle, a flashy resource in the data science world that works both as a platform for competition and for knowledge. The focus of the site is machine learning, something that is rather new to me. Being the awesome people that they are, Kaggle has put together a short series of guides for getting started, the [first of which](http://www.kaggle.com/c/titanic-gettingStarted/details/getting-started-with-excel) involves using Excel. But Excel is kinda boring, so I thought it would be fun to run through the guide using R and ggplot2 visualizations.

Hopefully if you're just learning R you will be able to gain some useful insights from walking through these basics procedures.

The competition that we're going to work on is [Titanic: Machine Learning from Disaster](http://www.kaggle.com/c/titanic-gettingStarted). The data set for this competition is intentionally small, so there are no worries about hardware limitations here, and, unfortunately, the only prize for winning this competition is knowledge (not too bad, in my opinion). Using real data from the sinking of the Titanic, we want to create a model that can predict whether a passenger would survive or not based on information regarding variables such as their sex, ticket class, and age. Pretty cool huh? Let's get started! 

### Getting the data ###
Obviously, the first thing we have to do to is load up the [data](http://www.kaggle.com/c/titanic-gettingStarted/data) into R. Download the _train_ and _test_ csv (comma separated value) files and load them up in R using the super easy _read.csv_ function:

{% highlight r %}
train <- read.csv("train.csv", stringsAsFactors = FALSE) 
test <- read.csv("test.csv", stringsAsFactors = FALSE)
{% endhighlight %}

Notice that I set the default option for _stringsAsFactors_ as _False_. While this isn't necessary--especially for our application here--it really makes a lot more sense to think about strings as strings rather than factors, especially for things like names. Of course, if we need to convert our strings to factors later we can easily do so using the _factor_ function.

The names _train_ and _test_ are pretty standard in machine learning, and it makes sense why: we use the _train_ data set to literally train our model, i.e. the data set contains things about the passengers and whether they survived, whereas the _test_ data set only contains things about the passengers and we use our model to determine whether they survived. We are then graded on the accuracy of our predictions by uploading the _test_ csv file to Kaggle. In real-world applications, often the "test" data set is created by removing a random sample from the original. 

### Finding patterns in the data ###
So we have the data loaded into R, now what? 
This is most difficult (and often most unappreciated) aspect of any data analysis: finding the trends and patterns in the data. Luckily, the Getting Started guide walks us through a few intuitions, so no cleverness necessary just yet. Specifically, we are going to take a look at Sex, Age, Passenger Class, and Ticket Fare and look for relationships between these variables and survivability. The R code used to explore each of these variables will be posted in each section. 

### Sex ###
Does one's gender tell us anything about their likelihood of surviving? This seems intuitive, so let's ask the data. Using _ggplot2_ we can quickly look at a bar-graph illustrating who survived based on sex. 
![excel-sex]({{ site.url }}/images/excel-sex.png)
Notice that if a passenger is female they are much more likely to survive than if they are a male. Using simple algebra in R, we can see that females have about a 74% survival rate while males have about a 19% survival rate.

So let's create a very simple model: we will predict that if a passenger is female, then they will survive and if a passenger is male, then they will not survive. In order to make our prediction, we have to first enter in our survived values into the test data frame and then save it to a new csv file. The test csv file is then uploaded directly Kaggle as a submission of our model. All of this is easy in R.

{% highlight r %}
# Look at proportion of survival by sex
qplot(factor(survived), data = train, fill = sex) + facet_wrap(~ sex)
male <- train$sex == "male"
female <- train$sex == "female"
lived <- train$survived == "1"
(length(train[male & lived, 1]) / length(train[male, 1])) * 100
(length(train[female & lived, 1]) / length(train[female, 1])) * 100
 
# prop male survived: 19%; prop female survived: 74%
# We will say that all women are going to live; all men are going to die
 
# Make the first gender-based prediction
test$survived[test$sex == "female"] <- 1
test$survived[test$sex == "male"] <- 0
write.csv(test, "genderbasedmodel.csv")
{% endhighlight %}

### Age ###
So we recognize that gender plays an important role in predicting survivability, but what about age? To make things easier, let's divide age into two groups: adult (age >18) and child (age <=18). Once again, this seems to match our intuitions on who might survive, but let's check it out using the same style bar-graph.
![excel-age]({{ site.url }}/images/excel-age.png)
Notice that I also kept the sex variable represented as color. We can see that children were more likely to survive than adults. Just as interestingly, we see the same gender patter where females are more likely to survive than males. Through calculation we can see that about 78% of female adults survived while only about 18% of male adults survived. The Getting Started Guide argues that this indicates that the age variable isn't telling us much more additional information than the sex variable, so we ignore it for now. The proportion calculation in the R code is a little tricky to understand, but basically we have to ensure that we do not include NA's (which there are quite a few of) in our percentage.

{% highlight r %}
# Improve the prediction by considering Age
adult <- train$age > 18
child <- train$age <= 18
train$age.bin[adult] <- "adult"
train$age.bin[child] <- "child"
train$age.bin[!adult & !child] <- NA
qplot(factor(survived), data = train, fill = sex) + facet_wrap(~ age.bin)
length(train[adult & male & lived & !is.na(train$age), 1]) / 
  length(train[adult & male & !is.na(train$age), 1])
length(train[adult & female & lived & !is.na(train$age), 1]) /
  length(train[adult & female & !is.na(train$age), 1])
# Age doesn't give much additional information (than sex) regarding survivability
{% endhighlight %}

### Passenger Class and Ticket Fare ###
What can Passenger Class and Ticket Fare tell us about survivability, and what's the relationship between the two? To make things easier, we can bin the ticket fares from less than $10, between $10 and $20, between $20 and $30, and greater than $30. Let's make the same bar graph again, this time representing fare with color.
![excel_pclass]({{ site.url }}/images/excel-pclass.png)
There are two immediate, interesting findings in this graphic. The first is that, as we would expect, survivability decreases as class increases. But we can also see that some passengers paid very different prices for the same class, even to extent that some passengers paid more for a third class ticket than others paid for a first class ticket. If we look at just the ticket fares we find a similar story, namely that the more a passenger paid for a ticket, the more likely they were to survive. 

{% highlight r %}
# Improve the prediction by considering Passenger Class & Fare
# Consider Fare price by bining from <10, 10-20, 20-30, >30
bin.1 <- train$fare < 10
bin.2 <- 10 <= train$fare & train$fare < 20
bin.3 <- 20 <= train$fare & train$fare <= 30
bin.4 <- 30 < train$fare
train$fare.bin[bin.1] <- "<10"
train$fare.bin[bin.2] <- "10-20"
train$fare.bin[bin.3] <- "20-30"
train$fare.bin[bin.4] <- ">30"
train$fare.bin <- factor(train$fare.bin, levels = c("<10", "10-20", "20-30", ">30"))
qplot(factor(survived), data = train, fill = fare.bin) + facet_wrap(~ pclass)
{% endhighlight %}

### Final Submission ###
With this new information we want to make one final submission. The Kaggle getting started tutorial utilizes pivot tables in Excel, so the form of our predictions is proportions. We say that any cohort of passengers that have a proportion of .5 or greater will survive, and that any cohort with a proportion less than .5 will not survive. This means that we still predict all males to not survive, but also that all women in third class who paid over $20 for their ticket will not survive. Here is a final graphic demonstrating this peculiar trend and the relevant  implementation of our prediction in R:
![excel-female3rd]({{ site.url }}/images/excel-female3rd.png)

{% highlight r %}
# Make the improved genderclass-based prediction
third <- subset(train, pclass == "3")
qplot(factor(survived), data = third, fill = sex) + facet_wrap(~ fare.bin)
test$survived[test$sex == "male"] <- 0
test$survived[test$sex == "female"] <- 1
test$survived[test$sex == "female" & test$pclass == "3" & test$fare >= "20"] <- 0
write.csv(test, "genderbased-classmodel.csv")
{% endhighlight %}

### Looking Forward ###
Now we've submitted two models, the second of which improves slightly on the first. But I can't help but wonder why women in third class who paid more for their ticket had a significantly  lower probability of survival compared to women as a whole--an observation that contradicts our intuitions. A possible explanation for this phenomenon is simply low sample size and coincidence, an issue demonstrated in our last plot. On the other hand, there could be a more underlying trend in the data that is responsible for this event that could possibly be sniffed out using exploratory data analysis. Perhaps this will constitute a future blog post? Regardless, we can imagine that information on the causation of this trend could be used to increase the predictive power of our model. 

From this exercise, I think it's clear that we want to develop a classifier that can delve deeper into the data than just aggregate proportions. While I don't have an exact idea of how I will tackle this competition as of yet, I'll continue to update with important findings as I come across them. Good luck to all those competing, and may the best model win.
