---
layout: post
title: "Biological Hierarchical Finite State Automata"
date: 2019-04-25 22:07:30 -0500
author: Nada Elnour
---


## Finite State Automata

Finite State Automata (FSA) are used in modelling reactive or event-driven systems. provided that the system's/problems are small. Once the problems are large, the number of states explode in size - i.e. the problems become unmanageable but the problem still requires an event-driven solution.

The cause of this state explosion is repetition.

Solution?

Group the states together to share transitions whenever possible. Devised by David Harel [^1], the groups become superstates composed of substates in this hierarchical FSA (or HFSA).

## HFSA

Take for example pyruvate dehydrogenase of the pyruvate dehydrogenase complex (PDC), E<sub>1</sub>. 

![pyDehydro](/imgs/pyDehydro.png)
In the above FSA, TPP is thiamine pyrophosphate. The FSA does not distinguish between the order of substrate binding, with either substrate priming E<sub>1</sub> to an intermediate state and the other's binding transitioning the system to the final (double circled) state. 

It's clear how one can simplify the above automaton provided the order of binding does not matter. For example,
![pyDehydro](/imgs/hfsm.png)

The first state transition takes unbound E<sub>1</sub> to the intermediate state, a superstate composed of equivalent substates. Within the superstate, E<sub>1</sub> either ends up as a TPP-bound enzyme (E<sub>1</sub>-P) or pyruvate-bound (E<sub>1</sub>-pyruvate). Addition of molecules of the same type does not transition the superstate to the final state. The second state transition represents the second option for either of the substates, excluding the option that reinforces the substate. That is, for E<sub>1</sub>-P, addition of TPP to the superstate reinforces the E<sub>1</sub>-P substate and subsequently the intermediate superstate. Addition of pyruvate to the superstate disallows reinforcement of E<sub>1</sub>-P and thus the superstate transitions to E<sub>1</sub>-P-pyruvate final state.

Because they are built bottom-up, the superstates and their components behave the same. A form of behavioural inheritance mimicking OOP inheritance, substate transition profiles can impart adaptation to the superstate. A substate can also override the superstate's *modus operandi* thereby contributing polymorphism. In short, Liskov's substitution principle applies for superstates and their substates.

There is a big assumption permeating this model: that the states are discrete, finite, and characterizable. The issue of discreteness can be addressed using thresholds, difficult as that approach may be. Finiteness and characterizability seem to me difficult to prove. It makes sense, for example, that an enzyme has only so many states, each of which contributing to a function. But is there an upper bound on how many functions an enzyme can have? 

At the heart of the matter, I guess, is whether
such modelling will ever cease to be exploratory. Probably not. Perhaps our best attempt at modelling is crowdsourcing.

[^1]:
    Harel, D. (1987) Statecharts: A Visual Formalism for Complex Systems. *Sci Comp Prog* 231--274.