[Back to menu](/README.md)

<h1 id = "0">Assignment 6</h1>

## contents

- [Exercise 2](#2)
- [Exercise 3](#3)
- [Exercise 4](#4)


<h2 id = "2">Exercise 2</h2>

```
Europe = read.table(file = "Europe.txt", header = T, dec = ",")
```

### a
```
X = Europe[2:7]
X_s = scale(X, center = T, scale = T)
Sigma = cor(X)
E = eigen(Sigma)
Y = X_s %*% E$vectors

plot(-Y[, 1], Y[, 2], type = "n", xlim = c(-6, 3), ylim = c(-6, 4))
text(-Y[, 1], Y[, 2], Europe$i..Country, cex = 0.6)

names = c("CPI", "UNE", "INP", "BOP", "PRC", "UN")
for (i in c(1:6)) {
    arrows(0, 0, -1.5 * E$vectors[i, 1], 1.5 * E$vectors[i, 2], length = 0.1, col = "blue")
    text(-1.8 * E$vectors[i, 1], 1.8 * E$vectors[i, 2], names[i], cex = 0.7, col = "blue")
}
```

### b
```
Rich_s = c(0, 0, 0, (3 * max(X[, 4]) - mean(X[, 4]))/sqrt(var(X[, 4])), 0, 0)
Y_rich = Rich_s %*% E$vectors

text(-Y_rich[1], Y_rich[2], "Rich", cex = 0.6, col = "red")
```

<h2 id = "3">Exercise 3</h2>

```
X = Europe[2:7]
X_s = scale(X, center = T, scale = T)
Sigma = cor(X)
E = eigen(Sigma)

get_A = function(i) {
    X_s %*% E$vectors[, 1:i] %*% t(E$vectors[, 1:i])
}
```

### i
```
print(get_A(1)[1:5,])
print(sum((get_A(1) - X_s) ^ 2))
```
### ii
```
print(get_A(2)[1:5,])
print(sum((get_A(2) - X_s) ^ 2))
```
### iii
```
print(get_A(3)[1:5,])
print(sum((get_A(3) - X_s) ^ 2))
```

<h2 id = "4">Exercise 4</h2>

```
Approx2dim = read.table(file = "Approx2dim.txt", header = T, dec = ",")
```

### i
```
plot(X$x1, X$x2, pch = 20, xlim = c(-4, 4), ylim = c(-3, 3))
abline(v = 0)
abline(h = 0)

reslm = lm(X$x2 ~ X$x1 - 1)
x2hatlm = reslm$fitted.values
abline(reslm, col = "blue")
points(X$x1, x2hatlm, pch = 20, col = "blue", cex = 0.8)

AE1 = sum((X$x2 - x2hatlm) ^ 2)
print(AE1)
```
### ii
```
Sigma = cor(X)
E = eigen(Sigma)
abline(a = 0, b = E$vectors[2] / E$vectors[1], col = "red")

X_s = scale(X, center = T, scale = F)
A = X_s %*% E$vectors[,1] %*% t(E$vectors[,1])
points(A, pch = 20, col = "red", cex = 0.8)

AE2 = sum((X - A) ^ 2)
print(AE2)
```

[Back to top](#0)