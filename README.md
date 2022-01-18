# The exact solution of LandS
This page is devoted to provide a proof on the exact solution of the classical stochastic optimization problem LandS  (Louveaux & Smeer, 1988) on its most common version presented by [Linderoth et al (2006)](https://dx.doi.org/10.1007/s10479-006-6169-8) and available on [this link](http://pages.cs.wisc.edu/~swright/stochastic/sampling/). I also write this page as a way to encourage other researchers and practicioners to try the (Generalized) Adaptive Partition Method on their stochastic problems.

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
where *S* is a set of scenarios. In [Linderoth et al (2006)](https://dx.doi.org/10.1007/s10479-006-6169-8), authors propose to solve this problem where for each plant *j=1..3* the demand *d<sub>j</sub><sup>s</sup>* has 100 equiprobable scenarios with values 0.04(*i*-1) for *i*=1...100. So the total number of scenarios is 10<sup>6. 

## Which is the optimal solution?
  The optimal solution is, as expected, **x<sup>*</sup>=(0.84, 3.40, 1.88, 5.88)** and the exact optimal value is **205.XXXX**

## How can I validate that this is true?
Well, using some aggregations and an exact optimization software like QSopt (https://github.com/jonls/qsopt-ex).
  
In this file, we present a partition of the 10<sup>6</sup> scenarios into 1041 subsets. For each subset *P*, we present the corresponding average demand *d<sub>j</sub><sup>P</sup>* and its probability (*|P|/10<sup>6</sup>*). If you solve the problem for these aggregated scenarios, you get a lower bound of the optimal solution (because this is equivalent to aggregate the last constraints of the problem for each subset *P*).  On the other hand, if you fix the solution *x*=(0.84, 3.40, 1.88, 5.88), then you can solve the remaining problem (more simply, solve each subproblem *y<sup>s</sup>* separately) and get an upper bound of the optimal solution. You should get the same bounds, showing the optimality of x<sup>*</sup>! **Very important: You need to do this using an exact solver, like QSopt,** which use rational numbers to avoid rounding errors. 
  
You can find here a Jupyter notebook to validate this result. It requires the Python interface of QSopt from https://github.com/jonls/python-qsoptex.
  
## How did you get this partition and this idea?
Using the marvelous **Adaptive Partition Method** ([Song & Luedtke, 2015](https://doi.org/10.1137/140967337), [Ramirez-Pico & Moreno, 2022](https://doi.org/10.1007/s10107-020-01609-8)) for two-stage stochastic linear problems. Starting will all scenarios aggregated into a single one, you solve the problem and get a candidate solution *x<sup>(t)</sup>*. Then, you solve the subproblems and refine the partition by grouping scenarios with the same optimal duals, and iterate.  On the LandS problem, it only requires 8 iterations!.

You can find here a Jupyter notebook that solve this problem using the Adaptive Partition Method. It requires Gurobi (https://www.gurobi.com) as solver. 
  
## Who are you?
I'm Eduardo Moreno, professor at Universidad Adolfo Ibáñez, Santiago, Chile. Please visit [my homepage](https://emoreno.uai.cl) and feel you free to contact me if you have any question or comment.
  

### Bibliography
  
- Linderoth, J., Shapiro, A., & Wright, S. (2006). The empirical behavior of sampling methods for stochastic programming. Annals of Operations Research, 142(1), 215-241. DOI:10.1007/s10479-006-6169-8
- Louveaux, F.V. & Smeers, Y. (1988). Optimal investments for electricity generation: A stochastic model and a test problem. In Y. Ermoliev and R. J.-B. Wets, editors, Numerical techniques for stochastic optimization problems, pages 445–452. Springer-Verlag.
- Ramirez-Pico, C., & Moreno, E. (2021). Generalized adaptive partition-based method for two-stage stochastic linear programs with fixed recourse. Mathematical Programming, to apper. DOI:10.1007/s10107-020-01609-8
- Song, Y., & Luedtke, J. (2015). An adaptive partition-based approach for solving two-stage stochastic programs with fixed recourse. SIAM Journal on Optimization, 25(3), 1344-1367. DOI:10.1137/140967337
