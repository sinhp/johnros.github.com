---
layout: post
title: "The Building Blocks of Machine Learning"
description: ""
category: 
tags:  [machine-learning]
---

One can easily be confused by the sea of methods and terms in machine learning.
I find the endless terminology confusing and counter productive. One might have a perfect understanding of a method "A", but is unaware that the new state of the art algorithm, "B++", is merely a small twist to his familiar "A". I have spent hours trying to disambiguate terms just to realize that a simple idea was occluded by terminology.

In this post, I try to collect the fundamental ideas underlying machine-learning. Most algorithms out there can be shown to be some compounding of these ideas:

$$
\newcommand{\loss}{l} % A loss function
\newcommand{\risk}{R} % The risk function
\newcommand{\riskn}{\mathbb{R}} % The empirical risk
\newcommand{\argmin}[2]{\mathop{argmin} _{#1}\set{#2}} % The argmin operator
\newcommand{\set}[1]{\left\{ #1 \right\}} % A set
\newcommand{\rv}[1]{\mathbf{#1}} % A random variable
\newcommand{\x}{\rv x} % The random variable x 
\newcommand{\y}{\rv y} % The random variable x 
\newcommand{\X}{\rv X} % The random variable x 
\newcommand{\Y}{\rv Y} % The random variable y
\newcommand{\X}{\rv z} % The random variable x 
\newcommand{\Y}{\rv Z} % The random variable y
\newcommand{\estim}[1]{\widehat{#1}} % An estimator
$$


## Risk Minimization

This is possibly the most fundamental concept in machine learning. 
The idea stems from (statistical) decision theory, and consists of defining what is the __loss__ incurred by making an error, $$\loss(Z;\theta)$$. 
Once defined, one would naturally seek to make predictions, $$\theta^*$$, that are accurate, i.e., minimize the average loss known as the __risk__: $$\risk(\theta):= \int \loss(Z;\theta) dZ$$, and $$\theta^*:=\argmin{\theta}{\risk(\theta)}$$.

As the whole population is typically inaccessible, we cannot compute the risk. Instead, we have access to a sample, and so we instead minimize the __empirical risk__ $$\riskn(\theta):= \frac  1n \sum \loss(Z_i;\theta)$$, and $$\estim{\theta^*}:= \argmin{\theta}{\riskn(\theta)}$$.

The vast majority of supervised and unsupervised learning algorithms are merely empirical risk minimizers (ERM).
Some examples include [Ordinary Least Squares](https://en.wikipedia.org/wiki/Ordinary_least_squares), [Maximum Likelihood estimation](https://en.wikipedia.org/wiki/Maximum_likelihood), [PCA](https://en.wikipedia.org/wiki/Principal_component_analysis).

Typical examples of algorithms that cannot be cast as pure ERM problems are __non-parametric__ methods like [k-nearest neighbour](https://en.wikipedia.org/wiki/K-nearest_neighbors_algorithm) and [kernel methods](https://en.wikipedia.org/wiki/Kernel_method). This is because  optimization in an infinite dimension is typically impossible (with exceptions, see the [kernel trick](#the-kernel-trick)).



## Inductive Bias
Inductive bias is the idea that without constraining the class of prediction function ("hypothesis" in the learning literature), there is non point in learning. Indeed, our predictions and approximation will be overfitted to the sample, and perform poorly on new data.

Inductive bias can be introduced in several ways:

- By restricting the functional form of your predictors (the "hypothesis class").
- By preferring simple solutions, i.e., adding regularization.

__Ridge regression__ demonstrates these two forms of inductive bias: we constrain the predictor to be linear in the features, and also add $$l_2$$ regularization. 


## Risk Estimation

Having defined a __loss__ function, we obviously seek for predictors and estimates that minimize the risk.
We may think that choosing the model with the smallest empirical risk, $$\riskn{\theta)}$$, may be a good way to compare and choose models. This, however, is a poor strategy that will certainly lead to __overfitting__. 
This is because $$\riskn(\estim{\theta^*})$$ is a biased estimate of $$\risk(\estim{\theta^*})$$. The "richer" the hypothesis class, the larger the bias, leading us to prefer more complicated models. 

To choose the best model, we would like some unbiased estimate of $$\risk(\estim{\theta^*})$$. 
Notable methods that aim at estimating the risk of the selected model, a.k.a. the __generalization error__, or __test error__ are:

- [Train, validate, test](https://en.wikipedia.org/wiki/Test_set) samples.
- [Cross Validation](https://en.wikipedia.org/wiki/Cross-validation_(statistics)).
- [Jackkinifing](https://en.wikipedia.org/wiki/Jackknife_resampling).
- [Mallow's Cp](https://en.wikipedia.org/wiki/Mallows's_Cp), [AIC](https://en.wikipedia.org/wiki/Akaike_information_criterion), [BIC](https://en.wikipedia.org/wiki/Bayesian_information_criterion), [MDL](https://en.wikipedia.org/wiki/Minimum_description_length), [SRM](https://en.wikipedia.org/wiki/Structural_risk_minimization), [DIC](https://en.wikipedia.org/wiki/Deviance_information_criterion), [HQC](https://en.wikipedia.org/wiki/Hannan%E2%80%93Quinn_information_criterion), and others.


## Dimensionality Reduction

The fact that a scientists/machine collected a set of $$p$$ variables, does clearly not mean that we need them all, or that we need them in the original scale.
Indeed, variables may include redundant information. 
It is thus of great interest, both for interpretability and for computational speedups, to remove the redundancy in the data by reducing its dimension. 

Some examples include:

- Variable selection such as [stepwise-regression](https://en.wikipedia.org/wiki/Stepwise_regression), and [LASSO](http://statweb.stanford.edu/~tibs/lasso.html).
- Linear-space embeddings such as [Principal Component Analysis](https://en.wikipedia.org/wiki/Principal_component_analysis), [Factor Analysis](https://en.wikipedia.org/wiki/Factor_analysis), [Independent Component Analysis](https://en.wikipedia.org/wiki/Independent_component_analysis), [Multidimensional Scaling](https://en.wikipedia.org/wiki/Multidimensional_scaling), [Partial Least Squares](https://en.wikipedia.org/wiki/Partial_least_squares_regression), and [Canonical Correlation Analysis](https://en.wikipedia.org/wiki/Canonical_correlation)
- Non-linear-space embeddings such as [Local MDS](https://en.wikipedia.org/wiki/Nonlinear_dimensionality_reduction#Local_multidimensional_scaling), [Locally Linear Embeddings](https://en.wikipedia.org/wiki/Nonlinear_dimensionality_reduction#Locally-linear_embedding), and [Self Organizing Maps](https://en.wikipedia.org/wiki/Self-organizing_map).



## Basis Augmentation
Basis augmentation consists of generating new variables from non linear combinations of existing ones. This is motivated by the observation that relation between variables may be linear in some non-linear transformation of these. 
Examples include:

- [Interactions](https://en.wikipedia.org/wiki/Interaction_(statistics)).
- [Neural Networks](https://en.wikipedia.org/wiki/Artificial_neural_network).
- [General Additive Models](https://en.wikipedia.org/wiki/Generalized_additive_model).
- [Projection Pursuit Regression](https://en.wikipedia.org/wiki/Projection_pursuit_regression).


## The Kernel Trick

As previously stated, it is typically impossible so solve some risk minimization problem over an infinite dimensional function space.
Exceptions arise, however, if using a particular type of [regularization](#inductive-bias).
Indeed, it was [Grace Whaba's](http://www.stat.wisc.edu/~wahba/) observation, when studying smoothing splines, that with the right regularization, the solution to a risk minimization problem in an infinite dimensional function space, is contained within a finite dimensional simple space (that of [cubic splines](https://en.wikipedia.org/wiki/Spline_(mathematics))).
This observation was later generalized to what is currently known as the Kernel Trick.

Informally speaking, the kernel trick states that by choosing an appropriate regularization, we can constrain the solution of an infinite dimensional [ERM](#risk-minimization) problem to a simple functional subspace, known as a [Reproducing Kernel Hilbert Space](https://en.wikipedia.org/wiki/Reproducing_kernel_Hilbert_space).
The reason that RKHS spaces appear in this context is that functions in RKHS can be represented as linear combinations of distances between points in the space, which is a far- from-trivial property of a function. 


## Generative Models

A data scientist trained as a Statistician will first think of a sampling distributions, a.k.a., a _generative model_. This may be an overkill for the simple purpose of discriminative analysis, dimensionality reduction and clustering. If, however, a generative model can be assumed, then it immediately lends itself to learning using likelihood principals. 

Assuming the generative model has a latent variable, allows the design of algorithms that pool information from different samples in a manner that no algorithm designer could have though of. Examples include:

- [Finite Mixtures](https://en.wikipedia.org/wiki/Mixture_model), [Hiddem Markov Models](https://en.wikipedia.org/wiki/Hidden_Markov_model),  [Hierarchical Dirichlet Processes](https://en.wikipedia.org/wiki/Hierarchical_Dirichlet_process), [Stochastic blockmodels](https://en.wikipedia.org/wiki/Community_structure) used for classification and clustering.
- [Factor Analysis](https://en.wikipedia.org/wiki/Factor_analysis), [Independent Component Analysis](https://en.wikipedia.org/wiki/Independent_component_analysis), [Canonical Correlations Analysis](https://en.wikipedia.org/wiki/Canonical_correlation), used for dimensionality reduction.
- [Latent Dirichlet Allocation](https://en.wikipedia.org/wiki/Latent_Dirichlet_allocation), and [Probabilistic Latent Semantic Indexing](https://en.wikipedia.org/wiki/Probabilistic_latent_semantic_analysis) used for topic modeling. 

