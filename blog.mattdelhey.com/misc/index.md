---
layout: default
title : 'Rough Theory'
category: 'misc'
highlighter: FALSE

---

<h1 class="entry-title"><a href="{{ post.url }}">Miscellaneous</a></h1>
Here are a few things which shouldn't be of any interest to you.

<center> [Essays](#essays)  / [Notes](#notes) / [Old Posts](#old) </center>

<a id="essays"></a>
<h2 class="entry-title"><a href="#essays">Essays</a></h2>

<!--
The following are a collection of essays I've written in my
undergraduate philosophy courses. I don't think any of them are worth
bragging about. I've tried to include a summary of what I attempt to
do in each paper.

You might notice my papers are not shining. The reason is known to
every student: the most interesting roads of inquiry are the most
treacherous. It is much better for one's grade (and sanity) to
approach writing in the so-called traditional analytic style, taking
up clarity at the cost of relevance and enforcing rigor of argument,
in the sense of certainty in one's conclusions, over the possibility
of establishing interesting truths. In this regard philosophy should
look to mathematics, which has long acknowledge that are many (perhaps
infinite, they say) truths but only some are worth the bother.

To write philosophically on questions of actual interest is an
academic pathology, which must explain the German popularity of the
Greeks.
-->

These are undergraduate essays: I don't think any of them are worth
bragging about. You've been warned.


<ul>
	<li>
		<a href="{{ site.url }}/essays/Quine-Husserl.pdf">
			Two Husserlian Objections to Quine's Naturalized Epistemology
		</a> (Spring 2014)
		<blockquote style="font-style:normal">
			Should we adopt Quine's naturalized epistemology? In "Philosophy as Rigorous Science", Husserl raises two objections to naturalism: (1) its lack of normative power and (2) its circular nature. I agree with the views purposed by David Smith and Sonja Rinofner that Quine could be taken to be advancing two distinct forms of naturalism, ontological or methodological. It seems as if the debate between epistemic naturalists and epistemic anti-naturalists has unavoidable ontological dependencies, turning on the paradox of intentionality, which concerns the nature-status attributed to the field of intentionality, i.e. whether we understand consciousness itself as part of the natural world.
		</blockquote>
	</li>
	<li>
		<a href="{{ site.url }}/essays/Spinoza-p13.pdf">
			Spinoza: Proposition 13, Ethics II
		</a> (Fall 2014)
		<blockquote style="font-style:normal">
			Spinoza's Proposition 13 asserts that the object of the idea constituting the human mind is indeed nothing but an existing body. In this paper I first observe that Spinoza's argument for Pr.13 is two-fold, each in the form of proof by contradiction. I offer a basic (i.e. naive) reconstruction of the argument and raise two objections, which I find fatal to the demonstration as reconstructed. Finally, I offer a second reconstruction that (1) mediates my objections by means of (2) better capturing Spinoza's implicit assumptions in the demonstration.
		</blockquote>
	</li>
	<li>
		<a href="{{ site.url }}/essays/Spinoza-imm-soul.pdf">
			On Spinoza's Doctorine of the Eternal Mind
		</a> (Fall 2014)
		<blockquote style="font-style:normal">
			In Propositions 22-23 of Ethics V, Spinoza turns his discussion to the eternality of the essence of body and the human mind. The full statement of the doctrine is Proposition 23: the human mind cannot be absolutely destroyed along with the body, but something of it remains and is eternal. This doctrine has important implications for Spinoza’s moral philosophy as well as his metaphysics and has thus been the focus of much scholarly attention. In this paper, I first put Spinoza’s argument on the table with a basic reconstruction. I then consider both Curley’s and Bennett’s interpretation of the argument and its relevant premises. I agree with Bennett in his objection to what he calls Curley’s symmetrical account, but aim to avoid Bennett’s conclusion that Spinoza’s argument is altogether wrong by exploring the possibility of a new interpretation of the relevant premises.
		</blockquote>
	</li>

	<li>
		<a href="{{ site.url }}/essays/Derrida.pdf">
			Derrida on the Possibility of Responsibility
		</a> (Spring 2013)
		<blockquote style="font-style:normal">
			 In this essay I ﬁrst sketch the essential context from
             Patocka's history of responsibility and then examine the
             justiﬁcations given for each move in Derrida's argument
             for the irreplaceable self and the possibility of
             responsibility.
		</blockquote>
	</li>
</ul>

<a id="notes"></a>
<h2 class="entry-title"><a href="#notes">Notes</a></h2>
The following are notes taken from lecture and course
materials in some of my undergraduate classes in statistics and
philosophy. It goes without saying that all content belongs to it's
original creators.
<h3>Statistics</h3>
<ul>
	<li>Stat 310: Intro to Probability and Mathematical Statistics (<a href="{{urls.media}}/stat310slides.rar"><strong>Lecture slides</strong></a>)</li>
	<li><a href="https://gist.github.com/af0d8f272a0ff1005127">Stat 405: Intro to Data Analysis</a> (<a href="{{urls.media}}/stat405slides.rar"><strong>Lecture slides</strong></a>)</li>
	<li>Stat 410: Intro to Regression and Statistical Computing (<a href="{{urls.media}}/stat410slides.rar"><strong>Lecture slides</strong></a>)</li>
	<li>Stat 640: Statistical Learning (<a href="http://www.stat.rice.edu/~gallen/stat640.html"><strong>Lecture outlines/code</strong></a>)</li>
</ul>
<h3>Philosophy</h3>
<ul>
	<li>Phil 201: History of Philosophy I &mdash; Ancient Greek Philosophy (<a href="{{urls.media}}/phil201slides.rar"><strong>Lecture slides</strong></a>)</li>
	<li><a href="{{urls.media}}/phil202.pdf">Phil 202: History of Philosophy II &mdash; Modern Philosophy</a></li>
	<li><a href="{{urls.media}}/phil306.pdf">Phil 306: Ethics</a></li>
	<li><a href="{{urls.media}}/phil312.pdf">Phil 312: Philosophy of Mind</a> (<a href="{urls.media}/phil312slides.rar"><strong>Lecture slides</strong></a>)</li>
</ul>


<a id="old"></a>
<h2 class="entry-title"><a href="#old">Old Posts</a></h2>
<ul>
{% for post in site.categories.old %}
<li>
  <h3 class="entry-title" id="{{ post.id }}">
    <a href="{{ post.url }}">{{ post.title }}</a>
  </h3>
</li>
{% endfor %}
</ul>
