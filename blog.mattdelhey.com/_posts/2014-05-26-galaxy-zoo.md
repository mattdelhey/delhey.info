---
layout: post
title: "My Solution To The Galaxy Zoo Challenge"
date:  2014-5-26
excerpt: "Over the course of the spring semester, I was asked to participate in a Kaggle competition of my choosing for my machine learning class. I ended up deciding on the rather interesting Galaxy Zoo problem. I didn't perform all too well but the competition was rather fierce. Turns out CNN's were the way to go (not too surprising)."

---

Over the course of the spring semester, I was asked to participate in
a Kaggle competition of my choosing for my machine learning class. I
ended up deciding on the rather interesting Galaxy Zoo problem. I
didn't perform all too well (145th out of 329) but the competition was
rather fierce. Turns out CNN's were the way to go&mdash;not too surprising.

Check out my
[final report](http://mjd2.blogs.rice.edu/files/2014/05/report.pdf)
and [github](https://github.com/mattdelhey/kaggle-galaxy). If you're
really bored, you can also see the minimalist
[poster](http://mjd2.blogs.rice.edu/files/2014/04/poster1.pdf) I used
in a presentation of my work.

Here's a sneak peak by means of the abstract:

<blockquote>
The Galaxy Zoo Challenge asks participants to predict proportions of
user classifications of galaxy morphologies. Using the Extra-Trees
algorithm and data preprocessing and augmentation, an error rate of
0.12859 and rank of 145/329 was achieved. The key idea behind the
solution was to mimic the success of convolutional neural networks by
implementing a large model with lots of randomness to combat
overfitting. While this approach performed moderately well, a feature
engineering focused approach would likely perform better given my
computational limitations.
</blockquote>
