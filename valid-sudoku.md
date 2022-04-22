## 1. question
https://leetcode.com/problems/valid-sudoku/

Determine if a 9 x 9 Sudoku board is valid. Only the filled cells need to be validated according to the following rules:

1. Each row must contain the digits 1-9 without repetition.
2. Each column must contain the digits 1-9 without repetition.
3. Each of the nine 3 x 3 sub-boxes of the grid must contain the digits 1-9 without repetition.

Note:

* A Sudoku board (partially filled) could be valid but is not necessarily solvable.
* Only the filled cells need to be validated according to the mentioned rules.

## solution
traverse all row, column and each 3*3 matrix, using hashset to store whether a digit presented.

## code
### rust
faster than 99%.

```rust
use std::collections::HashSet;

impl Solution {
    pub fn check(val: char, visited: &mut HashSet<char>) -> bool {
        if val == '.' {
            return true;
        } else if visited.contains(&val) {
            return false;
        } else {
            visited.insert(val);
            return true;
        }
    }

    pub fn is_valid_sudoku(board: Vec<Vec<char>>) -> bool {
        for i in 0..9 {
            let mut visited_i = HashSet::new();
            let mut visited_j = HashSet::new();

            for j in 0..9 {
                if !Self::check(board[i][j], &mut visited_i) {
                    return false;
                }
                if !Self::check(board[j][i], &mut visited_j) {
                    return false;
                } 
            }
        }
        
        for i_g in 0..3 {
            for j_g in 0..3 {
                let mut visited = HashSet::new();
                
                for i in i_g * 3..i_g * 3 + 3 {
                    for j in j_g * 3..j_g*3 + 3 {
                        if !Self::check(board[i][j], &mut visited) {
                            return false;
                        }
                    }
                }
            }
        }
        
        true
    }
}
```
