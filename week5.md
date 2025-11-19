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

# Tuesday

Date: 2025/11/18

Duration: 20min

## Problem 1 — [leetcode-717](https://leetcode.cn/problems/1-bit-and-2-bit-characters/)
- Difficulty: Easy
- Time spent: 20m

### Key idea
- 观察特点：`0` 必须是 *第一种字符*，移动 $1$ 位；`1` 必须是 *第二种字符*，且移动 $2$ 位
- 遍历即可

### Complexity
- Time: $O(n)$
- Space: $O(n)$

### Code
```cpp
class Solution {
public:
    bool isOneBitCharacter(vector<int>& bits) {
        int i=0;
        bool one = true;
        while(i<bits.size()) {
            if(bits[i++] == 0) {
                one = true;
            }
            else {
                one = false;
                ++i;
            }
        }
        return one;
    }
};
```

### Note & Mistake
- 注意观察特点！
- 开始想从后向前，观察到特点后 *从前向后*

# Wednesday

Date: 2025/11/19

Duration: 5min

## Problem 1 — [leetcode-2154](https://leetcode.cn/problems/keep-multiplying-found-values-by-two/)
- Difficulty: Easy
- Time spent: 5m

### Key idea
- 二分查找

### Complexity
- Time: $O(log(n))$
- Space: $O(1)$

### Code
```cpp
class Solution {
public:
    int findFinalValue(vector<int>& nums, int original) {
        sort(nums.begin(), nums.end());
        while(binary_search(nums.begin(), nums.end(), original)) {
            original *= 2;
        }
        return original;
    }
};
```

### Note & Mistake
- 二分查找：`binary_search(nums.begin(), nums.end(), num)`，返回 `bool` 表示 `num` 是否存在于 `nums` 中
- 注意：二分查找必须先 **排序！**