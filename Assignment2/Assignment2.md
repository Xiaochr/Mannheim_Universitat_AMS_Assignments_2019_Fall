<h1 id = "1">Assignment 2</h1>

## contents
- [Exercise 3](#3)
- [Exercise 5](#5)
- [Exercise 6](#6)

<h2 id = "3">Exercise 3</h2>

```
draw_graphs = function(A) {
    f = function(x1, x2) {
        A[1, 1] * x1 ^ 2 + (A[2, 1] + A[1, 2]) * x1 * x2 + A[2, 2] * x2 ^ 2
    }
    z = outer(x1, x2, f)
    persp(x1, x2, z, ticktype = "detailed")
    contour(x1, x2, z)
}
x1 = seq(-1, 1, le = 100)
x2 = x1
```
### i
```
A1 = matrix(c(1, 0, 0, 1), 2, 2)
draw_graphs(A1)
```
### ii
```
A2 = matrix(c(1, 0, 0, 2), 2, 2)
draw_graphs(A2)
```
### iii
```
A3 = matrix(c(1, 0, 0, 0.5), 2, 2)
draw_graphs(A3)
```
### iv
```
A4 = matrix(c(1, 0.5, 0.5, 1), 2, 2)
draw_graphs(A4)
```
### v
```
A5 = matrix(c(1, 0.8, 0.8, 1), 2, 2)
draw_graphs(A5)
```
### vi
```
A6 = matrix(c(1, -0.5, -0.5, 1), 2, 2)
draw_graphs(A6)
```
### vii
```
A7 = solve(A5)
draw_graphs(A7)
```
### viii
```
A8 = solve(A6)
draw_graphs(A8)
```

<h2 id = "5">Exercise 5</h2>

```
draw_contour_picture = function(A, c2) {
    f = function(x1, x2) {
        A[1, 1] * x1 ^ 2 + (A[2, 1] + A[1, 2]) * x1 * x2 + A[2, 2] * x2 ^ 2
    }
    z = outer(x1, x2, f)
    contour(x1, x2, z, levels = c2, asp = 1)
    len1 = sqrt(c2 / eigen(A)$values[1])
    len2 = sqrt(c2 / eigen(A)$values[2])
    arrows(0, 0, len1 * eigen(A)$vectors[1, 1], len1 * eigen(A)$vectors[2, 1], length = 0.1, asp = 1)
    arrows(0, 0, len2 * eigen(A)$vectors[1, 2], len2 * eigen(A)$vectors[2, 2], length = 0.1, asp = 1)
}
x1 = seq(-1.5, 1.5, le = 100)
x2 = x1
A = matrix(c(5, 4, 4, 5), 2, 2)
draw_contour_picture(A, 2)
```

<h2 id = "6">Exercise 6</h2>

```
A = matrix(c(13, -4, 2, -4, 13, -2, 2, -2, 10), 3, 3)
```
### a
```
E = eigen(A)
e = E$vectors
lambda = E$values
a = diag(lambda)
print(e)
print(a)
```
### b
```
f = function(i) {
    lambda[i] * e[, i] %o% e[, i]
}
```
#### i
```
print(f(1))
```
#### ii
```
print(f(1) + f(2))
```
#### iii
```
print(f(1) + f(2) + f(3))
```
### c
```
print(e %*% sqrt(a) %*% t(e))
```

[Back to top](#1)