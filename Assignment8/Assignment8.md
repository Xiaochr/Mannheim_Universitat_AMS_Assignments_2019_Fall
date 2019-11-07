[Back to menu](/README.md)

<h1 id = "0">Assignment 8</h1>

<h2 id = "2">Exercise 2</h2>

```
library(HSAUR)
n = length(voting[, 1])
J = matrix(rep(1, n ^ 2), n, n)
I = diag(rep(1, n))
H = I - 1 / n * J
Delta_2 = voting ^ 2
```

### a
```
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
### b
```
Lambda_2 = diag(E_Q$values[1:2])
Y_hat = E_Q$vectors[, 1:2] %*% sqrt(Lambda_2)
plot(Y_hat, type = "n", ylim = c(-6, 8), xlim = c(-12, 8))
text(Y_hat, colnames(voting), cex = 0.8)
```
### c
```
print(all(round(Y_hat, 4) == round(cmdscale(voting), 4)))
```

<h2 id = "3">Exercise 3</h2>

```
Preferences = read.table(file = "Preferences.txt", header = T, dec = ",")

X = scale(Preferences[2:21], center = T, scale = F)
```

### a
#### i
```
Q = X %*% t(X)
E_Q = eigen(Q)
Lambda_2 = diag(E_Q$values[1:2])
Y_hat_1 = E_Q$vectors[, 1:2] %*% sqrt(Lambda_2)
print(Y_hat_1[1:4, ])
```
#### ii
```
E = eigen(cov(X))
Y_hat_2_full = (X %*% E$vectors)
Y_hat_2 = (X %*% E$vectors)[, 1:2]
print(Y_hat_2[1:4, ])
```

### b
```
plot(Y_hat_2[, 1], Y_hat_2[, 2], type = "n")
text(Y_hat_2[, 1], Y_hat_2[, 2], Preferences$Name, cex = 0.6)

pca = princomp(covmat = cov(X))
print(summary(pca))
```
### c
```
names = colnames(Preferences)[2:21]
len = E$vectors[, 1] ^ 2 + E$vectors[, 2] ^ 2
len_sorted = sort(len, decreasing = T)
for (i in c(1:4)) {
    j = which(len == len_sorted[i])
    arrows(0, 0, -3 * E$vectors[j, 1], 3 * E$vectors[j, 2], length = 0.1, col = "blue")
    text(-3.4 * E$vectors[j, 1], 3.4 * E$vectors[j, 2], names[j], cex = 0.7, col = "blue")
}
```
### d
```
X_raw = Preferences[2:21]
judge = apply(X_raw, 2, sum)
for (i in c(1:20)) {
    if (judge[i] >= 0) {
        judge[i] = 1
    }
    else {
        judge[i] = -1
    }
}

con = judge
ind = -judge

for (i in c(1:20)) {
    con[i] = con[i] - mean(X_raw[, i])
    ind[i] = ind[i] - mean(X_raw[, i])
}

Y_con_full = con %*% E$vectors
Y_ind_full = ind %*% E$vectors
Y_con = con %*% E$vectors[, 1:2]
Y_ind = ind %*% E$vectors[, 1:2]

text(Y_con[, 1], Y_con[, 2], "conformist", cex = 0.8, col = "red")
text(Y_ind[, 1], Y_ind[, 2], "individualist", cex = 0.8, col = "red")

# print(Y_con[, 1], Y_con[, 2])
# print(Y_ind[, 1], Y_ind[, 2])
```

### e
```
Dist_c = function(X) {
    sqrt((X - Y_con) %*% t(X - Y_con))
}
Dist_i = function(X) {
    sqrt((X - Y_ind) %*% t(X - Y_ind))
}
dis_con = apply(Y_hat_2, 1, Dist_c)
dis_ind = apply(Y_hat_2, 1, Dist_i)

print(sqrt(mahalanobis(Y_hat_2, center = Y_con, cov = diag(1, 2))))
print(sqrt(mahalanobis(Y_hat_2, center = Y_ind, cov = diag(1, 2))))

print(dis_con)
print(dis_ind)

print(Preferences$Name[order(dis_con)])
print(Preferences$Name[order(dis_ind)])
```

### f
```
Dist_c_full = function(X) {
    sqrt((X - Y_con_full) %*% t(X - Y_con_full))
}
Dist_i_full = function(X) {
    sqrt((X - Y_ind_full) %*% t(X - Y_ind_full))
}
dis_con_full = apply(Y_hat_2_full, 1, Dist_c_full)
dis_ind_full = apply(Y_hat_2_full, 1, Dist_i_full)

# print(sqrt(mahalanobis(Y_hat_2_full, center = Y_con_full, cov = diag(1, 20))))
# print(sqrt(mahalanobis(Y_hat_2_full, center = Y_ind_full, cov = diag(1, 20))))

print(dis_con_full)
print(dis_ind_full)

print(Preferences$Name[order(dis_con_full)])
print(Preferences$Name[order(dis_ind_full)])
```

<h2 id = "3">Exercise 3</h2>

### a
```
Delta = matrix(rep(0, 23 * 23), 23, 23)
for (i in c(1:23)) {
    for (j in c(1:23)) {
        Delta[i, j] = 0.5 * (20 - sum(X_raw[i,] * t(X_raw[j,])))
    }
}

n = 23
J = matrix(rep(1, n ^ 2), n, n)
I = diag(rep(1, n))
H = I - 1 / n * J

Q = - 1 / 2 * H %*% Delta ^ 2 %*% H
E_Q = eigen(Q)
check_eu(E_Q)
```

### b
```
Lambda_2 = diag(E_Q$values[1:2])
Y_hat = E_Q$vectors[, 1:2] %*% sqrt(Lambda_2)
plot(Y_hat, type = "n")
text(Y_hat[, 1], Y_hat[, 2], Preferences$Name, cex = 0.8)
```

---

[Back to top](#0)