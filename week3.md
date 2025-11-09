# Monday

Date: 2025/11/3

Duration: 20min

Language: C++

## Problem 1 — [leetcode-1578](https://leetcode.cn/problems/minimum-time-to-make-rope-colorful)
- Difficulty: Medium
- Time spent: 20m

### Approach
- 维护 $[left, right]$ 区间上是相同颜色, `sum`表示该区间 **time** 之和, `max` 表示区间 **time** 最大值
    - 若 left 和 right 位置颜色相同，则移动 `right`, 更新 max 和 sum 等
    - 若不同，则 left 与 right 对齐, 更新 res, 重置 sum 和 max 等
- 由 **贪心思想** 知：要使区间内只留下一个气球，且 time 最小，则必须留下 time 最大的气球，删除其他
    > 代码中，每当 right 和 left 颜色不同时，最终答案 `res` 都需要 `+(sum-max)`

### Key idea
- 贪心
- 双指针

### Complexity
- Time: $O(n)$
- Space: $O(1)$

### Code
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

# Friday

Date: 2025/11/7

Duration: ~

## Problem 1 — [leetcode-2528](https://leetcode.cn/problems/maximize-the-minimum-powered-city)
- Difficulty: Difficult
- Time spent: ~

### Approach
将 `station` 转化为 `电量`；再使用 **二分思想**，给定 `ans` 验证是否可满足：
1. 初始化 `cnt=0` 表示* 为满足条件* 需要新建的电厂数
2. 遇到 `>=ans` 的电量，直接跳过
3. 若在 `i` 位置遇到 `<ans` 的电量 `elec`，在 $[i+r]$ 位置新建 `ans-elec` 座电厂(更新差分数组), `cnt+=ans-elec` 
    > 为什么 [i+r]?: 利用 **贪心** 思想  
    > 前 i 个位置均 >=ans，无需额外提供  
    > 因此，在 [i+r] 的位置上，既可以让第 i 位满足条件，又可以增加之后位置的电量，最小化需要新建的电厂数
4. 最后，若 `cnt<=k` 则二分缩小 ans；否则二分扩大 ans。直到收敛


### Key idea
- 贪心：每一步最优->整体最优
- 二分答案：求最小值
- 差分数组：前缀和即为 **电量**！

### Complexity
- Time: $O(n)$
- Space: $O(n)$

### Code
```cpp
class Solution {
    bool satiable(vector<long long>& diff, long long mid, int r, int k) {
        int n = diff.size()-1;
        long long sum = 0, cnt = 0;
        vector<long long> temp = diff;
        for(int i=0; i<n; ++i) {
            sum += temp[i]; // sum = i's electron
            if(sum<mid) {
                cnt += mid-sum;
                if(cnt>k) {
                    return false;
                }
                temp[min(n, i+2*r+1)] -= mid-sum;
                sum = mid;
            }
        }
        return true;
    }
public:
    long long maxPower(vector<int>& stations, int r, int k) {
        int n = stations.size();
        vector<long long> diff(n+1, 0); // 差分
        for(int i=0; i<n; ++i) {
            int left = max(0, i - r);
            int right = min(n, i + r + 1);
            diff[left] += stations[i];
            diff[right] -= stations[i];
        }
        long long left = ranges::min(stations), right = accumulate(stations.begin(), stations.end(), 0LL) + k; // 左闭右闭
        long long ans=0;
        while(left<=right) {
            long long mid = left+(right-left)/2;
            // cout << left << ' ' << right << ' ' << mid << endl;
            if(satiable(diff, mid, r, k)) {
                ans = mid;
                left = mid+1;
            }
            else {
                right = mid-1;
            }
        }
        return ans;
    }
};
```

### Note & Mistake
本题重点/难点：
- 想到 **二分答案** 的思路，转换为 *验证是否能满足条件* 的问题
- 验证时，需要采用贪心思想，尤其是 $3.$ 在 [i+r] 处新建电厂的想法
- 差分相关知识

犯错：
- 左闭右闭版二分答案：left<=right; ans=mid, left=mid+1; right=mid-1;
- 差分数组更新：diff[min(n, i+$2$*r+1)]-=add. 注意跨度是 `2*r+1`
