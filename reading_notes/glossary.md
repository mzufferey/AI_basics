## Akaike information criterion (AIC) 

Akaike information criterion (AIC) is an information-based model-selection criterion. 

*  given by the formula −2 ×log likelihood + 2*k*, where *k* is the number of parameters.
* favors simpler models by penalizing for the number of model parameters. 
* does not, however, account for the sample size. 
* as a result, the AIC penalization diminishes as the sample size increases, as does its ability to guard against overparameterization.

https://www.stata.com/manuals/bayesglossary.pdf

## Augmented Lagrangian methods

- a certain class of algorithms for solving constrained optimization problems
-  originally known as the **method of multipliers**, and was studied much in the 1970 and 1980s as a good alternative to penalty methods
- similar to penalty methods: they replace a constrained optimization problem by a series of unconstrained problems and add a penalty term to the objective
- different to penalty methods: they add yet another term, designed to mimic a Lagrange multiplier

([Wikipedia](https://en.wikipedia.org/wiki/Augmented_Lagrangian_method))



## Automatic relevance determination (ARD)

Automatic relevance determination (ARD) and the closely-related sparse Bayesian learning (SBL) framework are effective tools for pruning large numbers
of irrelevant features leading to a sparse explanatory subset.

[... in a generative model ] When large numbers of features are present relative to the signal dimension, the estimation problem is fundamentally
ill-posed. Automatic relevance determination (ARD) addresses this problem by regularizing the solution space using a parameterized, data-dependent prior distribution that effectively prunes away redundant or superfluous features 



[...] an automatic relevance determination (ARD) hyperparameter that determines the relevance of the *k*-th component of the input
vectors [...]  for the prediction of the output. ARD therefore can be viewed as an in-built mechanism for feature selection. 



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
    - <u>modified BDMM</u> (BDMM for constrained optimization algorithm from Platt and Barr 1988)
      - Lagrangian multiplier λ as the potential energy of an oscillating system; idea: introduce damping on this energy to prevent the system from oscillating eternally and make it converge
      - *upside*: works well on convex and concave Pareto front ! $\rightarrow$ **a tunable algorithm that works for stochastic GD**
      - *downside*: introduces an additional damping hyper-parameter, which:
        - trades the time to find the Pareto front with the time to converge to a solution on that front
        - but does not alter which solution is found, only how fast it is found

## Bayesian information criterion (BIC)

The Bayesian information criterion (BIC), also known as Schwarz criterion, is an information based criterion used for model selection in classical statistics.

* given by the formula −0.5 ×log likelihood + *k* × ln*n*, where *k* is the number of parameters and *n* is the sample size.
* favors simpler, in terms of complexity, models and it is more conservative than AIC.

https://www.stata.com/manuals/bayesglossary.pdf

## Calculus of variation

- problem to ﬁnd a value of $x$ that maximizes (or minimizes) a function $y(x)$
- similarly, in the calculus of variations we seek a function $y(x)$ that maximizes (or minimizes) a functional $F [y]$
  - of all possible functions $y(x)$, we wish to ﬁnd the particular function for which the functional $F [y]$ is a maximum (or minimum)
- example: calculus of variations can be used to show that the shortest path between two points is a straight line or that
  the maximum entropy distribution is a Gaussian

(Bishop, p. 703)

## Cardinality

- size of a finite set

([source](https://web.uvic.ca/~gmacgill/LFNotes/Cardinality.pdf))

## Deviance information criterion (DIC)

The deviance information criterion (DIC) is an information based criterion used for Bayesian model selection. 

* analog of AIC 
* given by the formula *D(θ) + 2 ×pD*, where *D(θ)*  is the deviance at the sample mean and *pD* is the effective complexity, a quantity equivalent to the number of parameters in the model. 
* models with smaller DIC are preferred

https://www.stata.com/manuals/bayesglossary.pdf

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



## Genetic programming

In artificial intelligence, genetic programming (GP) is a technique of evolving programs, starting from a population of unfit (usually random) programs, fit for a particular task by applying operations analogous to natural genetic processes to the population of programs. It is essentially a heuristic search technique often described as 'hill climbing', i.e. searching for an optimal or at least suitable program among the space of all programs. 

([Wikipedia](https://en.wikipedia.org/wiki/Genetic_programming))

## Koopman operator theory

Nonlinear fluid flows represent some of the most complex nonlinear systems in the nature and the industry. This complexity is in part due to the nonlinearity of the governing equations and partly because of the high -and possibly infinite- number of dimensions required to model the fluid continuum. As a result, most of the quantitative prediction and analysis done today relies on extensive numerical simulations and experiments. Unfortunately, the classic toolbox of dynamical systems theory, which relies heavily on geometric viewpoint, is not easily applicable to outcome of these numerical and experimental efforts. An alternative approach introduced since the 90's is centered on the data-driven methods such Fourier analysis and Proper Orthogonal Decomposition (POD). These methods, despite having partial success with the problems of fluid mechanics, generally suffer from the disconnect with the underlying mathematical theory and thus fail to provide a reliable framework for analysis of flows with arbitrary complexities.

The Koopman operator theory of dynamical systems provide a novel framework for data-driven analysis of nonlinear flows. In this theory, **the time evolution of observables is connected to the evolution of trajectories in the infinite dimensional state space of the flow, which in turn, enables us to represent the time-variation of each observable**, e.g. velocity at a given point, as a linear expansion of the Koopman operator invariants. This expansion, called **Koopman mode decomposition**, provides us with **a flow representation with two essential components**: 

1. **Koopman modes** which are the **global structures** in the flow field that grow, decay or oscillate in time 
2.  **Koopman eigenvalues** which correspond to the **time scales** of the growth, and oscillations of Koopman modes. 

The lid-driven cavity flow provides a good example for the Koopman mode decomposition: The distribution of Koopman eigenvalues is a clear indicator of the asymptotic dynamics and the Koopman modes show the dominant structures that are present in a wide range of Reynolds numbers from the periodic flow at Re = 10,000 to the chaotic flow at Re = 30,000.

([source](https://mgroup.me.ucsb.edu/koopman-operator-theory-and-fluid-mechanics))



## Lagrange multipliers

- aka. undetermined multipliers
- used to ﬁnd the stationary points of a function of several variables subject to one or more constraints

see note on [Lagrange multipliers](lagrange_multiplier.md)

(Bishop, p. 706)

## Marginal posterior distribution

In Bayesian context, a marginal posterior distribution is a distribution resulting from integrating out all but one parameter from the joint posterior distribution.

https://www.stata.com/manuals/bayesglossary.pdf

## MLE and OLS

The MLE is concerned about choosing the parameter which can maximize the  likelihood or equivalently the log-likelihood function. And then fit the model based on the trial estimated parameter value and calculate the  mean of the model. To find the iterative weighted and working dependence and based on this two and the design matrix we can estimate the best  parameter value.The OLS will take the parameter value which minimize the error ( residuals) of the model. It will take into acount that the residual sum of square  and derivate with respect to the parameter regression coefficient (beta) and set it to zero and then we will find the parameter value which  minimize the error(residual sum of square).



The ordinary least squares, or OLS is a method for approximately determining the unknown parameters located in a linear regression model. This method is obtained by minimizing the total of squared vertical distances between the observed responses within the dataset and the responses predicted by the linear approximation. Through a simple formula, you can express the resulting estimator, especially the single regressor, located on the right-hand side of the linear regression model. Also it is your overall solution in minimizing the sum of the squares of errors in your equation.

The Maximum likelihood Estimation, or MLE, is a method used in estimating the parameters of a statistical model, and for fitting a statistical model to data. Using the maximum likelihood estimation, you can estimate the mean and variance of the height of your subjects. The MLE would set the mean and variance as parameters in determining the specific parametric values in a given model.



> In a linear model, if the errors belong to a normal distribution the least squares estimators are also the maximum likelihood estimators.

As explained above we're actually(more precisely equivalently) using the MLE for predicting *y* values. And if the response variable has arbitrary distributions rather than the normal distribution, like Bernoulli distribution or anyone from the [exponential family](https://en.wikipedia.org/wiki/Exponential_family) we map the linear predictor to the response variable distribution using a [link function](https://en.wikipedia.org/wiki/Generalized_linear_model#Link_function)(according to the response distribution), then the likelihood function becomes the product of all the outcomes(probabilities between 0 and 1) after the  transformation. We can treat the link function in the linear regression  as the identity function(since the response is already a probability).



### Optimization

If there are no constraints on what values the decision variables can  take, we have an **unconstrained optimization problem**. This is a type of  problem encountered in linear regression. It is also called a **functional approximation** **problem** and is widely used in data science

Depending on type of objective function, constraints and decision variables, <u>optimization can be of various types</u>.

1. **Linear programming**: If the decision variable is continuous, the  functional form is linear and all constraints are also linear, it is  called a Linear programming problem.
   * If the objective function and constraints are linear and decision variable is an integer, it is called a **Linear integer programming** problem.
   * A special case might be decision variables can only take binary values (  0,1), then it is called a **Binary integer programming** problem.

2. **Non linear programming**: If the decision variable remains continuous and  either the objective function or constraints are non linear, it is  called a non linear programming problem.
   * If the objective function or constraints are non-linear and decision  variable is an integer, it is called a **Non-linear integer programming**  problem.

**Mixed-integer linear programming** problem: If the decision variable(x) is a mixed  variable and if the objective function(f) is linear and all the  constraints are also linear then this type of problem known as a  mixed-integer linear programming problem. So, in this case, the decision variables are mixed, the objective function is linear and the  constraints are also linear.

**Mixed-integer non-linear programming** problem: If the decision variable(x) remains  mixed; however, if either the objective function(f) or the constraints  are non-linear then this type of problem known as a mixed-integer  non-linear programming problem. So, a programming problem becomes  non-linear if either the objective or the constraints become non-linear.

**nonlinear programming (NLP)** is the process of solving an optimization problem where some of the constraints or the objective function are nonlinear

An **optimization problem** is one of calculation of the extrema (maxima, minima or stationary points) of an objective function over a set of unknown real variables and conditional to the satisfaction of a system of equalities and inequalities, collectively termed constraints. 

If the objective function is concave (maximization problem), or convex (minimization problem) and the constraint set is convex, then the program is called convex and general methods from **convex optimization** can be used in most cases.

If the objective function is quadratic and the constraints are linear, **quadratic programming** techniques are used.

If the objective function is a ratio of a concave and a convex function (in the maximization case) and the constraints are convex, then the problem can be transformed to a convex optimization problem using **fractional programming** techniques. 

By definition, all constraints that are not linear are **nonlinear**. Nonlinear expressions include relationships in which variables are squared, cubed, taken to powers other than one, or multiplied or divided by each other.  Models with nonlinear expressions are much more difficult to solve than linear models. 



## Pareto front

each point on the front represents a different trade-off between possibly conflicting objectives

- in the context of multi-objective optimization (= problems that have a set of optimal solutions; MOO)
- "MOO problems have a set of optimal solutions, the Pareto front, each reflecting a different trade-off
  between objectives. Points on the Pareto front can be viewed as an intersection of the front with a
  specific direction in loss space"

([Navon et al. 2021](https://arxiv.org/pdf/2010.04104.pdf))



## Precision

in Bayesian statistics, the precision is the inverse of the variance (aka the reciprocal of the variance)

$ϕ=1/σ^2 $

less precision = more uncertainty



## Predictive inference

In Bayesian statistics, predictive inference is inference about unobserved (future) data conditionally on past data and prior knowledge of model parameters. Predictive inference is based on prior predictive or posterior predictive distribution of model parameters.

https://www.stata.com/manuals/bayesglossary.pdf



## Radial basis function

A radial basis function (RBF) is a real-valued function $φ$ whose value depends only on the distance between the input and some fixed point, either the origin, so that $φ ( x ) = \hat{φ}( ‖ x ‖ ) $or some other fixed point $c$ called a center, so that $φ ( x ) = \hat{φ }(‖ x − c ‖ )$. Any function $φ$ that satisfies the property $φ ( x ) = \hat{φ} ( ‖ x ‖ )$is a radial function. The distance is usually Euclidean distance, although other metrics are sometimes used. They are often used as a collection $\{ φ_k \}_k$ which forms a basis for some function space of interest, hence the name. 

([Wikipedia](https://en.wikipedia.org/wiki/Radial_basis_function))

## Relaxation

In mathematical optimization and related fields, relaxation is a **modeling strategy**. A relaxation is an **approximation of a difficult problem by a nearby problem that is easier to solve**. A solution of the relaxed problem provides information about the original problem.

For example, **a linear programming relaxation of an integer programming problem removes the integrality constraint and so allows non-integer rational solutions.** A Lagrangian relaxation of a complicated problem in combinatorial optimization penalizes violations of some constraints, allowing an easier relaxed problem to be solved. Relaxation techniques **complement or supplement branch and bound algorithms** of combinatorial optimization; linear programming and Lagrangian relaxations are used to obtain bounds in branch-and-bound algorithms for integer programming.[1]

The modeling strategy of relaxation should not be confused with iterative methods of relaxation, such as successive over-relaxation (SOR); iterative methods of relaxation are used in solving problems in differential equations, linear least-squares, and linear programming.[2][3][4] However, iterative methods of relaxation have been used to solve Lagrangian relaxations.[5] 



## Saddle point

point on the surface of the graph of a function where the slopes (derivatives) in orthogonal directions are all zero (a critical point), but which is not a local extremum of the function

- example: when there is a critical point with a relative minimum along one axial direction (between peaks) and at a relative maximum along the crossing axis
- the name derives from the fact that the prototypical example in two dimensions is a surface that curves up in one direction, and curves down in a different direction

([Wikipedia](https://en.wikipedia.org/wiki/Saddle_point))



## Shapley values

A prediction can be explained by assuming that each feature value of the instance is a "player" in a game where the prediction is the payout. Shapley values – a method from coalitional game theory – tells us **how to fairly distribute the "payout" among the features**.

How much has each feature value contributed to the prediction compared to the average prediction?

A solution comes from cooperative game theory: the Shapley value, coined by Shapley (1953), is **a method for assigning payouts to players depending on their contribution to the total payout**. Players cooperate in a coalition and receive a certain profit from this cooperation.

- the "game" = prediction task for a single instance of the dataset
- the "gain" = the actual prediction for this instance minus the average prediction for all instances
- the "players" = the feature values of the instance that collaborate to receive the gain (= predict a certain value)

The Shapley value is **the average marginal contribution of a feature value across all possible coalitions** (in comparison, LIME suggests local models to estimate effects).

The interpretation of the Shapley value for feature value $j$ is: the value of the $j$-th feature contributed $ϕ_j$ to the prediction of this particular instance compared to the average prediction for the dataset.

The Shapley value works for both classification (if we are dealing with probabilities) and regression.

Intuitive understanding: 

- the feature values enter a room in random order
- all feature values in the room participate in the game (= contribute to  the prediction)
- the Shapley value of a feature value = the average change in the  prediction that the coalition already in the room receives when the  feature value joins them

**Advantages**

- the difference between the prediction and the average prediction is **fairly distributed** among the feature values of the instance (**efficiency** property of Shapley values)
  - $\ne$ to LIME which does not guarantee that the prediction is fairly distributed among  the features
  - Shapley value might be the only method to deliver a full  explanation; when the law requires explainability the Shapley value might be the only legally compliant method, because it is based on a solid theory and distributes the  effects fairly

* allows **contrastive explanations**
  * instead of comparing a prediction to the average prediction of the  entire dataset, you could compare it to a subset or even to a single  data point
  *  something that local models like LIME do  not have

* only explanation method with a **solid theory**
  * methods like LIME assume linear behavior of the machine learning model  locally, but there is no theory as to why this should work

**Disadvantages**

* requires **a lot of computing time**
  * because there are $2^k$ possible coalitions of the feature values and the "absence" of a  feature has to be simulated by drawing random instances, which increases the variance for the estimate of the Shapley values estimation
  * the exponential number of the coalitions is dealt with by sampling  coalitions and limiting the number of iterations $M$
  * decreasing $M$ reduces computation time, but increases the variance of the Shapley value
  * no good rule of thumb for the number of iterations $M$
    * should be large enough to accurately estimate the Shapley values, 
    * but  small enough to complete the computation in a reasonable time
  * in most real-world problems, only the approximate solution is feasible

* can be **misinterpreted**: the Shapley value of a feature value **is <u>not</u> the difference of the  predicted value after removing the feature** from the model training. 
  * rather: given the current set of feature values, **the contribution of a feature  value to the difference between the actual prediction and the mean  prediction** is the estimated Shapley value

* not a method for sparse explanations (explanations that contain few features): explanations created with the Shapley value method **always use all the features**
  * often we prefer selective explanations, such as those produced by LIME
  * another solution is [SHAP](https://github.com/slundberg/shap), which is based on the Shapley value, but can also provide explanations with few features

* the Shapley value returns a simple value per feature, but **no prediction model** ($\ne$ LIME)
  * it cannot be used to make statements about changes in prediction for changes in the input
* **access to the data needed** for calculating the Shapley value **for a new data instance**
  * access to the prediction function is not sufficient because you need  the data to replace parts of the instance of interest with values from  randomly drawn instances of the data
  * can only be avoided if you can create data instances that look like real data instances but are not actual instances from the training  data

* like many other permutation-based interpretation methods, suffers from **inclusion of unrealistic data instances when features are correlated**
  * to simulate that a feature value is missing from a coalition, we  marginalize the feature
    * achieved by sampling values from the feature’s marginal  distribution
    * fine as long as the features are independent
    * when features are dependent, then we might sample feature values that do not make sense for this instance. But we would use those to compute the feature’s Shapley value
  * one solution might be to permute correlated features together and get  one mutual Shapley value for them
  * another adaptation is conditional sampling: features are sampled  conditional on the features that are already in the team
    * fixes the issue of unrealistic data points
    * but a new issue is introduced: the resulting values are no longer the Shapley values to our game, since they violate the symmetry axiom



([Molnar 2021](https://christophm.github.io/interpretable-ml-book/shapley.html))



## Stationary point

in calculus, point on the graph of the function where the function's derivative is zero

- point where the function "stops" increasing or decreasing (hence the name)

([Wikipedia](https://en.wikipedia.org/wiki/Stationary_point))





## Symbolic regression

Symbolic regression searches the vast space of possible functional forms to arrive at a parsimonious description of the data. 

([Martin et al. 2018](https://royalsocietypublishing.org/doi/full/10.1098/rspb.2018.0422))

Symbolic regression is the task of identifying a mathematical expression that best fits a provided dataset of input and output values

([Valipour et al. 2021](https://arxiv.org/abs/2106.14131))

Symbolic Regression (SR) is a type of regression analysis that **searches the space of mathematical expressions to find the model that best fits a given dataset, both in terms of accuracy and simplicity**. No particular model is provided as a starting point to the algorithm. Instead, **initial expressions are formed by randomly combining mathematical building blocks** such as mathematical operators, analytic functions, constants, and state variables. Usually, a subset of these primitives will be specified by the person operating it, but that's not a requirement of the technique. The symbolic regression problem for mathematical functions has been tackled with a variety of methods, including recombining equations most commonly using genetic programming, as well as more recently methods utilizing Bayesian methods and physics inspired AI.

([Wikipedia](https://en.wikipedia.org/wiki/Symbolic_regression))





