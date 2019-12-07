[Back to menu](/README.md)

<h1 id = "0">Assignment 11</h1>

<h2 id = "1">Exercise 1</h2>

### a
```
plot(as.factor(SwissLabor[, 1]) ~ SwissLabor[, "age"] + SwissLabor[, "education"] + SwissLabor[, "income"] + SwissLabor[, "foreign"], ylevels = c("yes", "no"))
boxplot(SwissLabor[, "age"] ~ SwissLabor[, "youngkids"])
boxplot(SwissLabor[, "education"] ~ SwissLabor[, "foreign"])
```

### b
```
i = SwissLabor[which(SwissLabor[, "youngkids"] == 1 & SwissLabor[, "education"] >= 10), "age"]
ii = SwissLabor[which(SwissLabor[, "youngkids"] == 1 & SwissLabor[, "education"] < 10), "age"]
iii = SwissLabor[which(SwissLabor[, "youngkids"] == 2 & SwissLabor[, "education"] < 10), "age"]

print(mean(i))
print(mean(ii))
print(mean(iii))
```

### c
```
age_2 = SwissLabor[, "age"] ^ 2
logit = glm(SwissLabor[, "participation"] ~ SwissLabor[, "age"] +
                age_2 + SwissLabor[, "education"] + SwissLabor[, "income"] +
                SwissLabor[, "foreign"] + SwissLabor[, "youngkids"] +
                SwissLabor[, "oldkids"], family = binomial(link = "logit"))
probit = glm(SwissLabor[, "participation"] ~ SwissLabor[, "age"] +
                age_2 + SwissLabor[, "education"] + SwissLabor[, "income"] +
                SwissLabor[, "foreign"] + SwissLabor[, "youngkids"] +
                SwissLabor[, "oldkids"], family = binomial(link = "probit"))

print(summary(logit))
print(summary(probit))
```

### d
```
pred_logit = rep("no", length(logit$fitted.values))
pred_logit[which(logit$fitted.values > 0.5)] = "yes"
pred_logit = as.factor(pred_logit)

pred_probit = rep("no", length(probit$fitted.values))
pred_probit[which(probit$fitted.values > 0.5)] = "yes"
pred_probit = as.factor(pred_probit)

get_aper = function(pred, target) {
    sum(pred != target) / length(pred)
}

get_p_R_2 = function(pred, target) {
    N1 = sum(target == "yes")
    NER = N1 / length(pred)
    if (NER > 0.5) {
        NER = 1 - NER
    }
    return(1 - get_aper(pred, target) / NER)
}

APER_logit = get_aper(pred_logit, SwissLabor[, "participation"])
p_R_2_logit = get_p_R_2(pred_logit, SwissLabor[, "participation"])

APER_probit = get_aper(pred_probit, SwissLabor[, "participation"])
p_R_2_probit = get_p_R_2(pred_probit, SwissLabor[, "participation"])

print(APER_logit)
print(p_R_2_logit)
print(APER_probit)
print(p_R_2_probit)
```

### e
```
numeric_target = as.numeric(SwissLabor[, "participation"]) - 1
print(sum(numeric_target))
print(sum(logit$fitted.values))
print(sum(probit$fitted.values))

numeric_foreign = as.numeric(SwissLabor[, "foreign"]) - 1
print(t(numeric_target) %*% numeric_foreign)
print(t(logit$fitted.values) %*% numeric_foreign)
```

### f
```
x = seq(0, 1, length = 100)
aper_list = c()

for (cutoff in x) {
    pred_temp = rep(1, length(probit$fitted.values))
    pred_temp[which(probit$fitted.values > cutoff)] = 2

    aper_list = append(aper_list, get_aper(pred_temp, as.numeric(SwissLabor[, "participation"])))
}

plot(x, aper_list, pch = 20)
lines(x, aper_list)

print(x[which(aper_list == min(aper_list))])
```

### g
```
all_data = cbind(SwissLabor[, "age"], age_2, SwissLabor[, "education"], SwissLabor[, "income"],
                SwissLabor[, "foreign"], SwissLabor[, "youngkids"], SwissLabor[, "oldkids"])
no_data = all_data[which(SwissLabor[, "participation"] == "no"),]
yes_data = all_data[which(SwissLabor[, "participation"] == "yes"),]

no_mean = apply(no_data, 2, mean)
yes_mean = apply(yes_data, 2, mean)


predict_function = function(pred_data, S1, S2, mu1, mu2, actu_label) {
    n1 = length(no_data[, 1])
    n2 = length(yes_data[, 1])
    S_p = (n1 - 1) / (n1 + n2 - 2) * S1 + (n2 - 1) / (n1 + n2 - 2) * S2

    lhs = as.matrix(pred_data) %*% solve(S_p) %*% (mu1 - mu2) -
    (0.5 * t(mu1 - mu2) %*% solve(S_p) %*% (mu1 + mu2))[1, 1]

    label = rep(1, length(pred_data[, 1]))
    label[which(lhs < log(n2 / n1))] = 2

    aper = get_aper(label, as.numeric(actu_label))
    print(aper)
}

predict_function(all_data, cov(no_data), cov(yes_data), no_mean, yes_mean, SwissLabor[, "participation"])
```

### h
```
regression1 = lm(as.numeric(SwissLabor[, "participation"]) ~ SwissLabor[, "age"] +
                age_2 + SwissLabor[, "education"] + SwissLabor[, "income"] +
                SwissLabor[, "foreign"] + SwissLabor[, "youngkids"] + SwissLabor[, "oldkids"])

pred1 = regression1$fitted.values
print(summary(regression1))
print(1 - sum(pred1 >= 0 & pred1 <= 1) / length(pred1))

regression2 = lm(as.numeric(SwissLabor[, "participation"]) ~ SwissLabor[, "age"] +
                age_2 + SwissLabor[, "education"] + SwissLabor[, "income"] +
                SwissLabor[, "youngkids"] + SwissLabor[, "oldkids"])

pred2 = regression2$fitted.values
print(summary(regression2))

regression3 = lm(as.numeric(SwissLabor[, "participation"]) ~
                SwissLabor[, "education"] + SwissLabor[, "income"] +
                SwissLabor[, "foreign"] + SwissLabor[, "youngkids"] + SwissLabor[, "oldkids"])

pred3 = regression3$fitted.values
print(summary(regression3))
```

---

[Back to top](#0)