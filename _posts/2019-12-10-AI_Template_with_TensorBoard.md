---
title: "A generalizable tensorflow template with TensorBoard integration."
date: 2019-12-10
tags: [AI, machine learning, tensorflow, python]
categories: [code, machine learning]
header:
  image: "images/header_image2.png"
excerpt: "AI template with TensorBoard."
---
<img src="{{ site.url }}{{site.baseurl }}/images/tensorflow.png" alt=" TensorFlowLogo" width="50"/>
#In the previous exercise, I created a template for automating hyperparameters. For this exercise I would like to expand on some tools to understand our analysis.


```python
import tensorflow as tf
import datetime
print(tf.__version__)
```
    2.0.0

```python
from numpy.random import seed
seed(1)
tf.random.set_seed(1234)
```

#First thing we have to do use some so-called "magic" to activate TensorFlow's visualization tool, TensorBoard. This is done be the prefacing the command with "%". We also need to clear the logs so we don't encounter any run-to-run confusion.


```python
%load_ext tensorboard
!rm -rf ./logs/ 
```


```python
from tensorflow.keras.activations import sigmoid
def swish(x, beta = 1):
    return (x * sigmoid(beta * x))
```


```python
from tensorflow.keras.utils import get_custom_objects
from tensorflow.keras.layers import Activation
get_custom_objects().update({'swish': Activation(swish)})
```


```python
from tensorflow.keras.callbacks import Callback
class my_callbacks(Callback):
    def on_train_begin(self, logs=None):
        self.beginning_accuracy = 0.0

    def on_epoch_begin(self, epoch, logs={}):
        current_accuracy=self.beginning_accuracy

    def on_epoch_end(self,epoch,logs={}):
        current_accuracy=logs.get('accuracy')
        if current_accuracy>self.beginning_accuracy:
            test.update({'accuracy':current_accuracy,'epochs':epoch})
        self.beginning_accuracy = current_accuracy
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
test_parameters = ({'epochs':1, 'activation':'swish'},
                    {'epochs':5, 'activation':'swish'},
                    {'epochs':10, 'activation':'swish'},
                    {'epochs':1, 'activation':'relu'},
                    {'epochs':5, 'activation':'relu'},
                    {'epochs':10, 'activation':'relu'})

```

#In the following code we've started a log directory that saves our results with time/date. Also, we've created a tensorflow callback to connect the data and added this callback to our existing one.


```python
for test in test_parameters:
    model = tf.keras.models.Sequential([tf.keras.layers.Flatten(),
                                        tf.keras.layers.Dense(128, activation=test["activation"]),
                                        tf.keras.layers.Dense(10, activation=tf.nn.softmax)])
    model.compile(optimizer = 'adam',
                  loss = 'sparse_categorical_crossentropy',
                  metrics=['accuracy'],
                  verbose=0)
    
    log_dir="logs/fit/" + datetime.datetime.now().strftime("%Y%m%d-%H%M%S")
    
    tensorboard_callback = tf.keras.callbacks.TensorBoard(log_dir=log_dir, histogram_freq=1)


    model.fit(training_images,
              training_labels,
              epochs=test["epochs"],
              verbose=0,
              callbacks=[my_callbacks(),tensorboard_callback])

    model.evaluate(test_images,
                   test_labels,
                   verbose=0)
print(test_parameters)

```

    ({'epochs': 0, 'activation': 'swish', 'accuracy': 0.82645}, {'epochs': 4, 'activation': 'swish', 'accuracy': 0.8958167}, {'epochs': 9, 'activation': 'swish', 'accuracy': 0.9163833}, {'epochs': 0, 'activation': 'relu', 'accuracy': 0.82301664}, {'epochs': 4, 'activation': 'relu', 'accuracy': 0.891}, {'epochs': 9, 'activation': 'relu', 'accuracy': 0.9091833})


#And now for the magic...the following command opens tensorboard and tells it to use the log data we created.


```python
!kill 5500
```


```python
%tensorboard --logdir logs/fit
```



<iframe
    width="100%"
    height="800"
    src="http://localhost:6006"
    frameborder="0"
    allowfullscreen
></iframe>




```python

```
