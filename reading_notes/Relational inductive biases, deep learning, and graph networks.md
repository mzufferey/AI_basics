**Relational inductive biases, deep learning, and graph networks - Battaglia et al. 2018**





We explore how using relational inductive biases within deep learning architectures can facilitate learning about entities, relations, and rules for composing them. We

We present a new building block for the AI toolkit with a strong relational inductive bias—the graph network—which generalizes and extends various approaches for neural networks that operate on graphs, and provides a straightforward interface for manipulating structured knowledge and producing structured behaviors.

graph networks can support relational reasoning and combinatorial generalization, laying the foundation for more sophisticated, interpretable, and flexible patterns of reasoning

**combinatorial generalization**, that is, constructing new inferences, predictions, and behaviors from known building blocks

We represent complex systems as **compositions** of entities and their interactions

We use hierarchies to abstract away from fine-grained differences, and capture more general commonalities between representations and behaviors

We draw analogies by aligning the relational structure between two domains and drawing inferences about one based on corresponding knowledge about the other

the world is compositional, or at least, we understand it in compositional terms

modern deep learning methods (LeCun et al., 2015;
Schmidhuber, 2015; Goodfellow et al., 2016) often follow an **“end-to-end” design philosophy** which emphasizes minimal a priori representational and computational assumptions, and seeks to **avoid explicit structure and “hand-engineering”**.

we, too, reject the notion that structure and flexibility are somehow at odds or incompatible, and embrace both with the aim of reaping their complementary strengths

we see great promise in synthesizing new techniques by drawing on the full AI toolkit and marrying the best approaches from today with those which were essential during times when data and computation were at a premium

a class of models has arisen at the intersection of deep learning and structured
approaches, which focuses on approaches for reasoning about explicitly structured data, in particular graphs (e.g.

What these approaches all have in common is a capacity for performing computation over discrete entities and the relations between them

What sets them apart from classical approaches is how the representations and structure of the entities and relations—and the corresponding computations—can be learned, relieving the burden of needing to specify them in advance. Crucially,

Crucially, these methods carry **strong relational inductive biases**, in the form of specific architectural assumptions, which guide these approaches towards learning about entities and relations

**structure** as the product of composing a set of known building blocks

“**Structured representations**” capture this composition (i.e., the arrangement of the elements) and

“**structured computations**” operate over the elements and their composition as a whole

**Relational reasoning**, then, involves manipulating structured representations of entities and relations, using rules for how they can be composed.

We then present a general framework for entity- and relation-based reasoning—which we term graph networks—for unifying and extending existing methods which operate on graphs, and describe key design principles for building powerful architectures using graph networks as building blocks.

**relational inductive bias**. While not a precise, formal definition, we use this term to refer generally to **inductive biases** (Box 2) which impose constraints on relationships and interactions among entities in a learning process.

Building blocks such as “fully connected” layers are stacked into “multilayer perceptrons” (MLPs), “convolutional layers” are stacked into “convolutional neural networks” (CNNs), and a standard recipe for an image processing network is, generally, some variety of CNN composed with a MLP. This

This composition of layers provides a particular type of relational inductive bias—that of hierarchical processing—in which computations are performed in stages, typically resulting in increasingly long range interactions among information in the input signal. As

, the building blocks themselves also carry various relational inductive biases [e.g. convolutional component -> rel. inductive bias = locality; recurrent component -> rel. inductive bias = sequentiality]

various non-relational inductive biases are used in deep learning as well: for example, activation non-linearities, weight decay, dropout (Srivastava et al., 2014), batch and layer normalization (Ioffe and Szegedy, 2015; Ba et al., 2016), data augmentation, training curricula, and optimization algorithms all impose constraints on the trajectory and outcome of learning.

The implicit relational inductive bias in a **fully connected layer** is thus very weak: all input units can interact to determine any output unit’s value, independently across outputs

The differences between a fully connected layer and a **convolutional layer** impose some important relational inductive biases: locality and translation invariance (Figure

**Locality** reflects that the arguments to the relational rule are those entities in close proximity with one another in the input signal’s coordinate space, isolated from distal entities.

**Translation invariance** reflects reuse of the same rule across localities in the input.

These biases are very effective for processing natural image data because there is high covariance within local neighborhoods, which diminishes with distance, and because the statistics are mostly stationary across an image

A third common building block is a **recurrent layer** (Elman, 1990), which is implemented over a
sequence of steps. Here, we can view the inputs and hidden states at each processing step as the entities, and the Markov dependence of one step’s hidden state on the previous hidden state and the current input, as the relations. The rule for combining the entities takes a step’s inputs and hidden state as arguments to update the hidden state. The rule is reused over each step (Figure 1c), which reflects the relational inductive bias of **temporal invariance** (similar to a CNN’s translational invariance in space). For

there is no “default” deep learning component which operates on arbitrary relational structure

Invariance to ordering—except in the face of relations—is a property that should ideally be reflected by a deep learning component for relational reasoning

**Sets** are a natural representation for systems which are described by entities whose order is
undefined or irrelevant;

A natural way to handle such combinatorial explosion is to only allow the prediction to depend on symmetric functions of the inputs’ attributes

**permutation invariance** is not the only important form of underlying structure in
many problems.

two relational structures: one in which there are
no relations, and one which consists of all pairwise relations. Many

**Graphs**, generally, are a representation which supports arbitrary (pairwise) relational struc-
ture, and computations over graphs afford a strong relational inductive bias beyond that which convolutional and recurrent layers can provide.

our **graph networks** (GN) framework, which defines a class of functions for
relational reasoning over graph-structured representations. Our GN framework generalizes and extends various graph neural network, MPNN, and NLNN approaches (Scarselli et al., 2009a; Gilmer et al., 2017; Wang et al., 2018c), and supports constructing complex architectures from simple building blocks.

they can be implemented with functions other than neural networks, though here our focus is on neural network implementations.

we use “graph” to mean a directed, attributed multi-graph with a global attribute.

Our GN framework imposes several strong relational inductive biases when used as components in a learning process.

graphs can express **arbitrary relationships** among entities, which means the GN’s input determines how representations interact and are isolated, rather than those choices being determined by the fixed architecture.

graphs represent **entities and their relations as sets**, which are **invariant to permutations**.
This means GNs are invariant to the order of these elements6, which is often desirable.

a GN’s per-edge and per-node **functions are reused** across all edges and nodes, respectively.
This means GNs automatically support a form of **combinatorial generalization**

Graph Nets open-source software library: github.com/deepmind/graph nets
We have released an open-source library for building GNs in Tensorflow/Sonnet. It includes demos

we analyzed the extent to which relational inductive bias exists in deep learning architectures like MLPs, CNNs, and RNNs, and concluded that **while CNNs and RNNs do contain relational inductive biases, they cannot naturally handle more structured representations such as sets or graphs.**

We advocated for building stronger relational inductive biases into deep learning architectures by highlighting an underused deep learning building block called a graph network, which performs computations over graph-structured data. Our

**The structure of GNs naturally supports combinatorial generalization** because they do not perform
computations strictly at the system level, but also apply shared computations across the entities and across the relations as well.

One limitation of GNs’ and MPNNs’ form of learned message-passing (Shervashidze et al., 2011) is that it cannot be guaranteed to solve some classes of problems, such as discriminating between certain non-isomorphic graphs. Kondor

More generally, while graphs are a powerful way of representing structure information, they
have **limits**. For example, notions like recursion, control flow, and conditional iteration are not straightforward to represent with graphs, and, minimally, require additional assumptions