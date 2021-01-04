---
layout: blogpost
title: Theo Olausson
subtitle: Paper Review | Compositional Falsification of Cyber-Physical Systems with Machine Learning Components
topics: [paper-review, artificial-intelligence, machine-learning, formal-verification]
show: true
---
<p>
I love machine learning. It has always fascinated me how what one may
at first dismiss as simple curve fitting can overcome seemingly insurmountable tasks.
Plus, it's just a whole lot of fun training a model and constructing some nice plots.
</p>

<p>
At the same time, there are aspects of machine learning that worry me, particularly
as it gets "deeper". (One such worry I have is that the fact that the models are becoming so large;
see my <a href="/2020/05/12/quant-analysis-arxiv.html">earlier blogpost</a> on memory-constrained ML.)
My biggest worry of all is how we can actually tell what is happening in a model, and how it
will react if pushed outside of its comfort zone.
Ultimately, if ML-based systems are to be rolled out across society, it seems like a good idea
to know that we can make certain guarantees about their behaviour. 
</p>

<p>
As a result, I have been watching the research community surrounding verified, safe ML for quite some time.
I was therefore very happy when I was asked in my
<a href="https://www.inf.ed.ac.uk/teaching/courses/dmr/">Decision Making in Robots and Autonomous Agents</a>
class to write a short review of a paper in a relevant topic, the only requirements being that it had
to be written in the IEEE conference paper format and could not exceed four pages in length.
After some discussion with the professor, I settled on reviewing Dreossi et al.'s
<a href="https://arxiv.org/abs/1703.00978">Compositional Falsification of Cyber-Physical Systems with Machine Learning Components</a>,
using Seshia et al.'s earlier paper <a href="https://arxiv.org/abs/1606.08514">Towards Verified Artificial Intelligence</a>
to provide some background.
</p>
<p>
I put a lot of work into making my review as fair and comprehensive as possible, and ended up getting an A(1) in the homework.
Now that some time has passed, I have also been given the OK from the professor to share my paper online, so
if you're interested, you can find a <a href="/assets/review-verified-cps.pdf">link to the paper PDF here!</a>
</p>

<h3>Abstract</h3>
<p>
Systems based on Artificial Intelligence (AI) have become commonplace
in society, including those which are Cyber-Physical Systems (CPS)
employing Machine Learning (ML) for tasks such as perception.
As these systems become
more important in society, so does making sure that they
can be trusted with our well-being. In this paper,
we provide an overview of the challenges in verifying AI-based
CPS, basing our discussion on Seshia et al.'s influential paper
<em>Towards Verified Artificial Intelligence</em>.
We then review one recent effort for the verification of
CPS with ML components, Dreossi et al.'s <em>Compositional Falsification of Cyber-Physical Systems with Machine Learning Components</em>,
initially summarising but then also critically evaluating its contributions. Finally, we outline future work in this space.
</p>
