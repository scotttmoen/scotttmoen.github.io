---
title: "A generalizable tensorflow template with TensorBoard integration and inline image viewing."
date: 2019-12-12
tags: [AI, machine learning, tensorflow, python]
categories: [code, machine learning]
header:
  image: "images/header_image2.png"
excerpt: "AI template with TensorBoard and image viewing."
---
<img src="{{ site.url }}{{site.baseurl }}/images/tensorflow.png" alt=" TensorFlowLogo" width="50"/>
For this expansion of the generalizable template I'm going to add a function to view images and labels. This is useful for inspecting the data prior to fitting and also assessing the results of your model.

```python
import numpy as np
import tensorflow as tf
import matplotlib.pyplot as plt
import datetime
print(tf.__version__)
```

    2.0.0



```python
from numpy.random import seed
seed(1)
tf.random.set_seed(1234)
```


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

Here's the class names which will be useful for labeling our images.


```python
class_names=['T-shirt/top','Trouser','Pullover','Dress','Coat','Sandal','Shirt','Sneaker','Bag','Ankle_boot']
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

    ({'epochs': 0, 'activation': 'swish', 'accuracy': 0.82645}, {'epochs': 4, 'activation': 'swish', 'accuracy': 0.8958167}, {'epochs': 9, 'activation': 'swish', 'accuracy': 0.91635}, {'epochs': 0, 'activation': 'relu', 'accuracy': 0.82301664}, {'epochs': 4, 'activation': 'relu', 'accuracy': 0.891}, {'epochs': 9, 'activation': 'relu', 'accuracy': 0.90956664})

```python
!kill 9053
```

    /bin/sh: 1: kill: No such process
    



```python
%tensorboard --logdir logs/fit
```


    Reusing TensorBoard on port 6006 (pid 12841), started 0:16:53 ago. (Use '!kill 12841' to kill it.)




<iframe
    width="100%"
    height="800"
    src="http://localhost:6006"
    frameborder="0"
    allowfullscreen
></iframe>




```python
predictions = model.predict(test_images)
print(predictions)
```

    [[5.7263919e-06 6.6372614e-09 4.0063137e-06 ... 4.1030091e-03
      1.9248000e-06 9.9537212e-01]
     [2.2955917e-05 1.4473169e-11 9.9884146e-01 ... 4.7463745e-15
      4.1615325e-10 9.1840791e-17]
     [1.0521556e-10 1.0000000e+00 2.2429164e-12 ... 2.3259870e-27
      7.9439719e-17 1.8272301e-17]
     ...
     [2.6303707e-03 1.5326654e-10 1.7281596e-03 ... 6.4461265e-06
      9.9467367e-01 7.2260475e-10]
     [7.9042373e-08 9.9999738e-01 9.4543378e-09 ... 8.7363508e-16
      6.1607669e-13 7.4512601e-09]
     [1.9130493e-05 1.4222825e-07 2.7901824e-06 ... 1.4867900e-03
      4.7969297e-04 5.3793042e-06]]


This function prepares and displays whichever images and labels you assign as arguments. Its a good way of assessing your data prior, and after, classification.


```python
def show_images(images, labels):
        plt.figure(figsize=(10,10))
        grid_size = min(25, len(images))
        for i in range(grid_size):
                plt.subplot(5, 5, i+1)
                plt.yticks([])
                plt.xticks([])
                plt.imshow(images[i], cmap=plt.cm.copper)
                plt.xlabel(class_names[labels[i]],size=10,color='orange')

```

Here we use our prediction values and test images for the show_images function.


```python
show_images(test_images, np.argmax(predictions, axis = 1))
plt.show()
```


![png](output_22_0.png)

