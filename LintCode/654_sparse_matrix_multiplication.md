# <center>654 - Sparse Matrix Multiplication (M)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: Math, 2D Array
* Difficulty: Medium
* Company: Facebook, LinkedIn
* Link: https://www.lintcode.com/problem/sparse-matrix-multiplication

<br></br>



## Description
----
Given two Sparse Matrix A and B, return the result of AB.

You may assume that A's column number is equal to B's row number.

<br></br>



## Example
----
Input: 
```
[[1,0,0],[-1,0,3]]
[[7,0,0],[0,0,0],[0,0,1]]
```

Output:
```
[[7,0,0],[-7,0,3]]
```

Explanation:
```
A = [
  [ 1, 0, 0],
  [-1, 0, 3]
]

B = [
  [ 7, 0, 0 ],
  [ 0, 0, 0 ],
  [ 0, 0, 1 ]
]


     |  1 0 0 |   | 7 0 0 |   |  7 0 0 |
AB = | -1 0 3 | x | 0 0 0 | = | -7 0 3 |
                  | 0 0 1 |
```

<br></br>



## Solution
----
算法：
1. 常规方法，按照矩阵乘法执行。时间复杂度O(n^2 * (1 + n)) = O(n^2 + n^3) = O(n^3)
2. 以A为核心进行遍历，而非C。每个A[i][k]乘B[k][j]后，会被累加到C[i][j]。

   举例，若A矩阵2x3，B矩阵3x2，C矩阵2x2
		   A                 B              C
	  a00 , a01 , a02      b00 , b01      c00 , c01
	  a10 , a11 , a12      b10 , b11      c10 , c11
					  	   b20 , b21
   常规做法计算过程为遍历C，假设遍历到C[0][0]，计算C[0][0] = A[0][0] * B[0][0] + A[0][1] * B[1][0] + A[0][2] * B[2][0]
   改进的计算过程为遍历A，假设遍历到A[0][0]，A[0][0] * B[0][0]累加到C[0][0]，A[0][0] * B[0][1]累加到C[0][1]；遍历到A[0][1]，A[0][1] * B[1][0]累加到C[0][0]，A[0][]1 * B[1][1]累加到C[0][1]。对于常规做法，可发现是否为稀疏矩阵，无法优化，因为以C为核心。
   
   改进做法以A为核心，若A[i][k]为0，那么该元素不必进行相乘并累加的操作。

   具体流程为：
   1. 遍历A和B中每个元素，把非零元素转化成Element对象并存在数组中；
   2. 循环A和B中非零元素，计算A * B。

   时间复杂度分析：假设A为m * n，B为 n * k，转化Element时间复杂度为m * n + n * k。假设A和B中非零元素个数和为l，总的时间复杂度为m * n + n * k + l。

<br>



### Go
```go
func SparseMatrixMultiplication1(A, B [][]int) { // A: m * k  B: k * n res: m * n
	res := make([][]int, 0)
    if len(A) == 0 {
        return res
    }
    m, k, n := len(A), len(A[0]), len(B[0])
    for i := 0; i < m; i++ {
        tmp := make([]int, 0)
        for j := 0; j < n; j++ {
            val := 0
            for l := 0; l < k; l++ {
                val += A[i][l] * B[l][j]
            }
            tmp = append(tmp, val)
        }
        res = append(res, tmp)
    }
    
    return res
}
```

```go
type smElement struct {
    row int
    column int
    val int
}

func SparseMatrixMultiplication2(A, B [][]int) [][]int { // A: m * k  B: k * n res: m * n
    res := make([][]int, 0)
    if len(A) == 0 {
        return res
    }
    aElements, bElements := initElement(A), initElement(B)
    m, n := len(A), len(B[0])
    for i := 0; i < m; i++ {
        res = append(res, make([]int, n, n))
    }
    for _, a := range aElements {
        for _, b := range bElements {
            if a.column == b.row {
                res[a.row][b.column] += a.val * b.val
            }
        }
    }
    
    return res
}

func initElement(nums [][]int) []*smElement {
    res := make([]*smElement, 0)
    for k1, v1 := range nums {
        for k2, v2 := range v1 {
            if v2 != 0 {
                res = append(res, &smElement{k1, k2, v2})
            }
        }
    }
    
    return res
}
```

<br>
