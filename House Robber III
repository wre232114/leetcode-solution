## 1. Question
* https://leetcode.com/problems/house-robber-iii/

```
The thief has found himself a new place for his thievery again. There is only one entrance to this area, called root.

Besides the root, each house has one and only one parent house. After a tour, the smart thief realized that all houses in this place form a binary tree. It will automatically contact the police if two directly-linked houses were broken into on the same night.

Given the root of the binary tree, return the maximum amount of money the thief can rob without alerting the police.

 

Example 1:


Input: root = [3,2,3,null,3,null,1]
Output: 7
Explanation: Maximum amount of money the thief can rob = 3 + 3 + 1 = 7.
Example 2:


Input: root = [3,4,5,1,3,null,1]
Output: 9
Explanation: Maximum amount of money the thief can rob = 4 + 5 = 9.
 

Constraints:

The number of nodes in the tree is in the range [1, 104].
0 <= Node.val <= 104
```

## 2. Solution
Using Dynamic programming, if the robber robbed the root, then he can only rob starting from root's grandchild. The State Equation is `max(root.val + rob(root.left.left) + rob(root.left.right) + rob(root.right.left) + root(right.right), rob(root.left) + rob(root.right))`

## 3. Code
### 3.1 Implemention 1: Recursion and Memory Search
```rust
use std::rc::Rc;
use std::cell::RefCell;
use std::collections::HashMap;
impl Solution {
    pub fn rob(root: Option<Rc<RefCell<TreeNode>>>) -> i32 {
        Solution::rob_internal(root, 1, &mut HashMap::new())
    }
    
    fn rob_internal(root: Option<Rc<RefCell<TreeNode>>>, root_id: u32, cache: &mut HashMap<u32, i32>) -> i32 {
        if cache.contains_key(&root_id) {
            return *cache.get(&root_id).unwrap();            
        } else if root.is_none() {
            return 0;
        }
        let root = root.unwrap();
        let root = root.borrow();

        if root.left.is_none() && root.right.is_none() {
            cache.insert(root_id, root.val);
            return root.val;
        }
        
        let robbed_root = {
            let left_child_child_max_val = if let Some(left) = &root.left {
                let left = left.borrow();
                Solution::rob_internal(left.left.clone(), root_id * 2 * 2, cache) + Solution::rob_internal(left.right.clone(), root_id * 2 * 2 + 1, cache)
            } else {
                0
            };
            
            let right_child_child_max_val = if let Some(right) = &root.right {
                let right = right.borrow();
                Solution::rob_internal(right.left.clone(), (root_id * 2 + 1) * 2, cache) + Solution::rob_internal(right.right.clone(), (root_id * 2 + 1) * 2 + 1, cache)
            } else {
                0
            };
            
            root.val + left_child_child_max_val + right_child_child_max_val
        };
        
        let non_robbed_root = Solution::rob_internal(root.left.clone(), root_id * 2, cache) + Solution::rob_internal(root.right.clone(), root_id * 2 + 1, cache);
        
        let res = std::cmp::max(robbed_root, non_robbed_root);
        cache.insert(root_id, res);
        res
    }
}
```

### 3.2 Implemention 2: Iteration
```rust
```
