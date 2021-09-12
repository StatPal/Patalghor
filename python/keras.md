# Keras and co.

Keras is a machine learning framework which is less customizable but a beginner-friendly machine learning framework for python.
Keras has main three kind of API:
 - Sequential (easiest, can be used for architectures without any branches or complicacies, just consisting of liearly progressing layers)

In the code of keras, there are three main parts, **Defining the model, Compiling the model, and Fitting the model.** 
- We have to first define the architecture of the model, i.e., which node is connected to which one, with what activation function etc, are they sequentially added, etc.
- Then a model is compiled in the sense that `optimizer` and `loss` is added to the model structure.
- Then fit the model with the training data, with specified `epochs` and `batch_size` etc.
Usually, these kind of models does not train the model with the whole training data at once. 
It takes a small *batch*/part of the data and tweak the neural net weight's among the nodes in such a way that 
the prediction of the neural net gets more inclined to the new set of data... And then do the same thing again for a randomlys selected different set of data.
This small chunk size is the `batch_size.`


For all examples, we would strongly recommand to first create a virtual environment and work under that environment.

## Sequential API:
We would like to start with simplest version of a multi-layer perceptron
This version would load a data named `pima` database and create a multi-layer-perceptron model which would predict whether a person has diabetes or not.
It can be thought as an extension of logistic regression.



```python
## https://machinelearningmastery.com/tutorial-first-neural-network-python-keras/

# first neural network with keras
from numpy import loadtxt			## To load the text data
from keras.models import Sequential		## Sequential keras model
from keras.layers import Dense			## Dense layers to be added in the model
from keras.utils import plot_model		## As we want to plot and see the model architecture.


# load the dataset
dataset = loadtxt('data/pima-indians-diabetes.csv', delimiter=',')

# split into input (X) and output (y) variables
X = dataset[:,0:8]
y = dataset[:,8]




######################## KERAS MODEL ###################################
# DEFINE the keras model:
model = Sequential()
model.add(Dense(12, input_dim=8, activation='relu'))		## First layer after the input - dense layer with 12 nodes in this layer
model.add(Dense(8, activation='relu'))				## Next layer - dense layer with 8 nodes and activation fn 'relu'
model.add(Dense(1, activation='sigmoid'))			## Next (last) layer - dense layer with one node (for 0/1 output) , act. fn.: sigmoid

# COMPILE the keras model
model.compile(loss='binary_crossentropy', optimizer='adam', metrics=['accuracy'])

# FIT the keras model on the dataset
model.fit(X, y, epochs=150, batch_size=10, verbose=1)


# summarize layers
print(model.summary())

# plot graph
plot_model(model, to_file='multilayer_perceptron_graph_sequencial.png')


#################### PREDICT USING THE FITTED KERAS MODEL ###################################
# make class predictions with the model
predictions = model.predict_classes(X)
# summarize the first 5 cases
for i in range(5):
	print('%s => %d (expected %d)' % (X[i].tolist(), predictions[i], y[i]))


## See also  https://web.stanford.edu/class/cs20si/lectures/march9guestlecture.pdf
```




## Functional API:

### Example 1 (Simple multilayer perceptron)
Again, we would like to start with simple version of a multi-layer perceptron.
However, this API can do much more than the sequential one,
especially when you need branching connections to two or more nodes/joining from more than one nodes, connection to some previous layers etc. 




```python
# https://machinelearningmastery.com/keras-functional-api-deep-learning/

# neural network with keras
from numpy import loadtxt				## To load the text data
from keras.models import Model			## (Functional) keras model
from keras.layers import Input			## Input layer
from keras.layers import Dense			## Dense layers to be added in the model
from keras.utils import plot_model		## As we want to plot and see the model architecture.


# load the dataset
dataset = loadtxt('data/pima-indians-diabetes.csv', delimiter=',')

# split into input (X) and output (y) variables
X = dataset[:,0:8]
y = dataset[:,8]



################## KERAS MODEL (Functional API) ###########################
# DEFINE the keras model:
visible = Input(shape=(8,))
hidden1 = Dense(12, activation='relu')(visible)
hidden2 = Dense(8, activation='relu')(hidden1)
output = Dense(1, activation='sigmoid')(hidden3)
model = Model(inputs=visible, outputs=output)

# COMPILE the keras model
model.compile(loss='binary_crossentropy', optimizer='adam', metrics=['accuracy'])

# FIT the keras model on the dataset
model.fit(X, y, epochs=150, batch_size=10, verbose=1)


# summarize layers
print(model.summary())

# plot graph
plot_model(model, to_file='multilayer_perceptron_graph_functional.png')


#################### PREDICT USING THE FITTED KERAS MODEL ###################################
# make class predictions with the model
predictions = model.predict_classes(X)
# summarize the first 5 cases
for i in range(5):
	print('%s => %d (expected %d)' % (X[i].tolist(), predictions[i], y[i]))


```
So, one can see that, the same model can be written with a little more number of steps. But we would see that this is much more flexible.




##### details (SKIP at first reading):
If you are new-ish to Python, the syntax used in the functional API may be confusing.
It might seem that these are basically function of functions. Let's look at a depth.

Short notation previously used is:
```python
dense1 = Dense(32)(input)
```

The first bracket `(32)` creates the layer via the class constructor, the second bracket `(input)` is a function with no name implemented via the __call__() function, that when called will connect the layers.
```python
# create layer
dense1 = Dense(32)
# connect layer to previous layer
dense1(input)
```

or, more elaborately(?), 
```python
# create layer
dense1 = Dense(32)
# connect layer to previous layer
dense1.__call_(input)
```




### Example 2: (Convolutional Neural Network, with the model defined as a function)

Here, we would work with a (2D) image data with convolutional neural network layers instead of simply connected dense layers. 
Convolutional Layers makes use of the neighbouring pixels, take an window (corresponding to `kernel_size`) and convolutes(?) them to form the next layer element.
Usually, after the convolutional layer, Pooling is used (MaxPooling/AvgPooling). Batchnorm is also common to train the models possibly faster.



```python

# Convolutional Neural Network
from keras.models import Model
from keras.layers import Input
from keras.layers import Dense
from keras.layers import Flatten
from keras.layers.convolutional import Conv2D
from keras.layers.pooling import MaxPooling2D

from keras.datasets import mnist				## Required dataset load
from numpy import expand_dims
import numpy
from keras.utils import plot_model				## As we want to plot and see the model architecture.


def build_discriminator(img_shape):
	# visible = Input(shape=(28,28,1))
	visible = Input(img_shape)
	conv1 = Conv2D(32, kernel_size=4, activation='relu')(visible)
	pool1 = MaxPooling2D(pool_size=(2, 2))(conv1)

	conv2 = Conv2D(16, kernel_size=4, activation='relu')(pool1)
	pool2 = MaxPooling2D(pool_size=(2, 2))(conv2)
	
	flat = Flatten()(pool2)

	hidden1 = Dense(10, activation='relu')(flat)
	output = Dense(1, activation='sigmoid')(hidden1)
	model = Model(inputs=visible, outputs=output)

	# summarize layers
	print(model.summary())
	# plot graph
	plot_model(model, to_file='convolutional_neural_network.pdf', show_shapes=True, show_layer_names=True)
	return model



## The model receives black and white 64×64 images as input, 
# then has a sequence of two convolutional and pooling layers as feature extractors, 
# followed by a fully connected layer to interpret the features and 
# an output layer with a sigmoid activation for two-class predictions.



# Load the MNIST dataset
(X_train, y_train), (_, _) = mnist.load_data()

# Rescale [0, 255] grayscale pixel values to [-1, 1]
X_train = X_train / 127.5 - 1.
X_train = expand_dims(X_train, axis=3)

img_shape = (28, 28, 1)
our_model = build_discriminator(img_shape)
our_model.compile(loss='binary_crossentropy', optimizer='adam', metrics=['accuracy'])
our_model.fit(X_train, y_train, epochs=3, batch_size=2, verbose=1)


```




