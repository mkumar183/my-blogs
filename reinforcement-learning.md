### Reinforcement Learning 
Agent, Environment, Action, State, Next State, Reward. 

How do you model a RL Problem as NN problem. 
Difference between NN and Deep RL: 
1. In RL the states are not IID. These are sequencial. 
2. In Deep RL you are looking at total rewards rather than each reward

How is this problem solved ? 
This problem is solved by storing the samples in sequence (from one state to another state) 
and then taking these in batches. For each batch you look at total reward and based on that
adjust the weights rather than adjusting weights for each reward like in backpropagation.

Input vector: 
if state has 4 variables and action has 2 then input layer will have 6 neurons. 

What network will optimize:
for each state and action (a batch of) what is the total reward. It will then adjust the weights to maximize the rewards over the batch. this will in turn arrive at weights in such a way that best action will be selected. 

#### Markov State
You learnt that Markov state is some function of the knowledge base: that captures all relevant information from the knowledge base. The Markov assumption states that the current state contains all the necessary information about the past (states, actions and rewards) to take the next action. All these processes that work in accordance to Markov property are called Markov Decision Processes (popularly called MDPs).

value function of a state: value of a state. a relative number that tell how good particular state is. v(s)

action value function: value of an action in a state q(s,a). total reward given a particular state and action.
its immediate reward + future rewards with a discount factor.
