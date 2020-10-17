### PGM Important points

```
bays theorum : p(a|b) = p(b|a)*p(a)/p(b)
```

```
remember nCr theorum: 
creating r combinations out of n distinct objects.
```

#### Bayes' Theorem : Practice
There are two bags containing black and white balls. The first bag contains 7 white and 3 black balls, whereas the second one contains 4 white and 6 black balls. Suppose a bag is chosen at random and a ball is picked. If the ball is white, what is the probability that the ball was picked from the second bag?
7/11

#### Learn joint probability
Toss a Die, Toss a coin, toss another coin. 
Table to represent combinations
Die	Coin1	Coin2	Probability
1	H	H	1/24
1	H	T	1/24

This table will have 24 rows and all 3 events are independent.

##### How would you break up the conditional probability p(S(1) | L(1), E(0))?

- Probabilistic models predict probability of certain set belonging to a class. Logistics, PGM, NN with softmax are all Probabilistic models. 
- *Discriminative Models*: These are trained to answer a specific question. *Generative Models* do not have specific target. Its like Topic modelling. Generative models typically have more points to train compared to discriminative models. 
- Generative models can predict even when some of the feature values are not available. This is done using *marginalizationi*. Marginalization is a technique where you take subset of combinations ignoring certain features and make a prediction using join probability. 

Probabilistic Graphical Models belong to the generative class of models which can model all the relationship between the variables and answer any question asked even when partial data is provided. The process of marginalization used to answer such questions is computationally very inefficient and hence we need to reduce the size of the joint probability distribution table to make it quicker.

though Generative models are unsupervised learning algorithms, they are very different from the Machine Learning unsupervised models. The ML unsupervised algorithms are discriminative since they are meant to answer some specific questions like outlier detection while the Generative class of unsupervised models is not meant to answer specific questions, rather model the underlying source of data. In discriminative model given a datapoint its asked if it belongs to a particular cluster. However, this is not the case in PGM.


When there are variables that are independent (e.g. throwing dice, coin and coin), you do not need full probability table with 6*2*2 rows. You just need 3 tables 6+2+2=10 rows and probability of every combination can be derived using joint probability rules. This is the fundamental reason behind existence and use of PGMs. PGMs help in modeling dependencies in features so as to drastically reduce the computation. 


Graphs are intuitive, simple and natural way to capture dependencies between features.

Probability distribution function: Given values of features it can give you probability of the combination. E.g. if i want to know 6-head-head in above example.

v(x1,x2,x3,x4,x5)=p(x1,x2).p(x3,x4,x5) 
in above case x1,x2 are completely independent from x3,x4,x5 while these two are interdependent.

Graphs:
The number of edges = n(n-1)/2 where n is the number of nodes.
Number of graphs = 2**z where z is the maximum number of edges.

Prof. has introduced the concept of Markov Equivalence. It states that a graph and a probability distribution is Markov Equivalent if all the conditional independencies in the probability distribution are captured by the graph we build.

*Conditional Independence*: If x1 and x2 are conditionally independent on x3 then 
p(x1,x2|x3) = p(x1|x3) * p(x2|x3)
Its like if someone starts from point A via point B and reaches C then time to reach C independent of A conditioned on B. here B is x3. Given B, A and C are independent.

Ford car not starting whenever the customer took an ice cream other than vanilla flavour is also a case of conditional independence. As Prof. explained, the flavour of the ice cream and the engine startup time are dependent on each other but the moment we condition it on the order service time, they become independent.

*Independence equality equation*: Two variables are independent if their probabilities alone add to 1. 
x1      x2 	probability
0	0	0.2
0	1	0.3
1	0	0.2
1	1	0.3
here if you get prob of x1 = 0(.5), 1(.5) it adds to 1. similarly x2. So x1 and x2 are independent.
Marginalized over x3 means for all values of x3 add probabilities of x1 and x2

Dependence is often defined by correlation. It is measured using co-variance..
covariance = E[(x−μx).(y−μy)]  (mu x is mean of x)

#### Directed Graphs:
Joint Probability Distributions: These are computationally intensive.
100 variables 20 have 5 values, 30 have 3 values, 50 have 2 values. Total combinations would be
5**20 * 3**30 * 3**50 
Directed graphs make this problem simpler using conditional independence. 

Hammersley–Clifford Theorem: X1 -> X5, X6 ->X2 -> X3 -> X4, X1 -> X4
JPD=p(x1)×p(x2|x6)×p(x3|x2)×p(x4|x1,x3)×p(x5|x1)×p(x6|x1)
uses conditional independence theory (point A to B to C example)
Drastically reduces the combinations required.

CPD vs JPD : Conditional prob dist vs Joint prob dist
HMM (Hidden Markov Model) is Directed Graph Model too.
CPDs are much cheaper to compute as compared to JPDs. 
Hammersley–Clifford Theorem uses CPDs.

Conditional Independent = no direct edge between two variables in a graph

Model: contains 3 parts:
1. variables (nodes)
2. structure (edges)
3. parameters (conditional distributions involved) e.g p(a|b,c) is one of the parameter. 
CPDs are model parameters.
If you have above 3 then that means you have a graphical model. Using the graphical model now you can answer any questions. 
Answering questions means to compute the probabilities of some variable(s), given some evidence. This process is also called inference.

#### Undirected Graphical Models
There is no direction in edges. 
A⊥E|C,D means that A and E are independent given C and D (they are always connected via C and D)

A clique is a part of the graph in which all the nodes or vertices are fully connected to each other. A maximal clique is the one which has the highest number of nodes that satisfy the definition of a clique.

Energy function intuitively quantifies interaction between two cliques. If there is low energy between two clique then it means they belong to different classes. e.g. pixels belonging to foreground and background.

The total number of edges in an m × n image will be: 
Nc=(m×(n−1))+((m−1)×n)

The higher the energy of an image, the lower the probability. The order of energies is reverse of the order of probabilities of the images.


#### Markov Random Fields
*Global Markov Property*
Global Markov Property states that for sets of nodes A, B, and C, A ⊥ B| C iff C separates A from B in the graph G. In other words, when we remove all the nodes in C, if there are no paths connecting any node in A to any node in B, then the conditional independence property holds.
In Putin’s social graph, based on the Global Markov Property, we can say that India ⊥ Syria | Russia
(India is independent of Syria given Russia).

*Local Markov Property*
The smallest set of nodes that renders a node conditionally independent of all the other nodes in the graph is called its Markov blanket. In an undirected graphical model, a node’s Markov blanket is its set of immediate neighbours. This is called the undirected local Markov property.

In Putin’s social graph, based on the Local Markov Property, we can say that Russia ⊥ Rest of the world | {India, China, Germany, Syria, Ukraine, USA} (Russia is independent of the rest of the world given its social graph neighbours India, Germany, Syria, Ukraine, USA and China)

*Pairwise Markov Property*
According to the local Markov property, two nodes are conditionally independent given the rest of the nodes if there is no direct edge between them. This is called the pairwise Markov property.

In Putin’s social graph, based on the pairwise Markov Property, we can say that
India ⊥ Syria | {Russia, China, Germany, Ukraine, USA}
(India is independent of Syria given the rest of social graph members Russia, Germany, Ukraine, USA and China)

#### Image multi-class segmentation
Having understood a Markov Random Field, let's summarise the above lecture on multi-class image segmentation. This type of segmentation requires you to create a feature set of the image. The steps involved in the segmentation are:
- Extract features for each label/ class.
- Initialise the weight matrix for different classes
- Generate the starting point by calculating the softmax probabilities using these matrices for different classes.
- Use a suitable energy function to capture the pairwise discord between pixels of the image which becomes a function of the weight and bias matrices of the different classes.
- Minimize this sum of energies over all the pairwise interaction in the batch of images.

