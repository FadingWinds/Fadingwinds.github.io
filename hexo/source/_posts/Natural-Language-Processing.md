---
title: Natural Language Processing
date: 2020-02-17 17:39:55
author: FadingWinds
img:
categories: Machine Learning | 机器学习
tags: 
    - notes
    - updating
    - CT courses
summary: Notes for NLP.
top:
hidden:
password:
---

### Overview

Three type of models:

- Generative Models
- Discriminative Models, e.g. neural networks
- Graphical Models

The fundamental goal of NLP is to have **deep understanding of broad language**.

End systems that we want to build:

- **Simple**: spelling correction, text categorization, ...
- **Complex**: speech recognition, machine translation, information extraction, dialog interfaces, question answering, ...
- Unknown: human-level comprehension (probably not just NLP)

**Key problems** in NLP:

1. Ambiguity
   - Syntactic ambiguity
  
        e.g. *Stolen Painting Found by Tree* 
        
        In state-of-the-art ML, can reach ~95% accuracy for many languages when given many training examples
   - Semantic ambiguity 

        e.g. *Siri, call me an ambulance*

2. Scale  

3. Sparsity
   
   **Corpus** is a collection of text. Often annotated in some way. 

### Text Classification

Examples of classification problems: spam vs. non-spam, text genre, word sense, etc.

**Supervised Learning**:

- Naïve Bayes
- Log-linear models (Maximum Entropy Models)
- Weighted linear models and the Perceptron
- Neural networks

Learning from annotated data problem:

- Annotation requires specific expertise.
- Annotation is expensive.
- Data is private and not accessible.
- Often difficult to define and be consistent.

*Always think about the data, and how much of it your model needs (even better: think of the data first, model second).*

**Training data, development data and held-out data**: an important tool for estimating generalization is train on one set, evaluate during development on another, and use test data only once.

**Main ideas of text classification**:

- Representation as feature vectors
- Scoring by linear functions
- Learning by optimization

**Probabilistic classifiers**: two broad approaches to predict a class $y$

a) Joint / Generative models (e.g. Naïve Bayes)

- Work with a joint probabilistic model of data $P(X,y)$, weights are (often) local conditional probabilities.
- Often assume functional form for $P(X|y), P(y)$; represent p(y,X) as Naïve Bayes model, compute $y*=argmax_y p(y,X)= argmax_y p(y)p(X|y)$.
- Estimate *probabilities* from data.
- Advantages: learning weights is easy and well understood.

b) Conditional / Discriminative models (e.g. Logistic Regression)

- Work with conditional probability $p(y|X)$. We can then directly compute $y* = argmax_y p(y|X)$ (estimating $p(y|X)$ directly).
- Require numerical optimization methods.
- Estimate *parameters* from data.
- Advantages: Don’t have to model $p(X)$! Can develop feature rich models for $p(y|X)$.
- Popular in NLP.

#### Generative Approach: Naïve-Bayes Models

The generative story: **pick a topic, then generate a
document.**

**Assumption: All words (features) are independent given the topic (label).**

Order invariant for tokens.

Issues: underflow; large number of topics (a lot of computing).

NB Learning: **Maximum Likelihood Estimate**

In MLE, two parameters to estimate: 1) $𝑞(𝑦) = \theta_y$ for each topic $𝑦$; 2) $q(x|y) = \theta_{xy}$ for each topic $y$ and word $x$.

**Zero frequency problem** in MLE: if there is a zero in the calculation the whole product becomes zero, no matter how many other values you got which maybe would find another solution.

Learning by **count**: $\theta_y = C(y)/N$, $\theta_{xy} = C(x,y)/C(y)$. The learning complexity is **O(n)**.

*Word sense*: bag-of-words classification works ok for nouns, but verbal senses are less topical and more sensitive to structure (argument choice).

#### Discriminative Approach: Linear Models

Features are indicator functions which count the occurrences of certain patterns in the input. Initially we will have different feature values for every pair of input $X$ and class $y$.

**Block Feature Vectors**: Input has features, which are multiplied by outputs to form the candidates.

Different candidates will often share features.

In linear models, each feature gets a weight in $w$. We compute the scores and then according to the prediction rule, we choose the highest (positive) one.

<details>
<summary>Example of Linear Model</summary>

$\Phi(X, SPORTS) = [1 0 1 0 \space 0 0 0 0 \space 0 0 0 0]$

$\Phi(X, POLITICS) = [0 0 0 0 \space 1 0 1 0 \space 0 0 0 0]$

$\Phi(X, OTHER) = [0 0 0 0 \space 0 0 0 0 \space 1 0 1 0]$

$W = [1 \space 1 \space -1 \space 2, 1 \space -1 \space 1 \space -2, -2 \space -1 \space -1 \space 1]$

Respectively $SCORE= 0;2;-3$, thus $prediction = POLITICS$
</details>

(Multinomial) Naïve-Bayes is a linear model.

**Picking weights**:

- Goal: choose "best" vector $w$ given training data.
- The best we can ask for are weights that give best training set accuracy, but it's a hard optimization problem.

#### Discriminative Approach: Maximum Entropy Models (=Logistic Regression)

Use the scores as probabilities.

Learning: maximize the (log) conditional likelihood of training data $\{(X^{(i)}, y^{(i)})\}^N_{i=1}$

An equation is said to be a **closed-form solution** if it solves a given problem in terms of functions and mathematical operations from a given generally accepted set.

$L(w) = \Sigma^{N}_{i=1}logP(y^{(i)}|X^{(i)}; w), w^*=argmax_wL(w)$

-- $w^*$ doesn't have a closed-form solution. So the MaxEnt objective is an *unconstrained optimization problem*.

- Basic idea: move uphill from current guess.
- Gradient ascent / descent follows the gradient incrementally.
- At **local optimum**, derivative vector is **zero**.
- Will converge **if step sizes are small enough**, but **not efficient**.
- All we need is to be able to evaluate the function and its derivative. Once we have a function $f$, we can find a local optimum by iteratively following the gradient.
- For **convex** (curved or rounded outward) functions, **a local optimum will be global**. Convexity guarantees a single, global maximum value because any higher points are greedily reachable.

Basic gradient ascent isn’t very efficient, but there are simple enhancements which take into account previous gradients: conjugate gradient, L-BFGS. There are special-purpose optimization techniques for MaxEnt, like
iterative scaling, but they aren’t better.

**The optimum parameters are the ones for which each feature’s predicted expectation equals its empirical expectation.**

In logistic regression, instead of worrying about zero count in MLE, we worry about *large feature weights*. Use regularization (smoothing) for log-linear models (add a L2 regularization term to the likelihood to push weights to zero).

But even after regularization, MaxEnt still doesn't have a closed-form solution. We will have to differentiate and use gradient ascent.