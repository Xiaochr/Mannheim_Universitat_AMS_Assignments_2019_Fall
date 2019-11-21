[Back to menu](/README.md)

<h1 id = "0">Assignment 10</h1>

## Contents

- [Exercise 1](#1)
- [Exercise 3](#3)

<h2 id = "1">Exercise 1</h2>

```
students2008 = read.table(file = "students2008.txt", header = T, dec = ",")
```

### a
```
pairs(X[, 1:3], pch = X[, 4] + 15, col = X[, 4] + 1, asp = 1)
```
### b
```
for (i in c(1:3)) {
    boxplot(X[, i] ~ X[, 4], names = c("male", "female"))
}
```
### c
```
male_data = X[which(X["sex"] == 0),1:3]
female_data = X[which(X["sex"] == 1), 1:3]

male_mean = apply(male_data, 2, mean)
female_mean = apply(female_data, 2, mean)
cov_male = cov(male_data)
cov_female = cov(female_data)

print(male_mean)
print(female_mean)
print(cov_male)
print(cov_female)
```
### d
```
male_portion = length(male_data[, 1]) / length(X[, 1])
female_portion = length(female_data[, 1]) / length(X[, 1])
print(male_portion)
print(female_portion)
```
### e
```
plot(X[, 1], X[, 2], pch = X[, 4] + 15, col = X[, 4] + 1, asp = 1)

x1 = seq(150, 200, length = 40)
x2 = seq(40, 110, length = 40)

draw_distribution = function(mu1, mu2, sqr_sig11, sqr_sig22, rho12, level) {
    bivariate_normal = function(x1, x2) {
        1 / (2 * pi * sqr_sig11 * sqr_sig22 * sqrt(1 - rho12 ^ 2)) * exp(-1 / (2 * (1 - rho12 ^ 2)) * ((x1 - mu1) ^ 2 / (sqr_sig11 ^ 2)
        - 2 * rho12 * (x1 - mu1) / sqr_sig11 * (x2 - mu2) / sqr_sig22
        + (x2 - mu2) ^ 2 / (sqr_sig22 ^ 2)))
    }
    z = outer(x1, x2, bivariate_normal)
    
    contour(x1, x2, z, levels = level, add = TRUE, drawlabels = FALSE, asp = 1)
}

cor_male_2dim = cov2cor(cov_male[1:2, 1:2])
cor_female_2dim = cov2cor(cov_female[1:2, 1:2])
male_level = 1 / (2 * pi * sqrt(det(cov_male[1:2, 1:2]))) * exp(-0.5 * qchisq(0.95, 2))
female_level = 1 / (2 * pi * sqrt(det(cov_female[1:2, 1:2]))) * exp(-0.5 * qchisq(0.95, 2))

draw_distribution(male_mean[1], male_mean[2],
                  sqrt(cov_male[1, 1]), sqrt(cov_male[2, 2]),
                  cor_male_2dim[1, 2], male_level)
draw_distribution(female_mean[1], female_mean[2],
                  sqrt(cov_female[1, 1]), sqrt(cov_female[2, 2]),
                  cor_female_2dim[1, 2], female_level)

percentage_in_ellipse = function(data, mu, Sigma, n) {
    sum(mahalanobis(data, mu, Sigma) <= qchisq(0.95, 2)) / n
}

male_ratio = percentage_in_ellipse(male_data[, 1:2], male_mean[1:2],
                                   cov_male[1:2, 1:2], length(male_data[, 1]))
female_ratio = percentage_in_ellipse(female_data[, 1:2], female_mean[1:2],
                                   cov_female[1:2, 1:2], length(female_data[, 1]))

print(male_ratio)
print(female_ratio)
```
### f
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
### g
```
predict_function(X[, 1:3], cov_male, cov_female, male_mean, female_mean, X["sex"])
```

<h2 id = "3">Exercise 3</h2>

```
students2009 = read.table(file = "students2009.txt", header = T, dec = ",")
```
### f
```
predict_function(X_2009[, 1:2], cov_male[1:2, 1:2], cov_female[1:2, 1:2], male_mean[1:2], female_mean[1:2], X_2009["sex"])
```
### g
```
predict_function(X_2009[, 1:3], cov_male, cov_female, male_mean, female_mean, X_2009["sex"])
```

---

[Back to top](#0)