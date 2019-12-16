[Back to menu](/README.md)

<h1 id = "0">Final Review</h1>

## Contents

- [Basic Formulas](#1)
- [Contour and Persp](#2)
- [Ellipse](#3)
- [Multinormal Distribution](#4)
- [PCA](#5)
- [Biplot](#6)
- [Regression and Eigen Projection](#7)
- [PCFA](#8)
- [Distance Matrix](#9)
- [Clustering](#10)
- [Classification](#11)
- [Binary Response Model](#12)

<h2 id = "1">Basic Formulas</h2>

- Orthogonal Projection: 
    - $x_1$ on $x_2$: $\hat{y}_{x}=\frac{x^{T} y}{x^{T} x} x$
    - $X$ on $Y$: $\hat{\mathbf{y}}_{\mathbf{x}}=\mathbf{X}\left(\mathbf{X}^{T} \mathbf{X}\right)^{-1} \mathbf{X}^{T} \mathbf{y}$

```
((x2 %*% x1) / (x2 %*% x2)) * x2
x %*% solve(t(x) %*% x) %*% t(x) %*% y
```

- Multinormal distribution:
    - $f\left(x_{1}, x_{2}, \ldots, x_{p}\right)=\frac{1}{(2 \pi)^{p / 2} | \boldsymbol{\Sigma}^{1 / 2}} \exp \left(-0.5 \cdot(\mathbf{x}-\boldsymbol{\mu})^{T} \boldsymbol{\Sigma}^{-1}(\mathbf{x}-\boldsymbol{\mu})\right)$

```
f = function(x) {
    1 / ((2 * pi) ^ 2 * (det(Sigma) ^ 0.5)) * exp(-0.5 * t(x - mu) %*% solve(Sigma) %*% (x - mu))
}
```

- Bivariate distribution: 
    - $f\left(x_{1}, x_{2}\right)=\frac{1}{2 \pi \sqrt{\sigma_{11} \sigma_{22}} \sqrt{1-\rho_{12}^{2}}}\times \exp \left\{-\frac{1}{2\left(1-\rho_{12}^{2}\right)}\left[\frac{\left(x_{1}-\mu_{1}\right)^{2}}{\sigma_{11}}-2 \rho_{12}\left(\frac{x_{1}-\mu_{1}}{\sqrt{\sigma_{11}}}\right)\left(\frac{x_{2}-\mu_{2}}{\sqrt{\sigma_{22}}}\right)+\frac{\left(x_{2}-\mu_{2}\right)^{2}}{\sigma_{22}}\right]\right\}$

```
bivariate_normal = function(x1, x2) {
    1 / (2 * pi * sqrt(det(Sigma))) * exp(-1 / (2 * (1 - rho12 ^ 2)) * ((x1 - mu[1]) ^ 2 / (Sigma[1, 1]) 
    - 2 * rho12 * (x1 - mu[1]) / sqrt(Sigma[1, 1]) * (x2 - mu[2]) / sqrt(Sigma[2, 2]) 
    + (x2 - mu[2]) ^ 2 / (Sigma[2, 2])))
    }
```

- Approximating matrix and approximation error
    - $\hat{\mathbf{A}}=\mathbf{X} \mathbf{E}_{r} \mathbf{E}_{r}^{T}$
    - $A E=(n-1)\left(\hat{\lambda}_{r+1}+\hat{\lambda}_{r+2}+\ldots+\hat{\lambda}_{p}\right)$

```
get_A = function(i) {
    X_s %*% E$vectors[, 1:i] %*% t(E$vectors[, 1:i])
}
sum((get_A(1) - X_s) ^ 2)
```

<h2 id = "2">Contour and Persp</h2>

```
draw_graphs = function(A) {
    f = function(x1, x2) {
    }
    z = outer(x1, x2, f)
    persp(x1, x2, z, ticktype = "detailed")
    contour(x1, x2, z)
}
x1 = seq(-1, 1, le = 100)
x2 = x1
draw_graphs(A)
```

<h2 id = "3">Ellipse</h2>

```
draw_contour_picture = function(A, c2) {
    f = function(x1, x2) {
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

<h2 id = "4">Multinormal Distribution</h2>

```
x1 = seq(-3, 3, length = 40)
x2 = x1
draw_distribution = function(mu1, mu2, sqr_sig11, sqr_sig22, rho12) {
    bivariate_normal = function(x1, x2) {
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

- Density contour that contains 90 percent of the mass

```
f_chi = function(chi) {
    1 / ((2 * pi) ^ 2 * (det(Sigma) ^ 0.5)) * exp(-0.5 * chi)
}
f_chi(qchisq(0.9, 4))
```

<h2 id = "5">PCA</h2>

- PCA table: 

```
pca = princomp(covmat = Sigma)
summary(pca, loadings = T)

rho = matrix(data = NA, size, size)
for (i in 1:size) {
    for (j in 1:size) {
        rho[i, j] = E$vectors[i, j] * sqrt(E$values[j]) / sqrt(Matrix[i, i])
    }
}
print("The correlation coefficients between PCs and variables: ")
print(rho)
```

- Centralize: `X_s = scale(X, center = T, scale = F)`

- Standardize: `X_s = scale(X, center = T, scale = T)`

- Outlier: 

```
boxplot(Y)
boxplot.stats(Y[, 1])$out
outlier_index = which(Y[, 3] == boxplot.stats(Y[, 3])$out)
```

- Scree Plot: 

```
plot(1:6,lambda,type="l",main="Scree-Plot",xlab="Component #", cex.axis=1.5,cex.lab=1.5,cex.main=1.5)
```

<h2 id = "6">Biplot</h2>

```
Y = X_s %*% E$vectors

plot(-Y[, 1], Y[, 2], type = "n", xlim = c(-6, 3), ylim = c(-6, 4))
text(-Y[, 1], Y[, 2], Europe$i..Country, cex = 0.6)

names = c("CPI", "UNE", "INP", "BOP", "PRC", "UN")
for (i in c(1:6)) {
    arrows(0, 0, -1.5 * E$vectors[i, 1], 1.5 * E$vectors[i, 2], length = 0.1, col = "blue")
    text(-1.8 * E$vectors[i, 1], 1.8 * E$vectors[i, 2], names[i], cex = 0.7, col = "blue")
}
```

- Add another point in the biplot without changing the original points. Remember how to standardize the point!

```
Rich_s = c(0, 0, 0, (3 * max(X[, 4]) - mean(X[, 4]))/sqrt(var(X[, 4])), 0, 0)
Y_rich = Rich_s %*% E$vectors

text(-Y_rich[1], Y_rich[2], "Rich", cex = 0.6, col = "red")
```

<h2 id = "7">Regression and Eigen Projection</h2>

```
plot(X$x1, X$x2, pch = 20, xlim = c(-4, 4), ylim = c(-3, 3))
abline(v = 0)
abline(h = 0)

reslm = lm(X$x2 ~ X$x1 - 1)
x2hatlm = reslm$fitted.values
abline(reslm, col = "blue")
points(X$x1, x2hatlm, pch = 20, col = "blue", cex = 0.8)

AE1 = sum((X$x2 - x2hatlm) ^ 2)

# ----------------

Sigma = cor(X)
E = eigen(Sigma)
abline(a = 0, b = E$vectors[2] / E$vectors[1], col = "red")

X_s = scale(X, center = T, scale = F)
A = X_s %*% E$vectors[,1] %*% t(E$vectors[,1])
points(A, pch = 20, col = "red", cex = 0.8)

AE2 = sum((X - A) ^ 2)
```

<h2 id = "8">PCFA</h2>

- PCFA table: 

```
X_s = scale(X, center = T, scale = T)
R = cor(X)
E = eigen(R)

L = round(E$vectors %*% sqrt(diag(E$values)), 4)

comu = round(L[, 1] ^ 2 + L[, 2] ^ 2, 4)

sp_var = round(c(1, 1, 1, 1, 1, 1) - comu, 4)

prop = round(c(sum(L[, 1] ^ 2) / 6, sum(L[, 2] ^ 2) / 6), 4)
```

- Varimax

```
v_L = varimax(L[, 1:2])
v_L$rotmat
```

<h2 id = "9">Distance Matrix</h2>

- Check Euclidean matrix

```
n = length(voting[, 1])
J = matrix(rep(1, n ^ 2), n, n)
I = diag(rep(1, n))
H = I - 1 / n * J
Delta_2 = voting ^ 2

Q = - 1 / 2 * H %*% Delta_2 %*% H
E_Q = eigen(Q)
check_eu = function(E_Q) {
    if (all(E_Q$values >= 0)) {
        print("It is Euclidean. ")
    } else {
        print("It is not Euclidean. ")
    }
}

check_eu(E_Q)
```

<h2 id = "10">Clustering</h2>

- Cluster tree

```
cluster_3 = hclust(as.dist(Delta), method = "average")
plot(cluster_3, hang = -1)
rect.hclust(cluster_3, k = 4)
```

- Different color: 

```
label = rep(1, 16)
label[outlier_index] = 2
pairs(Y, pch = 20, col = label, cex = label)
```

- Different color according to the cluster tree: 

```
plot(Y_hat[, 1], Y_hat[, 2], type = "n", xlim = c(-0.8, 0.8), ylim = c(-0.8, 0.8))
text(Y_hat[, 1], Y_hat[, 2], col_names, cex = 0.8, col = cutree(cluster_3, k = 4))
```

<h2 id = "11">Classification</h2>

- Prediction

```
predict_function = function(pred_data, S1, S2, mu1, mu2, actu_label) {
    n1 = length(male_data[, 1])
    n2 = length(female_data[, 1])
    S_p = (n1 - 1) / (n1 + n2 - 2) * S1 + (n2 - 1) / (n1 + n2 - 2) * S2

    lhs = as.matrix(pred_data) %*% solve(S_p) %*% (mu1 - mu2) -
    (0.5 * t(mu1 - mu2) %*% solve(S_p) %*% (mu1 + mu2))[1, 1]

    label = rep(0, length(pred_data[, 1]))
    label[which(lhs < log(female_portion / male_portion))] = 1

    confusion = matrix(rep(0, 4), 2, 2)
    confusion[1, 1] = length(which(label + actu_label == 0))
    confusion[2, 2] = length(which(label + actu_label == 2))
    confusion[1, 2] = length(which(label - actu_label == 1))
    confusion[2, 1] = length(which(label - actu_label == -1))
    
    print(confusion)

    aper = (confusion[1, 2] + confusion[2, 1]) / length(pred_data[, 1])
    print(aper)
}
predict_function(X[, 1:2], cov_male[1:2, 1:2], cov_female[1:2, 1:2], male_mean[1:2], female_mean[1:2], X["sex"])
```

<h2 id = "12">Binary Response Model</h2>

- A certain kind of barplot

```
plot(as.factor(SwissLabor[, 1]) ~ SwissLabor[, "age"] + SwissLabor[, "education"] + SwissLabor[, "income"] + SwissLabor[, "foreign"], ylevels = c("yes", "no"))
```

- Logit, Probit

```
logit = glm(SwissLabor[, "participation"] ~ SwissLabor[, "age"] +
                age_2 + SwissLabor[, "education"] + SwissLabor[, "income"] +
                SwissLabor[, "foreign"] + SwissLabor[, "youngkids"] +
                SwissLabor[, "oldkids"], family = binomial(link = "logit"))

pred_logit = rep("no", length(logit$fitted.values))
pred_logit[which(logit$fitted.values > 0.5)] = "yes"
pred_logit = as.factor(pred_logit)
```

- Regression

```
regression1 = lm(as.numeric(SwissLabor[, "participation"]) ~ SwissLabor[, "age"] +
                age_2 + SwissLabor[, "education"] + SwissLabor[, "income"] +
                SwissLabor[, "foreign"] + SwissLabor[, "youngkids"] + SwissLabor[, "oldkids"])

pred1 = regression1$fitted.values
```



---

[Back to top](#0)