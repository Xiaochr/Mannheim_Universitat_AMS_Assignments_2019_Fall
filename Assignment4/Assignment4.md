<h1 id = "0">Assignment 4</h1>

## contents

- [Exercise 3](#3)
- [Exercise 4](#4)
- [Exercise 5](#5)
- [Exercise 6](#6)
- [Exercise 7](#7)

<h2 id = "3">Exercise 3</h2>

```
Sigma = matrix(c(1, 0.2, 0.4, -0.5, 0.2, 2, 0.8, 0, 0.4, 0.8, 2, 0, -0.5, 0, 0, 1), 4, 4)
Rho = cov2cor(Sigma)

get_pca_table = function(pca, Matrix, size) {
    print(summary(pca, loadings = T))
    E = eigen(Matrix)
    print("The eigenvalues: ")
    print(E$values)

    rho = matrix(data = NA, size, size)
    for (i in 1:size) {
        for (j in 1:size) {
            rho[i, j] = E$vectors[i, j] * sqrt(E$values[j]) / sqrt(Matrix[i, i])
        }
    }
    print("The correlation coefficients between PCs and variables: ")
    print(rho)
}

pca_cov = princomp(covmat = Sigma)
get_pca_table(pca_cov, Sigma, 4)

pca_cor = princomp(covmat = Rho)
get_pca_table(pca_cor, Rho, 4)
```

<h2 id = "4">Exercise 4</h2>

```
Sigma = matrix(c(1, 0, 0, 0, 2, 0, 0, 0, 3), 3, 3)
pca = princomp(covmat = Sigma)
get_pca_table(pca, Sigma, 3)
```

<h2 id = "5">Exercise 5</h2>

```
X = read.table(file = "outlier3dim.txt", header = T, dec = ",")

Sigma = cov(X)
E = eigen(Sigma)
X_s = scale(X, center = T, scale = F)
Y = X_s %*% E$vectors
colnames(Y) = c("y1hat", "y2hat", "y3hat")
```
### a
```
boxplot(Y)
boxplot.stats(Y[, 1])$out
boxplot.stats(Y[, 2])$out
boxplot.stats(Y[, 3])$out
outlier_index= which(Y[, 3] == boxplot.stats(Y[, 3])$out)
```
### b
```
label = rep(1, 16)
label[outlier_index] = 2
pairs(Y, pch = 20, col = label, cex = label)
```

<h2 id = "6">Exercise 6</h2>

```
students2008 = read.table(file = "students2008.txt", header = T, dec = ",")
attach(students2008)
heightweight = data.frame(height, weight)
heightweight = na.omit(heightweight)
detach(students2008)
attach(heightweight)

X = cbind(height, weight)
```
### a
#### pic1
```
x1 = seq(140, 220, length = 40)
x2 = seq(40, 110, length = 40)

draw_contour_pic = function(X, x1, x2) {
    Sigma = cov(X)
    mu = apply(X, 2, mean)

    f = function(x1, x2) {
        rho12 = (Sigma[1, 2] / sqrt(Sigma[1, 1] * Sigma[2, 2]))
        1 / (2 * pi * sqrt(det(Sigma))) * exp(-1 / (2 * (1 - rho12 ^ 2)) * ((x1 - mu[1]) ^ 2 / (Sigma[1, 1]) 
        - 2 * rho12 * (x1 - mu[1]) / sqrt(Sigma[1, 1]) * (x2 - mu[2]) / sqrt(Sigma[2, 2]) 
        + (x2 - mu[2]) ^ 2 / (Sigma[2, 2])))
    }
    z = outer(x1, x2, f)
    level = 1 / (2 * pi * sqrt(det(Sigma))) * exp(-0.5 * qchisq(0.95, 2))
    contour(x1, x2, z, levels = level, asp = 1, drawlabels = FALSE)

    points(X[,1], X[,2], pch = 20, cex = 1, asp = 1)

    c2 = qchisq(0.95, 2)
    A = solve(Sigma)

    len = c(sqrt(c2 / eigen(A)$values[1]), sqrt(c2 / eigen(A)$values[2]))

    draw_arrow = function(i, flag) {
        if (flag == 1) {
            arrows(mu[1], mu[2], mu[1] + len[i] * eigen(A)$vectors[1, i], mu[2] + len[i] * eigen(A)$vectors[2, i],length = 0.1, col = "red", asp = 1)
        }
        else {
            arrows(mu[1], mu[2], mu[1] - len[i] * eigen(A)$vectors[1, i], mu[2] - len[i] * eigen(A)$vectors[2, i], length = 0.1, col = "red", asp = 1)
        }
    }

    draw_arrow(1, 1)
    draw_arrow(1, 0)
    draw_arrow(2, 1)
    draw_arrow(2, 0)
}

draw_contour_pic(X, x1, x2)
```
#### pic2
```
Sigma = cov(X)
E = eigen(Sigma)
X_s = scale(X, center = T, scale = F)
Y = X_s %*% E$vectors
colnames(Y) = c("y1hat", "y2hat")

y1 = seq(-40, 40, length = 40)
y2 = seq(-30, 30, length = 40)

draw_contour_pic(Y, y1, y2)
```
#### pic3
```
boxplot(Y)
```
#### pic4
```
origin_par = par(no.readonly = T)
par(mfrow = c(2, 2))
plot(Y[, 1], height, pch = 20, cex = 0.8, asp = 1)
plot(Y[, 2], height, pch = 20, cex = 0.8, asp = 1)
plot(Y[, 1], weight, pch = 20, cex = 0.8, asp = 1)
plot(Y[, 2], weight, pch = 20, cex = 0.8, asp = 1)
par(origin_par)
```

### b
```
options(digits = 4)

Sigma = cov(X)
pca = princomp(X)
get_pca_table(pca, Sigma, 2)

options(digits = 7) # default
detach(heightweight)
```

<h2 id = "7">Exercise 7</h2>

```
students2008 = read.table(file = "students2008.txt", header = T, dec = ",")
attach(students2008)
body = data.frame(height, weight, shoesize)
body = na.omit(body)
detach(students2008)
attach(body)
```
### a
```
X = cbind(height, weight, shoesize)
X_s = scale(X, center = T, scale = T)
```
#### pic1
```
pairs(X_s, pch = 20)
```
#### pic2
```
R = cor(X)
E = eigen(R)
Y = X_s %*% E$vectors
boxplot(Y)
```
#### pic3
```
colnames(Y) = c("y1hat", "y2hat", "y3hat")
pairs(Y, pch = 20)
```
### b
```
options(digits = 4)
pca = princomp(X_s)
get_pca_table(pca, R, 3)
```

[Back to top](#0)