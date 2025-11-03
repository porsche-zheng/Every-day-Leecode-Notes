# Monday

Date: 2025/11/3

Duration: 20min

Language: C++

### Problem 1 — [leetcode-1578](https://leetcode.cn/problems/minimum-time-to-make-rope-colorful)
- Difficulty: Medium
- Time spent: 20m

#### Approach
- 维护 $[left, right]$ 区间上是相同颜色, `sum`表示该区间 **time** 之和, `max` 表示区间 **time** 最大值
    - 若 left 和 right 位置颜色相同，则移动 `right`, 更新 max 和 sum 等
    - 若不同，则 left 与 right 对齐, 更新 res, 重置 sum 和 max 等
- 由 **贪心思想** 知：要使区间内只留下一个气球，且 time 最小，则必须留下 time 最大的气球，删除其他
    > 代码中，每当 right 和 left 颜色不同时，最终答案 `res` 都需要 `+(sum-max)`

#### Key idea
- 贪心
- 双指针

#### Complexity
- Time: $O(n)$
- Space: $O(1)$

#### Code
```cpp
class Solution {
public:
    int minCost(string colors, vector<int>& neededTime) {
        int left = 0, right = 0, max = 0;
        long long sum = 0, res=0;
        
        while(left<colors.size()) {
            if(right<colors.size() && colors[right]==colors[left]) { // same color
                sum += neededTime[right]; // add to sum
                if(max<neededTime[right])
                    max = neededTime[right]; // update max
                ++right; // move to next
            }
            else { // tranverse over or not same color
                res += sum-max; // left max time babool, add others to res
                left = right;
                sum = max = 0; // clear sum and max
            }
        }
        
        return res;
    }
};
```