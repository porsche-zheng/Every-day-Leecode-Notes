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

# Thursday

Date: 2025/11/20

Duration: 30min

## Problem 1 — [leetcode-757](https://leetcode.cn/problems/set-intersection-size-at-least-two)
- Difficulty: 
- Time spent: 30m

### Approach
1. 先将 `intervals` 数组非递减排序，排序规则是：
    - 设排序对象是 $intervals[i]=[start_i, end_i]$ 和 $intervals[j]=[start_j, end_j]$
    - 若 $start_i<start_j$ 且 $end_i<end_j$，即 `i` 的开始比 `j` 开始位置小，且 `i` 结束比 `j` 结束位置小，则 $intervals[i]<intervals[j]$。例如 $[1, 3]<[2, 4]$
    - 若 $start_i<start_j$ 但 $end_i>end_j$，即 `interval[j]` 表示的区间∈ `intervals[i]` 所表示的区间，则 $intervals[i]>intervals[j]$。例如 $[1, 5]>[2, 3]$
2. 对于排序好的新 `intervals` 数组，开始顺序遍历，且在 `nums` 数组中记录已经选过的数字。
3. 对于遍历到的每一个 `intervals[i]`：若 `nums` 数组已经满足要求，即 `intervals[i]` 中至少有两个整数在 `nums` 中，则不做任何事情；若不满足要求，则采用贪心的思路，选择 `intervals[i]` 最后 $1$ 个或 $2$ 个值，直到满足条件。
4. 遍历结束后，按照上述方法得到的 `nums` 数组就是最小的 `nums` 数组，返回其大小即可。

#### Example
设题目给出的intervals = [[1,3],[3,7],[8,9]]
1. 排序。按照规则排序后，形式恰好保持不变，即[1, 3]<[3, 7]<[8, 9]
2. 遍历，并创建nums=空
    - 对于[1, 3]，由于nums=空，则贪心将末尾的3, 2加入nums。此时nums=[2, 3]
    - 对于[3, 7]，由于nums中只有3在区间内，则贪心地，只需将末尾7加入nums。此时nums=[2, 3, 7]
    - 对于[8, 9]，由于nums中没有数在区间内，则贪心地将末尾9, 8加入nums。此时nums=[2, 3, 7, 8, 9]
4. 遍历结束，得到nums=[2, 3, 7, 8, 9]。长度=5，则返回5！

#### Improve
- 排序规则 *清晰版*：按照 `end` 大小排序，即 `end` 小则 `interval` 小；若 `end` 一致，则 `start` **小**的 `interval` 反而**大**
- 查找 `interval` 在 `nums` 中含有的数，可用二分查找的方法
- 更进一步地，不需要 `nums` 数组，只需要 `nums0`, `nums1` 表示 **最新** 的 $2$ 个数即可！

### Key idea
- 合理排序
- 贪心

### Complexity
- Time: $O(n)$
- Space: $O(n)$ -> $O(1)$(改进后)

### Code
```cpp
class Solution {
public:
    int intersectionSizeTwo(vector<vector<int>>& intervals) {
        sort(intervals.begin(), intervals.end(),
        [](vector<int>& a, vector<int>& b) -> bool {
            return a[1]<b[1] || (a[1]==b[1] && a[0]>b[0]);
        });
        
        vector<int> nums;

        for(auto inter : intervals) {
            auto left = lower_bound(nums.begin(), nums.end(), inter[0]);
            auto right = upper_bound(nums.begin(), nums.end(), inter[1]);
            int dis = right-left;
            if(dis==0) { // no in nums
                nums.push_back(inter[1]-1);
                nums.push_back(inter[1]);
            }
            else if(dis==1) { // just one
                nums.push_back(inter[1]);
            }
            else {
                // more than two, do nothing
            }
        }
        
        // for(int num : nums) {
        //     cout << num << ' ';
        // }

        return nums.size();
    }
};
```

### Note & Mistake
- 先规定排序规则，并按规则排序是关键，贪心需要观察思考得出
- 注意排序规则中 `end` 相同时，`start` 小的 *反而* 大！
