# The exact solution of LandS
This page is devoted to provide a proof on the exact solution of the classical stochastic optimization problem LandS  (Louveaux & Smeer, 1988) on its most common version presented by Linderoth et al (2006) and available on [this link](http://pages.cs.wisc.edu/~swright/stochastic/sampling/). I also write this page as a way to encourage other researchers to try the Adaptive Partition Method on their stochastic problems.

## What is LandS?
LandS is an energy planning investment problem, where the goal is to decide the capacities of four new plants while minimizing allocation and operational costs. The problem can be formulated as its deterministic equivalent formulation:

![](master_problem.svg)
<!---
[MP] := \min_{x,y \geq 0}   \sum_{i=1}^4  c_i x_i &+ \sum_{s\in S} p_s \sum_{i=1}^3 \sum_{j=1}^3f_{ij}y_{ij}^s\\
\sum_{i=1}^4  x_i &\geq m\\
\sum_{i=1}^4 c_ix_i &\leq b\\
\sum_{j=1}^3 y_{ij}^s &\leq x_i ~\forall~ i\in 1\ldots 4, \forall s\in S\\
\sum_{i=1}^4 y_{ij}^s &\geq d_j^s  ~\forall~ j\in 1\ldots 3,  \forall s\in S
-->
where *S* is a set of scenarios. In Linderoth et al (2006), they propose to solve this problem where for each plant *j* the demand *d<sub>j</sub><sup>s</sup>* has 100 equiprobable values 0.04*(i-1) for i=1...100. So the total number of scenarios is 10<sup>6. 

## Which is the optimal solution?
  The optimal solution is, as expected, **x<sup>*</sup>=(0.84, 3.40, 1.88, 5.88)** and the exact optimal value is **205.XXXX**

## How can I validate that this is true?
Well, using some aggregations and an exact optimization software like QSopt (https://github.com/jonls/qsopt-ex).
  
In this file, we present a partition of the 10<sup>6</sup> scenarios into 1041 subsets. For each subset *P*, we present the corresponding average demand *d<sub>j</sub><sup>P</sup>* and its probability (*|P|/10<sup>6</sup>*). If you solve the problem for these aggregated scenarios, you get a lower bound of the optimal solution (because this is equivalent to aggregate constraints (4) and (5) for each subset *P*).  On the other hand, if you fix the solution x=(0.84, 3.40, 1.88, 5.88), you can solve the problem (more simply, solve each subproblem *y<sup>s</sup>* separately) and get an upper bound of the optimal solution. If you do this using an exact solver like QSopt, you will get the same bounds, showing the optimality of x<sup>*</sup>.
  
You can find here a Jupyter notebook to validate this result. It requires the Python interface of QSopt from https://github.com/jonls/python-qsoptex.
  
## How did you get this partition and this idea?
Using the marvelous **Adaptive Partition Method** (Song & Luedtke, 20XX, [Ramirez-Pico & Moreno, 2022](https://doi.org/10.1007/s10107-020-01609-8)) for two-stage stochastic problems. Starting will all scenarios aggregated into a single one, you solve the problem and get a candidate solution *x<sup>(t)</sup>*. Then, you solve the subproblems and refine the partition by grouping scenarios with the same optimal duals, and iterate.  On the LandS problem, it only requires 8 iterations!.

You can find here a Jupyter notebook that solve this problem using the Adaptive Partition Method. It requires Gurobi (https://www.gurobi.com) as solver. 
