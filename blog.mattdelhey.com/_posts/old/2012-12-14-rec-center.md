---
layout: post
title: "Exploring Rice University Recreation Center Data"
date: 2012-12-14
category: old
excerpt: "Here's an example of extracting interesting information from a real data set generated from our on-campus Recreation Center here at Rice University. The data set given to me contains information regarding all swipes into the Rec center in September, 2011, including time (rounded to the nearest fifteen minutes), sex, and status (e.g. graduate student, alumni, etc.)."

---
Here's an example of extracting interesting information from a real data set generated from our on-campus Recreation Center here at Rice University. A little background: The Rec Center is essentially our on-campus gym, open to both students (free) and community members (very not-free). To enter the Rec Center, a user must "swipe" in using their student ID or something similar.

The data set given to me contains information regarding all swipes into the Rec center in September, 2011, including time (rounded to the nearest fifteen minutes), sex, and status (e.g. graduate student, alumni, etc.). In total, the data counted 38,374 swipes, or about 1300 entries a day. Unfortunately, I'm not able to publicly release the data set but I have included all <a href="https://gist.github.com/mattdelhey/8637352">relevant R code as a gist</a>.


### Exploration: Basic Distributions ###
Exploring the general relationships between important variables is one way of getting a general feel of what's going on with the data. It goes without saying that we want an understanding of these basic distributions in the variables before asking any specific questions of the data. Here, I focus on the change in the number of swipes over time and the relationship between sex and status and the number of swipes.

<ul>
  <li>
<h4> Entries over <i>days of the month</i> </h4>
<center><img src="{{ site.url }}/images/rec-month.png" width="500" height="350" /></center>
The change in swipes over days of the month shows us that the Rec Center received about the same amount of usage over each week in September, with weekly usage decreasing slightly as the month progressed. Interestingly enough, there is a relative spike in Rec Center usage the day after Labor Day.
  </li>

<li>
<h4>Entries over <i>days of the week</i> </h4>
<center><img src="{{site.url}}/images/rec-week.png" width="500" height="350" /></center>
The change in swipes over the days of the week shows us that the Rec Center saw most of its usage during the work-week, peeking on Thursday.
</li>

<li>
<h4>Entries by <i>gender</i></h4>
As we might expect, there is a discrepancy between the entrance frequency of males and females. While men use the Rec Center noticeably more women, the difference is not as significant as one might think, as males only swiped in about 15% more than females in the month of September. Women also seem to go to the Rec Center on the same days of the week as men, fitting in with the male distribution for every day except Friday and Monday, for which women are relatively more likely to swipe on Friday and men more likely on Monday.
</li>

<li>
<h4>Entries by <i>status</i></h4>

<center>
<table width="400">
  <tr>
    <td>Status</td>
	<td>Count</td>
	<td>% Composition</td>
  </tr>
  <tr>
    <td>Undergraduate</td>
	<td>22806</td>
	<td>59%</td>
  </tr>
  <tr>
    <td>Graduate</td>
	<td>6881</td>
	<td>18%</td>
  </tr>
  <tr>
    <td>Staff</td>
	<td>3745</td>
	<td>10%</td>
  </tr>
  <tr>
    <td>Faculty</td>
	<td>1380</td>
	<td>4%</td>
  </tr>
  <tr>
    <td>Visiting Student</td>
	<td>338</td>
	<td>1%</td>
  </tr>
  <tr>
    <td>Community</td>
	<td>231</td>
	<td>1%</td>
  </tr>
  <tr>
    <td>Alumni</td>
	<td>161</td>
	<td>1%</td>
  </tr>
  <tr>
    <td>NA</td>
	<td>2832</td>
	<td>7%</td>
  </tr>
</table>
</center>

From the table we can notice that the majority of users of the Rec Center are students, making up 77% of the number of swipes. If we include staff and faculty, we are over 90% of the number of swipes. The remaining swipes are mostly those of unknown origin, most likely caused by a manual "swipe" in the Rec Center in situations where someone forgot their ID, etc. This makes it fair, I believe, to speculate that the majority of these unknown swipes fall in student/staff/faculty. This leads us to the unsurprising conclusion that the Rec Center primarily serves the Rice population and is not significantly utilized by non-affiliates.
</li>
</ul>

<h3>Concerns with the data</h3>
Anyone who has visited the Rec Center will now recognize a potential issue with the our data: All swipes are not created equal! In other words, swipes recorded do not necessarily reflect actual entrance and usage of the Rec Center. It is often the case that people swiping into the Rec Center are not properly recorded in one way or another. Additionally, we have no information on how long each user stays in the Rec Center or what they do while they are there. <em>We need to be careful about drawing conclusions about usage from information about swipes!</em>

With this caveat in mind, let's look at the data.

<hr />

<h3>Who is swiping in at the Rec Center late during extreme hours of the day?</h3>
Who is swiping in at the Rec Center early in the morning and late in the night, and do these people fit into the more abstract trends discussed above? The available data set can provide an interesting answer to this question. For an operational definition let us define extreme hours as the first two hours and last two hours before the Rec Center closes and let's also assume that the Rec Center hours for 2011 are the same as they are in 2012.

With this premise I wanted to understand both the sex of these extreme Rec Center swipes. Utilizing the definition of extreme swipes, I created two series of plots which show the distribution of sex within swipes either early or late: 

<center>
<table>
<tr>
<td><img src="{{ site.url }}/images/rec-first2.png" width="250" height="250" /></td>
<td><img src="{{ site.url }}/images/rec-last2.png" width="250" height="250" /></td>
</tr>
</table>
</center>

When we look at gender in the extreme-hour swiping data we find serious discrepancies in the proportions of sex that were not at all apparent in our exploratory analysis. On the average, we swipe data tells us that the majority of the users at the Rec Center are male, but in the last two hours before the Rec Center closes we see this majority significantly increased. For example, on Friday the in the last two hours the Rec Center is nearly 3:1 in favor of male swipes, which is much higher than the 14% increase we see on the whole. On the other hand, looking at the morning data paints an opposite picture. Whereas we usually think of the Rec Center as always being a male majority, it turns out that on Saturday mornings women actually outnumber men in swipes. 

We can also derive another interesting fact from these plots, namely that Sunday appears to be the most equitable day across gender. While every other day seems to fluctuate in its gender balance, Sunday stays relatively constant across the morning and evening. So what should we conclude about this data? I believe that this is evidence for the hypothesis that men prefer late at night use of the Rec Center whereas women prefer early morning use of the Rec Center. There are many reasons why this might be the case, and it seems completely plausible that this conclusion could be true.

<h3>What about status and year?</h3>
As I stated earlier, I would also like to exam the status of the swipes in relationship to time. I accomplish this by looking first at the general trends of each status over all hours, and then finally do a similar analysis of all hours from the perspective of undergraduate class. 

Our intuition tells us that we should expect different groups of people to utilize the Rec Center at different times depending on their schedule and preferences. Let's see if this holds up by looking at the follow plot:

<img src="{{ site.url }}/images/rec-status.png" />

The distributions of the swipe data over the course of a day divided up by status can really show us the different ways in which students might be using the Rec Center. Just as we predicted, similar groups of people swipe into the Rec Center at similar times and vice-versa. For example: notice the similarities between the distribution of Graduate swipes and Undergraduate swipes. Both have small peeks throughout the morning with a very large influx of swiping before dinner. On the other hand, notice the swipe distribution of Staff. There are three large peeks about equidistant apart, most likely marking the beginning and end of shifts. It seems like there is a serious correlation between when people are swiping into the Rec Center and when we would expect them to do so. 

Another interesting trend I would like to point out is that there seems to be two major categories of when the distribution of the status will peak: once in the morning, and once in the late-afternoon. What's really interesting is the distribution about these peaks. For the Alumni status, we see a largest peak in the morning. For Community, Faculty, and Staff we see peaking at the middle that about equals the mid-afternoon. As already mentioned, for Undergraduate and Graduate we see peaking in the mid-afternoon. This seems to match our intuition about students utilizing the Rec Center later in the afternoon while those more tied to the "real world" tend to use the Rec Center earlier in the morning. 

Let's see if this relationship holds for differences in the undergraduate class. This is hard because we have less intuition on how class year interacts with Rec Center swipe time, so let's just jump into the data:

<img src="{{ site.url }}/images/rec-year.png" />

From the distribution of swipes per year there is one major and interesting trend that jumps out to me: as the year increases from freshmen to senior, the distribution of swipes shifts from peaking most significantly in the late afternoon to peaking most significantly in the morning. In other words, as the year increases from freshmen to senior the distribution goes from most resembling the Undergraduate and Graduate distributions described previously to a distribution that, while still distinctly being Undergraduate, appears to adopt some of the qualities of the Alumni or Faculty distributions. The cause of this trend is most likely has to do with the distance one is the Rec Center. As year increases from freshmen to senior, so does the likelihood that one will be further away from the Rec Center and will thus use it differently and produce a different swipe distribution. 

While this is explanation is common sense to most of us, I believe this is an interesting find because of how well the idea is represented in the data. Even if we had no conception of how year might relate to when someone would use the Rec Center, the data clearly shows us that Seniors swipe differently than Freshmen and thus use the Rec Center in a noticeably different timeframe than Freshmen. Therefore I have shown one way in which status and year relate, in a meaningful and interesting way, to time within the swipes data set.
