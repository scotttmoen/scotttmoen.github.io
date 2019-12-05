#Using the previous basic model, I'm going to setup a template for automating hyperparameters. In this instance the complexity of the model isn't as important as developing the template.


```python
import tensorflow as tf
print(tf.__version__)
```


```python
from numpy.random import seed
seed(1)
tf.random.set_seed(1234)
```

##One of the incorporations will be a new activation function we need to create.


```python
from tensorflow.keras.activations import sigmoid
def swish(x, beta = 1):
    return (x * sigmoid(beta * x))
```

##We then add the newly created activation to the existing activation selections.


```python
from tensorflow.keras.utils import get_custom_objects
from tensorflow.keras.layers import Activation
get_custom_objects().update({'swish': Activation(swish)})
```

##Next in order to see the results of our hyperparameter selections, we'd like to see the accuracy of each epoch. Myy solution is to create a callback that only returns the epoch, and corresponding parameters, of the highest accuracy epoch.


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
            print('Newer accuracy is better than old')
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

##Here I've created a dictionary for items I want to include for testing: epochs,activation. I like using the dictionary because I think it helps readibility.


```python
test_parameters = ({'epochs':1, 'activation':'swish'},
                    {'epochs':5, 'activation':'swish'},
                    {'epochs':10, 'activation':'swish'},
                    {'epochs':1, 'activation':'relu'},
                    {'epochs':5, 'activation':'relu'},
                    {'epochs':10, 'activation':'relu'})

```

##Then we loop through our model, each time introducing our current dictionary values and appending the results to our "test parameters" dictionary for later analysis.


```python
for test in test_parameters:
    model = tf.keras.models.Sequential([tf.keras.layers.Flatten(),
                                        tf.keras.layers.Dense(128, activation=test["activation"]),
                                        tf.keras.layers.Dense(10, activation=tf.nn.softmax)])
    model.compile(optimizer = 'adam',
                  loss = 'sparse_categorical_crossentropy',
                  metrics=['accuracy'],
                  verbose=0)

    model.fit(training_images,
              training_labels,
              epochs=test["epochs"],
              callbacks=[my_callbacks()])

    model.evaluate(test_images,
                   test_labels,
                   verbose=0)
print(test_parameters)

```

({'epochs': 0, 'activation': 'swish', 'accuracy': 0.82645}, {'epochs': 4, 'activation': 'swish', 'accuracy': 0.8958167}, {'epochs': 9, 'activation': 'swish', 'accuracy': 0.9163667}, {'epochs': 0, 'activation': 'relu', 'accuracy': 0.82301664}, {'epochs': 4, 'activation': 'relu', 'accuracy': 0.891}, {'epochs': 9, 'activation': 'relu', 'accuracy': 0.90995})

###Note:Epochs are indices, hence 0,4,9 vs 1,5,10
