# Monday

Date: 2025/11/17

Duration: 10min

## Problem 1 — [leetcode-1437](https://leetcode.cn/problems/check-if-all-1s-are-at-least-length-k-places-away/)
- Difficulty: Easy
- Time spent: 10m

### Complexity
- Time: $O(n)$
- Space: $O(1)$

### Code
```cpp
class Solution {
public:
    bool kLengthApart(vector<int>& nums, int k) {
        int cnt=k;
        for(int n : nums) {
            if(n==1) {
                if(cnt<k) {
                    return false;
                }
                cnt = 0;
            }
            else {
                ++cnt;
            }
        }
        return true;
    }
};
```

### Note & Mistake
- 注意初始化 `cnt` 不要设置为 `INT_MAX`，否则 `++cnt` 会溢出！