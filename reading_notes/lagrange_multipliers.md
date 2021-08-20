## Lagrange multipliers

*(Bishop, p. 705 and following)*

for **optimizing a functional with respect to a probability distribution**, need to **maintain the normalization constraint on the probabilities**

- often most conveniently done using a **Lagrange multiplier**, which then **allows an unconstrained optimization** to be performed

sometimes called undetermined multipliers

used to ﬁnd the stationary points of a function of several variables subject to one or more constraints

problem of ﬁnding the maximum of a function $f (x1 , x2 )$ subject to a constraint relating $x 1$ and $x 2$ , written $g(x 1 , x 2 ) = 0$.

first approach:

- solve the constraint equation and thus express $x2$ as a function of $x1$ in the form $x2 = h(x1)$ 
- then be substituted into $f (x1 , x 2 )$ to give a function of $x 1$ alone of the form $f (x 1 , h(x 1 ))$
- maximum wrt $x 1$  then be found by differentiation in the usual way, to give the stationary value $x  1^*$ and the corresponding value $x 2$ given by $x2^* = h(x1^*  )$.

But :

- may be difﬁcult to ﬁnd an analytic solution of the constraint equation that allows $x 2$ to be expressed as an explicit function of $x 1$
- this approach treats $x 1$ and $x 2$ differently and so spoils the natural symmetry between these variables

A more elegant, and often simpler, approach is based on the **introduction of a parameter λ called a Lagrange multiplier**

Geometrical intuition:

- consider a $D$-dimensional variable $x$ with components $x 1 , . . . , x _{D}$ 
- the constraint equation $g(x) = 0$ then represents a $(D−1)$-dimensional surface in $x$-space
- at any point on the constraint surface the gradient ∇$g(x)$ of the constraint function will be orthogonal to the surface. 
  - consider a point $x$ that lies on the constraint surface, and consider a nearby point $x + \epsilon$ that also lies on the surface
  - Taylor expansion around $x$: $g(x + \epsilon) \simeq g(x) + \epsilon^{T}∇g(x)$
  - because both $x$ and $x + \epsilon$ lie on the constraint surface, we have $g(x) = g(x + \epsilon)$ and hence $\epsilon^{T}∇g(x) \simeq 0$. In the limit $||\epsilon||$ → 0 we have   T $\epsilon^{T}∇g(x) = 0$, and because $\epsilon$ is then parallel to the constraint surface $g(x) = 0$, we see that the vector ∇g is normal to the surface.

- next seek a point $x^*$ on the constraint surface such that $f (x)$ is maximized

  - such a point must have the property that the vector ∇$f (x)$ is also orthogonal to the constraint surface, because otherwise we could increase
    the value of $f (x)$ by moving a short distance along the constraint surface
  - thus **∇f and ∇g are parallel (or anti-parallel) vectors**, and so there must exist a parameter λ such that
    ∇f + λ∇g = 0 where $λ \neq  0$ is known as a **Lagrange multiplier**
    - $λ$ can have either sign

  

  the **Lagrangian function** is deﬁned by
  $$
  L(x, λ) ≡ f (x) + λg(x)
  $$

- the **constrained stationarity condition** is obtained by setting $∇_{ x} L = 0$

- the condition $∂L/∂λ = 0$ leads to the constraint equation $g(x) = 0$.



To ﬁnd the maximum of a function $ f (x)$ subject to the constraint $g(x) = 0$

- deﬁne the **Lagrangian function**
- then ﬁnd the **stationary point of $L(x, λ)$ with respect to both $x$ and $λ$**
  - for a $D$-dimensional vector x, this gives $ D + 1$ equations that determine both the stationary point $x^* $ and the value of $λ$
  - if we are only interested in $x^*$ , then we can **eliminate $λ$ from the stationarity equations** without needing to ﬁnd its value (hence the term ‘undetermined multiplier’)



Now consider the problem of maximizing $f (x)$ subject to an **inequality constraint** of the form $g(x) \geq 0$

2 kinds of solution possible:

1. the constrained stationary point lies in the region where $g(x) > 0$: the constraint is inactive
   - $g(x)$ plays no role and so the stationary condition is simply $∇f (x) = 0$
   - again corresponds to a stationary point of the Lagrange function but this with $λ = 0$
2. the point lies on the boundary $g(x) = 0$:  the constraint is said to be active
   - analogous to the equality constraint, corresponds to a stationary point of the Lagrange
     function with $λ \ne 0$
   - now, however, the sign of the Lagrange multiplier is crucial, because the function $ f (x) $ will only be at a maximum if its gradient is oriented away from the region $g(x) > 0$
   - therefore we have $∇f (x) = −λ∇g(x)$ for some value of $λ > 0$

For either of these two cases, $λg(x) = 0$ . Thus the solution to the problem of maximizing $f (x)$ subject to $g(x) \ge 0$ is obtained by optimizing the Lagrange function with respect to $x$ and $λ$ subject to the conditions:

1) $g(x) \ge 0$
2) $λ \ge 0$

3. $λg(x) = 0$

These are known as the **Karush-Kuhn-Tucker (KKT) conditions**.

Note that if we wish to minimize (rather than maximize) $f (x)$ subject to  $g(x) \ge 0$, then we **minimize** the Lagrangian function $L(x, λ) = f (x) − λg(x)$ with respect to $x$, again subject to $λ \ge 0$.



Straightforward extension of  the technique of Lagrange multipliers to the case of **multiple equality and inequality constraints**.

Straightforward extension to constrained **functional derivatives**.

