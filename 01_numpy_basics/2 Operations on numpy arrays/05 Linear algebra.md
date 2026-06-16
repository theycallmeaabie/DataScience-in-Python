## 4.6 Linear Algebra

Linear algebra operations such as matrix multiplication, matrix decomposition, determinants, and matrix inverses are fundamental in NumPy.

NumPy provides these capabilities through:

- `np.dot()`
- The `@` operator
- The `numpy.linalg` module

---

## Matrix Multiplication with `dot()`

For NumPy arrays:

```python
*   → Element-wise multiplication
dot() or @ → Matrix multiplication
```

### Example

```python
x = np.array([
    [1., 2., 3.],
    [4., 5., 6.]
])

y = np.array([
    [ 6., 23.],
    [-1.,  7.],
    [ 8.,  9.]
])
```

### Matrix Shapes

```text
x : (2 × 3)

[[1. 2. 3.]
 [4. 5. 6.]]

y : (3 × 2)

[[ 6. 23.]
 [-1.  7.]
 [ 8.  9.]]
```

Since the inner dimensions match (`3`), matrix multiplication is possible.

### Using `dot()`

```python
x.dot(y)
```

Output:

```python
array([[ 28.,  64.],
       [ 67., 181.]])
```

### Equivalent Form

```python
np.dot(x, y)
```

Output:

```python
array([[ 28.,  64.],
       [ 67., 181.]])
```

---

## How Matrix Multiplication Works

For the first element:

```text
(1×6) + (2×-1) + (3×8)
= 6 - 2 + 24
= 28
```

For the second element:

```text
(1×23) + (2×7) + (3×9)
= 23 + 14 + 27
= 64
```

Thus:

```python
array([[28., 64.],
       [67., 181.]])
```

---

## Matrix vs Element-Wise Multiplication

### Element-wise (`*`)

```python
a * b
```

Multiplies corresponding elements.

```text
[1, 2, 3]
*
[4, 5, 6]

=
[4, 10, 18]
```

### Matrix Multiplication (`@` or `dot()`)

```python
a @ b
```

Uses row-by-column multiplication.

---

## Matrix × Vector Multiplication

A matrix can also be multiplied by a 1D array (vector).

```python
x @ np.ones(3)
```

Output:

```python
array([ 6., 15.])
```

### Why?

```python
np.ones(3)
```

creates:

```python
array([1., 1., 1.])
```

Then:

```text
Row 1: 1 + 2 + 3 = 6
Row 2: 4 + 5 + 6 = 15
```

Result:

```python
array([6., 15.])
```

---

## The `numpy.linalg` Module

`numpy.linalg` provides advanced linear algebra functions.

Common functions:

| Function | Description |
|----------|-------------|
| `inv()` | Matrix inverse |
| `det()` | Determinant |
| `eig()` | Eigenvalues and eigenvectors |
| `svd()` | Singular Value Decomposition |
| `qr()` | QR decomposition |
| `solve()` | Solve linear systems |

---

## Matrix Inverse

```python
from numpy.linalg import inv, qr

X = rng.standard_normal((5, 5))
mat = X.T @ X
```

### Why `X.T @ X`?

```python
X.T
```

is the transpose of `X`.

```python
mat = X.T @ X
```

computes the matrix product of `Xᵀ` and `X`.

This produces a symmetric square matrix.

---

### Compute the Inverse

```python
inv(mat)
```

Output:

```python
array([[ 3.4993,  2.8444,  3.5956, -16.5538,  4.4733],
       [ 2.8444,  2.5667,  2.9002, -13.5774,  3.7678],
       [ 3.5956,  2.9002,  4.4823, -18.3453,  4.7066],
       [-16.5538, -13.5774, -18.3453,  84.0102, -22.0484],
       [ 4.4733,  3.7678,  4.7066, -22.0484,  6.0525]])
```

---

## Verifying the Inverse

A matrix multiplied by its inverse should produce the identity matrix.

```python
mat @ inv(mat)
```

Output:

```python
array([[ 1.,  0., -0.,  0., -0.],
       [ 0.,  1.,  0.,  0., -0.],
       [ 0., -0.,  1., -0., -0.],
       [ 0., -0.,  0.,  1., -0.],
       [ 0., -0.,  0., -0.,  1.]])
```

This is essentially:

```text
Identity Matrix

[[1 0 0 0 0]
 [0 1 0 0 0]
 [0 0 1 0 0]
 [0 0 0 1 0]
 [0 0 0 0 1]]
```

Small values like `-0.` occur because of floating-point precision.

---

## Transpose and Dot Product

```python
X.T @ X
```

or equivalently:

```python
X.T.dot(X)
```

computes the matrix product of `X` with its transpose.

### Example

If:

```python
X.shape = (5, 5)
```

then:

```python
X.T.shape = (5, 5)
```

and:

```python
X.T @ X
```

produces another `(5 × 5)` matrix.

This operation appears frequently in:

- Machine Learning
- Statistics
- Linear Regression
- Principal Component Analysis (PCA)

---

## Key Points

- `*` performs element-wise multiplication.
- `dot()` and `@` perform matrix multiplication.
- Matrix multiplication requires compatible dimensions.
- `numpy.linalg` provides advanced linear algebra routines.
- `inv()` computes a matrix inverse.
- `A @ inv(A)` produces the identity matrix.
- `X.T @ X` computes the product of a matrix and its transpose, a common operation in statistics and machine learning.