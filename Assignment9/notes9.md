[Back to menu](/README.md)

<h1 id = "0">Notes 9</h1>

- Construct a cluster tree

```
cluster_2 = hclust(dist(X, "euclidean"), method = "average")
plot(cluster_2, hang = -1)
rect.hclust(cluster_2, k = 4)
```
- Remember that you must supply *hclust()* with a distance matrix object. Use *as.dist(m)*

- Color the plot with the result of clustering. (Using *cutree()*)
```
plot(Y_hat[, 1], Y_hat[, 2], type = "n", xlim = c(-0.8, 0.8), ylim = c(-0.8, 0.8))
text(Y_hat[, 1], Y_hat[, 2], col_names, cex = 0.8, col = cutree(cluster_3, k = 4))
```

---

[Back to Top](#0)