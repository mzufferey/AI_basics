

## Augmented Lagrangian methods

- a certain class of algorithms for solving constrained optimization problems
-  originally known as the **method of multipliers**, and was studied much in the 1970 and 1980s as a good alternative to penalty methods
- similar to penalty methods: they replace a constrained optimization problem by a series of unconstrained problems and add a penalty term to the objective
- different to penalty methods: they add yet another term, designed to mimic a Lagrange multiplier

([Wikipedia](https://en.wikipedia.org/wiki/Augmented_Lagrangian_method))

## Basic Differential Multiplier Method (BDMM)

- gradient descent (GD) does not work  with  Lagrange multipliers: to converge, GD needs a stationary point to be local minimum 
  - but with LM, the critical point of the  energy in Lagrangian function needs not be an  attractor   
  - energies  involving  Lagrange  multipliers have  critical  points  which  tend  to  be  saddle points
- BDMM for constrained optimization: algorithm proposed by [Platt and Barr 1987](https://papers.nips.cc/paper/1987/file/a87ff679a2f3e71d9181a67b7542122c-Paper.pdf)
  - as an *alternative to  differential  gradient descent that estimates the  Lagrange multipliers, so that the constrained minima are attractors of the differential  equations,  instead of "repulsors"*
- modified BDMM: algorithm proposed by [Platt and Barr 1987](https://papers.nips.cc/paper/1987/file/a87ff679a2f3e71d9181a67b7542122c-Paper.pdf)
  - modification of the BDMM with  more robust convergence properties
  - alter the BDMM to have  a region of positive damping surrounding the constrained minima; adds a penalty force which does not change the position of the stationary points
- [example from TDS](https://www.engraved.blog/how-we-can-make-machine-learning-algorithms-tunable/):
  - objective: want to minimize a loss, subject to a given constraint: $Minimize\textrm{ }L_{0}(θ)\textrm{}subject\textrm{ }L_{1}(θ)≤ϵ$
  - can combine the two in a Lagrangian way: $L(θ,λ)=L_{0}(θ)−λ(ϵ−L_{1}(θ))$
  - the optimum will be a saddle point on this Lagrangian with a linear weighted combination of losses
  - GD does not work with saddle points ! Alternative methods
    - <u>solve-the-dual method</u>: a dual is constructed and solved to find the ideal $λ$, then optimize the Lagrangian with GD (it adds a tunable hyper-parameter $\epsilon$)
      - *issue*: we are still doing a linear trade-off between the losses, what does not play well with GD as it does not work with concave Pareto front (saddle point !), and you don't know in advance the shape of the Pareto front (see full explanation about the problems of linear combinations of losses [here](https://www.engraved.blog/why-machine-learning-algorithms-are-hard-to-tune/))
    - <u>hard constraint first method</u>: first optimize for the constraint with gradient descent, and to optimize for the primary loss only as long as this constraint is satisfied
      - *issue*: it works but you do not really treat the trade-off between the losses during your gradient descent (only one of the losses is considered at every step); also does not work well with stochastic GD
    - <u>basic differential multiplier method</u> (BDMM): works well for convex case
      - initial issue with the idea of using a Lagrangian to solve the constrained version: interaction between a fixed $λ$ found by solving the dual function and using gradient descent to minimize the other parameters
        - solution: **use a single gradient descent to find both the optimal parameters and Lagrangian multiplier simultaneously**
          - follow the gradient of the Lagrangian **downwards for the parameters** (gradient descent), but **upwards for the Lagrangian multipliers $λ$**  (gradient ascent)
            - should only take care that $λ$ is not becoming negative (since the constraint is formulated as an inequality)
            - we really want λ to become exactly zero when the constraint is satisfied $\rightarrow$ parametrising it as a softplus or even an exponential to keep it positive, is a bad idea !
      - *upsides*: works well on convex case; both losses are considered at every gradient step $\rightarrow$ applicable with stochastic GD as well
      - *downside*: on concave Pareto case, it does not converge and keeps oscillating (bias of cherry-picking when to stop the optimization process !)
    - <u>modified BDMM</u> (BDMM for constrained optimization algorithm from Platt and Barr 1987)
      - Lagrangian multiplier λ as the potential energy of an oscillating system; idea: introduce damping on this energy to prevent the system from oscillating eternally and make it converge
      - *upside*: works well on convex and concave Pareto front ! $\rightarrow$ **a tunable algorithm that works for stochastic GD**
      - *downside*: introduces an additional damping hyper-parameter, which:
        - trades the time to find the Pareto front with the time to converge to a solution on that front
        - but does not alter which solution is found, only how fast it is found

## Calculus of variation

- problem to ﬁnd a value of $x$ that maximizes (or minimizes) a function $y(x)$
- similarly, in the calculus of variations we seek a function $y(x)$ that maximizes (or minimizes) a functional $F [y]$
  - of all possible functions $y(x)$, we wish to ﬁnd the particular function for which the functional $F [y]$ is a maximum (or minimum)
- example: calculus of variations can be used to show that the shortest path between two points is a straight line or that
  the maximum entropy distribution is a Gaussian

(Bishop, p. 703)



## Duality

The duality principle provides that optimization problems may be viewed from either of two perspectives, the primal problem or the dual problem.

- the solution to the dual problem provides a lower bound to the solution of the primal (minimization) problem

 However the optimal values of the primal and dual problems need not be equal. 

- their difference is called the duality gap. 

For convex optimization problems, the duality gap is zero under a constraint qualification condition

- $\rightarrow$ given any linear program, there is another related linear program called the dual

([source](https://online-journals.org/index.php/i-jes/article/view/8224))

The **Lagrangian dual problem**

- obtained by forming the Lagrangian of a minimization problem by using nonnegative Lagrange multipliers to add the constraints to the objective function, and then solving for the primal variable values that minimize the original objective function
- this solution gives the primal variables as functions of the Lagrange multipliers, which are called dual variables, so that the new problem is to maximize the objective function with respect to the dual variables under the derived constraints on the dual variables (including at least the nonnegativity constraints)

([Wikipedia](https://en.wikipedia.org/wiki/Duality_(optimization)))



## Functional

- think of a function $y(x)$ as being an operator that, for any input value $x$, returns an output value $y$
- we can deﬁne a functional $F [y]$ to be an operator that takes a function $y(x)$ and returns an output value $F$
  - example: the length of a curve drawn in a two-dimensional plane in which the path of the curve is deﬁned in terms of a function
- in the context of machine learning, a widely used functional is the entropy $H[x]$ for a continuous variable $x$ because, for any choice of probability density function $p(x)$, it returns a scalar value representing the entropy of $x$ under that density
  - the entropy of $p(x)$ could equally well have been written as $H[p]$

(Bishop, p. 703)

## Lagrange multipliers

- aka. undetermined multipliers
- used to ﬁnd the stationary points of a function of several variables subject to one or more constraints

see note on [Lagrange multipliers](lagrange_multiplier.md)

(Bishop, p. 706)

## Pareto front

each point on the front represents a different trade-off between possibly conflicting objectives

- in the context of multi-objective optimization (= problems that have a set of optimal solutions; MOO)
- "MOO problems have a set of optimal solutions, the Pareto front, each reflecting a different trade-off
  between objectives. Points on the Pareto front can be viewed as an intersection of the front with a
  specific direction in loss space"

([Navon et al. 2021](https://arxiv.org/pdf/2010.04104.pdf))

## Saddle point

point on the surface of the graph of a function where the slopes (derivatives) in orthogonal directions are all zero (a critical point), but which is not a local extremum of the function

- example: when there is a critical point with a relative minimum along one axial direction (between peaks) and at a relative maximum along the crossing axis
- the name derives from the fact that the prototypical example in two dimensions is a surface that curves up in one direction, and curves down in a different direction

([Wikipedia](https://en.wikipedia.org/wiki/Saddle_point))

## Stationary point

in calculus, point on the graph of the function where the function's derivative is zero

- point where the function "stops" increasing or decreasing (hence the name)

([Wikipedia](https://en.wikipedia.org/wiki/Stationary_point))

