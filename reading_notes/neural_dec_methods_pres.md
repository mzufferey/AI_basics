---
marp: true
theme: gaia
color: #000
colorSecondary: #333
backgroundColor: #fff
paginate: true
size: 4:3
---
<style>
section {
  font-size: 25px;
}
img[alt~="center"] {
  display: block;
  margin: 0 auto;
}
</style>
<!-- _paginate: false -->
​
# 


(Zhao et al. 2021)

---


### adaptation of functional ANOVA

aim: feature-level inter- pretability that would let us characterise the sources of variation for individual features
$\rightarrow$ functional ANOVA decomposition of the decoder network $f^θ(z_i, c_i)$ to extract additive marginal and interaction effects
$$
f^θ(z_i, c_i) = f^θ_0 + f^θ_z (z_i) + f^θ_c (c_i) + f^θ_{zc}(z_i, c_i)
$$

with latent and fixed inputs collectively denoted $x:=(z,c)$ can be generalized as

$$
f^θ_0 + \sum_k f^θ_k(x_{ik}) + \sum_{k,l}f^θ_{kl}(x_k,x_l) +... + f^θ_{1,...D}(x))
$$


---


### need for constraints

without additional constraints, the decomposition is unidentifiable: the functional subspaces $f_I$ can be seen as functions defined on the same input space, being constant in the rest of coordinates, and these subspaces are overlapping ($f^θ_I(x_I)$ is a subset of $f^θ_{I,J}(x_I, x_J)$)

$\rightarrow$ without additional constraints, no interpretation since higher order terms can absorb the variability that could be explained by main effects or lower-order interactions



---


### integral constraints


as for functional ANOVA, integral constraints to turn this into a identifiable learning problem

constrain the marginal effects of every neural network $f^θ_I(x)$ to be zero:

$$
\int f_I(x_I)dx_i=0\textrm{ }for\textrm{ }all\textrm{ }i\in I
$$

direct consequences of the integral constraints:

$\rightarrow$ the functional subsbaces corresponding to $f^θ_I$ and $f^θ_{I,J}$ do not overlap anymore

$\rightarrow$ these functional subspaces are orthogonal in $L_2$

---

### variance decomposition

no overlap = identifiability

orthogonality = interpretable variance decomposition

e.g. for a 2D input $(x_1,x_2)$
* the decomposition is $f^θ_0$, $f^θ_1$, $f^θ_2$, $f^θ_{12}$
* with the corresponding integral constraints:  
    * $\int f^θ_1(x_1)dx_1=0$ 
    * $\int f^θ_2(x_2)dx_2=0$ 
    * $\int f^θ_{12}(x_1, x_2 )dx_2=0$ for all $x_1$
    * $\int f^θ_{12}(x_1, x_2 )dx_1=0$ for all $x_2$


---

### variance decomposition

similarly for $(c,z)$
* the decomposition is $f^θ_0$, $f^θ_c$, $f^θ_z$, $f^θ_{cz}$
* with the corresponding integral constraints:  
    * $\int f^θ_c(c)dx_c=0$ 
    * $\int f^θ_z(z)dx_z=0$ 
    * $\int f^θ_{cz}(c,z)dz=0$ for all $c$
    * $\int f^θ_{cz}(c,z)dc=0$ for all $z$


---

### how to do inference with integral constraints ?

= constrained optimization problem

$\rightarrow$ solved with Augmented Lagrangian (method of multipliers)

adapted to neural networks: Platt and Barr 1988


---

### optimization: 1D scenario - "loss design"

for a decoder $f^θ_(x)$: optimize the ELBO with the constraint $\int f^θ(x)dx = 0$

(= restrict $f^θ$ to a subspace such that  $\int f^θ(x)dx = 0$)

$\rightarrow$ augment the ELBO with additional penalty term(s) which will be = 0 when the constraints are fulfilled during optimization


---

### optimization: 1D scenario - penalty method 1

how to add penalty to the ELBO ?

1) penalty method, with fixed $c$

$$
c (\int f^θ(x)dx)^2
$$
- downside: no guarantees that the constraints will be fulfilled (no convergence)

---

### optimization: 1D scenario - penalty method 2 
how to add penalty to the ELBO ?

2) BDMM: add a penalty which is treated as a parameter (analogous to the use of Lagrange multipliers, cf. Platt and Barr 1988)
$$
\lambda \int f^θ(x)dx
$$
- optimize NN parameters as usual with gradient descent $λ^{t+1} =λ^t - η(\int f^θ (x)dx)$ 
- optimize $\lambda$ simultaneously with gradient ascent  $λ^{t+1} =λ^t + η(\int f^θ (x)dx)$ 

 (η = learning rate)

- downside: converge to zero, but with damped oscillation behaviour (slow convergence)

---

### optimization: 1D scenario - penalty method 3

how to add penalty to the ELBO ?

3) combine the 2 penalty terms in a hybrid constrained optimization objective (HDMM)

$$
\underset{θ,\phi}{min}\{-L^{θ,\phi}+λ \int f^θ (x)dx+c (\int f^θ (x)dx)^ 2\}
$$

$c$ is constant, $\lambda$ is optimized and $L$ is the ELBO of the VAE


this scheme corresponds to the MDMM of Platt and Barr (1988)


empirical observation that replacing fixed $c$ with a sequence of $c^1\le...\le c^T$ leads to faster convergence



---

### optimization with HDMM: 2D scenario

e.g. for a 2D input $(x_1,x_2)$
* functional decomposition: $f^θ_0$, $f^θ_1$, $f^θ_2$, $f^θ_{12}$
* integral constraints:  $\int f^θ_1(x_1)dx_1=0$ , $\int f^θ_2(x_2)dx_2=0$ , $\int f^θ_{12}(x_1, x_2 )dx_2=0$ for all $x_1$, $\int f^θ_{12}(x_1, x_2 )dx_1=0$ for all $x_2$
* corresponding penalty terms:
    * $\lambda_1\int f^θ_{1}(x_1)$
    * $\lambda_2\int f^θ_{2}(x_2)$
    * $\lambda_1(x_1) (\int f^θ_{12}(x_1, x_2 )dx_1)dx_2$ for every $x_1$
        (introduced LM $\lambda_1(x_1)$ indexed by a continuous-valued $x_1$)
   * $\lambda_2(x_2) (\int f^θ_{12}(x_1, x_2 )dx_2)dx_1$ for every $x_2$
    (introduced LM $\lambda_2(x_2)$ indexed by a continuous-valued $x_2$)

---

### optimization with HDMM: 2D scenario

similarly for $(c,z)$ input we can write:
* functional decomposition: $f^θ_0$, $f^θ_c$, $f^θ_z$, $f^θ_{cz}$
* integral constraints:  $\int f^θ_c(x_c)dx_c=0$ , $\int f^θ_z(x_z)dx_z=0$ , $\int f^θ_{cz}(c, z)dx_z=0$ for all $x_c$, $\int f^θ_{cz}(c,z )dx_c=0$ for all $z$
* corresponding penalty terms:
    * $\lambda_c\int f^θ_{c}(c)$
    * $\lambda_z\int f^θ_{z}(z)$
    * $\lambda_c(c) (\int f^θ_{cz}(c,z )dx_c)dx_z$ for every $c$
   * $\lambda_z(z) (\int f^θ_{cz}(c,z )dx_z)dx_c$ for every $z$

---

### integral calculation

integrals can be estimated using either quadrature or Monte Carlo estimates

---

### when have the constraints been satisfied ?


proposed approach:
- establish a desired tolerance threshold $ε$
- evaluate the integrals after optimisation 
- make sure that all NNs have been constrained to the desired functional subspaces within the desired tolerance


---





- $\lambda$

- <u>how to handle **multiple constraints**</u> ? illustrated with the case of 2-dimensional input $(x_1, x_2)$
  - e.g. for $x_1$
    - for satisfying the constraint $\int f^θ_{12}(x_1, x_2)dx_2 = 0$ for all $x_1$ in some intervals:
      - introduce a Lagrange multiplier $\lambda_{x_1}(x_1)$ indexed by a continuous-valued $x_1$
      - the additional penalty term will be: $\int \lambda (x_1)(\int f^θ_{12}(x_1, x_2)dx_1)dx_2$ 
  - similarly for $x_2$, the ELBO will be augmented by: $\int \lambda (x_2)(\int f^θ_{12}(x_1, x_2)dx_2)dx_1$ 
  - in addition with to penalty terms $\lambda_1 \int f^θ_1(x_1)dx_1)$ and $\lambda_2 \int f^θ_2(x_2)dx_2)$



* <u>how to **estimate these integrals** in practice</u> ? $\rightarrow$ using either quadrature or Monte Carlo estimates



* <u>how to know **whether the constraints have been (approximately) satisfied**</u> ?
  - establish a desired tolerance threshold $ε$ 
  - evaluate the integrals after optimisation to make sure that all NNs have been constrained to the desired functional subspaces within the desired tolerance