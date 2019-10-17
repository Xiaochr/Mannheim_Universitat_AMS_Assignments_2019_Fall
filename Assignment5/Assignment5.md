[Back to menu](/README.md)

<h1 id = "0">Assignment 5</h1>

## contents

- [Exercise 1](#1)
- [Exercise 2](#2)

<h2 id = "1">Exercise 1</h2>

```
X = read.table(file = "Universities.txt", header = T, dec = ",")
```
### a
```
X = Universities[2:7]
pairs(X)
Sigma = cor(X)
E = eigen(Sigma)
plot(1:6, E$values, pch = 20)
lines(1:6, E$values)
```
### b
```
print(round(Sigma, 4))
```
### c
```
library(iplots)
X_s = scale(X, center = T, scale = T)
Y = X_s %*% E$vectors

ihist(Y[, 1])
ihist(X$SAT)
ihist(X$Top10)
ihist(X$Accept)
ihist(X$SFRatio)
ihist(X$Expenses)
ihist(X$Grad)

ihist(Y[, 2])
ihist(X$SAT)
ihist(X$Top10)
ihist(X$Accept)
ihist(X$SFRatio)
ihist(X$Expenses)
ihist(X$Grad)
```

### d
```
get_pca_table = function(pca, Matrix, size) {
    print(summary(pca, loadings = T))
    E = eigen(Matrix)
    print("The eigenvalues: ")
    print(round(E$values, 4))

    rho = matrix(data = NA, size, size)
    for (i in 1:size) {
        for (j in 1:size) {
            rho[i, j] = E$vectors[i, j] * sqrt(E$values[j]) / sqrt(Matrix[i, i])
        }
    }
    print("The correlation coefficients between PCs and variables: ")
    print(round(rho, 4))
}

options(digits = 4)
pca_cor = princomp(covmat = Sigma)
get_pca_table(pca_cor, Sigma, 6)
```

### f
```
classify = function(Y, Name) {
    category = rep(0, length(Y[, 1]))
    category[which(Y[, 1] > 0 & Y[, 2] > 0)] = 1
    category[which(Y[, 1] < 0 & Y[, 2] > 0)] = 2
    category[which(Y[, 1] < 0 & Y[, 2] < 0)] = 3
    category[which(Y[, 1] > 0 & Y[, 2] < 0)] = 4

    print("PC1 > 0 and PC2 > 0: ")
    print(Name[which(category == 1)])

    print("PC1 < 0 and PC2 > 0: ")
    print(Name[which(category == 2)])

    print("PC1 < 0 and PC2 < 0: ")
    print(Name[which(category == 3)])

    print("PC1 > 0 and PC2 < 0: ")
    print(Name[which(category == 4)])
}

classify(Y, Universities$University)
```

<h2 id = "2">Exercise 2</h2>

```
X = read.table(file = "Europe.txt", header = T, dec = ",")
```

### a
```
X = Europe[2:7]
pairs(X)
Sigma = cor(X)
E = eigen(Sigma)
plot(1:6, E$values, pch = 20)
lines(1:6, E$values)
```
### b
```
print(round(Sigma, 4))
```
### c
```
library(iplots)
X_s = scale(X, center = T, scale = T)
Y = X_s %*% E$vectors

ihist(Y[, 1])
ihist(X$CPI)
ihist(X$UNE)
ihist(X$INP)
ihist(X$BOP)
ihist(X$PRC)
ihist(X$UN)

ihist(Y[, 2])
ihist(X$CPI)
ihist(X$UNE)
ihist(X$INP)
ihist(X$BOP)
ihist(X$PRC)
ihist(X$UN)
```

### d
```
options(digits = 4)
pca_cor = princomp(covmat = Sigma)
get_pca_table(pca_cor, Sigma, 6)
```

### f
```
classify(Y, Europe$i..Country)
```

[Back to top](#0)