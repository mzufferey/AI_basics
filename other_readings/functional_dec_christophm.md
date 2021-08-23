

#             [Interpretable Machine Learning](https://christophm.github.io/interpretable-ml-book/)          

## 5.1 Functional Decompositon

A prediction function is, or often can be simplified to, a function  that takes a high-dimensional input and outputs a 1-dimensional number. **Functional decomposition takes this high-dimensional function and splits it into lower-dimensional components**. The split allows to attribute effects to individual features and to  identify interactions between features. Functional decomposition is both an interpretation method and mental  model which immensily helps you to understand other interpretation  methods.

This chapter may be the most central in this book. While a full functional decomposition is usually infeasible in practice, it is very valuable to have understood the idea of functional  decomposition. Functional decomposition is a fundamental principal that underlies many  other techniques, or at least can act as a mental lens through which to  understand better what other methods do.

We start with a function that takes two features as input and produces a 1-dimensional output, the prediction. The function is:
$$
y = f(x_1, x_2) = 2 + e^{x_1} - x_2 + x_1 \cdot x_2
$$


We can visualize the function with a 3-D plot or a heatmap with contour lines:

Prediction surface of a two-dimensional function.

FIGURE 5.2: Prediction surface of a two-dimensional function.

The function takes on large values when $(x_1)$ is large and $(x_2)$ is low, and it takes on low values for large $(x_2)$ and low $(x_1)$. We can see both from the formula and the figure that there is an  interaction between the two features, meaning that the prediction is not merely a sum of each feature effect. Note that in a machine learning setup nobody will give us such a neat  formula, but we usually have access to the prediction/classification  function. Can we identify, just by having access to the function, the main effects and the interaction effect? This would tell us how each feature affects the prediction individually, and which part is due to interaction of both features. The solution is **functional decomposition**: to **decompose this  two-dimensional function into its components**. For a two-dimensional function $f$, which only depends on two input  features: $(f(x_1, x_2))$, we want **each component to represent a main  effect, interaction or intercept**:
$$
f(x_1, x_2) = f_0 + f_1(x_1) + f_2(x_2) + f_{1,2}(x_{1,2})
$$


The component $f_0$ is the intercept, components $f_1$ and  $f_2$ are the main effects of $x_1$ and $x_2$ and $f_{1,2}$ is  the interaction effect between the two features. **The main effects tell us how each feature affects the prediction,  independent of the values the other feature takes on. The interaction effect tells us what the effect of the features is  together. The intercept simply tells us what the prediction is when all feature  effects are set to zero.** Note that **the components themselves are functions (except for the  intercept) with different input dimensionalities**.

Let us decompose the function above. I will just give you the components for now, and the explanation for how to derive at the decomposition later. The intercept is $f_0\sim3.18$. Since the other components are functions, we can visualize them:

*Decomposition of a function.*

*FIGURE 5.3: Decomposition of a function.*

Do you think the components make sense given the true formula above? For now it is a bit mysterious why the intercept has this specific value. The feature $x_1$ shows an exponential effect as main effect, and $x_2$ shows a negative linear effect. This matches the formula that produced the predictions. The interaction term looks like a pringles. In more mathematical terms, it is a hyperbolic paraboloid as we would expect for $x_1 \cdot x_2$.

The example was two-dimensional. Before we go into the details how the components were computed, we define the decomposition for a more general setup.

### 5.1.1 Deeper Into the Mathematics

We start with a function that takes $p$ features as input, $f:  \mathbb{R}^p \mapsto \mathbb{R}^1$. This could be a regression function, but also the classification  probability for a certain class or the score for a certain cluster  (unsupervised machine learning). How does a composition look like in general?
$$
f(x) = & f_0 + f_1(x_1) + \ldots + f_p(x_p) \\  & + f_{1,2}(x_1, x_2) + \ldots + f_{1,p}(x_1, x_p) + \ldots +  f_{p-1,p} \\  & + \ldots + \\ & +  f_{1,\ldots,p}(x_1, \ldots,  x_p)
$$
*TODO: CONTINUE HERE*

Okay that’s really ugly and long, we shorten it a bit. We can make it more precise and shorter by indexing all possible subsets of feature combinations: $S\subseteq\{1,\ldots,p\}$. This contains all main effects, and all interactions, and also the empty set, which we need to define the intercept. The the function $f$ can be decomposed as:
$$
f(x) = \sum_{S\subseteq\{1,\ldots,p\}} f_S(x_S)\
$$


Here, $x_S$ is the vector of features in the index set $S$.

**A function that takes in a p-dimensional vector can be split into $2^p$ components.**
$$
|comps| = \sum_{i = 0}^p \binom{p}{i} = 2^p
$$




For example, if the function has 10 features, we already would have 1042 components. **With each additional feature, the number of components doubles** (!!). So, clearly it is not feasible for most functions to compute all the components. Another reasons against computing all components is that c**omponents with $|S|>2$ are difficult to visualize and interpret**.

### 5.1.2 How to Compute the Components

So far, our problem of functional decomposition is under-specified,  meaning there is no unique solution. The big problem with a functional decomposition is that it is quite  **arbitrary** if we don’t pose any limitations on how each of the components look like. Arbitrary means that **we can move an effect between main effect and a  higher interaction while the total prediction remains intact**. I can give you some food for thought. We start with the minimal requirement that summing up our components  (the $f_S$’s) actually sums up to the function $f$. That means that **no matter what input we put into $f$ and the  components, the outcome is equal**. Let’s say you have a 3-dimensional function. It actually does not matter how this function look like, but the  following decomposition would be valid: $f_0$ is 0.12. $f_1 = 2 \cdot x_1$ plus the number of shoes you own. $f_2$, $f_3$, $f_{1,2}$, $f_{2,3}$, $f_{1,3}$ are all zero. And finally to make this trick work, I define:
$$
f_{1,2,3} = f(x) - \sum_{S\subset\{1,\ldots,p\}} f_S(x_S)
$$


Not very meaningful, and quite deceptive if you would present this as the interpretation of your model. How to we prevent this ambiguity?

Some methods:

- Use a **restricted model**: Looking at you, linear regression model! If  you train a very restricted model, where we can identify within the  model the individual components, then we get a decomposition for free.  In a linear regression model without interactions specified, we only get an intercept and the first order effects. Each coefficient is then a  first order effect. If we add interaction terms, then the coefficient of that interaction term plus the form of the chosen interaction may be  interpreted as the second order interaction between the two respective  features.
- Stein. But very restricted, more interesting because of historic and academic reasons. The integrate does not really reflect the underlying  data distribution *TODO: Check in functional ANOVA paper how it is  computed.*
- More sophisticated is the **functional ANOVA**, which **works well in the  case where the features are independent**. This technique will be  explained in detail below.
- The **generalized functional ANOVA** is an extension of the functional  ANOVA, which can also **work for features that are dependent**. It is,  however, more difficult to compute. Also covered later in this chapter.
- [Accumulated Local Effects Plots](https://christophm.github.io/interpretable-ml-book/ale.html#ale) also allow a functional decomposition.
- See newer stuff (HDMR and whatnot)

### 5.1.3 Getting Decomposition for free

For functions such as the [linear regression model](https://christophm.github.io/interpretable-ml-book/limo.html#limo) or [generalized additive models (GAMs)](https://christophm.github.io/interpretable-ml-book/decomposition.html#extended-lm), we already get a decomposition for free.

*Write more here?*

Mention that this is source of confusion. e.g. you could have linear regression model with interaction term. You could see this as decomposition. But if you apply one of the methods that follows, especially for the following methods, the results might differ. So if we know that all interactions are zero, we can use the [GAM](https://christophm.github.io/interpretable-ml-book/decomposition.html#lm-extended) to model this function. What if we know that the maximum number of features involved in  interactions is 2, i.e., $|S|\leq{}2$? This would mean we could model it with an additive function that also  allows interactions between two features, such as MARS (CITE) or GA2M  (CITE).

Can a function exist where lower-components are zero, but the the interactions are non-zero? For example, components for \(X_1\) and \(X_2\) are zero, but their interaction is not? Yes! For example, the XOR problem where $Y=X_1XOR{}X_2$.

### 5.1.4 Functional ANOVA

The functional ANOVA was proposed by Hooker (2004)[27](https://christophm.github.io/interpretable-ml-book/decomposition.html#fn27). The requirement is that the model prediction function $f$ is square integrable. The decomposition is as any decomposition:
$$
f(x) = \sum_{S\subseteq\{1,\ldots,p\}} f_S(x_S)
$$


Hooker (2004) proposes to estimate the individual components as:
$$
f_S(x) = \int_{X_{-S}} \left( f(x) - \sum_{V \subset S} f_V(x)\right) d X_{-S})
$$


Ok, let’s take this thing apart. Instead we can also write:
$$
f_S(x) = \int_{X_{-S}} \left( f(x)\right) d X_{-S}) - \int_{X_{-S}} \left(\sum_{V \subset S} \right) d X_{-S})
$$

**The first part is the integral over the prediction function, with  respect of the features that are not in the set. This the same as the expectation of the function when we integrate out  features $X_{-S}$, and pretending that all features follow a uniform  distribution.** The second part are all the lower dimensional components, so we apply  some kind of centering. For example if $S=\{1,2\}$, meaning we look at the interaction effect  of the first two features, and let’s say there are, in total, 4  features. Since the formula is recursive in the sense that to compute higher order interactions, you have to also compute all lower-order interactions, we start with the lowest order, namely the $f_0$ component which is a  constant. For $f_0$, the subset $S=\{\emptyset\}$ and therefore -S contains  all features $X_S$. Therefore we get:
$$
f_S(x) = \int_{X} f(x) d X
$$


This is simply the prediction function where we integrated over all features. And it can also be seen as **the expectation of the function, but when we assume that all features are uniformly distributed**. Now that we have $f_0$, we can get the formula for $f_1$:
$$
f_1(x) = \int_{X_{-1}} \left( f(x) - f_0\right) d X_{-S})
$$


The formula for $f_2(x)$ is equivalent.

The the $V\subset{}S$ are ${1,2}$ and the formula becomes:
$$
f_{1,2}(x) = \int{_{\{3,4\}}} \left( f(x) - (f_0(x) + f_1(x) + f_2(x))\right) d X_{3},X_4
$$
This example shows how **each higher order effect is defined by  integrating over all other features, but also be removing all the  lower-order effects that are subsets of the higher-order effects**.

Hooker (2004) showed that **this fullfills a few desirable axioms**:

- <u>Zero Means</u>: $\int{}f_S(x_S)dX_s=0$ for each $S\neq\emptyset$.
- <u>Orthogonality</u>: $\int{}f_S(x_S)f_V(x_v)dX=0$ for $S\neq{}V$
- <u>Variance Decomposition</u>: Let $\sigma^2_{f}=\int{}f(x)^2dX$, then $\sigma^2(f) = \sum_{S \subseteq P} \sigma^2_S(f_S)$, where $P$ is the set of all features.

The **zero means axiom** implies that **all effects or interactions are  centered around zero**. The **orthogonality axiom** says that any **two components do not share  information**, meaning that, for example, the first order effect of  feature $X_1$ and the interaction term of $X_{1}$ and $X_2$ are  not correlated. The **variance decomposition** allows us to **split the variance** of the  function $f$ among the components, and guarantees that it really adds  up in the end.

Actually solved with (centered) PDPs.

When the feature are dependent, we may need the generalized functional ANOVA.

### 5.1.5 Generalized functional ANOVA for dependent features

But what do we do when features are dependent? If we simply integrate over the marginal distribution, we effectively  create a new dataset which does not match with the joint distribution,  but extrapolates to areas outside of the joint distribution.

Is it possible to define a decomposition which fulfills the desired  axioms, but in addition does not create data outside the distribution?

The answer is yes, as long as we know the joint distribution.

Hooker (2007) [28](https://christophm.github.io/interpretable-ml-book/decomposition.html#fn28) proposed the generalized functional anova, a decomposition that works  for dependent features. It is more general than the functional ANOVA that we encountered before, because it is defined in terms of projections of f onto the space of  additive functions.

The components are differently defined, namely:
$$
{f_S(x_S) | S \subset P} = argmin_{g_S \in L^2(\mathbb{R}^S)}_{S \in P} \int {\left(\sum_\S \subset P} g_S(x_S) - f(x)\right)^2 w(x)dx
$$


There is an additional constraint for the individual components:
$$
\forall f_S(x_S)| S \subset U: \int f_S(x_S) f_U(x_U) w(x)dx = 0
$$


Without this constraint, the solution would not be unique. We have not defined yet what \(w(x)\) really is. One solution would be to let \(w\) be the uniform measure on the unit cube. The natural choice would of course be that w is the joint probability function.

The estimation is done on a grid of points. We have two problems when doing the estimation.

First problem is that we need to define \(w(x)\). While the joint distribution is a good choice, there are few cases where we actually know the joint distribution. The second problem is that we solve this in linear regression and have  to do this theoretically for all components at once, meaning to solve  this in a model with \(2^{P - 1}\) components. For this for each feature a grid of feature values is defined.

### 5.1.6 Accumulated Local Effects Plots

Accumulated Local Effect Plots can also be used as a functional decomposition. The resulting components are not orthogonal, though. But they are pseudo-orthogonal.

When feature are independent of each other, then the result is the same as for functional ANOVA. (TODO check in ALE paper). What is the motivation for ALE instead of functional ANOVA? Functional ANOVA can be difficult from intuition for strongly correlated features.

TODO: Mention the disadvantage example in ALE paper for Generalized fANOVA for the correlated case.

### 5.1.7 Viewing Other Methods Through the Lens of Decomposition

You might want to come back to this chapter again if you have a good grasp on some of the other methods.

First on a high level: Feature effects are direct visualizations of the individual components. However we have to distinguish between total effect and isolated effect for the higher-order feature effects. PDP is total effect ALE is individual effect If you remove lower effects from PDP, you get fANOVA, at least for indepdentn feature case.

PDP is a direct decomposition, but with additional intercept difference. ALE is a decomposition. For permutation feature importance, Methods such as SHApley and co only describe a prediction with 1-dimensional effects. What happened with the interaction terms? They are divided among the individual effects. What happened in PFI with the interactions? They are alos divided among individual effects.

The SHAP interaction plots can also be better understood through decompositions.

There are many methods that produce individual explanations in the  form of \(f(X)=\sum_{p=1}^n\phi_j\), which attribute one number per  feature, see [Shapley Values](https://christophm.github.io/interpretable-ml-book/shapley.html#shapley), [LIME](https://christophm.github.io/interpretable-ml-book/lime.html#lime) and most [pixel attribution methods for neural networks](https://christophm.github.io/interpretable-ml-book/pixel-attribution.html#pixel-attribution), all of which you will encounter later in the book. Looking at them through functional decomposition: When methods fulfill the property of efficiency, like for example  Shapley values do, it means that the sum of attributions are equal to  the prediction. This means that we decomposed the function. But there are only first order effects, no interactions. It means that all the interactions have to be split among the individual values per feature. And we do not get separate interation effects.

### 5.1.8 Advantages

A rigorous way of thinking about high-dimensional functions.

Allows to understand most other interpretation methods much better.

Separation and attribution of effects.

### 5.1.9 Disadvantages

When output high-dimensional, than not as simple.

Makes little sense for images and text.

No clear superior way for the axioms.

Estimating higher effect components always difficult and no good way to visualize.

Functional ANOVA is very difficult to estimate, no available software.

ALE is an alternative for when features are dependent, but offers no variance decomposition.

Does not have an implementation besides (ALE)[#ale].

------

1. Hooker, Giles. “Discovering additive structure in black box functions.” Proceedings of the tenth ACM SIGKDD international  conference on Knowledge discovery and data mining. 2004.[↩︎](https://christophm.github.io/interpretable-ml-book/decomposition.html#fnref27)
2. Hooker, Giles. “Generalized functional anova  diagnostics for high-dimensional functions of dependent variables.”  Journal of Computational and Graphical Statistics 16.3 (2007): 709-732.[↩︎](https://christophm.github.io/interpretable-ml-book/decomposition.html#fnref28)