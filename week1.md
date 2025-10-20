# Monday

Date: 2025/10/20

Duration: 5m

### Problem 1 — [leetcode-2011](https://leetcode.cn/problems/final-value-of-variable-after-performing-operations)
- Status: Solved
- Difficulty: Easy
- Language: C++

#### Approach
按规则遍历即可

#### Code
```cpp
class Solution {
public:
    int finalValueAfterOperations(vector<string>& operations) {
        int x = 0;
        for(string& str : operations) {
            if(str == "X++" || str == "++X") {
                ++x;
            }
            else {
                --x;
            }
        }
        return x;
    }
};
```