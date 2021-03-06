## 1. Question
You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed. All houses at this place are arranged in a circle. That means the first house is the neighbor of the last one. Meanwhile, adjacent houses have a security system connected, and it will automatically contact the police if two adjacent houses were broken into on the same night.

Given an integer array nums representing the amount of money of each house, return the maximum amount of money you can rob tonight without alerting the police.

 
**Example 1**:
```
**Input**: nums = [2,3,2]
**Output**: 3
Explanation: You cannot rob house 1 (money = 2) and then rob house 3 (money = 2), because they are adjacent houses.
```

**Example 2**:
```
**Input**: nums = [1,2,3,1]
**Output**: 4
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
Total amount you can rob = 1 + 3 = 4.
```
**Example 3**:
```
**Input**: nums = [1,2,3]
**Output**: 3
```

**Constraints**:

* 1 <= nums.length <= 100
* 0 <= nums[i] <= 1000

## 2. Solution
Using dynamic programming, if the robber robbed house i, then he can't rob house i + 1. So for the house i, the robber has two options, rob it or not.
So the state transition equation is max(nums[i] + rob(i + 2), rob(i + 1)), if the robber robbed i, the next he can rob is i + 2, otherwise he can rob i + 1.
The max value is the maximum value of the two options.

In the solution above, we define the state is `whether the robber robbed house i`, using this state we can construct the equation `max(nums[i] + rob(i + 2), rob(i + 1))`.
But note in this question the houses is a cycle, whether the robber can rob the last house depends on the first house he robbed. This means we have to deal with the cycle,
and the value of rob(i) may change if the robber robbed the first house or not.

So we have to eliminate the cycle: if the robber robbed the first house(we mean the first house is nums[0]), he can't rob the last house. so the houses he can rob become nums[0..nums.len()-1], and it does not contains cycle.
If the robber didn't rob the first house, the houses he can rob is `nums[1..]`, and it does not contains cycle too.

See the code below for details:

## 3. Code
Runtime 0 ms, faster than 100%.

### Rust
```rust
use std::collections::{HashMap};

impl Solution {
     pub fn rob(nums: Vec<i32>) -> i32 {
         if nums.len() == 1 {
             return nums[0];
         }

         let mut cache_1 = HashMap::new();
         let mut cache_2 = HashMap::new();

         std::cmp::max(
             Self::rob_internal(0, &nums[0..nums.len() - 1], &mut cache_1),
             Self::rob_internal(0, &nums[1..], &mut cache_2),
         )
     }

     pub fn rob_internal(i: usize, nums: &[i32], cache: &mut HashMap<usize, i32>) -> i32 {
         if nums.len() == 1 {
             return *nums.last().unwrap();
         } else if i >= nums.len() {
             return 0;
         } else if cache.contains_key(&i) {
             return *cache.get(&i).unwrap();
         }

         let non_robbed_i = Self::rob_internal(i + 1, nums, cache);

         let first = nums[i];
         let robbed_i = first + Self::rob_internal(i + 2, nums, cache);

         let res = std::cmp::max(robbed_i, non_robbed_i);

         cache.insert(i, res);
         res
     }
 }
```
