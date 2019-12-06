---
title: "Basic AI Model that I'm going to use for developing a hyperparameter tuning template."
date: 2019-12-03
tags: [AI, machine learning, tensorflow, python]
categories: [code, machine learning]
header:
  image: "images/header_image2.png"
excerpt: "Code to develop optimization template."
---
<img src="{{ site.url }}{{site.baseurl }}/images/tensorflow.png" alt=" TensorFlowLogo" width="50"/>


This is going to be the basic model that I'm going to work with for the next few days. I want to setup a template for automating hyperparameters.


```python
import tensorflow as tf
print(tf.__version__)
```

    2.0.0



```python
from numpy.random import seed
seed(1)
tf.random.set_seed(1234)

```


```python
mnist = tf.keras.datasets.fashion_mnist 
```


```python
(training_images, training_labels), (test_images, test_labels) = mnist.load_data() 
```


```python
training_images  = training_images / 255.0
test_images = test_images / 255.0
```


```python
model = tf.keras.models.Sequential([tf.keras.layers.Flatten(),  
                                    tf.keras.layers.Dense(128, activation=tf.nn.relu),  
                                    tf.keras.layers.Dense(10, activation=tf.nn.softmax)]) 
```


```python
model.compile(optimizer = 'adam', 
              loss = 'sparse_categorical_crossentropy', 
              metrics=['accuracy']) 
```


```python
model.fit(training_images, training_labels, epochs=5)
```

    Train on 60000 samples
    Epoch 1/5
    WARNING:tensorflow:Entity <function Function._initialize_uninitialized_variables.<locals>.initialize_variables at 0x7fdc1c516b70> could not be transformed and will be executed as-is. Please report this to the AutoGraph team. When filing the bug, set the verbosity to 10 (on Linux, `export AUTOGRAPH_VERBOSITY=10`) and attach the full output. Cause: module 'gast' has no attribute 'Num'
    WARNING: Entity <function Function._initialize_uninitialized_variables.<locals>.initialize_variables at 0x7fdc1c516b70> could not be transformed and will be executed as-is. Please report this to the AutoGraph team. When filing the bug, set the verbosity to 10 (on Linux, `export AUTOGRAPH_VERBOSITY=10`) and attach the full output. Cause: module 'gast' has no attribute 'Num'
    60000/60000 [==============================] - 3s 52us/sample - loss: 0.5063 - accuracy: 0.8210
    Epoch 2/5
    60000/60000 [==============================] - 3s 47us/sample - loss: 0.3784 - accuracy: 0.8648
    Epoch 3/5
    60000/60000 [==============================] - 3s 47us/sample - loss: 0.3404 - accuracy: 0.8765
    Epoch 4/5
    60000/60000 [==============================] - 3s 47us/sample - loss: 0.3149 - accuracy: 0.8848
    Epoch 5/5
    60000/60000 [==============================] - 3s 49us/sample - loss: 0.2949 - accuracy: 0.8916





    <tensorflow.python.keras.callbacks.History at 0x7fdc1c4ca240>




```python
model.evaluate(test_images, 
               test_labels,
               verbose=0) 
```




    [0.3478685932993889, 0.8738]




```python

```

[Download](https://github.com/scotttmoen/code)
