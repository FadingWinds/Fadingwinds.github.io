---
title: 'Become Pythonic'
date: 2019-10-29 19:35:33
author: FadingWinds
img:
categories: Coding | 编程
tags: 
	- Python
	- notes
	- updating
summary: Personal notebook for Python 3. 
top:
hidden:
password:
---
*Note: This post doesn't include detailed theories. For those, please refer to other posts.*

### Built-in

#### Grammar

- `int()` returns the lower bound.
- Pay attention to the difference between `extend()` and `append()`, e.g. [1,2,4] vs. [[1,2],[4]].

#### Easy Ways
- Remember to use lambda for simple functions and use `[x for x in statement if statement]` for iterating.
- Python has a built-in dictionary `defaultdict()`, it provides default value given by the user 

#### Possible Mistakes

- `list.append()` will only return **None**, which indicates it has successfully complete the process. Use `list` to get to the changes.
- Things like a `range(0,n)` do NOT include n but do include 0.
- If let `a = b` where a,b are both lists, when you change a it will affect b too. Try `copy.deepcopy`(needs import) or `new = old[:]`.

#### Other in Doc

- The time complexity for `sort()` is O(nlogn).


### Useful Packages

#### Package List

- *"nltk"* is a useful package for natural language processing. The functions include tokenize (divide a sentence into words), strip stop words and punctuations, lemmatize (convert different forms of a word into one, e.g. run, ran, running, runs → run).
- The famous *"sklearn"*.

#### Pandas

- Do not iterate through rows of a *pandas* dataframe unless really necessary. Usually, just define a function and use `.apply(func)` instead. No parameter needed when calling.
- If want to make changes to an existing *pandas* dataframe, remember to write `inplace = True`.
- Combine *pandas* dataframes: `pandas.concat()`
- `pandas.read_csv()` is good for loading a data file. The "csv" could be replaced with other type of file.
- A *pandas* object could be converted to a numpy array using `to_numpy()`. 

#### Numpy

- 

