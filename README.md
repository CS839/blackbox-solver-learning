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

## Week of Feb 23

#### On a scale of 1-10, how do we rate our progress over the past week?
10: Did exactly what was planned

#### What did we accomplish from last week's tasks?
- Re-read the paper
- Of the limitations I listed, the only one they only lightly addressed the linear objective functions and the neural architecture complexity
    - For the linear objective, their only obvious reason is simplicity, but it may be related to their use of the "differentiation over argmin" strategy they're using. It may also be that having a linear objective lowers computational cost of calculating the BackwardPass, but I need to look at more of the background to understand that better.
    - For the architectural complexity, they solved several different problems using different networks and different solvers, but all the ones they used had the blackbox as the last part of the network. They did state that you could optionally have more neural layers after the solver but they didn't do it themselves. They didn't consider the possiblity of using multiple solvers or bypassing the solver with residual weights or anything similar.
- For learning parameters of the solver, I envision two possible routes. The first is that I can try to learn them directly inside the solver, but that may require splitting the blackbox and computing gradients in intermediate steps or something similar. A simpler method would be to add extra inputs to the solver which each correspond to one of the learnable parameters. Then the first part of the network not only learns the format required by the solver, but also learns appropriate solver parameters. Those parameters _could_ be further restricted so that each individual problem would not have different parameters but that the overall problem would have a certain set of parameters. It could be interesting to contrast that difference and see if there's a tradeoff in cost as well.

#### What problems or concerns do we have?
I don't fully understand how they're actually calculating the gradients across the solver. Or rather, I understand the process but not why it works or what parts of their framing (such as the linear objectives) are necessary to make it work.

#### What do we plan to accomplish do over the next week?
To get a better idea of how things work, I plan to look at some of the more recent and relevant referenced works, particularly involving differentiation by argmin and loss-augmented inference. I also plan to start looking at the code to understand the challenges of modifying it or introducing new solvers.

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
