

## Augmented Lagrangian methods

- proble

([Wikipedia](https://en.wikipedia.org/wiki/Augmented_Lagrangian_method))

## Calculus of variation

- problem to ﬁnd a value of $x$ that maximizes (or minimizes) a function $y(x)$
- similarly, in the calculus of variations we seek a function $y(x)$ that maximizes (or minimizes) a functional $F [y]$
  - of all possible functions $y(x)$, we wish to ﬁnd the particular function for which the functional $F [y]$ is a maximum (or minimum)
- example: calculus of variations can be used to show that the shortest path between two points is a straight line or that
  the maximum entropy distribution is a Gaussian

(Bishop, p. 703)

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

