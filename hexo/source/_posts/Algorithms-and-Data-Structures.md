---
title: Algorithms and Data Structures
author: FadingWinds
categories: Coding | 编程
tags:
  - updating
  - CT courses
  - notes
  - Python
  - Leetcode
summary: >-
  Mostly based on Python 3, and the main resource include some relevant courses
  and practices on Leetcode.
date: 2019-11-11 14:47:06
img:
top:
hidden:
password:
---

## Theories and Methods

### Dynamic Programming

#### Situation

A duplicate process, overlapping subproblems

Recursion in brute force, optimal substructure

#### Methods

- Memorization
- Table filling

### Other Frequently Encountered Skills

#### "Walker" and "runner" Solution

The former move one step every time while the latter move two. Use to detect cycles.

#### Prefix Sums

A new array in which `new[i] = sum(old[:i])`. Take O(n) to construct, but can make it constant time for the following operations.

e.g. Find subarrays of $k$ elements that has average value more than $t$.


## Examples of Problem/Solution

- Find the $N^{th}$ fibonacci number. [Dynamic Programming]
- Given two strings s and t, find the longest subsequence (same order but can skip some letters) common to both strings. [Dynamic Programming]
- Given array $a$ containing integers $[x_1, …, x_n]$, find integers $i, j$ such that $1 ≤ i ≤ j ≤ n$ and $\Sigma^{j}_{i}a$ is maximal. [Dynamic Programming]


## Application Reflex

## Others

### Tips

- Usually recursion takes more space and runs faster, while the "for" loop is the opposite.