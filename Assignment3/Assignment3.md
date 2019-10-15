[Back to menu](/README.md)

<h1 id = "0">Assignment 3</h1>

## contents
- [Exercise 1](#1)
- [Exercise 2](#2)
- [Exercise 4](#4)
- [Exercise 5](#5)
- [Exercise 7](#7)
- [Exercise 8](#8)

<h2 id = "1">Exercise 1</h2>

```
A = matrix(c(13, -4, 2, -4, 13, -2, 2, -2, 10), 3, 3)
C = cov2cor(A)
```
### a
```
print(C)
```
### b
```
print(eigen(C)$values)
print(eigen(C)$vectors)
```

<h2 id = "2">Exercise 2</h2>

```
x1 = seq(-3, 3, length = 40)
x2 = x1
draw_distribution = function(mu1, mu2, sqr_sig11, sqr_sig22, rho12) {
    bivariate_normal = function(x1, x2) {
        1 / (2 * pi * sqr_sig11 * sqr_sig22 * sqrt(1 - rho12 ^ 2)) * exp(-1 / (2 * (1 - rho12 ^ 2)) * ((x1 - mu1) ^ 2 / (sqr_sig11 ^ 2) 
        - 2 * rho12 * (x1 - mu1) / sqr_sig11 * (x2 - mu2) / sqr_sig22 
        + (x2 - mu2) ^ 2 / (sqr_sig22 ^ 2)))
    }
    z = outer(x1, x2, bivariate_normal)

    persp(x1, x2, z, main = "",
      cex.lab = 1.5, theta = 30, phi = 20, r = 50,
      d = 0.1, expand = 0.5, ltheta = 90, lphi = 180,
      shade = 0.75, ticktype = "detailed", nticks = 5, xlim = c(-3, 3),
      ylim = c(-3, 3), zlim = c(0, 0.2), asp = 1)
    contour(x1, x2, z, asp = 1)
}
```

### i
```
draw_distribution(0, 0, 1, 1, 0)
```
### ii
```
draw_distribution(0, 0, 1.5, 1, 0)
```
### iii
```
draw_distribution(0, 0, 1, 1, -0.8)
```
### iv
```
draw_distribution(0, 0, 1, 1, 0.8)
```

<h2 id = "4">Exercise 4</h2>

### a
```
mu = c(1, 0, -1, 2)
Sigma = matrix(c(1, 0.2, 0.4, -0.5, 0.2, 2, 0.8, 0, 0.4, 0.8, 2, 0, -0.5, 0, 0, 1), 4, 4)
f = function(x) {
    1 / ((2 * pi) ^ 2 * (det(Sigma) ^ 0.5)) * exp(-0.5 * t(x - mu) %*% solve(Sigma) %*% (x - mu))
}
```
#### i
```
x1 = c(0, 0, 0, 0)
print(f(x1))
```
#### ii
```
x2 = c(1, 1, 1, 1)
print(f(x2))
```
#### iii
```
x3 = c(1, 0, 1, 0)
print(f(x3))
```
### b 
```
f_chi = function(chi) {
    1 / ((2 * pi) ^ 2 * (det(Sigma) ^ 0.5)) * exp(-0.5 * chi)
}
```
#### i
```
print(f_chi(qchisq(0.95, 4)))
```
#### ii
```
print(f_chi(qchisq(0.9, 4)))
```
#### iii
```
print(f_chi(qchisq(0.8, 4)))
```

<h2 id = "5">Exercise 5</h2>

```
mu = c(1, 0.5)
Sigma = matrix(c(1, 0.8, 0.8, 1), 2, 2)

x1 = seq(-2, 4, length = 40)
x2 = x1
```
### a
```
f = function(x1, x2) {
    1 / (2 * pi * sqrt(1 - 0.8 ^ 2)) * exp(-1 / (2 * (1 - 0.8 ^ 2)) * ((x1 - 1) ^ 2 - 2 * 0.8 * (x1 - 1) * (x2 - 0.5) + (x2 - 0.5) ^ 2))
}

z = outer(x1, x2, f)
level = 1 / (2 * pi * sqrt(1 - 0.8 ^ 2)) * exp(-0.5 * qchisq(0.95, 2))
contour(x1, x2, z, levels = level, asp = 1, drawlabels = FALSE)
```
### b
```
c2 = qchisq(0.95, 2)
print(c2)
```
### c
```
len1 = sqrt(c2 / eigen(solve(Sigma))$values[1])
len2 = sqrt(c2 / eigen(solve(Sigma))$values[2])
print(len1)
print(len2)
```

<h2 id = "7">Exercise 7</h2>

```
students2008 = read.table(file = "students2008.txt", header = T, dec = ",")
attach(students2008)
heightweight = data.frame(height, weight)
heightweight = na.omit(heightweight) 
detach(students2008)
attach(heightweight)
```
### a
```
X = cbind(height, weight)
Sigma = cov(X)
mu = apply(X, 2, mean)

x1 = seq(140, 220, length = 40)
x2 = seq(40, 110, length = 40)

f = function(x1, x2) {
    rho12 = (Sigma[1, 2] / sqrt(Sigma[1, 1] * Sigma[2, 2]))
    1 / (2 * pi * sqrt(det(Sigma))) * exp(-1 / (2 * (1 - rho12 ^ 2)) * ((x1 - mu[1]) ^ 2 / (Sigma[1, 1]) 
    - 2 * rho12 * (x1 - mu[1]) / sqrt(Sigma[1, 1]) * (x2 - mu[2]) / sqrt(Sigma[2, 2]) 
    + (x2 - mu[2]) ^ 2 / (Sigma[2, 2])))
}

z = outer(x1, x2, f)
level = 1 / (2 * pi * sqrt(det(Sigma))) * exp(-0.5 * qchisq(0.95, 2))
contour(x1, x2, z, levels = level, asp = 1, drawlabels = FALSE)
```
### b
```
points(height, weight, pch = 20, cex = 1, asp = 1)

c2 = qchisq(0.95, 2)
A = solve(Sigma)

len1 = sqrt(c2 / eigen(A)$values[1])
len2 = sqrt(c2 / eigen(A)$values[2])

arrows(mu[1], mu[2],
       mu[1] + len1 * eigen(A)$vectors[1, 1], 
       mu[2] + len1 * eigen(A)$vectors[2, 1],
       length = 0.1, asp = 1)
arrows(mu[1], mu[2],
       mu[1] + len2 * eigen(A)$vectors[1, 2], 
       mu[2] + len2 * eigen(A)$vectors[2, 2],
       length = 0.1, asp = 1)
```
### c
```
count = 0
for (i in mahalanobis(X, mu, Sigma)) {
    if (i < c2)
        count = count + 1
}
ratio = count / length(height)
print(ratio)
```

<h2 id = "8">Exercise 8</h2>

```
X = read.table(file = "outlier3dim.txt", header = T, dec = ",")
mu = apply(X, 2, mean)
Sigma = cov(X)
```
### a
```
d2 = mahalanobis(X, mu, Sigma)
d1 = sqrt(d2)
```
#### i
```
boxplot(d1)
```
#### ii
```
boxplot(d2)
```
### b
```
print(max(d1))
max_index = which(max(d1) == d1)
print(max_index)
```
### c
```
y = rep(1, 16)
y[max_index] = 2
Y = cbind(X, y)

pairs(Y[1:3], pch = 20, asp = 1, col = c("black", "red")[unclass(Y$y)])
```

[Back to top](#0)