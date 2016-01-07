---
layout: post
category: old
title: 'Simulating Hidden Markov Models using R'
date:  2013-08-11
excerpt: "Ever wanted to explore Hidden Markov Models? It's extraordinarily easy using some of R's packages. Here I'm going to run through a quick example of modeling HMM's with the Viterbi algorithm and Posterior probabilities on a simulation using the HMM package, with a focus on computation and visualization rather than mathematics (that's all on Wikipedia, anyways)."

---
Ever wanted to explore Hidden Markov Chains? It's extraordinarily easy using some of R's packages. Here I'm going to run through a quick example of modeling HMM's with the Viterbi algorithm and Posterior probabilities on a simulation using the __HMM__ package, with a focus on computation and visualization rather than mathematics (that's all on [Wikipedia](http://en.wikipedia.org/wiki/Hidden_Markov_model) anyways!)


### Scenario ###
Consider a casino which uses a fair die most of the time, but occasionally they switch to a loaded die. The loaded die has probability 0.5 of a six and probability 0.1 for the numbers one to five. Assume that the casino switches from a fair die to a loaded die with probability 0.05 before each roll, and the probability of switching back is 0.1.

Here we can see that we're dealing with a simple, two-state HMM with the following transition probability and emission probability matrices:

`TPM = [.95  .05]` <br />
       &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`[.10  .90]`
	   
<br />

`EPM =  F [1/6   1/6   1/6   1/6   1/6   1/6]` <br />
       &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`L [1/10  1/10  1/10  1/10  1/10  1/2]`

I also took the time to create the following "graphic" of the process:

![hidden-markov-model]({{ site.url }}/images/hmm.png)

Not pretty, but it gets the job done in terms of aiding our understanding of the process. 

### Simulation ###
Let's simulate 500 of such dice rolls. To make things easier, I used the __dice__ function from the __TeachingDemos__ package, which supports loaded dice tosses from our EPM matrix. Here's how I ran my simulations:

{% highlight r %}
### Simulation & Modeling of Hidden Markov Model (HMM)
library(TeachingDemos)
library(HMM)
library(ggplot2)
set.seed(1)

### Define our variables
TPM <- matrix(c(.95, .05, 
                .1, .9), 2, byrow = TRUE)
EPM <- matrix(c(1/6, 1/6, 1/6, 1/6, 1/6, 1/6,
                1/10, 1/10, 1/10, 1/10, 1/10, 1/2), 2, byrow = TRUE)
simulations <- 500

### Create a dataframe to hold our results
dice <- rep(NA, simulations)
number <- rep.int(0, simulations)
results <- data.frame(dice, number)

### Simulate
# Assume we start with a fair dice
state <- "FAIR"
for (i in 1:simulations) {
  if (state == "FAIR") {
    # Check to see if we're staying with a FAIR dice
    p <- runif(1)
    if (p <= TPM[1,2]) {
      # If not, roll loaded dice
      roll <- dice(rolls = 1, ndice = 1, sides = 6, load = EPM[2,])[[1]]
      # Remember new state
      state <- "LOADED"
    }
    else {
      # Roll fair dice
      roll <- dice(rolls = 1, ndice = 1, sides = 6, load = EPM[1,])[[1]]
      # Remember old state
      state <- "FAIR"
    }
  }
  if (state == "LOADED") {
    # Check to see if we're staying with a LOADED dice
    p <- runif(1)
    if (p < TPM[2,1]) {
      # If not, roll fair dice
      roll <- dice(rolls = 1, ndice = 1, sides = 6, load = EPM[1,])[[1]]
      # Remember new state
      state <- "FAIR"
    }
    else {
      # Roll loaded dice
      roll <- dice(rolls = 1, ndice = 1, sides = 6, load = EPM[2,])[[1]]
      # Remember old state
      state <- "LOADED"
    }
  }
  # Save dice roll and state
  results[i, 1] <- state
  results[i, 2] <- roll
}
{% endhighlight %}

### Modeling ###
Now that we have a sequence of dice rolls from known states, let's see how well we can predict the states from the sequence. Here I'm going to focus on two basic and simple methodologies: (1) the Viterbi algorithm and (2) posterior probabilities. The mathematics for both of these methods can be easily found via Google if you're at all curious. Luckily for the lazy, the __HMM__ package contains functions for computing both of these predictions very easily. Here's how I did it:

{% highlight r %}
### Modeling
# Create hmm using our TPM/EPM
hmm <- initHMM(c("FAIR", "LOADED"), c(1, 2, 3, 4, 5, 6),
               transProbs = TPM, emissionProbs = EPM)
# Pull in results from the simulation
obs <- results[, 2]
# Save Viterbi/Posterior predictions as a new column
results$viterbi <- viterbi(hmm, obs)
results$posterior <- posterior(hmm, obs)[1, ]
results$posterior[results$posterior >= 0.5] <- "FAIR"
results$posterior[results$posterior < 0.5] <- "LOADED"
# Check out results
table(results$dice)
table(results$viterbi)
table(results$posterior)
{% endhighlight %}

Our simulation resulted in 2012 fair dice rolls and 988 loaded dice rolls, with a model accuracy of 90.97% for the Viterbi algorithm and 95.43% for posterior probabilities. 

### Visualization ###
Accuracy alone doesn't tell the full story. Let's visualize our sequence predictions to see how they compare to the true sequence. In the following plot, the true sequence is given at the top, and the two following sequences are our predictions. The red points in the prediction plots represent wrong predictions.


{% highlight r %}
### Plot predictions with true sequence
p1 <- ggplot(aes(x = seq_along(dice)), data = results) +
      geom_point(aes(y = dice)) + 
      ylab("State") + xlab("Dice Roll (In Sequence)") + ylab("State") +
      ggtitle("Actual Results")

p2 <- ggplot(aes(x = seq_along(dice)), data = results) +
        geom_point(aes(y = dice), color = "#F8766D") + 
        geom_point(aes(y = viterbi), color = "#00BFC4") +
        xlab("Dice Roll (In Sequence)") + ylab("State") +
        ggtitle("Viterbi Predictions")

p3 <- ggplot(aes(x = seq_along(dice)), data = results) +
      geom_point(aes(y = dice), color = "#F8766D") + 
      geom_point(aes(y = posterior), color = "#00BFC4") +
      xlab("Dice Roll (in sequence)") + ylab("State") +
      ggtitle("Posterior Predictions")

grid.arrange(p1, p2, p3, ncol = 1)
{% endhighlight %}

![hmm-vis]({{ site.url }}/images/hmm_vis.png)

Both prediction methods seem to model the known states pretty well, at least capturing the major trends. Finally, we can also take a peek at our posterior probabilities. 

{% highlight r %}
### Posterior probabilities
# Calculate Posterior probabilities for dice, save as new column
results$posterior_probabilities <- posterior(hmm, obs)[1, ]
rects <- data.frame(ymin = c(0, 0.5), ymax = c(0.5, 1), Prediction = c("Tails", "Heads"))
# Plot posterior probabilities
ggplot() + 
  geom_rect(data = rects, aes(xmin = -Inf, xmax = Inf, ymin = ymin, ymax = ymax, fill = Prediction), alpha = 0.2) +
  geom_line(data = results, aes(x = seq_along(dice), y = posterior_probabilities), color = "grey", size = 0.7) +
  geom_point(data = results, aes(x = seq_along(dice), y = posterior_probabilities), stat = "identity") +
  ggtitle("Posterior Probabilities") +
  xlab("Dice Roll (In Sequence)") + 
  ylab("Probability of Heads")
{% endhighlight %}

![hmm_posterior]({{ site.url }}/images/hmm_posterior.png)
