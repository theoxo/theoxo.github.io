---
layout: blogpost
title: Theo Olausson
subtitle: Dissertation | Towards the Automatic Synthesis of Cache Coherence Protocols
frontimg: /assets/dissertation1.jpg
topics: [computer-architecture, synthesis]
---
<p>Earlier this year I submitted my 4th/senior year dissertation, Towards the Automatic Synthesis of Cache Coherence Protocols. The dissertation summarized and presented work which I had been carrying out under the supervison of <a href="http://homepages.inf.ed.ac.uk/vnagaraj/">Dr. Vijay Nagarajan</a> since my sophomore year, and was awarded an A(2).
<br>
<a href="/assets/dissertation1.pdf">Link to dissertation PDF</a>
</p>

<h3>Abstract</h3>
<p>This paper presents extensive contributions towards
the automatic synthesis of
cache coherence protocols.
Motivated by the insight that even the most sophisticated current
approaches still assume the existence of a correct \emph{Stable State Protocol} (SSP),
an abstract specification of the protocol
under atomic conditions,
it targets these abstractions in particular.
It lays the groundwork for future research towards
synthesis of SSPs
by suggesting
a concrete correctness criterion for
SSPs,
presenting the first publicly available
dataset on which bug localization and synthesis
efforts can be compared,
and deriving a taxonomy of the bugs which
appear in cache coherence protocols.
Finally, two novel heuristic methods
for the localisation of bugs in SSPs are presented
and evaluated. The results of this evaluation indicate
that the two methods can often identify bugs
in protocols at a finer granularity than is currently possible
with standard model checking engines.
Thus, the methods may
serve as useful debugging tools when developing
cache coherence protocols, and open up for future
work towards synthesising fixes to the bugs.
</p>
