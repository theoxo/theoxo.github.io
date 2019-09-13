---
layout: subpage
title: Theo X Olausson
subtitle: A layperson's introduction to memory consistency
topics: [computer-architecture, tutorial]
---
<p>
Last Christmas I was reading <a href="https://dl.acm.org/citation.cfm?id=2028905"><em>A primer on memory consistency and cache coherence</em></a> in preparation for an internship. It’s a brilliant book, and covers both memory consistency and cache coherence–two fundamental aspects of modern computer architecture–in great detail. There was however one slight issue: I was getting <em>none</em> (or at least very little) of the memory consistency chapters. Now, a year and a half later, I feel as if I am finally getting the hang of it, and today I therefore wanted to offer you a perhaps slightly more accessible introduction to the topic.
</p>

<h3>What is Memory Consistency and what is a Memory Consistency Model?</h3>

<blockquote>
The memory model specifies the allowed behavior of multithreaded programs executing with shared memory.
</blockquote>
<small>
D.J. Sorin, M.D. Hill, D.A. Wood: "A primer on memory consistency and cache coherence".
</small>
<br>
<p>
Clearly, memory consistency is the act of keeping memory consistent, but
what exactly do we mean by <em>consistent</em>? To me, there are two criteria for shared memory
to be considered consistent:
<ol>
  <li> Given the program, it is at any given time possible to reason <b> accurately</b>, though perhaps <b> not precisely</b>, about what state the memory is in. </li>
  <li> There are mechanisms to narrow the precision of this if needed. </li>
</ol>
Reasonably, we then need to define some form of <em> model </em> to help us achieve this. Specifically, this model should tell us in no uncertain terms what behaviour is–and what isn't–allowed in a multi-threaded, shared-memory environment,
so that we can use this model, compare our program to it, and figure out what possible outcomes are permitted by the hardware.
</p>
<p>
To meet the second criteria, we need support from the ISA via special instructions that allow us to narrow the set of permitted outcomes. Luckily, as we will see in our case studies later, real-world memory models do indeed support this. For now, let's dive more deeply into the various models we can define, starting with the strictest we can think of and then increasingly relaxing the model as we realize it allows us to optimize our programs.
This will not be a complete survey of the memory consistency models which have been devised over the years, but rather just a quick survey of the landscape.
</p>

<h3>The Memory Consistency Model Design Space: A Brief Overview</h3>

<p>
<h4>The base: Strict Consistency</h4>
The simplest memory consistency model one can device is Strict Consistency, which simply demands two things:
<ol>
  <li> Every processor carries out its memory accesses in the order they appear in the program (often dubbed program order); </li>
  <li> All writes are made visible to all processors instantaneously.</li>
</ol>
However this is not very interesting as it is of course impossible to actually implement: the electricity can only travel so fast, so given that we need to physically connect the different processors together it becomes impossible to meet Strict Consistency's second criteria. Its first, however, is quite intuitive and therefore lies at the heart of our next model, Sequential Consistency.
</p>

<p>
<h4>The realist: Sequential Consistency (SC) </h4>
Sequential Consistency (often referred to simply as SC), first described by Lamport in 1979, is in many ways a realistic version of Strict Consistency. It is often described with the aid of this illustration:
</p>
<img  style="width:450px; height:300px; display:block; margin-left: auto; margin-right: auto;" src="/assets/SCdiagram.png" alt="A pictoral description of Sequential Consistency, showing several processor connected to the memory through a shared channel."/>
<p>
The idea that this illustration is hoping to portray is that at any given time, precisely one  processor is connected to the memory, allowing it to carry out its writes. Thus, when a processor is given the attention of the memory controller its writes will be seen not necessarily instantaneously but at least in the same order by all other processors, as none of them will be able to themselves carry out any writes until the processor relinquishes control of the memory. Like Strict Consistency, the processor itself executes each access in the order in which they appear in the program.
</p>
<p>
Ok, that makes sense, but why would we ever choose not to do this? Well as it turns out, you can squeeze a lot more performance out of a processor if you allow this assumption to be relaxed.
</p>
<h4>Weakening things up a bit: Total Store Order (TSO) and Processor Consistency (PC)</h4>
<p>
Suppose you’re preparing to cook a nice meal together with your partner. However, after deciding on what you want to eat, you realize you’re missing some key ingredients, so your partner heads off to the store. Now, you can sit there and wait for them to come back, but why not save some time and get started with what you have?
</p>
<p>
Similarly, if a processor wants to write to an address A and then read from an address B, chances are it doesn’t actually need that write to A to finish (i.e. be visible to all processors) before it executes the read from B. This is the motivation for Total Store Order and Processor Consistency, which both permit a processor to re-order its own reads before its own writes.
</p>
<p>
However, sometimes it may indeed be that you must nonetheless wait, just as you might think it to be a good idea to wait for your partner to come home with the can of tomatoes before you put the beans into your chilli. (If you recall our discussion on memory consistency before, this is where the second criteria for consistent memory comes in!) To get this level of control back, we introduce a special instruction–typically called a fence or a barrier– which simply guarantees that any memory access placed before the barrier will have finished before anything after the barrier is executed. (<em>Simple exercise for the reader: how can we use this special instruction to enforce SC behaviour on a TSO system (or indeed any system supporting such instructions)?</em>) Note that just like you would when waiting for that can of tomatoes, the processor might have to sit still for quite some time before moving on from this fence in order to give all the memory accesses time to finish, and thus any such barrier carries performance overhead.
</p>
<p>
<h4>The extremists: Relaxed consistency models</h4>
In the extreme case, we might not just allow a processor to reorder reads before writes, but we might also relax one, two, or perhaps all three of the other constraints (reads follow reads in program order, writes follow writes in program order, writes follow reads in program order).
This can allow us to do some cool stuff, like prioritizing memory accesses which can be fulfilled faster due to their location, or coalescing of requests.
</p>
<p>
Unsure what I meant by that last part? Think back again on your partner who has gone to the store to pick up the missing ingredients for the meal you are preparing together this evening. Suppose then that upon arriving at the store, they pick up only the first item on your shopping list. They then go up to the self-checkout counter, pay for it, and carry it out to the car. Once finished, they head back into the store and purchase the second item on your list, and so on. Obviously, this is extremely wasteful, and just like your partner could speed this up by buying everything at once (getting that delicious falafel to you all the quicker), one can improve the performance of a computer by coalescing requests to memory.
</p>
<p>
For a slightly more concrete example, perhaps I am adding some numbers up one at a time, and storing the (intermediate) results at A. Well then the other processor who is waiting for me to finish so that they can use the result of my calculation don’t really need to see the intermediate results at A, do they? We could effectively squash all the writes into one at the end containing only the final value.
</p>
<p>
Of course, working with such a relaxed model means we need even greater help from the instruction set so that we can enforce ordering when we need to. Luckily architectures implementing relaxed models often offer a plethora of different fences and barriers, allowing the programmer to micromanage the order as needed. Let’s look at some concrete examples of this!
</p>
<h3>Quick case studies from industry: x86 and ARM</h3>
<p>
To round this post off, I wanted to quickly put some of the above into context with some real world examples.
</p>

<p>
x86 has a wondrous history of being hilariously ambiguously specified, as is for example laid out in the abstract of the 2010 paper <a href="https://www.cl.cam.ac.uk/~pes20/weakmemory/cacm.pdf">"x86-TSO: A Rigorous and Usable Programmer’s Model for x86 Multiprocessors"</a>: 
<blockquote>
We review several recent Intel and AMD specifications, showing that all contain serious ambiguities, some are arguably too weak to program above, and some are simply unsound with respect to actual hardware.
</blockquote>
<br>
Ouch! This largely follows from the fact that specifications are often (even to this day) mainly written in ambiguous natural languages (e.g. english), as describing them mathematically would be nigh insurmountable. However, one cannot blame Intel or AMD for this: modern microarchitectures are incredibly complex and have evolved in the industry over decades, unlike the memory consistency models devised by researchers in academia.
Nonetheless, as the title of the paper might suggest, it turns out that x86 is “kind of” like TSO (and kind of not), which the authors show with the help of a series of litmus tests (very, very short programs ran many times over on the processor to see which ordering of accesses is possible). We thus expect x86 to support a fence instruction, and indeed it does: aptly named mfence, the instruction guarantees that all memory accesses before the mfence become globally visible before any of the instructions which follow it do. In order to avoid the full overhead of a mfence when only parts of its functionality are actually required, x86 also supports lfence and sfence, ordering only loads and stores (respectively).
</p>
<p>
ARM’s architecture(s), like the ARMv8 64-bit, fall on the weak end of the spectrum. Largely allowing any reordering unless the operations are implicitly ordered by certain dependencies, the architecture demands special care from the programmer in exchange for promises of great efficiency. (Perhaps this is because ARM first saw success targeting mobile computers such as smartphones, while Intel and AMD have for a long time focused on stationary computing, where performance (at least power consumption) is not as critical.) As a result, the ARMv8 architecture supports many types of barriers–the DataMemoryBarrier (which specifies only ordering), DataSystemBarrier (which specifies ordering and also won't complete until all previous accesses have) and the InstructionSynchronizationBarrier (affecting only the loading of instructions onto the CPU)–many of which also take arguments indicating whether they should apply to loads, stores, or both. In comparison to x86, programming on ARM is thus more complex, but the gain lies in the performance of the chip.
</p>
<h3>Rounding off</h3>
<p>
I hope you have found this brief introduction to the topic of memory consistency sufficiently gentle and helpful, even if only as a supplement to other sources of text. Though compilers largely hide the complexity involved in these models from most programmers, understanding memory consistency–and in the not-so-distant future, memory persistency–is a critical step in order to understand how modern computers work.
</p>
<p>
This is the first technical blog post I’ve written, and though it was incredibly challenging (honestly way harder than it sounds), I definitely feel like I learnt a lot from it; that being said, please contact me if you spot any mistakes or inconsistencies in this article, or in general if there’s anything you think I could do better.
<p>
Until next time!
</p>
</p>
