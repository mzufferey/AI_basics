## Constrained  Differential  Optimization 

(https://papers.nips.cc/paper/1987/file/a87ff679a2f3e71d9181a67b7542122c-Paper.pdf)

many optimization models of neural  networks  need constraints to  restrict the  space of outputs  to  a subspace which satisfies external criteria

optimizations using **energy methods** yield "forces" which act upon  the  state of the  neural  network

the **penalty method**,  in which **quadratic  energy  constraints** are  added  to  an  existing  optimization  energy,  has  become  popular  recently,  but  is  **not  guaranteed to satisfy  the  constraint conditions**  when  there  are  other forces  on  the  neural  model  or when  there 
are  multiple  constraints

**Basic  differential multiplier method** (BDMM), which  satisfies constraints  exactly

- create forces  which  gradually apply  the constraints  over time, using "neurons" that estimate Lagrange multipliers
- differential  version  of the  method  of multipliers from  Numerical Analysis
- the  differential  equations locally converge  to  a  constrained minimum



solutions to  a  constrained optimization problem  are restricted to  a  subset of the solutions of the  corresponding unconstrained optimization problem



a **constrained optimization problem** can be stated as 
$$
minimize f(x)=0,\textrm{ }subject\textrm{ } to\textrm{ } g(x)=0
$$

- $x$ =  **state** of the  neural  network,  a  position  vector in  a  high-dimensional  space
- $f(x)$ = **scalar energy**, which can be imagined as  the height of a landscape as a function of position $x$
- $g(x)=0$  =   scalar  equation  describing  a  **subspace  of the  state**  space



During  constrained  optimization,  the state $x$  should  be attracted to  the  subspace  $g(x)=0$,  then  slide along  the  subspace  until  it reaches  the 
locally smallest value of $f(x)$ on  $g(x)=0$. 



### The penalty method

$$
minimize\textrm{ }\epsilon_{penalty}(x) = f(x)+ c(g(x))^2
$$

- analogous  to  adding  a  rubber  band  which  attracts  the  neural  state  to  the  subspace  $g(x) =  0$
- adds  a  **quadratic  energy  term  which  penalizes  violations  of constraints**

- can be extended to fulfill  multiple constraints by  using more than  one rubber band

**Upsides**

- easy to use
- globally convergent to  the  correct answer 
- allows compromises between constraints.  

**Downsides**

- for  finite  constraint strengths, doesn't fulfill  the  constraints  exactly
  - using  multiple  rubber  band  constraints  is  like  building a  machine  out of rubber  bands: the  machine  would  not  hold  together  perfectly
- as  more constraints  are  added,  the  constraint  strengths  get  harder  to set,  especially  when  the  size  of  the network  
- dilemma to  the setting of the constraint strengths
  - if the strengths are small, then  the  system  finds  a  deep  local  minimum,  but does  not  fulfill  all  the  constraints
  - if the  strengths are  large,  then  the  system  quickly  fulfills  the  constraints,  but gets  stuck in  a poor local  minimum



### Lagrange multipliers 

$$
\epsilon_{Lagrange}(x) = f(x)+ \lambda g(x)
$$


$$
\triangledown\epsilon_{Lagrange}(x) = 0 = \triangledown f(x)+ \lambda \triangledown g(x)
$$


- also convert constrained  optimization problems  into  unconstrained extremization  problems ( a solution to  the equation is also  a  critical  point of the  energy)
- $\lambda$  =  **Lagrange multiplier** for  the  constraint $g(x) = 0$
- the  gradient of $f$  (= $\triangle f$) is  collinear  to  the  gradient of $g$  (= $\triangledown g$)  at the  constrained extrema (used in the design of BDMM)
- the  constant of proportionality  between  $\triangledown f$  and $\triangledown g$ is $-\lambda$
  - at the constrained minimum  $\triangledown f = -\lambda\triangledown g$ 



**Lagrange multipliers provide the extra degrees of freedom  necessary to  solve  constrained  optimization  problems.**





### Basic  Differential  Multiplier  Method (BDMM)  for  constrained  optimization 

- new  "neural"  algorithm  for  constrained  optimization,  consisting  of **differential  equations  which  estimate  Lagrange  multipliers**

- a  variation  of the method of multipliers



#### Gradient descent does not work with Lagrange multipliers

- energies  involving  Lagrange  multipliers,  however,  have  critical  points  which  tend  to  be  saddle 
  points
- gradient descent does not work  with  Lagrange multipliers,  because a critical point is an attractor not a local minimum ???

#### BDMM

- **alternative to  differential  gradient descent** that estimates the  Lagrange multipliers, so that **the constrained minima are attractors of the differential  equations**,  instead of "repulsors"
- extension  for  constrained  minimization  with  **multiple  constraints**
  - adding an extra neuron for every equality constraint and  summing all of the constraint forces 

-  extension  for  constrained  minimization  with  **inequality  constraints**
  - extra  slack  variables  to  convert  inequality  constraints  into  equality  constraints

#### Why  the  algorithm  works 

- the  function  $g(x)$ can  be  replaced  by  $kg(x)$, without  changing  the  location  of the  constrained  minimum

- as  $k$ (constant strength)  is  increased,  the  state  begins  to  undergo  damped  oscillation  about  the  constraint subspace  $g(x) = 0$
- as  $k$  is  increased further,  the  frequency  of the oscillations increase,  and the  time to  convergence increases





### Modified  Differential  Method of Multipliers 

- modification of the BDMM with  more robust convergence properties
- for a given constrained optimization problem, it is  frequently necessary to **alter the BDMM to have  a region of positive damping surrounding the constrained minima**
- the non-differential method of multipliers from  Numerical Analysis also has  this  difficulty
- Numerical Analysis combines  the  multiplier  method with the penalty method  to yield  a  modified multiplier method that  is  locally convergent around constrained minima
- the  BDMM is completely compatible  with  the  penalty  method.



### Conclusions 

- BDMM is a  modification  of a standard  constrained  optimization  algorithm,  which  improves  the  capability  of neural  networks  to 
  perform  constrained optimization

**Upsides** of BDMM  and  the  MDMM over  the  penalty  method:

- much  less stiff than  those of the  penalty method
  - very  large quadratic  terms are  not  needed by  the MDMM in order to  strongly enforce  the  constraints
  - the energy terrain  for  the penalty method looks like  steep canyons,  with  gentle floors;  finding  minima of these  types of energy 
    surfaces  is  numerically  difficult 
- the  steepness  of the  penalty  terms  is  usually  sensitive to  the  dimensionality  of the  space
  - the differential  multiplier  methods  are promising  techniques  for alleviating  stiffness
- differential  multiplier  methods  separate  the  speed  of fulfilling  the  constraints  from  the  accuracy  of fulfilling  the  constraints
  - in  the  penalty  method,  as  the  strengths  of a  constraint goes  to $\infin$,  the  constraint  is  fulfilled,  but  the  energy  has  many  undesirable  local  minima
  - multiplier methods  allow  one to  choose  how  quickly  to  fulfill  the  constraints
- The  BDMM fulfills  constraints  exactly  and  is compatible with  the  penalty  method
- Addition  of penalty  terms  in  the  MDMM  does  not  change  the  stationary  points of the  algorithm,  and  sometimes  helps  to  damp  oscillations and  improve  convergence
- BDMM and  MDMM are  in  the  form  of first-order  differential  equations:  they can be directly implemented in hardware
  -  BDMM  converges  globally  for  quadratic  programming
  - MDMM  is  provably  convergent  in  a  local  region  around  the constrained  minima
-  Other  optimization  algorithms have  similar  lo-cal  convergence  properties
  - the  global  convergence  properties  of the  BDMM  and  the  MDMM  are currently  under  investigation



In  summary,  the  differential  method  of multipliers  is  a  **useful  way  of enforcing  constraints**  on neural networks  for  enforcing  syntax  of solutions,  encouraging  desirable  properties  of solutions,  and making  crisp  decisions.