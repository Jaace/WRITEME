# Machine learning algorithms

A collection of minimal and clean implementations of machine learning algorithms.

### Why?

This project is targeting people who want to learn internals of ml algorithms or implement them from scratch.   
The code is much easier to follow than the optimized libraries and easier to play with.   
All algorithms are implemented in Python, using numpy, scipy and autograd. 

### Implemented:

  * Deep learning (MLP, CNN, RNN, LSTM)
  * Linear regression, logistic regression
  * Random Forests
  * Support vector machine (SVM) with kernels (Linear, Poly, RBF)
  * K-Means
  * Gaussian Mixture Model
  * K-nearest neighbors
  * Naive bayes
  * Principal component analysis (PCA)
  * Factorization machines
  * Restricted Boltzmann machine (RBM)
  * t-Distributed Stochastic Neighbor Embedding (t-SNE)
  * Gradient Boosting trees (also known as GBDT, GBRT, GBM, XGBoost)
  * Reinforcement learning (Deep Q learning)



### Installation
    
    
        git clone https://github.com/rushter/MLAlgorithms
        cd MLAlgorithms
        pip install scipy numpy
        python setup.py develop
    

### How to run examples without installation
    
    
        cd MLAlgorithms
        python -m examples.linear_models
    

### How to run examples within Docker
    
    
        cd MLAlgorithms
        docker build -t mlalgorithms .
        docker run --rm -it mlalgorithms bash
        python -m examples.linear_models
    

### Contributing

Your contributions are always welcome!   
Feel free to improve existing code, documentation or implement new algorithm.   
Please open an issue to propose your changes if they are big enough. 
