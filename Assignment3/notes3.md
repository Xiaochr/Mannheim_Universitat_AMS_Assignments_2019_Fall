<h1 id = "0">Notes 3</h1>

- For multivariate normal distributions: 
    - Understand the value of density function
    - Understand the relationship between density contour (which includes xx% of probability mass) and chisquare. 

- **Proof of Exercise 6!**

- Bivariate normal distribution: 

```
f = function(x1, x2) { 
    rho12 = (Sigma[1, 2] / sqrt(Sigma[1, 1] * Sigma[2, 2])) 
    1 / (2 * pi * sqrt(det(Sigma))) * exp(-1 / (2 * (1 - rho12 ^ 2)) * ((x1 - mu[1]) ^ 2 / (Sigma[1, 1]) 
    - 2 * rho12 * (x1 - mu[1]) / sqrt(Sigma[1, 1]) * (x2 - mu[2]) / sqrt(Sigma[2, 2]) 
    + (x2 - mu[2]) ^ 2 / (Sigma[2, 2]))) 
    }
```

- A more elegant version of **Exercise 7 # c**: 

Original: 
```
count = 0
for (i in mahalanobis(X, mu, Sigma)) {
    if (i < c2)
        count = count + 1
}
ratio = count / length(height)
print(ratio)
```

Improved: 
```
m=c(m1,m2)
n=length(height)
sum(mahalanobis(heightweight,m,S)<=c2)/n
```

- A more elegant version of **Exercise 8 # c**:

Original:
```
y = rep(1, 16)
y[max_index] = 2
Y = cbind(X, y)

pairs(Y[1:3], pch = 20, asp = 1, col = c("black", "red")[unclass(Y$y)])
```

Improved: 
```
increase=c(rep(1,9),2,rep(1,6))
pairs(outlier3dim,col=increase,pch=16,cex=increase)
```

[Back to Top](#0)