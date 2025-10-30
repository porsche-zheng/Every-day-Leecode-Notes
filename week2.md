# Monday

Date: 2025/10/27

### Problem 1 — [leetcode-2125](https://leetcode.cn/problems/number-of-laser-beams-in-a-bank)
- Difficulty: Medium
- Time spent: 10m

#### Approach
- 统计每一行 *安全设备* 的个数
- 2 个 *相邻的* *设备个数非 0* 行的个数相乘，并累加

#### Key idea
- 双指针（类似）
- 预处理

#### Complexity
- Time: $O(n)$
- Space: $O(n)$

#### Code
```cpp
class Solution {
public:
    int numberOfBeams(vector<string>& bank) {
        // int m=bank.size(), n=bank[0].size();
        // vector<int> cnt(n+1, 0);
        // int i=-1, j, ans = 0;
        // // 对于每一行的设备
        // while(i<m) {
        //     for(j=i+1; j<m; ++j) {
        //         // 获取每一行的后缀和
        //         for(int k=bank[0].size()-1; k>=0; --k) {
        //             if(bank[j][k] == '1') {
        //                 cnt[k] = cnt[k+1]+1;
        //             }
        //             else {
        //                 cnt[k] = cnt[k+1];
        //             }
        //         }
        //         // 若某行存在安全设备, 进行处理
        //         if(cnt[0] != 0) {
        //             if(i == -1) {
        //                 break;
        //             }
        //             for(int c=0; c<n; ++c) {
        //                 if(bank[i][c]=='1') {
        //                     ans += cnt[c];
        //                 }
        //             }
        //             break;
        //         }
        //     }
        //     // 结束后赋值
        //     i = j;
        // }
        
        int ans = 0;
        vector<int> cnt(bank.size(), 0);
        for(int i=0; i<bank.size(); ++i) {
            for(int j=0; j<bank[i].size(); ++j) {
                if(bank[i][j] == '1') {
                    ++cnt[i];
                }
            }
        }

        int i=0;
        while(i<bank.size()) {
            if(cnt[i]==0) {
                ++i;
                continue;
            }
            int j;
            for(j=i+1; j<bank.size(); ++j) {
                if(cnt[j] != 0) {
                    ans += cnt[i] * cnt[j];
                    break;
                }
            }
            i = j;
        }

        return ans;
    }
};
```

#### Note & Mistakes
注意读题！注释部分是理解错题意的代码，最初以为只能是 **左上右下** 位置关系才能有激光束

# Tuesday

Date: 2025/10/28

### Problem 1 — [leetcode-3354](https://leetcode.cn/problems/make-array-elements-equal-to-zero)
- Difficulty: Easy(?)
- Time spent: 30m(?)

#### Approach
求总和 `sum`，并判断奇偶？
- 若偶数
    1. 先找到第一个满足条件的 0 元素
    2. 从它开始，遍历直到非 0，即求出 cnt
    3. 直接返回答案 $2*cnt$
- 若奇数
    1. 先找到第一个满足 $cnt_0$ 要求的元素
    2. 遍历求出 $cnt_0$
    3. 遍历到非 0 元素时，判断是否为 1？
        - 若是，则从下一个元素开始遍历，并计数，直到非 0，即求出 $cnt_1$
        - 若不是，由于数据范围知，一定大于 1，则 $cnt_1=0$
    4. 返回最终答案


#### Key idea
经过观察和示例注意到：
- 若 `sum`（`nums`的总和）为偶数：最终结果为 $2*cnt$，`cnt` 指的是符合下列条件的索引 `i` 的个数
    - `nums[i]` 值为  0
    - 左右两边和相等，均为 $\frac{sum}{2}$
        > 因为左右两个方向均合理，所以应乘二
- 若 `sum` 为奇数，最终结果为 $cnt_0+cnt_1$
    > $cnt_0$ 指符合下列条件的元素个数（等价于索引个数）
    - 元素值为 0
    - 左边和为 `floor(sum/2)` 即 `int(sum/2)`
    > $cnt_1$ 同理，其左边和为 `(sum+1)/2` == `sum/2+1`

#### Complexity
- Time: $O(n)$
- Space: $O(1)$

#### Code
```cpp
class Solution {
public:
    int countValidSelections(vector<int>& nums) {
        int sum = 0;
        for(size_t i=0; i<nums.size(); ++i) {
            sum += nums[i];
        }

        size_t half_sum = 0;
        size_t ans = 0;

        for(size_t i=0; i<nums.size(); ++i) {
            half_sum += nums[i];
            if(half_sum == sum/2) {
                size_t cnt = 0;
                if(nums[i]==0) ++cnt;
                while(++i<nums.size() && nums[i]==0) {
                    ++cnt;
                }
                if(sum % 2==0) {
                    ans += 2*cnt;
                }
                else {
                    ans += cnt;
                }
                if(i!=nums.size()) {
                    --i; // --i 与 loop ++i 抵消
                }
            }
            if(sum%2 == 1 && half_sum ==  sum/2+1) {
                size_t cnt = 0;
                if(nums[i]==0) ++cnt;
                while(++i<nums.size() && nums[i]==0) {
                    ++cnt;
                }
                ans += cnt;
                if(i!=nums.size()) {
                    --i;
                }
            }
            if(sum%2 == 0 && half_sum>sum/2 ||sum%2 == 1 && half_sum>sum/2+1) {
                    return ans;
                }
        }

        return ans;
    }
};
```

#### Note & Mistake
- 最好最开始就分类讨论，否则代码逻辑不清晰
    > 上述代码最开始统一讨论，在细节处才分类讨论
- 注意奇数的情况较复杂
    > 最开始我以为奇数没有符合条件的位置，若奇数直接返回 0，导致错误
    > 后来没有意识到奇数只能一个方向，导致错误
- 注意循环的选择，`while` OR `for`?
    - for 的外层总是自增，有时需要在内层自减?
    - while 可只在内层控制，需要记得自增!


# Wednesday

Date: 2025/10/29

Duration: 5min

### Problem 1 — [leetcode-3370](https://leetcode.cn/problems/smallest-number-with-all-set-bits)
- Difficulty: Easy
- Time spent: 5m

#### Key idea
$2^{log_2(n)+1}-1$

#### Complexity
- Time: $O(1)$
- Space: $O(1)$

#### Code
```cpp
class Solution {
public:
    int smallestNumber(int n) {
        int l = log2(n);
        return pow(2, l+1)-1;
    }
};
```

#### Note
- $log_2(n)$ in cpp: `log2(n)`, $lg(n)$ in cpp: `log10(n)`, $ln(n)$ in cpp: `log(n)`
- $log_m(n)$ in cpp: `log(n)/log(m)`
    > `log` return `double` by default
- $m^n$ in cpp: `pow(m, n)`
- quick power: $O(log(n))$
```cpp
    long long power(long long a, long long n) {
    long long res = 1;
    while (n > 0) {
        if (n & 1) res *= a;  // 若当前二进制位为1，乘上对应的a^(2^k)
        a *= a;               // 准备 a 的下一位：a = a^2, 第 k 次循环时 a = a_orign^(2^k)
        n >>= 1;              // 准备 n 的下一位
    }
    return res;
    }
```

# Tuesday

Date: 2025/10/30

Duration: 5min

### Problem 1 — 
- Difficulty: Difficult(Easy?)
- Time spent: 5m

#### Approach
使用**动态规划**的思路：
- 只需一个*数*，表示当前元素前的子数组所需要的最小操作次数
- 若 `taget[i]>taget[i-1]`：则必须额外操作，且需要多 `target[i]-target[i-1]` 步操作
- 若 `target[i]<=target[i-1]`：则操作 `target[i-1]` 时，可以同时操作 `target[i]`，因此不需要额外操作

遍历数组，一次判断大小关系，并更新*表示结果的数*即可

#### Key idea
- 差分
- Dynamic Plan(?)

#### Complexity
- Time: $O(n)$
- Space: $O(1)$

#### Code
```cpp
class Solution {
public:
    int minNumberOperations(vector<int>& target) {
        size_t cnt = target[0];
        for(int i=1; i<target.size(); ++i) {
            if(target[i]>target[i-1]) {
                cnt += target[i]-target[i-1];
            }
        }
        return cnt;
    }
};
```

#### Note & Mistake
以下是阅读leetcode官方题解的笔记：

思路：求出数组 `target` 中相邻两元素的差值，保留大于 $0$ 的部分，累加即为答案。

证明: 设 `d` 是元素组 `target` 的差分数组
- d[0]=target[0]
- d[i]=targrt[i]-target[i-1], i!=0

则每次在 `target[left]~target[right]` *操作(每次-1)*，就是在
- `--d[left]`
- `++d[right+1]`, right<n-1

至此，可以将 `target` 区间操作转化为 `d` 的边界 $2$ 个值的操作。易知在 `d` 上的最小操作数等于在 `target` 上操作数，且该数的下界为 $\sum_0^n{max(d[i], 0)}$。可由以下方法构造得到操作的过程，即存在一系列操作，使得操作数等于该下界，则此下界即为最小

- 若 `d[i]<0`，则选择 left<i, right=i，使得 `d[i]` 变为 0
    > 若 `d[i]<0`，则一定存在 `j<i`，使得 `d[j]>0`。具体证明略
- 最后会剩下非负数项，对于每一个`d[i]>0`，则选取 left=i, right=n，则可以将 `d` 变为全 0 数组

