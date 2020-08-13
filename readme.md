## Modulo Experiments

This notebook depicts an attempt at getting a neural network to learn a modulo function.

Read more about the motivation behind this and view more infographics on the method at: https://delvingintotech.wordpress.com/2020/08/12/can-a-neural-network-learn-the-modulo-function

The general consensus is that a neural network can fit any function (Cybenko, 1990) http://cognitivemedium.com/magic_paper/assets/Cybenko.pdf

However, given that the neurons in a traditional neural network are typically only adding or subtracting the input weights * input data value, it is difficult to approximate multiplication and division.

A modulo involves calculating both addition/subtraction and multiplication/division, hence it is fundamentally challenging.

However, can a neural network learn a concept, such as how to calculate a modulo function? Such concepts are typically calculated using a procedural set of instructions, and is not exactly the domain of traditional neural networks.

The modulo function is basically the remainder. Some examples are:
- 5 % 2 = 1
- 5 % 3 = 2
- 6 % 2 = 0

One example of a set of instructions for modulo of a % b is as follows:
1) Calculate c = a//b (the floor of a/b)
2) Calculate modulo = a - c*b

Dependencies:
- tensorflow
- numpy

## Inputs/Outputs

Train Inputs:
250000 random integers from 0 to 2^20 (binarized)

Test Inputs:
10000 random integers from 2^20 to 2^21 (binarized)

Outputs:
Modulo of the input with factor 7

## Base Model
The base model in this notebook uses 3 different models:
- MLP with 1 hidden layer of 1000 nodes with ReLU activation, output softmax layer of 7 nodes
- MLP with 2 hidden layers of 1000 nodes with ReLU activation, output softmax layer of 7 nodes
- a ResNet with 3 layers, with skip connections between all layers, output softmax layer of 7 nodes

## Baseline Results
MLP with 1/2 hidden layers of 1000 nodes, ResNet: 
We are able to get a train accuracy of up to 100%, but the test accuracy tends to 0%!
Strong overfitting here.
This also means that the model is unable to generalize.

# What can we do?
We try five approaches:
- Add divisor as input
- Using whole numbers instead of binary as input
- Few-shot learning
- One-shot learning
- Zero-shot learning with X-factor

# Final model: 1 MLP layer after Conv1D gives about 75% test accuracy, and 2 MLP layers after Conv1D gives about 99% test accuracy. A tremendous improvement!

Overall, the modulo experiments have been interesting. It shows that the neural network cannot approximate any function easily, especially if it is out-of-sample.

In order to evaluate out-of-sample test cases, either one-shot learning or few-shot learning is required, with an expressive enough network structure.

For zero-shot learning, it seems that the structure to evaluate the unknown data needs to be inside the network. Here, for modulo, the Conv1D to evaluate 3 bits at a time is important. For CNNs, zero-shot learning should also be possible if the general concept of the image processing can be done on generic data, and not contingent on just a particular training set.

Moving on, I believe existing neural networks are not sufficient for learning concepts. It is more suited for pattern recognition and generalizing within a known domain. Perhaps we would need to do graphical models to better mimic the structure of the various functional domains within the brain, coupled with memory in order to link these concepts together.

I am keen to see how to improve the neural networks to do this. Current neural networks are a first step, but they are not the answer yet.
