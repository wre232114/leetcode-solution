## 1. Question
https://leetcode.com/problems/different-ways-to-add-parentheses/

```
Given a string expression of numbers and operators, return all possible results from computing all the different possible ways to group numbers and operators. You may return the answer in any order.

 

Example 1:

Input: expression = "2-1-1"
Output: [0,2]
Explanation:
((2-1)-1) = 0 
(2-(1-1)) = 2
Example 2:

Input: expression = "2*3-4*5"
Output: [-34,-14,-10,-10,10]
Explanation:
(2*(3-(4*5))) = -34 
((2*3)-(4*5)) = -14 
((2*(3-4))*5) = -10 
(2*((3-4)*5)) = -10 
(((2*3)-4)*5) = 10
 

Constraints:

1 <= expression.length <= 20
expression consists of digits and the operator '+', '-', and '*'.
All the integer values in the input expression are in the range [0, 99].
```

## 2. Solution
First we need to parse the expression expr to a token sequence, e.g. '2-10-1' to [2, '-', 10, '-', 1]. because the number's width maybe 2 like '10'.

Then we divide the token sequence into 2 part and traverse the different part recursively. e.g. '2-10-1' to '2-10','1' and '2','10-1'. then merge the result.

see the code below.

## 3. Code
### 3.1 rust
```rust
use std::collections::HashMap;

macro_rules! get_num {
    ($v:expr) => {{
        match $v {
            Token::Num(num) => num,
            _ => panic!("never be here"),
        }
    }};
}

macro_rules! get_op {
    ($v:expr) => {{
        match $v {
            Token::Op(op) => op,
            _ => panic!("never be here"),
        }
    }};
}

macro_rules! ex_op {
    ($op:expr, $left_v:expr, $right_v:expr) => {{
        match $op {
            Op::Add => $left_v + $right_v,
            Op::Sub => $left_v - $right_v,
            Op::Mul => $left_v * $right_v,
        }
    }};
}

impl Solution {
    pub fn diff_ways_to_compute(expression: String) -> Vec<i32> {
        let token_sequence = Self::parse_expression(&expression);
        Self::internal(&token_sequence, &mut HashMap::new())
    }

    fn internal(ts: &[Token], cache: &mut HashMap<String, Vec<i32>>) -> Vec<i32> {
        let key = to_string(ts);

        if cache.contains_key(&key) {
            cache.get(&key).unwrap();
        }

        if ts.len() == 0 {
            return vec![];
        } else if ts.len() == 1 {
            return vec![get_num!(ts[0])];
        } else if ts.len() == 3 {
            let left_v = get_num!(ts[0]);
            let op = get_op!(ts[1]);
            let right_v = get_num!(ts[2]);
            return vec![ex_op!(op, left_v, right_v)];
        }

        let mut i = 1;
        let mut ret = vec![];

        while i < ts.len() {
            let left_v = Self::internal(&ts[0..i], cache);
            let op = get_op!(ts[i]);
            let right_v = Self::internal(&ts[(i + 1)..], cache);

            for lv in left_v {
                for rv in &right_v {
                    ret.push(ex_op!(op, lv, *rv));
                }
            }

            i += 2;
        }

        ret
    }

    fn parse_expression(expression: &str) -> Vec<Token> {
        let mut token_sequence = vec![];

        let chars: Vec<char> = expression.chars().collect();
        let mut start_pos = 0;
        let mut last_token_pos = 0;

        while start_pos < chars.len() {
            let c = chars[start_pos];
            let mut op = None;

            if c == '*' {
                op = Some(Token::Op(Op::Mul));
            } else if c == '+' {
                op = Some(Token::Op(Op::Add));
            } else if c == '-' {
                op = Some(Token::Op(Op::Sub));
            }

            if let Some(op) = op {
                let num = i32::from_str_radix(&expression[last_token_pos..start_pos], 10).unwrap();
                token_sequence.push(Token::Num(num));
                last_token_pos = start_pos + 1;
                token_sequence.push(op);
            }

            start_pos += 1;
        }

        let num = i32::from_str_radix(&expression[last_token_pos..start_pos], 10).unwrap();
        token_sequence.push(Token::Num(num));

        token_sequence
    }
}

#[derive(Copy, Clone)]
enum Op {
    Add,
    Sub,
    Mul,
}

enum Token {
    Num(i32),
    Op(Op),
}

fn to_string(ts: &[Token]) -> String {
    let mut ret = String::new();

    for t in ts {
        ret += &t.to_string();
    }

    ret
}

impl ToString for Token {
    fn to_string(&self) -> String {
        match self {
            Token::Num(i) => i.to_string(),
            Token::Op(op) => match op {
                Op::Add => '+'.to_string(),
                Op::Sub => '-'.to_string(),
                Op::Mul => '*'.to_string(),
            },
        }
    }
}
```
