## 1. Question
https://leetcode.com/problems/pascals-triangle/

```
Given an integer numRows, return the first numRows of Pascal's triangle.

In Pascal's triangle, each number is the sum of the two numbers directly above it as shown:


 

Example 1:

Input: numRows = 5
Output: [[1],[1,1],[1,2,1],[1,3,3,1],[1,4,6,4,1]]
Example 2:

Input: numRows = 1
Output: [[1]]
 

Constraints:

1 <= numRows <= 30

```

## 2. Solution
the pascals triangles is like following:

```
    1
   1  1
  1  2  1
 1 3  3  1
```
the position (i, j) ,i is the row number the j is the column number.

Using Dynamic Programing is the best choice, the equation is `(i, j) = (i - 1, j - 1) + (j - 1, j)`.

## 3. Code
init the triangle first, then calculate (i, j) using the equation.

### 3.1 rust

```rust
impl Solution {
    pub fn generate(num_rows: i32) -> Vec<Vec<i32>> {
        let num_rows = num_rows as usize;
        let mut triangles = Vec::with_capacity(num_rows);
        
        for i in 0..num_rows {
            let mut row_i: Vec<i32> = Vec::with_capacity(i + 1);
            
            for j in 0..i+1 {
                if j == 0 || j == i {
                    row_i.push(1);
                } else  {
                    row_i.push(0);
                }
            }
    
            triangles.push(row_i);
        }
        
        for i in 2..num_rows {
            for j in 1..(triangles[i].len() - 1) {
                triangles[i][j] = triangles[i - 1][j - 1] + triangles[i - 1][j];
            }
        }
        
        triangles
    }
}
```

