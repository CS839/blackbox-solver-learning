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
## Week of April 19

#### On a scale of 1-10, how do we rate our progress over the past week?
6: I didn't do what I intended but I made overall progress

#### What did we accomplish from last week's tasks?
- I mostly focused on preparing my presentation
- I got some statistics about the difference between A* and Dijkstra and found that there is at least an interesting optimization difference between the two in terms of speed on different problems
- I made some progress on implementing a multiple-blackbox approach

#### What problems or concerns do we have?
I had forgotten that I needed to prepare for the early presentaiton, so I hadn't planned out my time well in advance.

#### What do we plan to accomplish do over the next week?
Finish the multi-blackbox approach and show that it works.

---
## Week of April 12

#### On a scale of 1-10, how do we rate our progress over the past week?
8: I didn't update as frequently as I should have but I also got further than I planned.

#### What did we accomplish from last week's tasks?
- I got the code completely working and figured out how to configure it
- I figured out where they were doing the backward pass through the blackbox
- I implemented an A* blackbox algorithm as a drop-in replacement for their Dijkstra's algorithm implementation
    - The network was still able to learn with this, so my version was valid

#### What problems or concerns do we have?
None

#### What do we plan to accomplish do over the next week?
I want to test my A* algorithm vs Dijkstra's. It should give the same paths (ie. the optimal one) for any input, and thus, in theory, should cause the same updates. I'm planning to try implementing a different version which attempts to have learnable hyperparameters. Specifically, I'd like to be able to learn the best neighbor function and the best heuristic cost function.

---
## Week of Mar 30

#### On a scale of 1-10, how do we rate our progress over the past week?
5: Getting the code was significantly more challenging than expected and due to the lack of a CUDA-enabled system, the actual runtime is significant so it's quite challenging to actually test things

#### What did we accomplish from last week's tasks?
- I spent most of my time adjusting things and testing to actually get the code to run the first place
- I haven't figured out anything additional about how they implement their backward pass due to all my time spent on running the code

#### What problems or concerns do we have?
None

#### What do we plan to accomplish do over the next week?
Now that I can run the code, even if it's slow, I should be able to test and figure out exactly how the learning is being done. Since there seems to be no explicit mention of their backward pass it may prove simpler than expected to add additional blackbox solvers.

---
## Week of Mar 23

#### On a scale of 1-10, how do we rate our progress over the past week?
2: I did what I said I was going to do, but it took an extra week (+ Spring Break) and didn't update for the previous week

#### What did we accomplish from last week's tasks?
- Started catching up after Spring Break and Coronavirus stuff put me a while behind...
- I haven't found a detailed enough description of "differentiation over argmin" to fully understand it, but I've gotten enough to understand how they use it in the paper. Looking at other papers, there are ones that do an argmin optimization of non-linear functions, so that implies that it should be possible to use blackbox solvers with non-linear objectives.
- I found that loss-augmented inference was also called loss-adjusted inference and is used as a technique to allow a perceptron-like algorithm to be influenced by a loss function. Since the standard perceptron update just uses a linear difference, loss-augmented inference is used to update based on a loss function which can be based on the task the perceptron is to be used for. Specifically, this is useful if there is no perfect w* which achieves 0 loss, so the loss function helps to optimize in a specific way.
- I looked at the code for a while (they only provided code for their Dijkstra's algorithm solver), and haven't found any reference to their formulation of the Backward Pass in Algorithm 1 in the paper. Specifically, the Backward Pass should require calling the Solver (in this case, Dijkstra's Shortest Path Algorithm) to find the effective gradients. However, I haven't found any place in the code that explicitly calls the Solver outside of the single forward pass use. While the loss in the forward pass is indeed based on the output of Dijkstra's algorithm, I haven't found a direct connection between the weights and the loss that would consistute using the loss directly to calculate the gradient. I'll continue investigating and actually testing the code next week to see if there's hidden functionality or if their code is in fact doing something different here.

#### What problems or concerns do we have?
Just that it's been hard to adjust to online classes, but I hope to do better moving forward.

#### What do we plan to accomplish do over the next week?
Investigate the code more to figure out where their backward pass rule is coming from. If I can understand thta, I should be able to figure out how they fit in other blackbox solvers and oculd potentially add my own or adjust them.

---
## Week of Mar 1

#### On a scale of 1-10, how do we rate our progress over the past week?
3: I only looked at the papers a little bit, and didn't end up looking at the code

#### What did we accomplish from last week's tasks?
Because I was very busy, I only looked at the papers a little bit, but not really enough to gain the understanding I needed.

#### What problems or concerns do we have?
None.

#### What do we plan to accomplish do over the next week?
Basically the same thing I was planning for this week, but hopefully I'll have more time to work on it.
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
