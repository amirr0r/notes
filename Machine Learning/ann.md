# Artificial Neural Networks (ANNs)

**Key idea:** look at the brain architecture for inspiration on how to build an intelligent machine.

**ANNs are at the very core of Deep Learning.**

First **introduced in 1943** by the neurophysiologist **Warren McCulloh** and the mathematician **Walter Pitts** in their paper: [_"A logical calculus of ideas immanent in nervous activity"_](https://scholar.google.com/scholar?q=A+Logical+Calculus+of+Ideas+Immanent+in+Nervous+Activity+author%3Amcculloch).

There are a few good reasons to think that the current wave of interest in ANNs will not face an [AI winter](https://en.wikipedia.org/wiki/AI_winter):

- Huge quantity of data available for training.

- Computing power has tremendously increase _(since the 1990's)_ &rarr; so we can train ANNs in a reasonable amount of time.

- Training algorithms have been improved.

- Many popular products are now based on ANNs which pulls more and more attention and funding toward them, resulting in more and more progress, and even more amazing products.

## The Perceptron

Invented in **1957** by **Frank Rosenblatt**,  and largely inspired by _Hebb's rule_, the **_Perceptron_** is one of the simplest ANN architectures.

> <u>Architecture type</u>: Threshold Logic Unit (TLU) or _Linear threshold unit (LTU)_.

**Inputs** are numbers. **Each <u>connection</u> has <u>weight</u>.** The TLU **neuron computes a <u>weighted sum of its inputs</u>** and then apply a <u>**step function**</u> to that sum. Its result is the **output** of the ANN.

Example:

> **TODO**: put a scheme.

>  The [_Heaviside step function_](https://en.wikipedia.org/wiki/Heaviside_step_function) and the [_sign function_](https://en.wikipedia.org/wiki/Sign_function) are the most commonly used step functions in Perceptrons.

The <u>_Perceptron_ is composed of **one single layer of TLUs**</u> with <u>each TLU connected to all inputs</u>.

**When all the neurons in a layer are connected to every neuron in the previous layer, it's called a _dense layer_**.

> Scikit-learn `Perceptron()` is equivalent to `SGDClassifier(loss="perceptron", eta0=1, learning_rate="constant", penalty=None)`.

### Multilayer Perceptron (MLP)

Whenn an ANN <u>contains multiple layers</u>, it is called a **D**eep **N**eural **N**etwork (DNN).

The _MLP_ is composed of **_input layer_**, **_hidden layers_** (one or more layers of TLUs) and the **_output layer_** which is the final layer.

> All MLP layers are _dense layers_.

> _**upper layers**_ are closed to the output layer and _**lower layers**_ are closed to the input.

> **_Feedforward neural net_ (FNN)** is a type of architecture where the signal flows only in one direction _(from inputs to outputs)_.

MLP can be used for both **regression** and **classification** tasks.

#### Backpropagation training algorithm

For each training instance the backpropagation algorithm:

1. Makes a prediction _(**forward pass**)_.
2. Measures the error.
3. Goes through each layer in reverse to measure the error contribution from each connection _(**reverse pass**)_.
4. Finally slightly tweaks the connection weights to reduce the error _(Gradient Descent step)_.

##### _Activation functions_

For the _output layer_:

- **ReLU** or _**softplus**_ activation function &rarr; guarantee tha the output will always be positive.
- **Logistic function** or **hyperbolic tangent** &rarr; guarantee that the prediction will within a given range of values.

##### _Loss functions_

> Typically use the **mean squared error** in training.

- **mean absolute error**: if there are a lot of outliers in the training set.
- **Huber loss**: combination of both.


<u>Typical architecture to perform regression with MLP</u>:

Hyper-parameter             | Value 
----------------------------|-------------------------------
Number of **inputs**        | One per feature
**input activation**        | ReLU
Number of **hidden layers** | 1-5 _(depends on the problem)_
**Neurons per hidden layer**| 10-100 _(depends on the problem)_
**hidden activation**       | None or ReLU/Softplus _(if positive outputs)_ or Logistic/Tanh _(if bounded outputs)_
Number of **outputs**       | One per prediction dimension
**output activation**       | Mean Squared Error or if many outliers Mean Absolute Error /Huber

___

- [AI cheet sheets](https://becominghuman.ai/cheat-sheets-for-ai-neural-networks-machine-learning-deep-learning-big-data-678c51b4b463)