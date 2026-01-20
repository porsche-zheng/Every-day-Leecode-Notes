# Monday

Date: 2026/1/19

Duration: 35min

## Problem 1 — [leetcode-1292](https://leetcode.cn/problems/maximum-side-length-of-a-square-with-sum-less-than-or-equal-to-threshold)
- Difficulty: Medium
- Time spent: 35m

### Key idea
- *二维* **前缀和**
- *动规*
- *二分法*
- **枚举**

### Approach
1. 预处理。计算二维前缀和数组
    - `P[i][j]` 表示 `mat[0][0]`+...+`mat[i][j]`
    - `P[i][j]` = `P[i - 1][j]` + `P[i][j - 1]` - `P[i - 1][j - 1]` + `A[i-1][j-1]`
    - 注意：上述公式 i, j 从 1 开始，`P[i][j]` 对应 `A[i-1][j-1]`，可以简化 *边缘情况* 
2. 二分答案。根据 `left` 和 `right`，判断边长为 `mid` 时，是否满足条件
    - left 从 0 开始，确保 left 总是 **满足条件**
    - right 从 min(m, n)+1 开始，确保 right 总是 **不满足条件**
3. 验证答案。从 $row=mid, col=mid$ 开始，验证 *以 row, col 为 **右下角**, 以 mid 为 **边长** 的正方形元素和是否 $\leq$ 阈值*
    - 若 `mid` 满足，则更新 `left=mid`
    - 若不满足，则更新 `right=mid`
    - 直到 `left+1>right`。例如 $[3, 4)$，返回 left ($3$) 为最终答案

### Complexity
- Time: $O(N*M)$
- Space: $O(N*M*log(min(N,M)))$

### Code
```cpp
class Solution {
public:
    int maxSideLength(vector<vector<int>>& mat, int threshold) {
        int m = mat.size(), n = mat[0].size();
        vector<vector<int>> pre_sum(m+1, vector<int>(n+1, 0));
        for(int i=1; i<=m; ++i) {
            for(int j=1; j<=n; ++j) {
                pre_sum[i][j] = pre_sum[i-1][j] - pre_sum[i-1][j-1] + pre_sum[i][j-1] + mat[i-1][j-1];
            }
        }

        int left = 0, right = min(m, n)+1;
        while(left+1 < right) {
            int mid = left + (right-left)/2;
            bool leq = false;
            for(int i=mid; i<=m; ++i) {
                for(int j=mid; j<=n; ++j) {
                    int sum = pre_sum[i][j] - pre_sum[i-mid][j] - pre_sum[i][j-mid] + pre_sum[i-mid][j-mid];
                    if(sum <= threshold) {
                        leq = true;
                        break;
                    }
                }
                if(leq) {
                    break;
                }
            }
            if(leq) {
                left = mid;
            }
            else {
                right = mid;
            }
        }
        return left;
    }
};
```

### Note & Mistake
- 明确二分查找的 left 和 right 满足 / 不满足情况
- 明确开 / 闭区间
- 明确终止条件

在此题中，left 总满足，right 总不满足；左开右闭区间


# Tuesday

Date: 2026/1/20

Duration: 15min

## Problem 1 — [leetcode-3314](https://leetcode.cn/problems/construct-the-minimum-bitwise-array-i/)
- Difficulty: Easy
- Time spent: 15m

### Approach （官解）

注意到：$ans+1$ 的作用是将 $ans$ 中从低位到高位第一个 $0$ 变成 $1$ 并且使该 $0$ 之前的全部低位 $1$ 变为 $0

- 1000 + 1 = 1001, 从右向左，第一位是 $0$, 把他变成 $1$

- 1011 + 1 = 1100, 从右向左，第三位是 $0$, 把他变成 $1$, 并把前两位的 $1$ 都变成 $0$

- 111 + 1 = 1000 同理

则 $ans∣(ans+1)$ 的作用是将 $ans$ 中从低位到高位第一个 $0$ 变成 $1$

- 1000∣(1000+1) = 1001

- 1011∣(1011+1) = 1111

- 111∣(111+1) = 1111

由此可知，对于 $x$ 二进制位从低位到高位的第一个 $0$ 之前的所有 $1$，任取一个 $1$ 变为 $0$ 都可以求得一个 $ans$，使得 $ans∣(ans+1)=x$

- x = 1011, ans = 1001 即可

- x 是 *质数*, 如果 $x \neq 2$, 则他的最低位不可能是 $0$, 否则会被 $2$ 整除

- 若 $x=2$, 则 $ans=-1$, 即不存在符合条件的 $ans$

因此我们只需要找到每个 $x$ 中第一位 $0$ 的位置 $pos$ 并将 $pos−1$ 处的 $1$ 变为 $0$ 即可

### Key idea （个人解答）
- 暴力枚举

### Complexity
- Time: $O(N^2)$
- Space: $O(1)$

### Code
```cpp
class Solution {
public:
    vector<int> minBitwiseArray(vector<int>& nums) {
        int n = nums.size();
        vector<int> ans(n, -1);
        for(int i=0; i<n; ++i) {
            for(int j=0; j<nums[i]; ++j) {
                if( (j | (j+1)) == nums[i] ) {
                    ans[i] = j;
                    break;
                }
            }
        }
        return ans;
    }
};
```

### Note & Mistake
题目表述模糊，`OR` 在题目中是 **位运算** 的按位或

在 cpp 中 `|` 的优先级低于 `==`，因此需要 `()`