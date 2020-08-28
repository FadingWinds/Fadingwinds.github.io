---
title: Natural Language Processing
date: 2020-04-16 17:39:55
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

[TOC]

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

<details>
<summary>
This section is about steps to conduct a NLP research. Unfold to see details.
</summary>

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

</details>

### Computation Graphs

Computation graphs are the descriptive language of deep learning models.

Functional description of the required computation.

Can be instantiated to do two types of computation: forward and backward computation.

#### Structure

- A *node* is a {tensor, matrix, vector, scalar} value
- An *edge* represents a function argument (and also data dependency).
- A node with an incoming edge is a *function* of that edge’s tail node.
- A node knows how to compute its value and the value of its derivative w.r.t each argument (edge) times a derivative of an arbitrary input $\frac{dF}{df(u)}$.

#### Algorithms

**Forward propagation**

- Loop over nodes in topological order
- Compute the value of the node given its inputs
- Given my inputs, make a prediction (or compute an "error" with respect to a "target output")

**Backward propagation**

- Loop over the nodes in reverse topological order starting with a final goal node
- Compute derivatives of final goal node value with respect to each edge’s tail node
- How does the output change if I make a small change to the inputs?

**Static declaration**

- Phase 1: define an architecture (maybe with some primitive flow control like loops and conditionals)
- Phase 2: run a bunch of data through it to train the model and/or make predictions

**Dynamic declaration**

- Graph is defined implicitly (e.g., using operator overloading) as the forward computation is executed

**Batching**

- Packing a few examples together has significant computational benefits
- CPU is helpful, but with GPU you get to use all the GPU cores, which is world changing. 
- Easy with simple networks, but gets harder as the architecture becomes more complex

  - Complex networks may include different parts with varying length
  - It is very hard to batch complete examples this way
  - But you can still batch sub-parts across examples, so you alternate between batched and non-batched computations


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

\* My understanding of dev vs. test: dev data is the one that you have correct labels to, while test data is like real new data coming in.

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

**Steps** for the online (binary $y = \plusmn 1$) perceptron algorithm:

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

#### Two types of machine learning

***Representation Learning*** attempts to automatically learn good features and representations.

***Deep Learning*** attempts to learn multiple levels of representation of increasing complexity/abstraction.

#### Parameters

- Weights: $w$ and  $b$
- Activation function (If dropped and single neuron, becomes perceptron)
- Hidden layer numbers and neuron numbers

#### Process

Neural net -> several MaxEnt models

Hidden layer figures out what to do by *learning*.

Training:

- Without hidden layer: supervised, just like MaxEnt
- With hidden layer: latent units -> not convex; back propagate the gradient, about the same but no guarantee
  
#### Probabilistic Output from Neural Nets

Normalize the output activations with ***softmax***:

$$
y = softmax(o) \\
softmax(o_i) = \frac{exp(o_i)}{\Sigma^k_{j=1}exp(o_j)}
$$

\* $o$ is the output layer.

Usually no non-linearity before softmax.

#### Word Embeddings

Representing words with **one-hot** vectors.

<details>
<summary>Dimensionality (unfold)</summary>

- Size of vocabulary
- 20K for speech
- 500K for broad-converage domains
- 13M for Google corpora

</details>

The word embeddings should represent each word with a *dense low-dimensional vector*.

Low-dimensional << vocabulary size

If trained well, similar words will have similar vectors.

Word embeddings as features, e.g. sentiment classification.

Feature based models: bag of words

<details>
<summary>Practical Tips (unfold)</summary>
- Select network structure appropriate for the problem, e.g. window vs. recurrent vs. recursive
- Gradient checks to identify bugs
- Parameter initialization
- If model isn't powerful enough, make it larger; else regularize to avoid overfitting
- Know your non-linearity function and its gradient 
</details>

**Debugging**

- Verify value of initial loss when using softmax.
- Perfectly fit a single mini-batch.
- If learning fails completely, maybe gradients stuck.

  - Check learning rate
  - Verify parameter initialization
  - Change non-linearity functions

**Avoid overfitting**

- Reduce model size a little
- L1 and L2 regularization
- Early stopping
- Dropout

Word embeddings vs. sparse vectors:

- Count vectors: sparse and large
- Embedded vectors: small dense
- More contested advantage other than dimensionality: better generalization

## Lexical Semantics

### Word Sense Disambiguation (WSD)

**Lemma (citation form)**: Basic part of the word, same stem, rough semantics. One lemma can have many meanings.

**Wordform**: The "inflected" word as it appears in text.

<details>
<summary>Examples (click to unfold)</summary>

Wordform: banks, sung

Lemma: bank, sing

</details>

**Sense (word sense)**: A discrete representation of an aspect of a word’s meaning.

**Homonyms**: Words that share a form but have unrelated, distinct meanings.

- Homographs: bank/bank
- Homophones: write/right

Homonyms in NLP: Information retrieval, machine translation, text-to-speech

**Synonyms**: Different words that have same meanings in some or all contexts.

- Very few examples for perfect synonyms

**Antonyms**: Senses that are opposites with respect to one feature of meaning.

**Hyponymy and Hypernymy**: One sense is a hyponym/subordinate of another if the first sense is more specific, denoting a subclass of the other; On the contrary, the other will be hypernym/superordinate.

<details>
<summary>Examples (click to unfold)</summary>
- Car is a hyponym of vehicle
- Fruit is a hypernym of mango
</details>

#### Wordnet

A hierarchically organized lexical database.

- Each word in WordNet has at least one sense
- Each sense has a gloss (textual description)
- The synset (synonym set), the set of near-synonyms, is a set of senses with a shared gloss

<details>
<summary>Example (click to unfold)</summary>

Chump as a noun with the gloss: "a person who is gullible and easy to take advantage of"

This sense of "chump" is shared with 9 words: chump1, fool2, gull1, mark9, patsy1, fall guy1, sucker1, soft touch1, mug2

All these senses have the same gloss -> they form a synset
</details>

#### Supervised WSD

Given a lexicon (e.g., WordNet) and a word in a sentence, the goal is to classify the sense of the word.

Linear model:

$$
p(sense | word, context) \propto e^{\theta *\phi(sense, word, context)}\\
p(sense | word, context) \frac{e^{\theta *\phi(sense, word, context)}}{\Sigma_{s'}e^{\theta *\phi(s', word, context)}} \\
$$

#### Unsupervised WSD

Goal: induce the senses of each word and classify in context

1. For each word in context, compute some features
2. Cluster each instance using a clustering algorithm
3. Cluster labels are word senses

### Semantic role labeling (SRL)

- Some word senses (a.k.a. predicates) represent events
- Events have participants that have specific roles (as arguments)
- Predicate-argument structure at the type level can be stored in a lexicon

#### PropBank

A semantic role lexicon.

run.01 (operate) ---- Frame

- ARG0 (operator) 
- ARG1 (machine/operation)
- ARG2 (employer)
- ARG3 (co-worker)
- ARG4 (instrument)
----Semantic roles

\*FrameNet, an alternative role lexicon

Task for semantic role labeling: given a sentence, disambiguate predicate frames and annotate semantic roles

#### Role Identification

Classification models similar to WSD

Sentence spans -> potential roles

Score can come from any classifier (linear, SVM, NN)

#### Word Similarity

Given two words, predict how similar they are.

The Distributional Hypothesis: You shall know a word by the company it keeps.

Given a vocabulary of $n$ words, represent a word $w$ as: $w =(f_1, f_2, f_3, ..., f_n)$

$f_i$: Binary (or count) features indicating the presence of the $i^{th}$ word in the vocabulary in the word’s context

Similarity can be measured using vector distance metrics. e.g. cosine similarity, gives values between -1 (completely different), 0 (orthogonal), and 1 (the same).

**Vector-space Models**

- Words represented by vectors
- In contrast to the discrete class representation of word senses
- Common methods (and packages): Word2Vec, GloVe

#### Word2Vec

- Method (and open-source package) for learning word vectors from raw text
- Widely used across academia/industry
- Goal: good word embeddings, aka. embeddings are vectors in a low dimensional space; similar words should be close to one another
- Two models: skip-gram, CBOW

#### Skip-Gram Model

Given: corpus $D$ of pairs $(w, c)$ where $w$ is a word and $c$ is context.

Context may be a single neighboring word (in window of size $k$).
Consider the parameterized probability $p(c|w;\theta)$.

Goal: maximize the corpus probability:

$argmax_{\theta} \Pi_{(w,c)\in D}p(c|w;\theta)$

The important thing is **how we parametrize the probability distribution**.

If $d$ is the dimensionality of the vectors, we have $d \times |V| + d \times |C|$ parameters.

The log of the objective sums over all context is **not tractable in practice**. It can be approximated with ***negative sampling***.

Negative sampling for skip-gram:

- Efficient way of deriving word embeddings
- Consider a word-context pair $(w,c)$, the probability that it was not observed is $1-p(D=1|w,c)$.
- Parameterization: $p(D=1|w,c)=\frac{1}{1+e^{-v_cv_w}}$
- New learning objective： $argmax_\theta\Pi_{(w,c)\in D}p(D=1|w,c)\Pi_{(w,c)\in D'}p(D=0|w,c)$
- For a given $k$, the size of $D'$ is $k$ times bigger than $D$
- Each context c is a word
- For each observed word-context pair, $k$ samples are generated based on unigram distribution

Intuition for skip-gram model: **words that share many contexts will be similar**.

## Language Model

### Problem Overview

**Setup**: assume a (finite) vocabulary of words $V=\{ the, a, man, telescope, Beckham, two, Madrid,...\}$, We can construct an (infinite) set of strings

**Data**: given a training set of example sentences

**Problem**: estimate a probability distribution over sentences

*Why would we ever want to do this?*

Answer: (Automatic) Speech Recognition (ASR): audio in, text out

<details>
  <summary>Click to unfold and see some funny examples</summary>
  Wreck a nice beach? -- Recognize speech

  Eye eight uh Jerry? -- I ate a cherry
</details> 

#### Learning language models

**Goal**: Assign useful probabilities $P(X)$ to sentences $X$

*Input*: many observations of training sentences $X$

*Output*: system that can compute $P(X)$

Probabilities should broadly indicate plausibility of sentences, e.g. P("I saw a van") >> P("eyes awe of an")

This is not only about grammar, e.g. P("infected curly chair") =(almost) 0 (yes, I made this one up)

### Models

Empirical distribution over training sentences...

- Doesn't generalize at all
- Need to assign non-zero probability to previously unseen sentences

Decompose Probability...

- Assume word choice depends on previous words only
- Not really better because last word still represents complete event

Markov assumption

- P(english | this is written in)
- P(english | is written in)
- P(english | written in)
- P(english | in)
- P(english)


#### The Noisy Channel Model

Goal: predict sentence given acoustics

$$
w^* = argmax_XP(X|a)
= argmax_XP(a|X)P(X)
$$

- Language model: Distributions over sequences of words (sentences) -- $P(X)$
- Acoustic model: Distributions over acoustic waves given a sentence -- $P(a|X)$

ASR Noisy Channel System:

Language Model: source $P(X)$ -> $X$ -> Acoustic Model: channel $P(a|X)$ -> $a$

observed $a$ -> decoder (LM&AM) -> best $X$

Other systems with similar structure: *MT Noisy Channel System (translation)*, *Caption Generation Noisy Channel System*

#### N-gram Models

**Unigram**

- Generative process: pick a word, pick a word, repeat...until you pick STOP
- Big **problem**: P(the the the the) >> P(a real sentence)

**Bigram (and more)**

- Generative process: pick start, pick a word conditioned on previous one, repeat until to pick STOP
- k-gram: conditioning on k-1 previous words
- Learning: estimate the distribution

***Well-defined Distributions - proof for unigrams***

For all string $X$ (of any length), $P(X) \geq 0$.

Claim: the sum over string of all length is 1: $\Sigma_XP(X)=1$

RNN language models are surprisingly not necessarily well defined distributions.

***Parameters for n-gram models*** 

Maximum likelihood estimate - relative frequency

$$
q_{ML}(w) = \frac{c(w)}{c()}, q_{ML}(w|v) = \frac{c(v,w)}{c(v)}, q_{ML}(w|u,v) = \frac{c(u,v,w)}{c(u,v)}, ...
$$

where $c$ is the empirical counts on a training set.

General approach:

- Take a training set $D$ 


### Continuous representations
