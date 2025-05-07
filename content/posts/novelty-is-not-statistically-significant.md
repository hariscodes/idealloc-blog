---
title: "novelty is not statistically significant"
date: 2025-05-06
hideComments: true
---

Internet culture, and by extension modern popular culture, has developed a very strange relationship with science. There
is simultaneously a stronger-than-ever desire for "rational thinking" and consensus-driven mental models, and a virulent,
anti-intellctual impatience for things unknown. People want to just have the answers to things they believe that, given the pace
of human progress over the last 150 years, we should have.

The myriad public and private school systems in the United States have their flaws, but I distinctly remember learning
(well before I studied biostatistics more formally in graduate school) what a null hypothesis was, and the fundamentals
of the scientific method. Namely, that an experimental observation can only be used to disprove a prediction, not affirm it.
In this sense, science is not concerned with arriving at "the correct" answer. Its goal is to deduce, when given some information,
if a hypothesis can be proven false. Scientists evaluating an idea or studying a phenomenon will "walk" themselves to a theory
of understanding, produced by either repeatedly failing to prove something wrong, or repeatedly proving things wrong. Or both.

The raw output of science, theories and endless waves of new questions, might make some people *uncomfortable*. Even more
uncomfortable are all the caveats, the nuances, and (horror of horrors) the regular death of established theory due to
observational erosion; data falling like rain upon the canyons of our understanding and carving them anew.

This meticulous, creative exercise - half minesweeper, half painting a beach in your head while staring into the reflection
of the liquid in a water cooler - is the foundation of human technological progress since late antiquity. It is driven by
insight and an unquenchable desire to *know* that can be traced like a line through the history of our species.
Maybe not everwhere and maybe not at the same time (see Sub-Roman Britain), but there's never been a time in
history where someone was not delving into the unknown, driven by curiosity.

This is why AI will not replace engineering jobs.

# wait, what?

Inferential statistics (hereafter referred to simply as "statistics") are weird. They are a very human tool, in that they
are kind of magical when they are well-designed, but can be used in incredibly stupid ways. Even well-trained, well-intentioned people
regularly misapply or misinterpret them. They can help you answer specific questions by quanitfying relationships between variables,
but they aren't meaningful in isolation. Statistics must be interpreted in the context of the study in which they were measured.

It's up to the scientist (and their consultants), to take statistical results of experiments and use them to disprove (or
fail to disprove) their *a priori* hypothesis. It's not automatic. You don't hit a certain p-value and declare yourself King
of All that is Right and Good. The last step requires the researcher to use their insight to tie the math to the idea. It's
a step, like evaluating your assumptions, that good peer reviewers will hone in on for critique.

Sometimes I like to relive particular eras in MMA, so a few days ago I caught myself scrolling through a feed of Nate Diaz interviews
(which are comedy gold, if you're curious). As you might imagine, it was inevitable that clips from Joe Rogan's podcast would start
popping up in this feed as well. One of these clips was from his interview with Neil DeGrasse Tyson, where Joe opened with,
"Do you think that it would be beneficial to teach probability and statistics to people?". Almost in spite of myself, I stopped scrolling
to watch.

After letting out an excited noise of affirmation, Dr. Tyson went on to describe...something? An analysis? a study? I'm not sure what
it was, but some sort of scientific exercise looking at the phenomenon of a "Hot Hand" in basketball. His assertion, or the assertion
of the thing that he agreed with, was that the "Hot Hand" didn't actually exist because, if you're hitting 50% outside the paint,
the likelyhood that you'll hit several shots in a row is "high".

Even if he was absolutely blazed out of his skull, this was so bizarrely backwards that my jaw hit the floor. I had to listen back
to the clip several times to be sure I heard it correctly, then found the original conversation and listened to that to make sure
it was not cut oddly for the short. When you study an observable phenomenon, the statistics cannot tell you if the thing exists or
not. It can only tell you if its existence is anomalous at a specific resolution. The fact that someone making 50% of their
three-point shots has a statistically insignificant relationship with number of shots made in a row cannot be used to conclude
that the Hot Hand phenomenon isn't real; it just means that it can't be explained by that specific statistic.

What happens when you look at it with regards to defensive matchups? Scheme? Specific dynamics between two players? Someone getting
"hot" just means that something about their circustances is making them play above their average for some amount of time. Perhaps their
warm up was particularly effective today, and they play well until they get shoulder checked and some pain causes them to start missing.
Alteratively, maybe they play well until a defensive adjustment is made to prevent a player from exploiting a weakness that they were using
to score.

The thing that's interesting is not using the statistics to determine if there's a fallacy or not, it's figuring out under what circumstances
a Hot Hand can be reliably observed, or even manufactured. It could be impossible to figure that out because there are too many variables at play,
but to simply dismiss the phenomenon because the some math says that it can't explain it is such an uninteresting and incurious position to take.
In other words, its unscientific.

LLMs are fundamentally regurgitative. I don't just mean in the sense that the code they produce looks like vomit, but more that they are
*by design* incapable of producing insight. They have no curiosity. Their worlds are static snapshots of human-produced training material.
They cannot push against the established canon, because the canon dictates the boundaries of their knowlege base. To an LLM, Hot Hands are
a fallacy until they aren't, or they aren't until they are. If you ask an LLM, it will just tell you that it's an ongoing debate and cite it's
sources. This is actually really useful behavior, but if it were up to an AI agent to continue research, our understanding of the topic would not
have been meaningfully advanced past 1985. There's no desire to challege convention or revisit well-established principles. If you asked it what
avenues should be explored next, it'll just parse ideas from the conclusions of the papers it downloaded and fire them at you. At the end of the
day, it's just computing, not really *thinking*.

I think this is easily illustrated with an exercise.

# word problems

I love word problems. I have since I was a child. Especially the ones with the little gotchas you have to think through. It turns
out that LLMs struggle to solve even simple ones consistently. Here's one I made up for fun:

> Billy is stoked that Michael is willing to part ways with some of the ultra-rare baseball cards that he's collected.
> Michael has 3 pristine Mickey Mantles, 2 slightly worn Mark McGwires, 4 slightly worn Mickey Mantles, and a barely-recognizable Babe Ruth.
> The base prices of Mickey Mantle cards are $200, Mark McGwire cards are $50, and Babe Ruth is $1000.
> The condition of the cards modifies the price, where pristine is 100% value, slightly-worn is 75%, worn is 50%, very worn is 25%, and barely-recognizable is 5%.
> After the deal, Michael takes the proceeds and buys an set of classic gaming consoles for $500 and 9 games for $22 each. He has $27 left. What cards does Michael still have?

Every single model I tried got this wrong. What's illuminating is that they all got them wrong in *exactly the same way*. Let me share the output
from Gemeni 2.5 Pro, Claude 3.7 Sonnet, and ChatGPT 4o (going to skip the math because they're all the same):

Gemeni:

>Looking at his initial inventory, the only combination of cards that totals exactly $600 is the 3 pristine Mickey Mantles ($3 * $200 = $600).
>
>Therefore, the cards Michael still has are the ones whose value equals the amount he didn't receive in the sale.
>
>Michael still has the 3 pristine Mickey Mantles.

Claude:

>Now I need to determine which combination of cards Michael sold to get exactly $600.
>Looking at the possible combinations:
>
>If Michael sold all 3 pristine Mickey Mantles: $600 ✓
>
>This matches exactly the $600 we calculated!
>Therefore, Michael still has:
>
>2 slightly worn Mark McGwires ($37.50 each)
>4 slightly worn Mickey Mantles ($150 each)
>1 barely-recognizable Babe Ruth ($50)

ChatGPT:

>Therefore, Michael sold:
>4 slightly worn Mickey Mantles
>
>2 slightly worn Mark McGwires
>
>1 barely-recognizable Babe Ruth
>
>And Michael still has:
>3 pristine Mickey Mantles
>
>✅ Final Answer: Michael still has 3 pristine Mickey Mantle cards.

I also did this with ChatGPT o3 to see how a "reasoning model" would do, and despite taking the longest to come up with an answer, it ended up at the
same conclusion as all the other models, except that it had to run O(n^4) python code to get to it. It did give me a nicely formatted table though:

>To raise the $725 he spent ( $500 for the console set + 9 × $22 for the games + $27 left over), Michael could sell:
>
>| Card (type & condition)         | Value each            | Quantity sold | Sub‑total |
>| ------------------------------- | --------------------- | ------------- | --------- |
>| Mickey Mantle – slightly worn   | \$200 × 75 % = \$150  | 4             | \$600     |
>| Mark McGwire – slightly worn    | \$50 × 75 % = \$37.50 | 2             | \$75      |
>| Babe Ruth – barely recognisable | \$1 000 × 5 % = \$50  | 1             | \$50      |
>| **Total**                       |                       |               | **\$725** |
>
>That combination uses every slightly‑worn Mantle, both McGwire cards, and the (low‑grade) Babe Ruth—leaving the three pristine Mickey Mantle cards in Michael’s collection.
>
>So after the deal, Michael still has 3 pristine Mickey Mantle cards.

First off, all four of the models discounted the other possible answers without explaination - 4 slightly-worn Mickey Mantles are also worth $600.
None of them suggested that the answer could be either groups of Mickey Mantles cards that were left over. All except ChatGPT o3 missed the possiblity of
selling 1 pristine Mickey Mantle, 3 slightly-worn Mickey Mantles, and 2 Mark McGwires, but even it discarded that for reasons it didn't explain.
If you're thinking that the "gotcha" is that there are three correct answers here, though, you're wrong. There's only one.

The thing that all four models discarded was the first sentence of the problem.

> Billy is stoked that Michael is willing to part ways with some of the ultra-rare baseball cards that he's collected.

Poor Billy. None of these models give him any agency at all. He isn't just a passenger in Michael's world though, nor is he just going to accept
whatever the hell Michael decides to give to him. He wants the ultra-rare stuff, and he's specifcally happy that Michael is willing to part ways with it!
There's no reason to be happy if he wasn't getting those pristine Mickey Mantles in the deal. He'd also prefer to get the Babe Ruth, even if it
is damaged, instead of more Mickey Mantles (most of which are slightly-worn). The actual answer is, therefore, that the only cards left are the
4 slightly-worn Mickey Mantles.

You may be thinking that my question wasn't fair, but by word problem standards my example is very pedestrian. First of all, I did not include
many other combinations of things that would add up to the remaining amount. Second, I layed the language of the problem out in really clean terms to make
the required math easy to parse, which word problems often try to obfuscate to some degree. Third, I (mostly) stuck to nice, round whole numbers and avoided
floating-point math, which LLMs are known to struggle with. Finally, the only qualitative context I added to the problem was the first line. Really
tricky word problems contain several different, sometimes conflicting, peices of information that must be used to narrow the correct answer down.

Let's take the tack that this is too hard for an LLM to solve without some hints. When I ask Gemini about the possibility of it being the four,
slightly-worn Mickey Mantles instead, it correctly identifies that there are two responses and concludes:

> Without further information about which specific cards Michael was willing to part with or keep (e.g., prioritizing condition, character, etc.), we cannot definitively say whether he kept the 3 pristine Mantles or the 4 slightly-worn Mantles. Both are mathematically consistent with the total amount of money he received from the sale.

When I point out the qualitative statement at the beginning that should help it narrow down the answer, it says:

>Considering Michael is a collector who values "ultra-rare" cards and is described as being willing to part with some, it is highly probable that he would keep the cards that are most desirable to a collector. Between pristine cards and slightly-worn versions of the same player, collectors typically value condition very highly.
> Therefore, the most likely scenario is that Michael, the collector, chose to keep the 3 pristine Mickey Mantles (the ones in the best condition of a desirable player). This means the cards he parted ways with for $725 were the 4 slightly-worn Mickey Mantles, the 2 slightly-worn Jerry McGuires, and the barely-recognizable Babe Ruth. Billy would indeed be "stoked" to acquire Mickey Mantles (even slightly-worn ones) and a Babe Ruth card.
> The qualitative statement helps us infer Michael's likely motivation as a collector, leading to the conclusion that he kept the cards most valuable in terms of condition.
> Michael still has the 3 pristine Mickey Mantles.

Even leading the model by the nose to the answer, it can't figure it out. Michael clearly doesn't value ultra-rare cards if he's willing to part with
them. In fact, even a cursory reading of the problem makes it clear that that Michael is shifting from baseball card collection to classic video game
collection. All the other models ran into the same pitfalls, including ChatGPT o3; although it had to run some more terrible python code to ensure maximum
ozone depletion in the process. All models were able to correctly reason about the answer after I gave it to them, which is nice I guess.

The real problem is that an average, word-problem-loving middle schooler could answer this problem, even if they don't know who Mickey Mantle is,
as long as they focus on value as a proxy for rarity. It's not even particularly novel or interesting. LLMs, however, seem to be pretty
bad at solving them; an idea that is supported by the [current](https://aclanthology.org/2024.findings-acl.524.pdf) [literature](https://aclanthology.org/2024.findings-naacl.72.pdf)
on the subject.

One quote, in particular, stands out to me:
> The poor performance of LLMs on MCGSM8K confirms the incapable ability of most LLMs in mathematical
> reasoning process evaluation. In particular, the performance on GSM8K-Judgement exhibits that it
> is easier to identify incorrect solutions. However, merely fine-tuning with correct solutions improves
> the performance in evaluating correct solutions, but leads to a huge drop in evaluating incorrect solutions.

Huh...why does that happen?

# good strategy requires self-awareness

In the last section of Richard Rumelt's book *Good Strategy/Bad Strategy*, he describes a scenario where he's requested a paragraph-long diagnosis on
the woes of TiVo from some executives he's running a strategy retreat for. Each of them read the case the night before, and each of them had written
something down regarding TiVos market position, competitors, production issues, etc. Instead of asking them about the substance of their positions,
Dr. Rumelt instead asks them to describe how they arrived at their conclusions.

Most of them are stumped. With some gentle proding, every person admits that (independent of the merits of their diagnosis) they wrote down what
amounted to the first thing that came to their head.

LLMs mirror this behavior to a T, but unlike humans, they cannot be coached out of it. They don't deliver insight, because they aren't self-aware.
They cannot question their own assumptions. They cannot stop to reconsider their position because they cannot stop vomiting statistically-significant
tokens into API responses. Therefore, they are universally susceptible to the pitfalls of producing Bad Strategy, and incapable of producing the insight
required to synthesize Good Strategy. This is why, if you train them on what "correctness" looks like (as the researchers I quoted previously did), they
will struggle to distinguish it from "incorrectness" that is semantically similar.

An LLM cannot tell you what to do because it has no risk tolerance. It can't stop you from choosing the wrong course of action because the world has too much
context to fit into its window and it cannot intuit relevance. This should make sense, because an LLM is just a statistical model. Just as science is
not run by the Church of The p-value, strategy - i.e., novelty - cannot be left to the machines. The model can't search for what we haven't shown it,
and it can't create something that is unlike what we've already made.

There will be a wave of people who vehemently disagree with this, and will attempt to create or use "strategy agents". I recommend finding out which
companies would have leadership so bankrupt of integrity that they would abdicate strategy work, then build businesses to crush them.

# AI, what is it good for?

I think LLMs are going to get really good at reducing toil. In fact, I think we're pretty close already to my whole wish list. I regularly send deep
research bots off to parse hundreds of web pages on my behalf. If you build a good interface, you can have an LLM take a decent stab at implementing
it. One of the things I'm most excited for is reclaiming the part of my brain that knows Regex.

I'm not even remotely scared that LLM agents are going to take engineering jobs, for the same reason that a mechanic shouldn't be afraid of losing their
job because their employer purchases a software diagnostic tool. Good engineering is a creative endeavor where design is driven by
context as much as it is a specific problem. It's about finding an elegant solution to the maze of constraints that you're presented with.
In other words: engineering is more akin to strategy than computation.

We're better off leaning into the strengths of LLMs, than attempting to make them suck the joy out of our lives by automating the process of wonder,
insight, and discovery. They can't do it to anything approaching an acceptable standard, and we risk letting our collecive intellectual muscles atrophy
in letting them try.

Sometimes, the answer is that simple. Let the machines crunch the numbers, and let us have the fun.
