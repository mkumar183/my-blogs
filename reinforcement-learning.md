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
