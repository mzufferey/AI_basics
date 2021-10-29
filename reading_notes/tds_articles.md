### [Optimization](https://medium.com/analytics-vidhya/optimization-acb996a4623c)

An **optimization problem** consists of maximizing or minimizing a real  function by systematically choosing input values from an allowed set and computing the value of the function

We choose values for x so that this function f is either maximized or  minimized. There might be certain constraints on x which have to be  satisfied while solving the optimization problem i.e. we can choose x  only in certain regions or sets of values.

In real life, optimization helps improve the efficiency of a system. 

It helps to find minimum error or best solution for a problem.

in regression, Optimization helps find a minimum value for the loss function.

If there are no constraints on what values the decision variables can  take, we have an **unconstrained optimization problem**. This is a type of  problem encountered in linear regression. It is also called a **functional approximation** **problem** and is widely used in data science

almost all Machine Learning algorithms can viewed as solutions to optimization problems

*Components of Optimization*

1. Objective function (either maximum or minimum)
   *  f(x) which we are  trying to either maximize or minimize. In general, we talk about  minimization problems because if you have a maximization problem with  f(x), it can be converted to a minimization problem with -f(x).

2. Decision variables
   * x which we can choose to minimize the function. So, we usually write this as min f(x).

3. Constraints
   * a ≤ x ≤ b which constrains this **x** to a certain set of values.

Depending on type of objective function, constraints and decision variables, <u>optimization can be of various types</u>.

1. **Linear programming**: If the decision variable is continuous, the  functional form is linear and all constraints are also linear, it is  called a Linear programming problem.
   * If the objective function and constraints are linear and decision variable is an integer, it is called a **Linear integer programming** problem.
   * A special case might be decision variables can only take binary values (  0,1), then it is called a **Binary integer programming** problem.

2. **Non linear programming**: If the decision variable remains continuous and  either the objective function or constraints are non linear, it is  called a non linear programming problem.
   * If the objective function or constraints are non-linear and decision  variable is an integer, it is called a **Non-linear integer programming**  problem.

**Mixed-integer linear programming** problem: If the decision variable(x) is a mixed  variable and if the objective function(f) is linear and all the  constraints are also linear then this type of problem known as a  mixed-integer linear programming problem. So, in this case, the decision variables are mixed, the objective function is linear and the  constraints are also linear.

**Mixed-integer non-linear programming** problem: If the decision variable(x) remains  mixed; however, if either the objective function(f) or the constraints  are non-linear then this type of problem known as a mixed-integer  non-linear programming problem. So, a programming problem becomes  non-linear if either the objective or the constraints become non-linear.

Univariate optimization is a non-linear optimization problem with no constraints.  In Univariate optimization, there is only one decision variable.

min f(x)

x*∈* R,

 x is the decision variable and f is the objective function. x is continuous as it can have an infinite set of values

in certain cases, the local and global minima might not coincide. Such functions are known as **Non-convex functions**

Gradient descent finds the optimal values of β0 and β1 that minimize loss function using the following steps:

1. Randomly guess the initial values of β0 (bias or intercept) and β1 (feature weight).

2. Calculate the estimated value of the outcome variable Ŷi for initialized values of bias and weights.

3. Calculate the mean square error function (MSE).

4. Adjust the β0 and β1 values by calculating the gradients of the error function.

5. Repeat steps 1 to 4 for several iterations until the error stops  reducing further or the change in cost is infinitesimally small.





**nonlinear programming (NLP)** is the process of solving an optimization problem where some of the constraints or the objective function are nonlinear

An **optimization problem** is one of calculation of the extrema (maxima, minima or stationary points) of an objective function over a set of unknown real variables and conditional to the satisfaction of a system of equalities and inequalities, collectively termed constraints. 

If the objective function is concave (maximization problem), or convex (minimization problem) and the constraint set is convex, then the program is called convex and general methods from **convex optimization** can be used in most cases.

If the objective function is quadratic and the constraints are linear, **quadratic programming** techniques are used.

If the objective function is a ratio of a concave and a convex function (in the maximization case) and the constraints are convex, then the problem can be transformed to a convex optimization problem using **fractional programming** techniques. 

By definition, all constraints that are not linear are **nonlinear**. Nonlinear expressions include relationships in which variables are squared, cubed, taken to powers other than one, or multiplied or divided by each other.  Models with nonlinear expressions are much more difficult to solve than linear models. 