HTML header:  <script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js">
MathJax.Hub.Config({
    config: ["MMLorHTML.js"],
    extensions: ["tex2jax.js","TeX/AMSmath.js","TeX/AMSsymbols.js"],
    jax: ["input/TeX"],
    displayAlign: "left",
    displayIndent: "2em",
    tex2jax: {
        inlineMath: [ ['$','$'], ["\\(","\\)"] ],
        displayMath: [ ['$$','$$'], ["\\[","\\]"] ],
        processEscapes: false
    }
});
</script>

# Matrix Chain multiplication #

Given four matrices $P$, $Q$, $R$, $S$, find the minimum number of multiplications required for $P \times Q \times R \times S$.

## Background ##

### Multiply two matrices ###

To multiply to matrices $A$ and $B$ the number of columns of matrix $A$ must match the number of rows of matrix $B$. Let's  take

$$A = \left( \begin{array}{ccc}
a_{1,1} & a_{1,2} & a_{1,3} \\
a_{2,1} & a_{2,2} & a_{2,3}
\end{array} \right)$$

$$B = \left( \begin{array}{cc}
b_{1,1} & b_{1,2} \\
b_{2,1} & b_{2,2} \\
b_{3,1} & b_{3,2}
\end{array} \right)$$

$$C = \left( \begin{array}{cc}
a_{1,1} \cdot b_{1,1} + a_{1,2} \cdot b_{2,1} + a_{1,3} \cdot b_{3,1} &
a_{1,1} \cdot b_{1,2} + a_{1,2} \cdot b_{2,2} + a_{1,3} \cdot b_{3,2} \\
a_{2,1} \cdot b_{1,1} + a_{2,2} \cdot b_{2,1} + a_{2,3} \cdot b_{3,1} &
a_{2,1} \cdot b_{1,2} + a_{2,2} \cdot b_{2,2} + a_{2,3} \cdot b_{3,2}
\end{array} \right)$$

For example

$$A = \left( \begin{array}{ccc}
3 & 2 & 1 \\
1 & 0 & 2
\end{array} \right)$$

$$B = \left( \begin{array}{cc}
1 & 2 \\
0 & 1 \\
4 & 0
\end{array} \right)$$

$$C = \left( \begin{array}{cc}
(3 \cdot 1 + 2 \cdot 0 + 1 \cdot 4) &
(3 \cdot 2 + 2 \cdot 1 + 1 \cdot 0) \\
(1 \cdot 1 + 0 \cdot 0 + 2 \cdot 4) &
(1 \cdot 2 + 0 \cdot 1 + 2 \cdot 0)
\end{array} \right)
=
\left( \begin{array}{cc}
7 & 8 \\
9 & 2
\end{array} \right)$$

So the number of scalar multiplications you need for multiplying matrix $A$ with $m$ rows , and $k$ columns to matrix $B$ with $k$ rows and $n$ columns is $|m \cdot k \cdot n|$. In our example $|2 \cdot 3 \cdot 2| = 8$ multiplications are needed.

### Multiply three (or more) matrices ###

There is only one possible way of multiplying two matrices. In fact you can't switch the order of the operands like you would be able to when dealing with normal numbers. So while $7 \cdot 5 = 5 \cdot 7$ you can't (normally) do that with matrices. That's because the matrix product is (normally) not _commutative_ meaning that $A \times B \neq B \times A$ (with $\times$ being the operator for matrix multiplication).

But it is _associative_. That means that in whatever order you do the operations, the result will always be the same. So given three matrices $A$, $B$, and $C$ there are three ways of multiplying them

$$A \times B \times C = A \times (B \times C) = (A \times B) \times C $$

and all give the same result.

And this is where it gets interesting. While the result is the same, a different order might yield different numbers of scalar multiplications needed for the matrix product. Remember that the number of scalar multiplications needed for two matrices is $m \cdot k \cdot n$.

Given the following matrices (and their dimensions) $A_{2,3}$, $B_{3,2}$, and $C_{2,3}$ consider the two ways of multiplying these matrices.

1. $D = (A \times B) \times C$
2. $D = A \times (B \times C)$

Let's examine the first case $D = (A \times B) \times C$:

1. $(A \times B)$ will result in intermediate matrix $Z$ with two rows and two columns using $2 \cdot 3 \cdot 2$ scalar multiplications
2. $Z \times C$ will result in the result matrix $D$ with two rows and three columns using $2 \cdot 2 \cdot 3$ scalar multiplications
3. So you need $(2 \cdot 3 \cdot 2) + (2 \cdot 2 \cdot 3) = \mathbf{24}$ multiplications.

In the second case $D = A \times (B \times C)$:

1. $(B \times C)$ will result in intermediate matrix $Z$ with three rows and three columns using $3 \cdot 2 \cdot 3$ scalar multiplications
2. $A \times Z$ will result in the result matrix $D$ with two rows and three columns using $2 \cdot 3 \cdot 3$ scalar multiplications
3. So you need $(3 \cdot 2 \cdot 3) + (2 \cdot 3 \cdot 3) = \mathbf{36}$ multiplications.

As you can see although the result is the same the order of operations impacts the number of scalar of multiplications. That means that if you have three or more matrices and want to find the minimum number of scalar multiplications to create a chain matrix product you will have to optimize  for the order of operations, which is like defining parentheses and using the associativity of the matrix product to your advantage.

## The Problem ##

So the problem given at the beginning is a more general optimization problem. Given a chain of matrices

$$(A_1 \times A_2 \times A_3 \dots A_{i-1} \times A_i x A_{i+1} \dots A_{n-1} \times A_n)$$

parenthesize them in a way so that the number of scalar multiplications is minimal.

## The Ways to Parenthesize a Matrix Chain ##

Before diving into a solutions. Let's first analyze in what big of a problem we are getting into with this.

Given two matrices there is only one way to parenthesize the chain

$$(A_1 \times A_2)$$

If there are three matrices there are two ways

$$\begin{array}{ll}
((A_1 \times A_2) \times A_3) \\
(A_1 \times (A_2 \times A_3))
\end{array}$$


If there are three matrices there are five ways

$$\begin{array}{ll}
((A1 \times A2) \times (A3 \times A4)) \\
(((A1 \times A2) \times A3) \times A4) \\
((A1 \times (A2 \times A3)) \times A4) \\
(A1 \times ((A2 \times A3) \times A4)) \\
(A1 \times (A2 \times (A3 \times A4)))
\end{array}$$

From this we can calculate the number of different parenthesizations $P$

$$\begin{array}{ll}
	P(1) = 1 \\
	P(n) = \sum_{k=1}^{n-1} P(k)P(n-k)
\end{array}$$

This series of numbers is related to the [Catalan Numbers](http://en.wikipedia.org/wiki/Catalan_number). In particular $P(n) = C(n − 1)$, where $C(n)$ is the $n$th Catalan number

$$C_n = \frac{1}{n+1}{2n\choose n}$$

Using [Stirling's Approximation](http://en.wikipedia.org/wiki/Stirling's_approximation) we find out that $C(n) \in \Omega (\frac{4^n}{n^{3/2}})$ Since $4^n$ is exponential and $n^{3/2}$ only polynomial the exponential will dominate, implying that function grows very fast. Thus, finding the optimal solution will not be practical except for very small $n$.

## Implementation, Top Down ##

Even if trying every parenthesization is only computationally feasible for small $n$, let's try to write an application to just do that first. A working solution is better than no solution.

The matrix chain multiplication problem lends itself beautifully to the dynamic programming approach. There is a structure, in that case the parenthesization that can be broken down into subproblems, which solutions can be combined to form a global solution. Since matrices can not be reordered it makes sense to think of them as a sequence of matrices represented by an array $p$ of matrix dimensions with the first element being the height of the matrix, other subsequent elements being the width of the following matrices. Let $A_{i..j}$ be the result of the multiplication of matrix $A_i$ through $A_j$  It is easy to see that $A\_{i..j}$ is a ${p_{i−1} \times p_j}$ matrix. So splitting up a chain at position ${k}, 1 \leq k \leq n−1$

$$A_{1..n} = A_{ 1..k} \times A_{k+1..n}$$

The problem is then reduced to choosing the correct splitting position ${k}$. This can be solved recursively for every subchain and storing the cost of each subproblem in a costs array.

The pseudocode

```
int RecursiveMatrixChain(array p, int i, int j) {
  // basic case
  if (i == j) m[i, i] = 0;
  else {
    // initialize
    m[i, j] = infinity;
    // try all possible splits
    for k = i to j − 1 do {
      cost = RecursiveMatrixChain(p, i, k)
        + RecursiveMatrixChain(p, k + 1, j)
        + p[i − 1] * p[k] * p[j];

      // update if better
      if (cost < m[i, j]) then
        m[i, j] = cost;
      }
    }
  // return final cost
  return m[i,j];
}
```

The implementation in Java

```java
public class RecursiveMatrixChainMultiplication {

  int[][] costs;

  @Override
  public int matrixChainOrder(int[] matrixDimensions) {
    int length = matrixDimensions.length;
    costs = new int[length][length];

    return matrixChainOrder(matrixDimensions, 1, length - 1);
  }

  private int matrixChainOrder(int[] dimensions, int left, int right) {
    if (left == right) {
      costs[left][right] = 0;
    } else {
      costs[left][right] = Integer.MAX_VALUE;
      for (int k = left; k < right; k++) {
        int cost = matrixChainOrder(dimensions, left, k)
            + matrixChainOrder(dimensions, k + 1, right)
            + dimensions[left - 1] * dimensions[k] * dimensions[right];
        if (cost < costs[left][right]) {
          costs[left][right] = cost;
        }
      }
    }
    return costs[left][right];
  }

}
```
The problem of course is that this means that we still find the optimal solution in exponential time. The main problem is that we revisit each possible sub-solution multiple times.



## Resources ##

- [Geeks for Geeks, Dynamic Programming | Set 8 (Matrix Chain Multiplication)](http://www.geeksforgeeks.org/dynamic-programming-set-8-matrix-chain-multiplication/)
- [Jie Gao, Analysis of Algorithms](http://www.cs.sunysb.edu/~jgao/CSE548-fall07/David-mount-DP.pdf)
- [Rashid Bin Muhammad, Matrix-chain Multiplication  Problem](http://www.personal.kent.edu/~rmuhamma/Algorithms/MyAlgorithms/Dynamic/chainMatrixMult.htm)

## Further Reading ##

If you want to go even further. There is a paper from Heejo Lee et al. that parallizes the computation of operations costs for each subsequence.

Heejo Lee, Jong Kim, Sungje Hong, and Sunggu Lee. [Processor Allocation and Task Scheduling of Matrix Chain Products on Parallel Systems](http://ccs.korea.ac.kr/pds/tpds03.pdf). IEEE Trans. on Parallel and Distributed Systems, Vol. 14, No. 4, pp. 394–407, Apr. 2003
