### Understanding DQN 

#### Q-Learning 
Q value for all cells is zero. 
Iterations are done and Q values are updated (based on reward system). 
In case of robot having to cross the Q-values of cells next to the exit cells will get updated. 
Then similarly Q-values of adjacent cells and so on. 
Thus in lot of iterations Q-values will get filled. 

#### Epsilon Greedy Approach
This is to balance exploitation vs exploration. When epsilon value is high it chooses exploratory approach (takes actions for which rewards are not known or less). As network gets trained it epsilon value starts decreasing and it starts to take more exploitation approach. i.e. takes actions that are known to deliver better results. 

Take a game where you need to go from point A to B. You have 3 possitble paths. 
A, p11, p12, p13, B
A, p21, p22,      B
A, p31,           B

Each step is -1, Reaching B is +100. Q Learning table will be 
A p11  = -1
A p21  = -1
A p31  = -1
p11 P12 = -1
p12 p13 = -1
p13 B = 100
p21 p22 = -1
p22 B = 100
p31 B = 100

This can be initialized. Now if you update Q value at each cell it will be: 

A p11  = -1  = -1-1-1+100 = 97
A p21  = -1  = 98
A p31  = -1  = 99
p11 P12 = -1 = 98
p12 p13 = -1 = 99
p13 B = 100 = 100
p21 p22 = -1 = 98
p22 B = 100 = 100
p31 B = 100 = 100

Since here number of permutations are less and paths are defined, in a single iteration we know what 
is the best action start from A. But this method illustrates how Q-Learning works. It takes expected value at each nodes and then keep adjusting with every exploration.

#### DQN as NN
In DQN Modeled as NN, input layer is state + action vector. Output is Q-value for each Each action. So if there are 5 actions possible from a given state then there are 5 outputs. Q-value is maximized over a batch of inputs (rather than against a target value). 


Reference: 
[Q-Learning](https://www.freecodecamp.org/news/an-introduction-to-q-learning-reinforcement-learning-14ac0b4493cc/)

[dqn basics](https://tomroth.com.au/dqn-basics/)

[dqn theory](https://jaromiru.com/2016/09/27/lets-make-a-dqn-theory/)
