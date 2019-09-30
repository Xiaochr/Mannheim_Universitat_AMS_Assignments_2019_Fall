# Assigment 1

## Exercise 1

```
x1 = c(1, 4, 1, 3, 2, -6)
x2 = c(-1, 5, 2, 4, 1, -1)
x3 = c(-1, 5, 1, 0, -1, 3)
```

### a
```
print(2 * x1 - x2 + x3)
```

### b
#### i
```
print(x1 %*% x2)
```
#### ii
```
print(x2 %*% x3)
```
#### iii
```
print(x1 %*% x3)
```

### c
```
print(x1 %o% x2)
```

### d
```
print(sqrt(sum(x1 ^ 2)))
print(sqrt(sum(x2 ^ 2)))
print(sqrt(sum(x3 ^ 2)))
```

### e
#### i
```
print(((x2 %*% x1) / (x2 %*% x2)) * x2)
```
#### ii
```
print(((x3 %*% x1) / (x3 %*% x3)) * x3)
```

## Exercise 3
```
draw_vectors = function(X1, Y1, X2, Y2) {
    x = 0
    y = 0
    x1 = X1
    y1 = Y1
    x2 = X2
    y2 = Y2
    v1 = c(x1, y1)
    v2 = c(x2, y2)
    plot(x, y, xlim = c(0, 4), ylim = c(-0.2, 1.2),
         axes = FALSE, frame.plot = FALSE, ann = FALSE,
         type = "n", asp = 1)
    arrows(x, y, x1, y1, length = 0.1, col = "blue")
    text(x1 + 0.1, y1 + 0.1, labels = "x")

    arrows(x, y, x2, y2, length = 0.1, col = "red")
    text(x2 + 0.1, y2 + 0.1, labels = "y")

    v3 = ((v2 %*% v1) / (v2 %*% v2)) * v2
    arrows(x, y - 0.1, v3[1], v3[2] - 0.1, length = 0.1)
    lines(c(x1, v3[1]), c(y1, v3[2]), lty = 2)
}

draw_vectors(2, 1, 3, 0)
draw_vectors(2, 1, 3, 1)
```

## Exercise 4
```
m1 = matrix(c(3, 2, 1, 0, 2, 1, -1, 4, 3), 3, 3)
m2 = matrix(c(1, 5, 2, 1, -1, 3), 2, 3)
v1 = c(1, 4, 1)
v2 = c(-1, 5, 2)
```

### i
```
print(m1 %*% v1)'
```
### ii
```
print(m2 %*% v1)
```
### iii
```
print(v1 %*% m1 %*% v1)
```
### iv
```
print(m2 %*% m1)
```
### v
```
print(t(m1) %*% m1)
```
### vi
```
print(t(m2) %*% m2)
```
### vii
```
print(m1 %*% t(m1))
```
### viii
```
print(m2 %*% t(m2))
```


## Exercise 5
```
A = matrix(c(2, 1, 3, 4, 3, 8, -2, 2, 0, -4, 5, 1, -1, 3, 4, 1), 4, 4)
```
### a
```
print(det(A))
```
### b
```
m2 = matrix(c(1, 5, 2, 1, -1, 3), 2, 3)
print(det(t(m2) %*% m2))
```
### c
```
print(round(solve(A), digits = 4))
```

## Exercise 6
```
x = matrix(c(1, 0, 0, 0, 0, 1), 3, 2)
y = c(1, 3, 2)
print(x %*% solve(t(x) %*% x) %*% t(x) %*% y)
```