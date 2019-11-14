[Back to menu](/README.md)

<h1 id = "0">Assignment 9</h1>

## Contents

- [Exercise 2](#2)
- [Exercise 3](#3)

<h2 id = "2">Exercise 2</h2>

```
Personality2019 = read.table(file = "Personality2019.txt", header = T, dec = ",")

X = scale(Personality2019[2:length(Personality2019)], center = TRUE, scale = FALSE)
```

### a
```
E = eigen(cov(X))
Y = X %*% E$vectors

plot(Y[, 1], Y[, 2], type = "n", xlim = c(-2, 2), ylim = c(-2, 2))
text(Y[, 1], Y[, 2], Personality2019$Name, cex = 0.6)

pca = princomp(covmat = cov(X))
print(summary(pca))
```

### b
```
col_names = colnames(Personality2019[2:length(Personality2019)])
len = E$vectors[, 1] ^ 2 + E$vectors[, 2] ^ 2
len_sorted = sort(len, decreasing = T)
for (i in c(1:4)) {
    j = which(len == len_sorted[i])
    arrows(0, 0, 3 * E$vectors[j, 1], 3 * E$vectors[j, 2], length = 0.1, col = "blue")
    text(3.4 * E$vectors[j, 1], 3.4 * E$vectors[j, 2], col_names[j], cex = 0.7, col = "blue")
}
```

### c
```
blank = rep(0, 32)
me = blank
me[29] = me[25] = me[21] = me[17] = me[13] = me[9] = me[5] = me[1] = 1
ch = blank
ch[2] = ch[6] = ch[10] = ch[14] = ch[18] = ch[22] = ch[26] = ch[30] = 1
sa = blank
sa[3] = sa[7] = sa[11] = sa[15] = sa[19] = sa[23] = sa[27] = sa[31] = 1
ph = blank
ph[4] = ph[8] = ph[12] = ph[16] = ph[20] = ph[24] = ph[28] = ph[32] = 1

X_mean = apply(Personality2019[2:length(Personality2019)], 2, mean)
me_c = me - X_mean
ch_c = ch - X_mean
sa_c = sa - X_mean
ph_c = ph - X_mean

c_person = rbind(me_c, ch_c, sa_c, ph_c)
Y_c = c_person %*% E$vectors
c_names = c("melancholic", "choleric", "sanguine", "phlegmatic")

text(Y_c[, 1], Y_c[, 2], c_names, cex = 0.6, col = "green")
```

### d
```
uns = me + ch
st = ph + sa
intro = ph + me
extro = ch + sa

uns_c = uns - X_mean
st_c = st - X_mean
intro_c = intro - X_mean
extro_c = extro - X_mean

d_person = rbind(uns_c, st_c, intro_c, extro_c)
Y_d = d_person %*% E$vectors
d_names = c("unstable", "stable", "introverted", "extroverted")

text(Y_d[, 1], Y_d[, 2], d_names, cex = 0.6, col = "red")
```

### e
```
Delta = as.matrix(dist(Personality2019[2:length(Personality2019)], "manhattan"))

n = length(Delta[, 1])
J = matrix(rep(1, n ^ 2), n, n)
I = diag(rep(1, n))
H = I - 1 / n * J
Delta_2 = Delta ^ 2

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

### f
```
Lambda_2 = diag(E_Q$values[1:2])
Y_hat = E_Q$vectors[, 1:2] %*% sqrt(Lambda_2)
plot(Y_hat[, 1], Y_hat[, 2], type = "n", xlim = c(-15, 15), ylim = c(-15, 15))
text(Y_hat[, 1], Y_hat[, 2], Personality2019$Name, cex = 0.8)
```

### g
```
cluster_2 = hclust(dist(X, "euclidean"), method = "average")
plot(cluster_2, hang = -1)
rect.hclust(cluster_2, k = 4)
```

<h2 id = "3">Exercise 3</h2>

```
Personality = read.table(file = "Personality.txt", header = T, dec = ",")
```

### a
```
R = cor(Personality)
J = matrix(rep(1, length(R)), 32, 32)
Delta = J - R

cluster_3 = hclust(as.dist(Delta), method = "average")
plot(cluster_3, hang = -1)
rect.hclust(cluster_3, k = 4)
```

### b
```
n = length(Delta[, 1])
J = matrix(rep(1, n ^ 2), n, n)
I = diag(rep(1, n))
H = I - 1 / n * J
Delta_2 = Delta ^ 2

Q = - 1 / 2 * H %*% Delta_2 %*% H
E_Q = eigen(Q)
Lambda_2 = diag(E_Q$values[1:2])
Y_hat = E_Q$vectors[, 1:2] %*% sqrt(Lambda_2)

plot(Y_hat[, 1], Y_hat[, 2], type = "n", xlim = c(-0.8, 0.8), ylim = c(-0.8, 0.8))
text(Y_hat[, 1], Y_hat[, 2], col_names, cex = 0.8, col = cutree(cluster_3, k = 4))
```

### c
```
ch = ch * 2
sa = sa * 3
ph = ph * 4

label = me + ch + sa + ph
plot(Y_hat[, 1], Y_hat[, 2], type = "n", xlim = c(-0.8, 0.8), ylim = c(-0.8, 0.8))
text(Y_hat[, 1], Y_hat[, 2], col_names, cex = 0.8, col = label)
```

---

[Back to top](#0)