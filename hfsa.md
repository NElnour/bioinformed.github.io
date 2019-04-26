---
layout: post
title: "Hierarchical Finite State Automata"
date: 2019-04-25 22:07:30 -0500
author: Nada Elnour
---


## Finite State Automata

Finite State Automata (FSA) are used in modelling reactive or event-driven systems. provided that the system's/problems are small. Once the problems are large, the number of states explode in size - i.e. the problems become unmanageable but the problem still requires an event-driven solution.

The cause of this state explosion is repetition:
![pyDehydro](/imgs/pyDehydro.png)

Solution?

Group the states together to share transitions whenever possible. Devised by David Harel [^1], the groups become superstates composed of substates in this hierarchical FSA (or HFSA).

## HFSA

Because they are built bottom-up, the superstates and their components behave the same. A form of behavioural inheritance mimicking OOP inheritance, substate transition profiles can impart adaptation to the superstate. A substate can also override the superstate's *modus operandi* thereby contributing polymorphism. In short, the Liskov substitution principle applies for superstates and their substates.

[^1]:
    Harel, D. (1987) Statecharts: A Visual Formalism for Complex Systems. *Sci Comp Prog* 231--274.