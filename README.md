# Kohonen Self-Organizing Map (SOM)

This repository contains an implementation of a Kohonen Self-Organizing Map (SOM), also known as Kohonen Network, in Python. This neural network is used for unsupervised learning and clustering, particularly for visualizing high-dimensional data in lower dimensions.

## Features

- **Customizable Grid Topology**: Choose between rectangular and hexagonal grid topology for neurons.
- **Neighborhood Functions**: Supports Gaussian and Mexican Hat neighborhood functions.
- **Learning Rate and Neighborhood Decay**: Gradual decay of learning rate and neighborhood size over time.
- **BMU (Best Matching Unit)**: Efficient calculation of the best matching unit for input vectors.
- **Clustering**: Cluster data based on proximity in the SOM grid and visualize the results.

## How it Works

### 1. Initialization

The SOM grid is initialized as a 2D grid of neurons, each represented by a weight vector. The weights are randomly initialized:

$$
\text{weights} \sim U(0, 1)
$$

Where $$U(0, 1)$$ represents the uniform distribution.

### 2. Best Matching Unit (BMU)

For each input vector, the Best Matching Unit (BMU) is the neuron whose weight vector is closest to the input vector. This is determined using Euclidean distance:

$$
\text{BMU} = \underset{i,j}{\text{argmin}} || x - w_{i,j} ||
$$

Where:
- $$\(x\)$$ is the input vector
- $$\(w_{i,j}\)$$ is the weight vector of the neuron at position $$\((i,j)\)$$
- $$\(|| \cdot ||\)$$ denotes the Euclidean distance.

### 3. Neighborhood Function

The influence of the BMU on neighboring neurons is determined by a neighborhood function. Two functions are supported:

- **Gaussian**:
$$\text{neighborhood}(d, \sigma) = \exp\left( -\frac{d^2}{2\sigma^2} \right)$$

- **Mexican Hat**:
$$\text{neighborhood}(d, \sigma) = \left( 1 - \frac{d^2}{\sigma^2} \right) \exp\left( -\frac{d^2}{2\sigma^2} \right)$$

Where:
- $$\(d\)$$ is the distance from the BMU to the neighboring neuron
- $$\(\sigma\)$$ is the neighborhood radius, which decays over time.

### 4. Weight Update

The weights of the neurons are updated based on their proximity to the BMU. The closer a neuron is to the BMU, the more its weights are adjusted to be closer to the input vector:

$$w_{i,j} = w_{i,j} + \alpha \cdot \text{neighborhood}(d, \sigma) \cdot (x - w_{i,j})$$

Where:
- $$\(\alpha\)$$ is the learning rate, which also decays over time
- $$\(x\)$$ is the input vector.

### 5. Learning Rate and Neighborhood Decay

Both the learning rate and the neighborhood radius decay over time to allow the network to converge:

- Learning rate decay:
$$\alpha(t) = \alpha_0 \cdot \exp\left( -\frac{t}{\lambda} \right)$$

- Neighborhood decay:
$$\sigma(t) = \sigma_0 \cdot \text{decay factor}$$

Where:
- $$\(\alpha_0\)$$ is the initial learning rate
- $$\(\sigma_0\)$$ is the initial neighborhood radius
- $$\(\lambda\)$$ is a time constant.

### 6. Topology

The grid can have either a rectangular or hexagonal topology, which affects how the distances between neurons are calculated:
- **Rectangular**: Standard Euclidean distance.
- **Hexagonal**: Distance based on the hexagonal grid layout.

### 7. Clustering

The SOM clusters data points by mapping them to neurons in the grid. Points that are close in the input space will be mapped to neighboring neurons in the grid.

## Usage

1. **Initialize the SOM**: Define the grid size, input dimensions, and other parameters.
2. **Train the SOM**: Call the `train` method with your data, specifying the number of iterations and other optional parameters.
3. **Visualize Clusters**: Use the built-in methods to visualize the SOM grid and the clusters formed.
