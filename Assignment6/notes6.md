[Back to menu](/README.md)

<h1 id = "0">Notes 6</h1>

- **Draw Biplot**

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

- Add another point in the biplot without changing the original points. Remember how to standardize the point! 

```
Rich_s = c(0, 0, 0, (3 * max(X[, 4]) - mean(X[, 4]))/sqrt(var(X[, 4])), 0, 0)
Y_rich = Rich_s %*% E$vectors

text(-Y_rich[1], Y_rich[2], "Rich", cex = 0.6, col = "red")
```

- Calculate the approximating matrix $\mathbf{A}$

```
X_s = scale(X, center = T, scale = T)
Sigma = cor(X)
E = eigen(Sigma)

get_A = function(i) {
    X_s %*% E$vectors[, 1:i] %*% t(E$vectors[, 1:i])
}
```

- Approximation error: 

```
sum((get_A(1) - X_s) ^ 2)
```

[Back to Top](#0)