## 1. Question
https://leetcode.com/problems/time-needed-to-inform-all-employees/

```
A company has n employees with a unique ID for each employee from 0 to n - 1. The head of the company is the one with headID.

Each employee has one direct manager given in the manager array where manager[i] is the direct manager of the i-th employee, manager[headID] = -1. Also, it is guaranteed that the subordination relationships have a tree structure.

The head of the company wants to inform all the company employees of an urgent piece of news. He will inform his direct subordinates, and they will inform their subordinates, and so on until all employees know about the urgent news.

The i-th employee needs informTime[i] minutes to inform all of his direct subordinates (i.e., After informTime[i] minutes, all his direct subordinates can start spreading the news).

Return the number of minutes needed to inform all the employees about the urgent news.

 

Example 1:

Input: n = 1, headID = 0, manager = [-1], informTime = [0]
Output: 0
Explanation: The head of the company is the only employee in the company.
Example 2:


Input: n = 6, headID = 2, manager = [2,2,-1,2,2,2], informTime = [0,0,1,0,0,0]
Output: 1
Explanation: The head of the company with id = 2 is the direct manager of all the employees in the company and needs 1 minute to inform them all.
The tree structure of the employees in the company is shown.
 

Constraints:

1 <= n <= 105
0 <= headID < n
manager.length == n
0 <= manager[i] < n
manager[headID] == -1
informTime.length == n
0 <= informTime[i] <= 1000
informTime[i] == 0 if employee i has no subordinates.
It is guaranteed that all the employees can be informed.
```

## 2. Solution
Dynamic Programming, O(n) solution:

Define the time a manager needed to inform all the of its employees is `dp(i)`. The State Transition Equation is `dp(i) = max(all dp(employee of i) + inform_time[i])`.

But the given input of this problem is parent of the tree node(the manager array, not a traditional tree struct from parent to child).
Converting the manger array to a tree structure is expensive, so we should invert the transition equation to `dp(manager[i]) = max(dp(manager[i]), dp[i] + inform_time[manager[i]])`, and traver from child to parent using bfs.

See the code below, note that the `emp_cnt` map records the employees number of a manager. a manager should be reached after all of its employees being handled(what the emp_cnt[&m] == 1 do).

## 3. Code
```rust
impl Solution {
    pub fn num_of_minutes(n: i32, head_id: i32, manager: Vec<i32>, inform_time: Vec<i32>) -> i32 {
        let mut leafs = std::collections::VecDeque::new();
        let mut dp = inform_time.clone();
        let mut emp_cnt = std::collections::HashMap::new();

        for emp in 0..n {
            if inform_time[emp as usize] == 0 {
                leafs.push_back(emp as usize);
            }

            if emp != head_id {
                emp_cnt
                    .entry(manager[emp as usize] as usize)
                    .and_modify(|v| *v += 1)
                    .or_insert(1);
            }
        }

        while !leafs.is_empty() {
            let head = leafs.pop_front().unwrap();
            let m = if manager[head] == -1 {
                continue;
            } else {
                manager[head] as usize
            };

            dp[m] = std::cmp::max(dp[m], inform_time[m] + dp[head]);

            if emp_cnt[&m] == 1 {
                leafs.push_back(m);
            } else {
                emp_cnt.entry(m).and_modify(|v| *v -= 1);
            }
        }

        dp[head_id as usize]
    }
}
```
