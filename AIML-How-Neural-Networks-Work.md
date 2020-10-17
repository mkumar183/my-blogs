### How neural networks work
Fundamentally, each neuron in a neural network is just a mathematical function. Each neuron computes a weighted sum of its inputs—the larger an input's weight, the more that input affects the neuron's output. This weighted sum is then fed into a non-linear function called an activation function—a step that enables neural networks to model complex non-linear phenomena.

The power of Rosenblatt's early perceptron experiments—and of neural networks more generally—comes from their capacity to "learn" from examples. A neural network is trained by adjusting neuron input weights based on the network's performance on example inputs. **If the network classifies an image correctly, weights contributing to the correct answer are increased, while other weights are decreased. If the network misclassifies an image, the weights are adjusted in the opposite direction.**

The fortunes of neural networks were revived by a famous 1986 paper that introduced the concept of backpropagation, a practical method to train deep neural networks.

Suppose you're an engineer at a fictional software company who has been assigned to create an app to determine whether or not an image contains a hot dog. You start with a randomly initialized neural network that takes an image as an input and outputs a value between 0 and 1—with a 1 meaning "hot dog" and a 0 meaning "not a hot dog."

To train the network, you assemble thousands of images, each with a label indicating whether or not the image contains a hot dog. You feed the first image—which happens to contain a hot dog—into the neural network. It produces an output value of 0.07—indicating no hot dog. That's the wrong answer; the network should have produced a value close to 1.

The goal of the backpropagation algorithm is to adjust input weights so that the network will produce a higher value if it is shown this picture again—and, hopefully, other images containing hot dogs. To do this, the backpropagation algorithm starts by examining the inputs to the neuron in the output layer. Each input value has a weight variable. The backpropagation algorithm will adjust each weight in a direction that would have produced a higher value. The larger an input's value, the more its weight gets increased.

My commentary:
In a NN you have lets say 10 inputs (these are like 10 features or predictors in linear regression). Now what a linear regression algorithm finds out is the coefficients for these features that contributes to the final output. In NN, these coefficients are replaced by weights and middle layer neurons. The output layer has final output. Which is then matched with the desired outcome. Depending upon desired and actual outcomes match or not, the weights are adjusted. Weights are adjusted very little at a time, so that not to disrupt the learning that has happened so far. By iteratively adjusting these weights over thousands of such inputs we arrive at final weights which can then do an accurate prediction. This is a simple mechanism of how neural networks function. 

Reference
[iRosenblatt's](https://arstechnica.com/science/2019/12/how-neural-networks-work-and-why-theyve-become-a-big-business/#:~:text=A%20neural%20network%20is%20trained,while%20other%20weights%20are%20decreased.)
