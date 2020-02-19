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
mathjax: true
---

## Overview

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

### Meta NLP

This section is about steps to conduct a NLP research.

#### Literature review: should be done early

- Avoid re-invent the wheel
- Learn about common tricks, resources, and libraries

1. Do a keyword search on *Google Scholar*, *Semantic Scholar*, or *the ACL Anthology*.
2. Download the papers that seem most relevant.
3. Skim the abstracts, intros, and previous work sections.
4. Identify papers that look relevant, appear often, or have lots of citations on Google Scholar, and download them. Then repeat.

Places to find the most trustworthy papers:

- *NLP*: Proceedings of ACL conferences (ACL, NAACL, EACL, EMNLP, CoNLL, LREC), Journal of Computational Linguistics, TACL, COLING, arXiv*
- *Machine Learning/AI*: Proceedings of NIPS, ICML, ICLR, AAAI, IJCAI, and arXiv*
- *Computational Linguistics*: Journals like Linguistic Inquiry, NLLT, Semantics and Pragmatics

What to mention in literature review:

- General problem/task definition
- Relevant methods and results
- Comparisons with your work and other related work
- Open issues

#### Acquiring Datasets

- Existing datasets
  
  ACL anthology, Linguistic Data Consortium (LDC), Look for datasheets when available (e.g. QuAC)
- Wild datasets
  
  e.g. Ubuntu Dialogue Corpus, StackOverflow Data (warning: easy to violate copyright and terms of service)
- Build own datasets
  
  Either write detailed guidelines, and work with experts; or write simple guidelines, and crowdsource

- Generate datasets
  
  It's super easy, but artificial data does not reflect the real world.

#### Quantitative Evaluation

1. Follow prior work and use existing metrics
   - If it's a new task, create a metric before you start testing. It must be independent of your model.
2. Use ablations to study the effectiveness of your choices (and don’t adopt fancy solutions that don’t really help)
   - e.g. MLP sentiment classifier with GloVe embeddings, MLP sentiment classifier with random embeddings, MaxEnt classifier with GloVe embeddings, MaxEnt classifier with random embeddings
3. Consider controlled human evaluation when standard, and even when less standard
   - e.g. summarization, machine translation, generation
4. Test for statistical significance when differences are small and models are complex
5. Consider extrinsic evaluation on *downstream tasks* (what the field calls those supervised-learning tasks that utilize a pre-trained model or component)
6. Negative results are also important.

#### Qualitative Evaluation and Error Analysis

Goal: convince that your hypothesis is correct.

Interesting hypotheses are often hard to evaluate with
standard/intuitive quantitative metrics.

Start with:

- Look to prior work
- Show examples of system output
- Identify qualitative categories of system error and count them
- Visualize your embedding spaces with tools like t-SNE or PCA
- Visualize your hidden states with tools like LSTMVis
- Plot how your model performance varies with amount of data
- Build an online demo if ambitious (which I'm probably not :thinking:)

**Formative evaluation**: guiding further investigations
- Typically: lightweight, automatic, intrinsic
- Compare design option A to option B
- Tune *hyperparameters* (parameter whose value is set before the learning process begins): smoothing, weighting, learning rate

**Summative evaluation**: reporting results
- Compare your approach to previous approaches
- Compare different major variants of your approach
- Generally only bother with human or extrinsic evaluations here

*Common mistake: Don’t save all your qualitative evaluation
for the summative evaluation.*

#### Hyperparameter Tuning

- Must tune the hyperparameters of your baselines just as thoroughly as you tune them for any new model you propose
- Failure to do this invalidates your comparisons
- As always, don't tune on test set.
- Read the fine print while you’re doing your literature review to get a sense of what hyperparameters to worry about and what values to expect.
- If you’re not sure whether to tune a hyperparameter, you probably should.

Methods:

- Grid search: Inefficient (but common)
- Bayesian optimization: Optimal, but public packages aren’t great.
- Good read: random search - easy, and near-optimal
  - Define distributions over all your hyperparameters.
  - Sample N times for N experiments.
  - Look for patterns in your results.
  - Adjust the distributions and repeat until you run out of
resources or performance stops improving.

#### Other

**Biases**

Deploying biased models in the wrong places can lead to harms far worse than bad user experiences.

Model de-biasing can be complex, political, and maybe even impossible to do fully, and it may harm performance on reasonable metrics.

<details>
<summary>Unfold to see a bias example</summary>

In training data, women appear in cooking scenes 33% more
often than men.

In model’s labeling of similar test data, women are detected in cooking scenes 68% more often than men.
</details>

**Talk about data**

- What your data looks like, why it was collected, and what kind of information your system learn from it
- Who (country, region, gender, native language, etc.) produced the text and labels in your dataset
- Any known biases in your dataset (including the obvious ones)





## Text Classification

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

### Generative Approach: Naïve-Bayes Models

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

### Discriminative Approach: Linear Models

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

### Discriminative Approach: Maximum Entropy Models (=Logistic Regression)

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

### Perceptron Algorithm

Iteratively processes the training set, reacting to training errors. Can be thought of as trying to drive down training error.

- Online (or batch)
- Error driven
- Simple, additive updates

#### Binary Class

**Steps** for the online (binary $y = \plusmn1$) perceptron algorithm:

1. Start with **zero** weights
2. Visit training instances $(X^{(i)}, y^{(i)})$ one by one, until all correct
   - Make a prediction: $y^* = sign(w ·\Phi(X^{(i)}))$ (sign means whether it >(=) 0)
   - If correct ($y^*==y^{(i)}$): no change, go to next example!
   - If wrong: adjust weights $w = w - y^* \Phi(X^{(i)})$

The perceptron finds a separating hyperplane.

If add bias, algorithm stays the same.

#### Multiclass

For multiclass situation, we will have:

- A weight vector for each class
- Score (activation) of a class $y$: $w_y·\Phi(x)$
- Compare all possible outputs, prediction highest score wins

**Perceptron learning traits**:

- No counting or computing probabilities on training set
- Separability: some parameters get the training set perfectly correct
- Convergence: if the training is separable (e.g. could divide into two classes with 100% accuracy for binary case), perceptron will
eventually converge
- Mistake Bound: the maximum number of mistakes (binary case)
related to the margin or degree of separability

**Perceptron problems**:

- Noise: if the data isn’t separable, weights might
thrash
  - Averaging weight vectors over time can help (averaged perceptron)
- Mediocre generalization: finds a “barely” separating solution
- Overtraining: test / held-out accuracy usually
rises, then falls (overtraining is a kind of overfitting)

### A Note for Features: TF/IDF

- More frequent terms in a document are more important.
- May want to normalize term frequency (TF) by dividing by the frequency of the most common term in the document.
- Terms that appear in many different documents are less indicative (thus inverse document frequency) 

### Neural Networks

***Representation Learning***

