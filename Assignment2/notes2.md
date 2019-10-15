[Back to menu](/README.md)

<h1 id = "0">Notes 2</h1>

- A standard procedure of drawing persp and contour: 

```
x1 = seq(-1, 1, le = 100)
x2 = x1
draw_graphs = function(A) {
    f = function(x1, x2) {
        function of x1, x2
    }
    z = outer(x1, x2, f)
    persp(x1, x2, z, ticktype = "detailed")
    contour(x1, x2, z)
}
```

- In `z = outer(x1, x2, f)`, function **f** should have 2 parameters *x1* and *x2*. 

- Draw axis for oval contour: 

```
len1 = sqrt(c2 / eigen(A)$values[1])
len2 = sqrt(c2 / eigen(A)$values[2])
arrows(0, 0, len1 * eigen(A)$vectors[1, 1], len1 * eigen(A)$vectors[2, 1], length = 0.1, asp = 1)
arrows(0, 0, len2 * eigen(A)$vectors[1, 2], len2 * eigen(A)$vectors[2, 2], length = 0.1, asp = 1)
```

[Back to Top](#0)