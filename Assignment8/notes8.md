[Back to menu](/README.md)

<h1 id = "0">Notes 8</h1>

- A more elegant way to calculate Euclidean distance: (not typing formula by hand, but use mahalanobis function)

```
print(sqrt(mahalanobis(Y_hat_2_full, center = Y_con_full, cov = diag(1, 20))))
print(sqrt(mahalanobis(Y_hat_2_full, center = Y_ind_full, cov = diag(1, 20))))
```

- Use the number of disagreement distance matrix, then no need to mean-center data. 

- Check whether is Euclidean

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

- A more elegant way to mean-center data: 

```
Xbar=apply(Preferences[,-1],2,mean)
conformist=ifelse(Xbar>=0,1,-1)-Xbar
individualist=ifelse(Xbar>0,-1,1)-Xbar
```






---

[Back to Top](#0)