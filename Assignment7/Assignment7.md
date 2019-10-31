[Back to menu](/README.md)

<h1 id = "0">Assignment 7</h1>

## Exercise 3

```
Europe = read.table(file = "Europe.txt", header = T, dec = ",")
```

### a
```
X = Europe[2:7]
X_s = scale(X, center = T, scale = T)
R = cor(X)
E = eigen(R)

L = round(E$vectors %*% sqrt(diag(E$values)), 4)
print(L[, 1:2])

comu = round(L[, 1] ^ 2 + L[, 2] ^ 2, 4)
print(comu)

sp_var = round(c(1, 1, 1, 1, 1, 1) - comu, 4)
print(sp_var)

prop = round(c(sum(L[, 1] ^ 2) / 6, sum(L[, 2] ^ 2) / 6), 4)
print(prop)
```

### b
```
v_L = varimax(L[, 1:2])
print(v_L)

v_comu = round(v_L$loadings[, 1] ^ 2 + v_L$loadings[, 2] ^ 2, 4)
print(v_comu)

v_sp_var = round(c(1, 1, 1, 1, 1, 1) - v_comu, 4)
print(v_sp_var)

names = c("CPI", "UNE", "INP", "BOP", "PRC", "UN")
plot(L[, 1], L[, 2], pch = 20)
text(L[, 1], L[, 2] - 0.05, names, cex = 0.6)
arrows(0, 0, 0.5 * v_L$rotmat[1, 1], 0.5 * v_L$rotmat[2, 1], length = 0.1)
arrows(0, 0, 0.5 * v_L$rotmat[1, 2], 0.5 * v_L$rotmat[2, 2], length = 0.1)
arrows(0, 0, -0.5 * v_L$rotmat[1, 1], -0.5 * v_L$rotmat[2, 1], length = 0.1)
arrows(0, 0, -0.5 * v_L$rotmat[1, 2], -0.5 * v_L$rotmat[2, 2], length = 0.1)
```

### c
```
rotmat = round(v_L$rotmat, 4)
print(rotmat)

print(eigen(cov(L[, 1:2]))$vectors)
```

### d
```
Psi = round(diag(diag(R - L[, 1:2] %*% t(L[, 1:2]))), 4)
print(round(R - L[, 1:2] %*% t(L[, 1:2]) - Psi, 4))
```

[Back to top](#0)