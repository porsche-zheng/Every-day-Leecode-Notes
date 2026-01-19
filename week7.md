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
