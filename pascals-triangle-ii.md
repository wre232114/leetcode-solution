## 1. Question
https://leetcode.com/problems/pascals-triangle-ii/

```
Given an integer rowIndex, return the rowIndexth (0-indexed) row of the Pascal's triangle.

In Pascal's triangle, each number is the sum of the two numbers directly above it as shown:


 

Example 1:

Input: rowIndex = 3
Output: [1,3,3,1]
Example 2:

Input: rowIndex = 0
Output: [1]
Example 3:

Input: rowIndex = 1
Output: [1,1]
 

Constraints:

0 <= rowIndex <= 33
```

## 2. Solution
In this case, just one row should be returned. so we just need two rows to store the result. the equation is `row[j] = last_row[j-1] + last_row[j]`, and swap last_row row each time.

## 3. Code
### 3.1 rust
```rust
impl Solution {
    pub fn get_row(row_index: i32) -> Vec<i32> {
        let row_index = row_index as usize + 1;
        let mut last_row = Vec::with_capacity(row_index);
        let mut row = Vec::with_capacity(row_index);
        
        for i in 0..row_index {
            last_row.push(0);
            row.push(0);
        }

        last_row[0] = 1;
        
        for i in 1..row_index {
            row[0] = 1;
            
            for j in 1..i {
                row[j] = last_row[j - 1] + last_row[j];
            }
            
            row[i] = 1;
            std::mem::swap(&mut last_row, &mut row);
        }
        
        
        last_row
    }
}
```
