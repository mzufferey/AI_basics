## How can we make machine learning algorithms tunable ?

*(source: https://www.engraved.blog/how-we-can-make-machine-learning-algorithms-tunable)*



Desiderata to have tunable hyper-parameters: 

- **the hyper-parameter has semantic meaning**
  - this would allow finding the preferred solution in one go, without  having to iterate multiple times over various parameters to narrow down  the area of interest
- by tuning the hyper-parameter, we should **be able to find any solution on the Pareto front**
  - every solution on the Pareto front should have a value  for the hyper-parameter for which the optimisation algorithm finds that  solution

In order to achieve this, we **reframe our  optimisation problem as a Lagrangian optimisation problem** instead. 

- choose one of the losses as the primary loss and put a constraint on the other loss
- goal: minimise the primary loss subject to the  second loss being less than a value $ε$. In symbols:

$Minimize  L_{0}(θ)  subject  L_{1}(θ)≤ϵ$

The Lagrangian looks the same as the original total loss linearly combined

$L(θ,λ)=L_{0}(θ)−λ(ϵ−L_{1}(θ))$



Where and whether we converge on this constrained problem has been mathematically formalised in the Karush–Kuhn–Tucker (KKT)  conditions

- from those conditions, we know that the optimum we are  looking for, will be a saddle point on this Lagrangian with a linear  weighted combination of losses
- we need to combine that insight with the inability of gradient descent to find saddle points to notice  that there might be an issue with problems that have concave Pareto  fronts.



### The Solve-The-Dual method

Typical method from Lagrangian optimisation: 

- a dual is constructed and solved to find the ideal $λ$
- optimise the Lagrangian with gradient descent
- plugging in this value for $λ$, and optimising the Lagrangian with gradient descent will solve the constrained optimisation proble
  - it seems $ε$ can be used to tune the optimum: we can nail the trade-off we want, without running our optimisation processes multiple times to tune this $ε$ hyper-paramet
  - but, there is an issue: we  not seem to converge on the Pareto front, but some of the solutions  found with gradient descent are downright ignoring our hard constraint ! (cf. [other blog post](hard_to_tune.md))
    - even though we solved a dual to find the optimal $λ$ parameter that matches our $ε$, **we are still making a linear trade-off between our losses**
    - this does not play well with gradient descent optimisation.
    -  this approach does not work in the general case
      - when the Pareto front is concave, constraints can be ignored and you still cannot always find all good solutions by  tuning your hyper-parameter
      - but in general, you do not know the shape  of your Pareto front, so you do not know which case you are in





### The Hard Constraint First Method

An alternative method, which does work, is to **first optimise for the constraint with gradient descent, and to optimise for  the primary loss only as long as this constraint is satisfied**.  

- with this approach, the constraint will always be satisfied upon  convergence, and the other loss minimised
- works for both  the concave and the convex case

Major downside:

-  **we do not really treat the trade-off between the losses** during the gradient descent
  - only one of  the losses is being considered at every step
  - in general, this makes  this approach less desirable for many applications
  - example: a common failure case is when your constraint has an obvious solution,  which however brings it to a part of the space where the primary loss  has barely any gradient to follow. 
  - convergence can be another issue,  when every time you solve the constraint, you undo the progress you have made on the primary loss.

- **this method does not work that well when you want to use stochastic gradient descent rather than gradient descent**.
  - since the constraint is defined on the average loss across all data,  you do not want to enforce the hard constraint on a sample of your data  where it is not satisfied yet, as long as it is satisfied in the general case
  - this issue is hard to overcome.

However, this method is easy to implement and might actually work sufficiently for your particular problem.

### Basic Differential Multiplier Method

Could we not **use a single gradient descent to find both the  optimal parameters and Lagrangian multiplier** simultaneously?

Example of an algorithm:

- follow the gradient of the Lagrangian **downwards for the parameters, but upwards for $λ$**
  - gradient descent for the parameters, but gradient ascent for the Lagrangian multipliers
-  as long as we take care that $λ$ is not becoming negative (constraint formulated as an inequality), we should be golden
- we really want $λ$ to become exactly zero when the constraint is satisfied! 
  - parametrising it  as a softplus or even an exponential to keep it positive, is a bad idea, even though you might find it in a number of publications.

Upsides of this approach

- works nicely on the convex case
- at every gradient step, both of the losses are considered
- applicable to stochastic gradient descent as well, at the cost of a little additional intricacy of understanding how Lagrangians work
- it is still possible to use a single gradient  evaluation for both the gradient descent and the gradient ascent (-> computational complexity broadly stays the same)

Downside:

- it does not solve our original problem in the general case,
  - on the concave Pareto front case, the performance is fair, but it does not converge to a solution
  - this optimisation method keeps oscillating on the Pareto front, unable to  settle on a good solution
    - can give the  appearance of finding better solutions, but it still is not tunable
      - what is done: cherry-pick to select the most suitable solution for a given problem; but hard to use when  manual intervention is not possible (e.g. in for  instance reinforcement learning where optimization is just one piece of the puzzle).

## Modified Differential Method of Multipliers

Easier to understand after we identify on an intuitive level  **what is causing the oscillations**

If we follow the behaviour of the parameters as it oscillates  around the optimum

- as long as the constraint is violated, $λ$ keeps increasing
- when we are suddenly satisfying the constraint again, $λ$ is still large
  - it will take a number of steps before the gradient descent will push $λ$ back to 0.
- as long as $λ$ is positive, the solution is pushed further away from the constraint
- eventually, $λ$ becomes 0, the constraint is ignored and the optimisation process  continues 
- but when the solution accidentally hits the constraint  again, the whole cycle repeats

Intuitively, you can think of the  Lagrangian multiplier $λ$ as the potential energy of an oscillating system.

Damping on  this energy with the **Modified Differential Method of Multipliers**

- **prevent the system from oscillating** eternally, and make it converge instead.



Upside:

- works nicely for both convex and concave Pareto fronts

Downside:

- add a damping  hyper-parameter
  - trades the time to find the  Pareto front with the time to converge to a solution on that front
  - does not alter which solution is found, only how fast it is found

Despite the hyperparameter we have **a tunable algorithm that works for stochastic gradient descent** ! 

**We can use this Modified Differential Method of Multipliers to tune the balance between the losses in a semantically useful way using  stochastic gradient descent, *no matter the shape of the invisible Pareto front*.**

**Wherever you see a linear  combination of losses being optimised with gradient descent, this more  principled approach could be used.**  In those cases the constrained  reformulation of that problem is going to **yield more general and tunable algorithm**.

