# Monday

Date: 2025/10/13

Duration: 10m

### Problem 1 — [leetcode-2273](https://leetcode.cn/problems/find-resultant-array-after-removing-anagrams)
- Status: Solved
- Difficulty: Easy
- Time spent: 10m
- Language: C++

#### Approach (short)
`for` 循环 $+$ 题目逻辑判断

#### Key idea
- 对每一步 `for` 循环，利用 `is_diff` 函数判断 `res.last` 与 `str` 是否是 **字母异位词**

- `If false`: `res.push_back(str)`

#### Complexity
- Time: O(nklogk)
- Space: O(n)

#### Code
```cpp
class Solution {
    bool is_diff(string w0, string w1) {
        sort(w0.begin(), w0.end());
        sort(w1.begin(), w1.end());
        return (w0 == w1);
    }

public:
    vector<string> removeAnagrams(vector<string>& words) {
        vector<string> res;
        res.push_back(words[0]);

        for(int i=0; i<words.size(); ++i) {
            if( !is_diff(res[res.size()-1], words[i]) ) {
                res.push_back(words[i]);
            }
        }

        return res;
    }
};
```

# Tuesday

Date: 2025/10/14

Duration: 20min

---

### Problem 2 — [leetcode-3349](https://leetcode.cn/problems/adjacent-increasing-subarrays-detection-i)
- Status: Solved
- Difficulty: Easy
- Language: C++

#### Approach (short)
- 维护 `interupt` 数组
- 判断其性质

#### Key idea
- 记录每一个 **非递增的位置**，存储到 `interupt` 数组中
- 判断 `interupt` 是否存在相邻元素之差 $>=2*k$ 或 连续 $2$ 个相邻元素之差 $>=k$ 的情况

#### Complexity
- Time: O(n)
- Space: O(n)

#### Code
```cpp
class Solution {
public:
    bool hasIncreasingSubarrays(vector<int>& nums, int k) {
        vector<int> interupt;
        interupt.push_back(0);

        for(int i=1; i<nums.size(); ++i) {
            if(nums[i] <= nums[i-1]) {
                interupt.push_back(i);
            }
        }

        interupt.push_back(nums.size());

        bool flag = false;
        for(int i=1; i<interupt.size(); ++i) {
            if(interupt[i]-interupt[i-1] >= 2*k) return true;

            if(interupt[i]-interupt[i-1] >= k) {
                if(flag) return true;
                flag = true;
            }
            else {
                flag = false;
            }
        }

        return false;
    }
};
```

---

#### Mistakes
开始未考虑 相邻元素之差 $>=2*k$ 的情况

```txt
nums = [-15,19]
k = 1
```
