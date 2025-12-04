# Thursday

Date: 2025/12/4

Duration: 20min

## Problem 1 — [leetcode-2211](https://leetcode.cn/problems/count-collisions-on-a-road)
- Difficulty: Medium
- Time spent: 20m

### Approach
初始化 `ans=0` 表示答案, `numR=0` 表示当前位置*左边连续* `R` 的个数 遍历 `directions` 数组
- `ch == 'R'`: `numR` 计数 `+1`, 表示连续的 R 个数增加 1
- `ch == 'S'`: `ans` 添加 `numR` 个, 因为连续的 `numR` 个 R 都会碰撞; numR 必须清空, 因为连续中断
- `ch == 'L'`:
    - 若 numR==0, 表明左边最近的是 'L' 或 'R', 可能 +1;
    - ans 添加 `numR+1` 个, 因为最近的 1 个 'R' 会碰撞 2 次, 其余均 1 次; 清空 `numR`

### Key idea
模拟

### Complexity
- Time: $O(n)$
- Space: $O(1)$

### Code
```cpp
class Solution {
public:
    int countCollisions(string directions) {
        int ans=0, numR=0;
        bool stay = false;
        for(char ch : directions) {
            if(ch == 'R') {
                ++numR;
            }
            else if(ch == 'S') {
                stay = true;
                ans += numR;
                numR = 0;
            }
            else if(ch == 'L') {
                if(numR) {  // rights
                    ans += numR+1;
                    numR = 0;
                    stay = true;
                }
                else if(stay) {
                    ++ans;
                }
            }
        }

        return ans;
    }
};
```
