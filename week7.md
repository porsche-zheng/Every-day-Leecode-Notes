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


# Monday

Date: 2026/3/2

Duration: 40min

## Problem 1 — [leetcode-1536](https://leetcode.cn/problems/minimum-swaps-to-arrange-a-binary-grid/)
- Difficulty: Medium
- Time spent: 40min

### Approach
1. 先获取每一行后缀 $0$ 的个数, 构建成新的 `vector` zeros
2. 对于第 `i` 行 (从 0 开始), 寻找后缀 0 的个数 $\geq n-1-i$ 的 **最近**行 `j`
    > - 第 0 行至少后缀 $n-1$ 个 0
    > 
    > - 如果第 1 行和第 2 行均满足条件, 优先选择第 1 行
    >
    > - 如果找不到, 即不存在满足条件的 `j`, 则直接 `return -1`

3. `j` (j>=i) 行向前移动到 `i` 位置, 每交换相邻行, 结果+1
    > 实际上交换 `zeros` 即可, 不需要真的交换 grid
4. 循环 $2. 3.$ 步骤, 直到 `i=n-1` 行

### Key idea
- 冒泡排序
- 贪心

### Complexity
- Time: $O(n^2)$
- Space: $O(n)$

### Code
```cpp
class Solution {
public:
    int minSwaps(vector<vector<int>>& grid) {
        int n = grid.size();
        vector<int> zeros(n, 0);
        for(int i=0; i<n; ++i) {
            int j=n-1;
            while(j>=0 && grid[i][j]==0) {
                --j;
            }
            zeros[i] = n-1-j;
        }

        int res = 0;
        for(int i=0; i<n-1; ++i) {
            for(int j=i; j<n; ++j) {
                if(zeros[j]>=n-1-i) {
                    for(int k=j; k>i; --k){
                        swap(zeros[k], zeros[k-1]);
                        ++res;
                    }
                    break;
                }
                if(j==n-1) {
                    return -1;
                }
            }
        }

        return res;
    }
};
```

### Note & Mistake
- 思路和官方近似, 主要是 **冒泡** 和 **贪心**

- 代码实现略缺少易读性, 尤其是变量名不清晰


# Thuesday

Date: 2026/3/3

Duration: 30min

## Problem 1 — 
- Difficulty: Medium
- Time spent: 30min

### Approach
1. 构造法
    - 观察发现, $i>2$ 时, $reverse(invert(S_{i-1}))$ 就是把 $S_{i-1}$ 中间位变成 0
    - 因此, 直接拼接出 $S_n$, 返回 $S_n[k-1]$ 即可
2. 递归法
    - $len(S_n)=2^n-1$
    - 若 k 恰好在 "中间", 返回 1
    - 若 k 在 "前面", 相当于 $S_{n-1}$ 中找 k
    - 若 k 在 "后面", 相当于 $S_{n-1}$ 中找 $2^n-k$ 再翻转
        > 观察得到 $S_n$ 总是 *对称相反* 的
        > - "0 **1** 1"
        > - "011 **1** 001"
3. 巧妙方法：看不懂, 想不出

### Key idea
- 递归
- 找规律

### Complexity
- Time: $O(n)$
- Space: $O(2^n)?$

### Code
```cpp
class Solution {
public:
    char findKthBit(int n, int k) {
        if (n==1) {
            return '0';
        }
        string cur = "011";
        int idx = 2;
        while(idx<n) {
            // cout << idx << ' ' << cur << endl;
            string temp = cur;
            temp[temp.length()/2] = '0';
            cur += "1" + temp;
            ++idx;
        }
        // cout << idx << ' ' << cur << endl;
        return cur[k-1];
    }
};
```

### Note & Mistake
- 上述 `Code` 是 **构造法**, 空间复杂度较大
- 可采用 **递归法** 求解
- 至于更巧妙的方法, 可遇不可求

- 至于一些 "观察" 的证明, 不求甚解

# Saturday

Date: 2026/3/7

Duration: ~

## Problem 1 — 
- Difficulty: Meidum 
- Time spent: ~

### Approach
观察到, 类型一的操作顺序不会影响结果. 即可以先进行 n 次类型一的操作, 再进行 m 次类型二的操作

因此, 可以从 0 到 len 枚举类型一操作的个数 n. 即先类型一 n 次, 再记录类型二的操作个数, 找到所有情况最小的

> 如果原字符串是 `11100`, 则有下列 5 种枚举情况
> 1. `11100`
>   - 变为 `10101` 需要 2 次
>   - 变为 `01010` 需要 3 次. 观察到 2+3=5
>   - 规律：变为 `101...` 的形式所需要的次数 + 变为 `010...` 的形式所需要的次数 = 字符串长度
>   - $O(n)$ 时间内得出需要 2 次
> 2. `11001`
>   - `11100` 到 `10101` 的后 4 位不变, 第 1 位 `1` 移动到最后, 且需要 `0` 才能交替, 于是增加 1 次, 最终变成 `01010` 需要 3 次
>   - 这里表述可能不清楚, 需要自己领会
>   - 根据上面的规律得, 变成另一种形式需要 3 次
>   - $O(1)$ 时间内得出需要 2 次
> 3. `10011` 需要 2 次
> 4. `00111` 需要 2 次
> 5. `01110` 需要 1 次
> 
> 最终结果是 1 次

但是, 如果真的一个个枚举, 时间复杂度无法通过, 因此需要滑动窗口 & 动态规划的思路

利用上一次的已有的结果, 更新当前结果, 以及最终答案！

需要多枚举几次, 找到更新规律！

### Key idea
- 滑动窗口
- 枚举
- 动态规划 (?)

### Complexity
- Time: $O(n)$
- Space: $O(n)$ 可优化到 $O(1)$

### Code
```cpp
class Solution {
public:
    int minFlips(string s) {
        int cnt = 0, res = 0, len=s.size();
        // 只进行类型二操作, 得到 "010..." 得形式, 需要 cnt 次
        for(int i=0; i<s.size(); ++i) {
            if(s[i] != '0'+i%2) {
                ++cnt;
            }
        }
        // 第一次更新 res
        res = min(cnt, len-cnt);
        // i 表示将 s[i] 放置最后
        for(int i=0; i<s.size()-1; ++i) {
            cnt -= s[i] != '0'+i%2;
            cnt += s[i] != '0'+(s.size()+i)%2;
            res = min({res, cnt, len-cnt});
        }
        return res;
    }
};
```

### Note & Mistake
- min 函数可以比较多个数字, `min({a, b, c})`

- min 函数得比较对象不可以是 `str.size()` 的表达式, 如 `s.size()-cnt`, 必须先将 `s.size()` 赋值给 `int len`
