# Project Title
## Authors
T.J. (Timothy) Wilder

## Problem and technique description

#### What is the problem you're trying to solve?
The general goal is to allow new types of solvers (such as algorithmic solvers for shortest path, max flow, etc.) to be included as elements of neural network architectures.

#### What have people done about it and what are the limitations of existing techniques?
- https://openreview.net/forum?id=BkevoJSYPB, Differentiation of Blackbox Combinatorial Solvers by Marin Vlastelica Pogančić, Anselm Paulus, Vit Musil, Georg Martius, Michal Rolinek
- Code: https://sites.google.com/view/combinatorialgradients/home
- Limitations:
    - Unable to learn/update the parameters of the blackbox solver
    - Only works with linear objective functions
    - While we can prove things about neural networks and blackbox solvers individually, what can we prove about them when combined?
    - How complex can architectures be? They interposed blackbox solvers at the end of networks, but could they be put into the middle or have weights that circumvent the solver? Are there cases we could use multiple solvers?
#### What do you propose to do?
I propose to look into these limitations and select one or two which I can remove (perhaps under some restrictions), prove they cannot be removed, or produce a concrete reason why they should not or could not practically be removed.

#### What are your measures of success?
- Success would depend on the exact limitation, but is generally weakening or removing a restriction or proving that it can't be
---

## Week of Feb 16

#### On a scale of 1-10, how do we rate our progress over the past week?
9: Got a concrete idea of what the project is going to be with a few possible directions.

#### What did we accomplish from last week's tasks?
N/A

#### What problems or concerns do we have?
Some of these angles may be very challenging, but that's more for this week...

#### What do we plan to accomplish do over the next week?
Re-read the paper to better understand each of the limitations and get a clearer idea on what they've tried to address vs ignored.
