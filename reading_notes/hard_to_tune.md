## Why are machine learning algorithms hard to tune?

*(source: https://www.engraved.blog/why-machine-learning-algorithms-are-hard-to-tune)*



 linear combinations of losses



lack of multi-objective treatment leads to difficulties in tuning the hyper-parameters for these machine learning algorithms



While there are single-objective problems, it is common for these objectives to be given additional regularisation.



the regularisers, weight decay and lasso

-  when you add these regularisations, you effectively have  created a multi-objective loss for your problem.
-  both the original loss $L_{0}$ and the regulariser loss should kept low.
- tune the balance between the two using a $\lambda$ parameter.

e.g.:

$L(θ)=L0(θ)+λ∑∣θ∣$



As a consequence, losses found in e.g. VAE’s are effectively  **multi-objective**

1)  maximally cover the data
2) stay close to the prior distribution

In this  case, occasionally **KL annealing** is used to introduce a tunable parameter $\beta$ to help handle the multi-objectiveness of this loss.

$L(θ)=E_{q_{ϕ}(z∣x)}[log\textrm{ }p_{θ}(x∣z)] −βD_{KL}(q_{ϕ}(z∣x)∥p(z)) $



multi-objectiveness also reinforcement learning

GAN-loss is a sum between the discriminator and the generator loss:



-> All of these losses try  to **optimise for multiple objectives simultaneously**, and argue that the  optimum is found in balancing these often contradicting forces. 

- In some  cases, the sum is more ad hoc and a hyper-parameter is introduced to  weigh the parts against each other.
- In some cases, there are clear  theoretical foundations on why the losses are combined this way, and no  hyper-parameter is used for tuning the balance between the parts. 



In this post, we hope to show you **this approach of combining losses may  sound appealing, but that this linear combination is actually precarious and treacherous**. 



Example:

- unhappy about the tradeoff  between the two losses
- introduce a scaling coefficient $α$ on the second loss 
- hope:  when tuning this $α$, we can choose the trade-off between the two losses and select the point we are most happy
- however, that is not what is always happening: sometimes no good trade-off between the two losses
- most of the time, a point where  the two losses were more balanced is a more preferred solution



### **Why does this method sometimes work, and why does it sometimes fail to give you a tunable parameter?** 

Outputs depend ons the model: the model parameters $θ$ have different effects on the output

Pareto front for a given optimization = the set of all solutions  achievable by a model, which are not dominated by any other solution

-  = the set of achievable losses, where there is no  point where *all* of the losses are better
- no matter trade-off is chosen between the two losses, the preferred solution  always lies on the Pareto front
- by tuning the hyper-parameter of the loss, we usually hope to merely find a different point on that same  front.

**When the Pareto front is convex, we can  achieve all possible trade-offs by tuning our $α$-parameter**. However, when the Pareto front is concave, that approach does not seem to work well anymore.

### Why does gradient descent optimisation fail for concave Pareto fronts?

Total loss in 3D: plane of total loss in relation to each of the losses.

The point where the optimisation  process halts is the result of the optimisation process (will alway send up in the optimum, no matter the wobble).

- by tuning $α$, this space stays a plane, only the tilt of this plane is changed
- **convex** case: any solution on the Pareto curve can be achieved by tuning $\alpha$
  - for all  values of $α$ every starting point of the optimisation  process will converge on the same solution
- **concave** case: go always downwards, but sometimes can roll more to the left, sometimes more to the right
  - by tuning $α$, the plane is tilting in exactly the same way as in the convex case, but because of the shape of the Pareto front, only two points on that front will ever be reached, namely the points on the ends of the concave  curve
  - the point that we want to reach cannot be found with gradient descent based methods because it is a **saddle  point**.
  - when we tune $α$ we tune how many of the starting points end up in  one solution versus the other, but we cannot tune to find other  solutions on the Pareto front

### What kinds of problems do these linear combinations cause?

Problems with using this linear combination of losses approach:

- even without a hyper-parameter that weighs off between the losses, **gradient descent will not try to balance between counteracting forces**

  - depending on the solutions which are achievable by your model, it may  very well completely ignore one of the losses to focus on the other or  vice versa, depending on where you initialised the model

- when a hyper-parameter is introduced, **this hyper-parameter is tuned on a try-and-see basis**

  - wasteful and laborious approach, usually involving multiple iterations of running gradient  descent

- **the hyper-parameter cannot tune for all optima**

- for practical applications, **it is always unknown whether the Pareto front is convex and therefore whether these loss weights are tunable**. 

  - whether they are good hyper-parameters depends on how the model is  parameterised, and how this affects the Pareto curve.
  - it is  impossible to visualise or analyse the Pareto curve for any practical  application 

- if using linear weights to find trade-offs, need an explicit proof that the  whole Pareto curve is convex **for the specific model in use**

  - using a loss which is convex with respect to the output of your  model is not enough to avoid the problem

  - if the parametrisation space is large, which is always the case if the optimisation involves  weights inside a neural network, one can forget about attempting such a  proof

  - showing convexity of the Pareto  curve for these losses based on some intermediate latents is not  sufficient to show that a parameter is tunable

  - the convexity really  needs to depend on the parameter space, and what the Pareto front of the achievable solutions looks like.

    

Note that  **in most applications, Pareto fronts are not either convex or concave,  but they are a mix of both**. 

- so not only do we have a hyper-parameter $α$ which cannot find all solutions, **depending on the initialisation it  might find a different convex part** of the Pareto curve. 
- this parameter and the initialisation mix with each other in confusing ways: if you tune your parameter slightly  hoping to move the optimum slightly, you can suddenly jump to a  different convex part of the Pareto front, even when keeping the  initialisation the same.

