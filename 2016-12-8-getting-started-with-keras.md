---
layout: post
title: "Getting Started With Keras"
comments: false
description: ""
category: code
keywords: keras neural-networks machine-learning
---

Keras is a high level framework for building neural networks on top of `Tensorflow` or `Theano`.
Knowing how to use Keras (or a alternative to it) will speed up your prototyping process when working with Neural Networks.
It's also a good place for beginners to start, as it doesn't require much code to get a neural network running.
In Keras, your first neural network can be as simple as this:

    from keras.models import Sequential
    from keras.layers import Dense
    
    model = Sequential()                                      # (1)
    input = Dense(64, input_shape=(52,), activation="relu")   # (2)
    model.add(input)                                          # (3)
    hidden = Dense(7, activation='softmax')                   # (4)
    model.add(hidden)
    model.compile(optimizer='rmsprop', 
      loss='categorical_crossentropy', metrics=['accuracy'])  # (5)

This creates a neural network for classification with 290 inputs, a hidden layer of size 64, and an output layer of size 3.
(1) Create a sequential NN. Each layer will feed into the next in a sequence.
An alternative to a sequential network is a [Multi-input multi-output](https://keras.io/getting-started/functional-api-guide/#multi-input-and-multi-output-models).

(2) Create a layer with 290 inputs and 64 outputs. The number of outputs are the only required positional argument for a `Dense` layer.
You need to explicitly set your input size for the first layer in your network. Your input size is the number of features you are using.
[Activation](https://keras.io/activations/) describes how you want your nodes to activate.
You can use `tanh`, `softmax`, `linear` and many other options.
Activations are out of the scope of this tutorial, but you can learn more [here](http://cs231n.github.io/neural-networks-1/#modeling-one-neuron).

(3) Add a layer to your model. The order with which you call add describes the sequence of layers in your model.

(4) Create our hidden layer.
The number of inputs to this layer is implied by the number of outputs by the previous layer, so we only need to set the number of outputs we want.
In this case, we want 7, because we have 7 output classes.
We're using [softmax](http://cs231n.github.io/linear-classify/#softmax) as our activation because it converts our output values into a probability distribution.

(5) Compile our model. This is where we can set our optimizer, loss function, callbacks, number of epochs to run and many other useful things.

### To use our model:

    model.fit(x_train, y_train)
    predictions = model.predict(x_test)
    
### Prerequisites
This guide assumes you've used `Pandas`, `Numpy`, and `Jupyter` before. If you haven't, it might help you to check out [The python machine learning stack]({{site.url}}/articles/2016-11/machine-learning-toolkit-installation)

### Installing Keras

    pip install tensorflow keras

### Let's Build a neural net around an example problem

It's worth noting, for many tasks,  you'll likely get good results more quickly with a simpler model.  For example, if you just drop a `RandomForestClassifier` or `XGBoost` onto your problem. Also, the specifics of building Convolutional Neural Networks is out of the scope of this post.

### Fetch forest covertype dataset
It has 54 numerical features and 7 output classes. This is a great example classification dataset.

    import pandas as pd
    from sklearn.datasets import fetch_covtype
    forest_covertypes = fetch_covtype()
    target = pd.DataFrame(forest_covertypes.target)
    features = pd.DataFrame(forest_covertypes.data)
    
    n_features = features.shape[1]
    n_classes = len(target[0].value_counts())
    
### Center and scale features

    centered = features - features.mean()
    scaled_centered = centered / centered.std()
    
### Put some data aside for final testing

    X_train, X_test, y_train, y_test = train_test_split(scaled_centered, target, test_size=0.2, random_state=0)
    
### Create our model

    from keras.models import Sequential
    from keras.layers import Dense, Dropout
    from keras.optimizers import Adam
    
    model = Sequential()
    model.add(Dense(64, input_shape=(n_features,), activation="relu"))
    model.add(Dropout(.5))
    model.add(Dense(32, activation="relu"))
    model.add(Dropout(.5))
    model.add(Dense(n_classes, activation='softmax'))
        
    adam = Adam(lr=0.001, decay=0.00001)
    model.compile(optimizer=adam, loss='categorical_crossentropy',metrics=['accuracy'])
    
    model.fit(X_train.as_matrix(), pd.get_dummies(y_train[0]).as_matrix(), validation_split=0.2, nb_epoch=20, batch_size=32)
    
### Test it on output we haven't seen before

    from sklearn.metrics import accuracy_score
    predicted = model.predict_classes(X_test.as_matrix())
    accuracy_score(predicted, y_test[0])

OK, I'm going to start by plopping down the full code, then we can dig into the interesting bits.
    
### Layer types
Dense: fully connected NN layer  
Activation - OK so you can create a whole layer object for activations and pass it into your model, but it seems like extra code. Just pass the activation you want as an argument.

### Important considerations for getting good results:
With a framework like Keras, it's easy to quickly throw a model together.
However, it can still be a challenge to get good results using a neural network.
For beginners, it can be really hard to know how to improve your results after you have an initial model running.

### scale feature values
We already did this in our example:

    centered = features - features.mean()
    scaled_centered = centered / centered.std()

If you forget to do this, certain features can overpower others in your network and gradient descent will improve slowly to the degree that it feels like it isn't improving at all! 

### recommended optimizers: sgd w/ momentum or adam

To use adam:

    from keras.optimizers import Adam
    adam = Adam(lr=0.0001)
    ...
    model.compile(optimizer=adam, loss='categorical_crossentropy', metrics=['accuracy'])
    
### playing with learning rate:
![Learning Rates](http://cs231n.github.io/assets/nn3/learningrates.jpeg "Image lifted from cs231")
Image lifted from [cs231](http://cs231n.github.io/neural-networks-3/). This is a great resource to learn about neural networks.

If your learning rate is too low, your model will take too long to converge. If it is too high, your results will bounce around and likely never converge. You want to get it just right. People will add a decay to their learning rate, so their model improves quickly in the beginning and converges at the end. In Keras, you  set your learning rate when you create your optimizer. It's the `lr` parameter. You can further adjust it using the `decay` param.

### Preventing overfitting: Dropout and regularization

You add dropout as its own layer into your network.
dropout:

    model.add(Dropout(.5))

You add regularization as a parameter to a layer. `W_regularizer` is regularization on the weights in the layer. `b_regularizer` is regularization on the bias in that layer.

l2 regularization:

    model.add(Dense(32,  activation="relu", W_regularizer=WeightRegularizer(l2=0.01), b_regularizer=WeightRegularizer(l2=0.01)))

### References
[Keras Docs](https://keras.io/)
[Convolutional Neural Networks for Visual Recognition Course](http://cs231n.github.io/neural-networks-1/)
[Efficient Object Localization Using Convolutional Networks](https://arxiv.org/pdf/1411.4280.pdf)

### Siblings
It's worth noting that Keras isn't the only game in town. There are many frameworks designed for rapidly prototyping neural networks. Some other notable ones include:
[TFLearn](http://tflearn.org/)
[Neon](https://github.com/NervanaSystems/neon)
[NoLearn](https://github.com/dnouri/nolearn)
[Lasagne](https://github.com/Lasagne/Lasagne)