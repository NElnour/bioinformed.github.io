---
layout: post
title: "G&#246;del, Escher, Bach: on Emergence and Projections"
date: 2019-10-20 20:07:30 -0500
author: Nada Elnour
---

Earlier this week I decided to read Douglas Hofstadter's [G&#246;del, Escher, Bach: an Eternal Golden Braid](https://www.amazon.com/G%C3%B6del-Escher-Bach-Eternal-Golden/dp/0465026567) or GEB for short. I first knew about this book as part of Lisa's sugegsted reading list and it came with a warning. In fact, within the many praises sung for GEB online the same warning is communicitated time and time again: not for the faint-hearted. If I remember correctly, there was even a reviewer who "finally understood the book after 8 years". So, since it's Halloween season and the general impression that I have gotten from the reviews that it is a conceptually-dense book without paragraphs of technical jargon, I thought '*why not?*'

GEB presents the intersection of music, mathematics, and computer science via their underlying formalisms in two parts. First, it introduces their commonalities with the intention to explore how formal systems can be built. To that end, we begin wrangling the *MU* puzzle by its rules, keeping in mind the strategies belying our decisions towards a solution. Hofstadter emphasizes the solution to the puzzle less so because it is those strategies that will generalize to building complex artificial inetlligence systems &mdash; the focus of the second part.

The MU puzzle is a simple formal language system. The language of the puzzle is *M, I, U* for which the puzzle is whether one can construct *MU* from *MI* given the following rules:
1. if the last character of the string is *I*, one can concatenate *U* to the end;
2. for any $$x \in \{M, I, U\}^*$$, one can change $$Mx$$ to $$Mxx$$;
3. *III* can be replaced with *U* but not *vice versa*;
4. A substring of $$U^n$$ can be simplfied to one $$U$$ but not *v.v.*.
Hofstadter here introduces the concept of axioms (rules), and the difference between theorem (locally defined as an instance of a set of axioms and language) and Theorem (the usual mathematical definition), and constructing of a decision tree systematically to list the possible solutions or theorems. 

What I find profound here, however, is his message of entering and exiting a system in order to work with it. Sure, the example Hofstadter provided was along the lines of knowing when to quit solving a problem &mdash; i.e., looking at the puzzle in terms of its solvability. What about stepping outside of the system in which the solver operates to solve the puzzle, though? Take the classic 9 dots, 4 lines puzzle: we want to connect all 9 dots organized in a $$3\times 3$$ grid on a surface with exactly four straight lines and without lifting the drawing utensil off the surface. To work within the system here is to see the grid as the only surface there is to work with. To step out of it requires the solver to acknowledge that there is a surface outside of the grid for which the rules of the puzzle say nothing &mdash; can be used limitlessly. What's interesting with this view shift, is the message that sometimes the solution is in that which the problem does not regulate. What's even more interesting is that there are two viewpoints for the same problem. In the case of the 9 dots, 4 lines puzzle, one of them leads to a solution, while the other leads to tears. I wonder though if there are systems for which several viewpoints yield a solution, and if there are, how would we reconcile them? In such puzzles, we find the concept of mathematical projection, where projections along the different axes (the viewpoints) yield different solution silhouettes, but they are all ultimately produced by the same "master" solution. How cold we construct this master solution, and practically what would it mean? Conversely, if we find several solutions from different viewpoints do all of the solutions point to a master solution &mdash; i.e. are they reconcilable?

The more I read, the more I understand that GEB is about manufacturing emergence in pyhsical systems. That is, going from legos to lego machines to self-relexive lego-machine-making lego machines. I'm still in my early days with this book, so it's very likely that many of the questions I posed above are addressed later on or not at all in the event that I misunderstood Hofstadter's direction. But, I'm enjoying the fugues the book refernces to illustrate self-reflexivity, so this will be my reading roster for the next year or eight. 

I will keep you posted.
