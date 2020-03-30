---
title: 'Becoming Pythonic'
date: 2019-10-29 19:35:33
author: FadingWinds
img: https://od.lk/s/MzBfMjA0OTgxNzJf/Python.jpg
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

- Remember to use lambda for simple functions and use `[x for x in statement if statement]` for iterating. And this is called *list comprehension*.
- Python has a dictionary `defaultdict()`, it provides default value given by the user. `collections.Counter()` could be a substitute if the default value is $0$. 
- Use `zip()` to make two lists into one with tuple: a, b -> (a,b) with len(a)/len(b). **Note**: need to convert to list manually in Python 3.
- Use `del` and `gc.collect()` to free RAM storage.

#### Possible Mistakes

- `list.append()` will only return **None**, which indicates it has successfully complete the process. Use `list` to get to the changes.
- Things like a `range(0,n)` do NOT include n but do include 0.
- If let `a = b` where a,b are both lists, when you change a it will affect b too. Try `copy.deepcopy`(needs import) or `new = old[:]`.

#### Other in Doc

- The time complexity for `sort()` is O(nlogn).

#### Differences Between Python 2 and 3

- Calculation of integer is different. In python 2, sometimes need to use "2.0" instead of "2" to get correct answers.
- Print, of course.

****

### Useful Packages

#### Overall

- Default packages: csv, math, re, ...
- **tqdm**: progress bar package. For iPython Notebook, remember to use `tqdm_notebook` instead.

#### Pandas

- Do not iterate through rows of a *pandas* dataframe unless really necessary. Usually, just define a function and use `.apply(func)` instead. No parameter needed when calling. It will pass either the selected column or the entire row (select column inside the function instead).
- If want to make changes to an existing *pandas* dataframe, remember to write `inplace = True`.
- Combine *pandas* dataframes: `pandas.concat()`
- `pandas.read_csv()` is good for loading a data file. The "csv" could be replaced with other type of file.
- A *pandas* object could be converted to a numpy array using `to_numpy()`. 

#### Numpy

- Use `np.zeros()` to create a n-d array with zeros as initial values.
- `np.nan != np.nan`! Which means if you assign `np.nan` to some variables, you can't use `==np.nan` to check it. Use `isna()` instead.

#### NLTK

- For natural language processing.
- Tokenizer to split sentences into words.
- Lemmatizer to eliminate the effects between different word senses (convert different forms of a word into one, e.g. run, ran, running, runs → run).

#### PyTorch

- If loss is `NaN`, the most possible reason is something's wrong with the data.
- `cuda()` needs GPU.
- To convert `cuda()` object to numpy arrays, need to add `.detach().cpu().clone().numpy()`

