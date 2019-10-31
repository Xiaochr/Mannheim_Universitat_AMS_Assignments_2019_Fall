[Back to menu](/README.md)

<h1 id = "0">Notes 7</h1>

- Exercise 1

a. $E(F_1) = E(F_3)$ &nbsp; **T**
	
b. $E(F_2F_3) = 1$ &nbsp; **F**
	
c. $E(\epsilon_3^2) = 1$ &nbsp; **F**
	
d. The covariance between $\epsilon_1$ and $\epsilon_2$ is 0 &nbsp; **T**
	
e. The correlation between $\epsilon_1$ and $\epsilon_2$ is 0 &nbsp; **T**
	
f. $\epsilon_1$ and $\epsilon_2$ are independent &nbsp; **F**

- If you want to select diag of a matrix as a diag matrix, you need to diag twice, like: 

```
diag_A = diag(diag(A))
```
Otherwise it will be a vector whose elements are the diag values. 

- The rotated axises are the columns of rotation matrix, **NOT** the rows of it! (pay attention to [1, 1][2, 1] and [1, 2][2, 2])
```
arrows(0, 0, 0.5 * v_L$rotmat[1, 1], 0.5 * v_L$rotmat[2, 1], length = 0.1)
arrows(0, 0, 0.5 * v_L$rotmat[1, 2], 0.5 * v_L$rotmat[2, 2], length = 0.1)
arrows(0, 0, -0.5 * v_L$rotmat[1, 1], -0.5 * v_L$rotmat[2, 1], length = 0.1)
arrows(0, 0, -0.5 * v_L$rotmat[1, 2], -0.5 * v_L$rotmat[2, 2], length = 0.1)
```

---

[Back to Top](#0)