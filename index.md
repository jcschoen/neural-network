## Project Scripts
One with a test:
```python
import numpy as np


def nonlin(x, deriv=False):
    if(deriv == True):
        return x * (1-x)

    return 1 / (1+np.exp(-x))

# input data
X = np.array([[0, 0, 1],
             [0, 1, 1],
             [1, 0, 1],
             [1, 1, 1]])

# output data
y = np.array([[0],
              [1],
              [1],
              [0]])

np.random.seed(1)

# synapses
syn0 = 2*np.random.random((3, 4)) - 1
syn1 = 2*np.random.random((4, 1)) - 1

# training step
for j in range(60000):
    l0 = X
    l1 = nonlin(np.dot(l0, syn0))
    l2 = nonlin(np.dot(l1, syn1))

    l2_error = y - l2

    if(j % 10000) == 0:
        print("Error Rate: " + str(np.mean(np.abs(l2_error))))

    l2_delta = l2_error * nonlin(l2, deriv=True)

    l1_error = l2_delta.dot(syn1.T)

    l1_delta = l1_error * nonlin(l1, deriv=True)

    # update synapse weights
    syn1 += l1.T.dot(l2_delta)
    syn0 += l0.T.dot(l1_delta)

print("Output after training")
print(l2)

n = np.array([[0, 1, 1],
             [0, 0, 1],
             [1, 1, 1],
             [1, 0, 1]])

j0 = n
j1 = nonlin(np.dot(j0, syn0))
j2 = nonlin(np.dot(j1, syn1))


print("Output after testing")
print(j2)
```
Default from a tutorial:
```python
import numpy as np


def nonlin(x, deriv=False):
    if(deriv == True):
        return x * (1-x)

    return 1 / (1+np.exp(-x))

# input data
X = np.array([[0, 0, 1],
             [0, 1, 1],
             [1, 0, 1],
             [1, 1, 1]])

# output data
y = np.array([[0],
              [1],
              [1],
              [0]])

np.random.seed(1)

# synapses
syn0 = 2*np.random.random((3, 4)) - 1
syn1 = 2*np.random.random((4, 1)) - 1

# training step
for j in range(60000):
    l0 = X
    l1 = nonlin(np.dot(l0, syn0))
    l2 = nonlin(np.dot(l1, syn1))

    l2_error = y - l2

    if(j % 10000) == 0:
        print("Error Rate: " + str(np.mean(np.abs(l2_error))))

    l2_delta = l2_error * nonlin(l2, deriv=True)

    l1_error = l2_delta.dot(syn1.T)

    l1_delta = l1_error * nonlin(l1, deriv=True)

    # update synapse weights
    syn1 += l1.T.dot(l2_delta)
    syn0 += l0.T.dot(l1_delta)

print("Output after training")
print(l2)
```
Highly compressed:
```python
import numpy as np


# sigmoid function
def nonlin(x, deriv=False):
    if (deriv == True):
        return x * (1 - x)
    return 1 / (1 + np.exp(-x))


# input dataset
X = np.array([[0, 0, 1],
              [1, 0, 0],
              [1, 1, 0],
              [1, 1, 1]])

# output dataset
y = np.array([[0, 1, 1, 0]]).T

# seed random numbers to make calculation
# deterministic (just a good practice)
np.random.seed(1)

# initialize weights randomly with mean 0
syn0 = 2 * np.random.random((3, 1)) - 1

for iter in range(60000):
    # forward propagation
    l0 = X
    l1 = nonlin(np.dot(l0, syn0))

    # how much did we miss?
    l1_error = y - l1
    if (iter % 10000) == 0:
        print("Error Rate: " + str(np.mean(np.abs(l1_error))))
    # multiply how much we missed by the
    # slope of the sigmoid at the values in l1
    l1_delta = l1_error * nonlin(l1, True)

    # update weights
    syn0 += np.dot(l0.T, l1_delta)

print("Output After Training: ")
print(l1)
```
[Link](https://github.com/jcschoen/neural-network)