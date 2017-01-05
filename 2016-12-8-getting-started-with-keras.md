---
layout: post
title: "Getting Started With Keras"
comments: false
description: ""
keywords: ""
---

## Let's Build a neural net around an easy problem

It's worth noting for many tasks (including the one we'll be using as an example) you'll likely get good results much more quickly (and possibly better results overall) if you just drop a RandomForestClassifier or XGBoost onto your problem. Also, this guide isn't going to dig into convolutional NN.

## Prerequisites
This guide assumes you've used Pandas, Numpy, and jupyter before Jupyter. If you haven't, it might help you to check out [The python machine learning stack]

## installation

    pip install tensorflow keras
    
OK, I'm going to start by plopping down the full code, then we can dig into the interesting bits.

## Usage

    from keras.models import Sequential
    from keras.layers import Dense, Dropout
    from keras.optimizers import Adam
    
    kc = Sequential()  # Create a sequential NN
    kc.add(Dense(64, input_shape=(290,), activation="relu"))  #
    kc.add(Dropout(.5))
    kc.add(Dense(32, activation="relu"))
    kc.add(Dropout(.5))
    kc.add(Dense(3, activation='softmax'))
    
    adam = Adam(lr=0.0001, decay=0.000001)
    kc.compile(optimizer=adam, loss='categorical_crossentropy',metrics=['accuracy'])

    # kc = KerasClassifier(keras_model)
    # kc = keras_model()
    kc.fit(X_train.as_matrix(), pd.get_dummies(y_train).as_matrix(), validation_split=0.2, nb_epoch=200, batch_size=32)

    predicted = kc.predict_classes(X_test.as_matrix())
    
## Layer types
- Dense: fully connected NN layer
- Activation - OK so you can create a whole layer object for activations and pass it into your model, but it seems like extra code. Just pass the activation you want as an argument.

## Actually getting things working:

### recommended optimizers: sgd w/ momentum or adam
To use adam:

    from keras.optimizers import Adam
    adam = Adam(lr=0.0001)
    ...
    model.compile(optimizer=adam, loss='categorical_crossentropy', metrics=['accuracy'])
    
### playing with learning rate:
![alt text](http://cs231n.github.io/assets/nn3/learningrates.jpeg "Image lifted from")
Image lifted from [cs231](http://cs231n.github.io/neural-networks-3/). This is a great resource to learn about neural networks.

You can set your learning rate when you create your optimizer. It's the lr parameter. You can further adjust it using the decay constructor param on your optimizer.

### scale feature values

    centered = cleaned_train[cols] - cleaned_train[cols].mean() # Subtract the mean from all values
    centered = centered / centered.std()  # divide by the standard deviation of all columns
    
Dropout / regularization

## Reference
[convolutional neural networks for visual recognition](http://cs231n.github.io/neural-networks-1/)
[Efficient Object Localization Using Convolutional Networks](https://arxiv.org/pdf/1411.4280.pdf)
