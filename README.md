# PageRank Algorithm Implementation

A Python implementation of the PageRank algorithm following the mathematical formulation presented in the seminal paper by Bryan and Leise. This project was developed as part of the **Computational Linear Algebra for Large Scale Problems** course.

## Overview

PageRank is the algorithm originally used by Google to rank web pages in search results. It models the web as a directed graph and computes an "importance score" for each page based on the link structure. The key insight is that a page is important if it is linked to by other important pages.

The algorithm computes the dominant eigenvector of the Google matrix:

$$M = (1 - m)A + mS$$

where:
- $A$ is the column-stochastic link matrix
- $S$ is the teleportation matrix with all entries equal to $1/n$
- $m$ is the damping factor (typically 0.15)

## Features

- **Power Method Implementation**: Efficient iterative computation of the PageRank vector
- **Dangling Node Handling**: Proper redistribution of probability mass from pages with no outgoing links
- **Scalable Design**: Adjacency list representation for large-scale graphs
- **Validation**: Tested on example graphs from the reference paper
- **Convergence Analysis**: Numerical study of the power method convergence rate

## Repository Structure

```
.
├── README.md
├── pagerank.ipynb      # Main Jupyter notebook with implementation and analysis
└── hollins.dat         # Sample dataset (Hollins University web graph)
```

## Mathematical Background

### The Link Matrix

For a web of $n$ pages, the link matrix $A \in \mathbb{R}^{n \times n}$ is defined as:

$$A_{ij} = \begin{cases} 1/n_j & \text{if page } j \text{ links to page } i \\ 0 & \text{otherwise} \end{cases}$$

where $n_j$ is the number of outgoing links from page $j$.

### The PageRank Equation

The PageRank vector $\mathbf{x}$ satisfies:

$$\mathbf{x} = M\mathbf{x}$$

making it the eigenvector of $M$ corresponding to eigenvalue 1.

### Power Method

The PageRank vector is computed iteratively:

$$\mathbf{x}^{(k+1)} = M\mathbf{x}^{(k)}$$

Convergence is guaranteed since $M$ is a positive column-stochastic matrix, and the rate depends on the second largest eigenvalue $|\lambda_2| \leq 1 - m$.

## Exercises Solved

This implementation includes solutions to the following exercises from the reference paper:

### Exercise 11
Modified the web graph of Figure 2.1 by adding a fifth page with bidirectional links to page 3. Computed the new PageRank vector and ranking.

**Results:**
- PageRank vector: $(0.237, 0.097, 0.349, 0.138, 0.178)$
- Ranking: $3 > 1 > 5 > 4 > 2$

### Exercise 14
Analyzed the convergence of the power method by tracking the $\ell^1$-norm error $\|M^k x_0 - q\|_1$ for various iterations.

**Results:**
| k | $\|M^k x_0 - q\|_1$ | Ratio |
|---|---------------------|-------|
| 1 | 4.22e-01 | 0.784 |
| 5 | 4.97e-02 | 0.563 |
| 10 | 4.20e-03 | 0.614 |
| 50 | 1.21e-11 | 0.633 |

- Theoretical contraction bound: $c = 0.94$
- Second largest eigenvalue: $|\lambda_2| = 0.611$

## Validation Results

### Figure 2.1 (4-page web)
- PageRank: $(0.368, 0.142, 0.288, 0.202)$
- Ranking: $1 > 3 > 4 > 2$
- Iterations: 36

### Figure 2.2 (5-page disconnected web)
- PageRank: $(0.2, 0.2, 0.285, 0.285, 0.03)$
- Ranking: $3 = 4 > 1 = 2 > 5$
- Iterations: 2

### Hollins Dataset (6013 nodes, 23876 edges)
- Converged in 138 iterations
- Top page: Node 2 (score: 0.0199)

## Usage

### Requirements

```
numpy
```

### Running the Notebook

```bash
jupyter notebook pagerank.ipynb
```

### Basic Usage

```python
import numpy as np

# Define a simple link matrix
A = np.array([
    [0, 0, 1, 0.5],
    [1/3, 0, 0, 0],
    [1/3, 0.5, 0, 0.5],
    [1/3, 0.5, 0, 0]
])

# Compute PageRank
scores, iterations = pagerank(A, m=0.15)
print("PageRank scores:", scores)
print("Ranking:", np.argsort(scores)[::-1] + 1)
```

## Reference

This implementation is based on:

> **Bryan, K., & Leise, T.** (2006). *The $25,000,000,000 Eigenvector: The Linear Algebra Behind Google*. SIAM Review, 48(3), 569-581.
>
> Available at: [www.rose-hulman.edu/~bryan](http://www.rose-hulman.edu/~bryan)

**Abstract:** Google's success derives in large part from its PageRank algorithm, which ranks the importance of webpages according to an eigenvector of a weighted link matrix. Analysis of the PageRank formula provides a wonderful applied topic for a linear algebra course.

## Course Information

- **Course**: Computational Linear Algebra for Large Scale Problems
- **Instructors**: Andrea Borio, Francesco Della Santa, Adriano Festa
- **Academic Year**: 2025-2026

## License

This project is for educational purposes as part of academic coursework.

## Acknowledgments

- The original PageRank algorithm was developed by Larry Page and Sergey Brin at Stanford University
- Thanks to Bryan and Leise for their excellent pedagogical paper on the mathematics behind PageRank
