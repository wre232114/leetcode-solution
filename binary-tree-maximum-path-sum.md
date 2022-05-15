## 1. Question
https://leetcode.com/problems/binary-tree-maximum-path-sum

```
A path in a binary tree is a sequence of nodes where each pair of adjacent nodes in the sequence has an edge connecting them. A node can only appear in the sequence at most once. Note that the path does not need to pass through the root.

The path sum of a path is the sum of the node's values in the path.

Given the root of a binary tree, return the maximum path sum of any non-empty path.

 

Example 1:


Input: root = [1,2,3]
Output: 6
Explanation: The optimal path is 2 -> 1 -> 3 with a path sum of 2 + 1 + 3 = 6.
Example 2:


Input: root = [-10,9,20,null,null,15,7]
Output: 42
Explanation: The optimal path is 15 -> 20 -> 7 with a path sum of 15 + 20 + 7 = 42.
 

Constraints:

The number of nodes in the tree is in the range [1, 3 * 104].
-1000 <= Node.val <= 1000
```

## 2. Solution


## 3. Code
### 3.1 rust
```rust
// Definition for a binary tree node.
// #[derive(Debug, PartialEq, Eq)]
// pub struct TreeNode {
//   pub val: i32,
//   pub left: Option<Rc<RefCell<TreeNode>>>,
//   pub right: Option<Rc<RefCell<TreeNode>>>,
// }
// 
// impl TreeNode {
//   #[inline]
//   pub fn new(val: i32) -> Self {
//     TreeNode {
//       val,
//       left: None,
//       right: None
//     }
//   }
// }
use std::rc::Rc;
use std::cell::RefCell;
use std::collections::HashMap;
use std::cmp::max;

impl Solution {
    pub fn max_path_sum(root: Option<Rc<RefCell<TreeNode>>>) -> i32 {
        if root.is_none() {
            return 0;            
        }

        let (c, nc, f) = Self::post_traverse(root);
        max(f, max(c, nc))
    }
    
    fn post_traverse(root: Option<Rc<RefCell<TreeNode>>>) -> (i32, i32, i32) {
        let root = root.unwrap();
        let root = root.borrow();
        let val = root.val;
        let mut v_contains = vec![val];
        let mut v_not_contains = vec![];

        if root.left.is_some() {
            let (c, nc, f) = Self::post_traverse(root.left.clone());
            v_not_contains.push(max(f, max(c, nc)));
            v_contains.push(c + val);
        }

        if root.right.is_some() {
            let (c, nc, f) = Self::post_traverse(root.right.clone());
            v_not_contains.push(max(f, max(c, nc)));
            v_contains.push(c + val);
        }

        let res = (v_contains.iter().fold(i32::MIN, |acc, v| {
            if *v > acc {
                *v
            } else {
                acc
            }
        }), v_not_contains.iter().fold(i32::MIN, |acc, v| {
            if *v > acc {
                *v
            } else {
                acc
            }
        }), if root.left.is_some() && root.right.is_some() {
            v_contains[1] + v_contains[2] - v_contains[0]
        } else {
            i32::MIN
            });
        res
    }
}
```
