## DAY0: 2025/6/27

$1.$ [leetcode-1284](https://leetcode.cn/problems/minimum-number-of-flips-to-convert-binary-matrix-to-zero-matrix/)

### 1. 预备知识：位运算
对于十进制数而言

#### 1.1. & 位与运算
先将 2 *十进制* 数转化为 *二进制* 数，每一位做 **与** 运算（即当且仅当两位均为 $1$ 时，结果才为 $1$ ），最后将运算得到的 *二进制数* 转化为 *十进制数*，得到结果

##### 1.1.1. x & 1
- 实质：当 x = 7 时
    ```
    7 -> 0111  
    1 -> 0001
    7 & 1 = 0001
    ```

- 作用：得到 `x` 的二进制表示的最低位。  
  因为任何数 & 1，只有 *最低位* 与 x 的最低位相同，其他位均为 $1$

- 例如
    - `x = 3`：x 二进制表示为 11，`x & 1` = 1；
    - `x = 6`：x 二进制表示为 110，`x & 1` = 0。

#### 1.2. | 位或运算

- 十进制 -> 二进制
- 每一位进行 **或** 运算
- 二进制 -> 十进制

#### 1.3. ^ 异或运算
- decimal -> binary
- 对每一位进行 **异或运算**（即 当且仅当两个位的数不同，结果才为 $1$）
- binary -> decimal

#### 1.4. ~ 按位取反（一元运算符）
高位不足时，默认原始为 $0$，则转化后会变回 $1$

#### 1.5. << 与 >> 位移运算

##### 1.5.1. `x << y`
将 x 的二进制数 左移 y 位，最后 y 位 补 $0$
- 例如： `6 << 3` = `110 << 3` = `110000`
- 效果：对 x 乘以 y 个 2（ $2^y$ ）

> 注意：优先级 $< + - \times \div$

##### 1.5.2. `x >> y`

##### 1.5.3. x & (1 << k)

- 作用：判断 `x` 二进制表示的第 **k** 位（最低位为第 0 位）是否为 $1$

- 例如
    - `x= 3`: x 的二进制表示为 11，`x & (1 << 1)` = `11 & 10` = `10` > `0`，说明第 1 位为 1；

    - `x = 5`: x 的二进制表示为 101，`x & (1 << 1)` = `101 & 10` = `0`，说明第 1 位不为 1。

### 2. 法一 广度优先搜索

#### 2.1. 思路：
- 先将 初始状态 *入队*；
- 每一次 获取 对顶状态，并求出 它所有的 *相邻状态*；
- 若 相邻状态 未被访问，则入队；否则，continue；
- 直到 矩阵全 $0$ 或 队列为空（没有方法）

#### 2.2. 细节

##### 2.1.1. 相邻状态 
指的是：当前状态下，反转一个地方后，得到的新状态

##### 2.1.2. 矩阵 与 整数 的互相转化：

- 方法： 将矩阵看作是二进制数的展开
```
    0 1 0
    1 0 1
    1 1 0
```
可看作： ` 010 101 110 `，转化为 *十进制* 为 `174`

- 好处
    - 队列是 `int` 型队列；
    - 可根据此设置 hash 表，用于检查下一状态是否已经遍历，即是否应当 *入队*

- 函数
    - `encode()`
    - `decode()`

#### 2.3. 注意事项/易错
- `IntToMatrix()` 中，`num >> 1` 无法改变 `num` 的值，正确应为： `num >> = 1`  
- `BFS()` 中，`step` 需要 **正确地** 记录；代码采用 *stack（栈）* 的方法

#### 2.4. [代码](.\1284_filp_to_zero.hpp)  

$2.$ [每日一题：leetcode-2014](https://leetcode.cn/problems/longest-subsequence-repeated-k-times/description/)

## DAY1: 2025/6/29

**分治专题(divide-and-conquer)**  
$1.$ [leetcode-4](https://leetcode.cn/problems/median-of-two-sorted-arrays/description/?envType=problem-list-v2&envId=divide-and-conquer)

### 1. 预备知识
- 分治思想
- 二分查找

### 2. 法一 二分查找
#### 2.1. 思路 -> 寻找第 $k$ 小的元素
1. 求出`nums1[k]`与`nums2[k]`, $k = \frac{(m+n)}{2}$
2. 比较大小
- `nums1[k/2-1]` $<$ `nums2[k/2-1]`: 排除 `num1` 的前 $k$ 个元素
- `nums1[k/2-1]` $>$ `nums2[k/2-1]`: 排除 `num2` 的前 $k$ 个元素
- `nums1[k/2-1]` $=$ `nums2[k/2-1]`
3. 再在排除后的新数组中继续查找 $k - k/2$ 个数
#### 2.2. 特殊情况的处理
- 若 `A[k/2-1]` 或 `B[k/2-1]` 越界, 则选取数组中的最后一个元素, 同时注意此时 $k$ 应减去根据具体排除的数目, 并不一定是 $k = k - k/2$
### 3. 法二 分割数组

## DAY2: 2025/7/8

$1.$ [leetcode-142](https://leetcode.cn/problems/linked-list-cycle-ii/description/)  

### 1. 预备知识
- 循环链表数学推导
- STL 哈希表用法

### 2. 法一 双指针法
#### 2.1. 思路
1. 先找到一个环内结点：
2. 在链表头`head`处设置快慢指针`fast`&`low`，`fast`每次移动$2$步，`low`每次移动$1$步，直到他们相遇，相遇点是一个环内的节点
3. 再寻找环头：
4. 在链表头和相遇点各设置一指针，分别为`index1`, `index2`，两指针均每次移动$1$步，直到相遇，相遇点是环的头节点
#### 2.2. 思路推导/证明
[代码随想录-证明](https://www.programmercarl.com/0142.%E7%8E%AF%E5%BD%A2%E9%93%BE%E8%A1%A8II.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)
### 3. 法二 哈希表法
思路：设置**hash表**，直到某节点存在，这个节点即为链表头

## DAY3: 2025/7/10

$1.$ [leetcode-49](https://leetcode.cn/problems/group-anagrams/description/)  

### 1. 预备知识
- `string`类型可以内部排序：e.g.**eat->aet**
- `Hash Tabal`可以用任何形式作为**Hash Fundtion**
- 并查集

### 2. 法一 Hash Table
思路：
- Hash Function：某个`string`经过排序后得到的`sorted_string`
- Hash Table：遍历`strs`中每一个`string`，**sorted_string相同**的所有`string`都是*anagram*
- 此外，还可以使用**计数**作为**Hash Function**/**Hash Table的Key**。总之，需要合理设计**Key**

### 3. 法二 并查集
思路：
- 从头至尾遍历：
- 遍历的每一步：当前`i`的*string*与`i`前的每一个*string*调用*function*`isAnagram()`
    - 若`Ture`则调用`union`，并`break`，判断下一个`str[i+1]`；
    - 若`False`则继续`continue`，直到`str[0]`
- 最后得到所有*anagram*的并查集，按照*root*进行分类

## DAY4: 2025/7/12

**哈希表相关(Hash Table)**  

### $1.$ [leetcode-1](https://leetcode.cn/problems/two-sum/description)
#### 思路：
- 使用`unordered_map`，`key`表示数值*num*，`value`表示索引*index*
- **从头至尾遍历**，搜索当前`hash_table`中，也就是`i`前的元素中是否含有`target-num[i]`
- 若存在，$2$次`vector<int> res.push_back`后，返回`res`
- 若不存在，添加/更新`<nums[i], i>`的键值对

> 注：更新指的是假设有多个`num[i]`，即`i`前已经存在`num[i]`时，会更新对应的索引。但由于题目已保证：只存在一对正确答案且一个数最多使用一次，因此这多个`num[i]`与结果无关，也就是说$2*num[i]≠target$，因此更新索引也无妨

##### 最大收获：从头到尾遍历 并且 始终只检查 已遍历部分 的思路

### $2.$ [leetcode-454](https://leetcode.cn/problems/4sum-ii/description)
#### 思路：两两处理
1. 用一个`unordered_map`记录`nums1`和`nums2`每个数相加结果的次数。即：*key = a+b for int a in nums1 for int b in nums2* & *value = count of key*
2. 遍历`nums3`与`nums4`，在`map`中查找 *-(c+d) for c in num3 for d in nums4* 出现的次数

#### 题目关键：$4$ 个数组相互独立

## DAY5: 2025/7/15

**双指针**：可将$O(n^2)$时间复杂度降低至$O(n)$，即下降一个数量级！

### $1.$ [leetcode-15](https://leetcode.cn/problems/3sum/description/)
#### 思路：
1. 先排序；可从示例中**结果都已排序**来*推断*；
2. **嵌套循环**的主思路不变：即先最外$1$层`for`循环，遍历`i`从$0$到`nums.size()-2`；
3. 每一个`for`循环
   - 初始化`left=i+1, right=nums.size()-1`
   - 进行`while(left<right)`循环；
4. 每个`while`循环：
   - 计算`sum = nums[i] + nums[left] + nums[right]`；
   - 判断`sum`与$0$的关系：
     - 若`sum>0`，`--right`
     - 若`sum<0`，`++left`
     - 若`sum==0`，添加到`ans`中
5. 去重操作：
   - 若`i!=0 && nums[i]==nums[i-1]`，`continue`
   - 若`left!=i+1 && nums[left]==nums[left-1]`，`continue`
   - 若`right!=nums.size() && nums[right]==nums[right]+1`，`continue`；
6. 剪枝操作：`if(nums[i]>0)`，`break`

#### [代码](15_3sum.hpp)
-- 时间复杂度：$O(n^2)$
-- 空间复杂度：$O(1)$

### $2.$ [leetcode-18](https://leetcode.cn/problems/4sum/description/)
#### 思路：

## DAY6: 2025/7/27

### $1.$ [leetcode-344](https://leetcode.cn/problems/reverse-string/)
主要代码：
```cpp
for(int i=0; i<s.size()/2; ++i)
    swap(s[i], s[s.size()-i-1]);
```

### $2.$ [leetcode-541](https://leetcode.cn/problems/reverse-string-ii/description/)
主要代码：
```cpp
int i;
for(i=0; i+2*k<s.size(); i=i+2*k)
    reverse(s+i, s+i+k);
if(i+k>s.size()) reverse(s+i, s+s.size());
else reverse(s+i, s+i+k);
```

### $3.$ [替换数字](https://kamacoder.com/problempage.php?pid=1064)
#### **原地替换**思路：
1. 先将*string*`resize()`成结果大小$s.size()-num_val+num_val*6$
2. **从后往前**，遇到数字*倒着*填充`numeber`，遇到`char`直接赋值
3. 直至`i==0`

##### 巧妙之处
- 先`resize()`保证长度匹配
- 从后向前填充，降低时间复杂度为$O(n)$，且**新串索引**永远不会*超过***原穿索引**

## DAY7: 2025/8/6

### $1.$ [leetcode-](https://leetcode.cn/problems/reverse-words-in-a-string/)
#### 思路：
- 先翻转每一个*单词*
- 再翻转整个*句子*
- 恰好得到最终结果

### $2.$ [右旋字符串](https://kamacoder.com/problempage.php?pid=1065)
#### 思路：
- 将原字符串分为$2$部分
- 先翻转各部分，再整体翻转得到答案

### $3.$ [leetcode-](https://leetcode.cn/problems/repeated-substring-pattern/)

### $4.$ [leetcode-](https://leetcode.cn/problems/implement-queue-using-stacks/)
#### 思路：
- `pop`: 在$B$栈中直接 `pop`；A, B, $2$个栈相互倒换，恰好可以让$A$栈栈底成为$B$栈栈顶，达到**先入后出**的效果
- `push`: 直接 `push` 到$A$栈；若当前元素都在$B$栈，则先*倒*在$A$栈中

### $5.$ [leetcode-](https://leetcode.cn/problems/implement-queue-using-stacks/)
#### 思路：
- 仅用**一个队列**即可实现栈
- `pop`: *出队+入队*操作$N$次，使得队尾元素成为对头元素时，直接使用队列的`front()`操作
- `push`: 直接`push()`到队列尾

### $6.$ [leetcode-](https://leetcode.cn/problems/sliding-window-maximum/)
#### 单调双端队列 (deque)
##### 思路：
- 维护一个单调递减的双端队列，队头是队列中*索引未过期的*元素的最大值
- 遍历的每一次：从队头删除*不在滑动窗口内的*索引元素 `while(que.front <= min_index) que.pop_front()`； 在队尾删除*小于当前值*的元素 `while(nums[que.back] <= nums[i]): que.pop_back()`
- 每一次操作后的队头 `que.front()` 就是当前滑动窗口的最大值

### $7.$ [leetcode-347](https://leetcode.cn/problems/top-k-frequent-elements/)
#### 数据结构
- 哈希表 `unordered_map`
- 优先队列 `priority_queue`
#### 思路
- 先用 `unordered_map` 统计每个数字出现的频率
- 再 `make_pair(frequent, num)` 并加入 *大项堆* 中排序
- 最后从堆中 `pop` 出前 $k$ 个元素即可

## DAY8: 2025/8/7

### $1.$ [leetcode-222](https://leetcode.cn/problems/count-complete-tree-nodes/)
#### 完全二叉树性质
- *完全二叉树* 中，若干节点为根的树是 *满二叉树*
- *满二叉树* 的节点数是 $2^h-1$
#### 思路：递归 + 完全二叉树性质
- 终止条件：`root==nullptr`, `return 0`
- 判断是否是满二叉树：`while(l) l=l->left`与`while(r) r=r->right`的迭代次数相同
    > 注：优化的关键一步，直接得出节点数$2^h-1$
- 单层递归逻辑：`return 1 + countNodes(root->left) + countNodes(root->right)`

## DAY8 (cont.): 2025/8/12

### $1.$ [leetcode-416](https://leetcode.cn/problems/partition-equal-subset-sum/)
#### 思路：动态规划
- 计算 `nums` 的和 `sum`
    - `sum` 为奇数，`return false`
    - `sum` 为偶数，计算 `target = sum/2`
- `nums[i]` 即是 *物品* 的重量，也是 *物品* 的价值
- 构建 dp 数组，`dp[i][j]` 类型为 `int`，表示只考虑 `nums` 的前 `i` 个数，容量为 `j` 的包能容纳的数的最大和
- `return dp[nums.size()][target] == target`
#### 另解
- 改变 dp 数组的含义：`dp[i][j]` 类型为 `bool`，表示只考虑前 `i` 个数，有和为 `j` 的数的组合
- 一维滚动数组 `dp[j]`

### $2.$ [leetcode-1049](https://leetcode.cn/problems/last-stone-weight-ii/)
#### 思路：转化为题目 $1.$ 0-1背包问题
- 本质上想要将 `stones` 分成 $2$ 个和最相近的数组，他们的差就是最后的值
- 利用动态规划计算最大的 `dp[target](target=sum/2)`，`return sum - 2 * dp[target]`

## DAY9: 2025/8/19

### $1.$ [leetcode-377](https://leetcode.cn/problems/combination-sum-iv)
#### 题目本质：完全背包 求*排列*
##### 动态规划五部曲
- `dp[i]`: 表示和为`i`的排列个数
- 递推公式：`for(int num : nums) dp[i] += dp[i-num]`
- 初始化：`dp[0] = 1`
- 遍历顺序：求 *排列* 外层 for 循环背包，内层 for 循环物品
> 求 *组合* 外层 for 循环物品，内层 for 循环背包

## DAY10: 2025/8/26

### $1.$ [leetcode-322](https://leetcode.cn/problems/coin-change/)
#### 动态规划五部曲
- `dp[i]`: 表示可以凑成 `i` 所需的 **最少的硬币个数**
- 递推公式：$dp_i = min(dp_i, dp_{i-coin}+1)$
- 初始化：`vector<int>dp(amount+1, 10005); dp[0]=0`
- 遍历顺序：*外层物品，内层背包*（求组合） 或 *外层背包，内层物品*（求排列）均可，因为问的是 **最少硬币个数**

#### 代码
```cpp
class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {
        vector<int> dp(amount+1, 10005);
        dp[0] = 0;
        for(int coin : coins) {
            for(int i=coin; i<=amount; ++i) 
                dp[i] = min(dp[i], dp[i-coin]+1);
        }
        if (dp[amount]==10005) return -1;
        else return dp[amount];
    }
};
```

### $2.$ [leetcode-279](https://leetcode.cn/problems/perfect-squares/)
#### 思路
- 自己构建 `nums` 数组，其中的数都是完全平方数且都小于等于 `n`
- 类似 $1.$ 通过动态规划求解最小个数

#### 代码
```cpp
class Solution {
public:
    int numSquares(int n) {
        vector<int> nums;
        for(int i=1; i*i<=n; ++i) nums.push_back(i*i);
        vector<int> dp(n+1, 10005);
        dp[0] = 0;
        for(int num : nums) {
            for(int i=num; i<=n; ++i)
                dp[i] = min(dp[i], dp[i-num]+1);
        }
        return dp[n];
    }
};
```

### $3.$ [leetcode-139](https://leetcode.cn/problems/word-break/)
#### 背包 DP 五部曲  $O(n^3)$
- `dp[i]`：表示是否能用字典构成 `s.substr(0,i)` 的子字符串
- 递推公式：$dp[i]=(dp[j] \space and \space s.substr(j, i-j+1) \space in \space wordDict)$
- 初始化：`vector<bool> dp(s.size()+1, false)`，`dp[0]=true`
- 遍历顺序：先背包（字符串长度）后物品（字典中单词）

#### 另解：区间 DP $O(n^4)$
```cpp
class Solution {
public:
    bool wordBreak(string s, vector<string>& wordDict) {
        vector<vector<bool>> dp(s.size(), vector<bool>(s.size(), false));
        for(int l=0; l<s.size(); ++l)
            for(int i=0; i<s.size()-l; ++i) {
                int j= l+i;
                for(string word : wordDict)
                    if(word==s.substr(i,l+1)) {
                        dp[i][j] = true;
                        break;
                    }
                if(dp[i][j]) continue;
                for(int k=i; k<j; ++k)
                    if(dp[i][k]&&dp[k+1][j]) {
                        dp[i][j] = true;
                        break;
                    }
            }
        return dp[0][s.size()-1];
    }
};
```

## DAY11: 2025/8/28

### $1.$ [leetcode-198](https://leetcode.cn/problems/house-robber/)
#### 法一
- `dp[i]`: 表示考虑下标 `i` 及以内的房屋，最多可偷窃的金额
- $dp[i]=max(dp[i-2]+nums[i], dp[i-1])$
  > $dp[i-2]+nums[i]$ 表示选择 `i` 号房屋的最大金额；  
  > $dp[i-1]$ 表示不选择 `i` 号房屋的最大金额
- 初始化：`dp[0]=nums[0]`, `dp[1]=max(nums[0][1])`
- 递归顺序：从前到后遍历 `i`

#### 法二：树形 DP + 记忆化搜索
- `dp[i][0], dp[i][1]`: 分别表示不选择和选择 `i` 房屋的最大金额
- `dfs` 遍历树，并用记忆化搜索避免重复，降低时间复杂度

### $2.$ [leetcode-213](https://leetcode.cn/problems/house-robber-ii/)
#### 思路
房子成环，分别考虑 **去头** 和 **去尾** 两种情况，比较得出最大值
- 去头：对索引 `0` 到 `size()-2` 的房子做 *打家劫舍*
- 去尾：对索引 `1` 到 `size()-1` 的房子做 *打家劫舍*
- 比较 $2$ 者最大值

### $3.$ [leetcode-337](https://leetcode.cn/problems/house-robber-iii/)
#### 思路
房子成树，做 **树形 DP**  
##### 实现
```cpp
vector<int> dfs(TreeNode* node) {
    if(!node) return vector<int>{0, 0};
    vector<int> left = dfs(node->left);
    vector<int> right = dfs(node->right);
    vector<int> res(2, 0);
    res[0] = max(left[0], left[1]) + max(right[0], right[1]);
    res[1] = left[0] + right[0] + node->val;
    return res;
}
```

## DAY12: 2025/9/5

### $1.$ [leetcode-2749](https://leetcode.cn/problems/minimum-operations-to-make-the-integer-zero)
#### 注意到：
- 若进行了 $k$ 次运算后结果为 0，则 $num1-k*nums2 = n$，其中 $n$ 满足：
- $n>0$：即 若 $num1-k_i*nums2$ 已经为负数，则可`return -1`
- $n>=k$：因为 `k` 次选择后最小数为每次都选 `0` 得到的 `k`
- $n$ 对应二进制数所含 $1$ 的数量 $<=k$：此时，$n$ 总是可以拆分为 `k` 个 $2$的指数 相加，如 `1001=10+100+1`，虽然 `1001` 中仅有 $2$ 个 $1$，但可以分解为 $3$ 个数相加

> 若 $>k$，则一定會有 $1$ 無法滿足，如 `1011` 最少需要 `1000+10+1` 這 $3$ 個 $2$ 的幂次數 相加

#### 思路：枚举法
- 枚举 `k`，计算 `n=num1-k*num2`
- 若 `n<0`，`return -1`，表示无法满足题意
- 若 `one_num(n)<=k && n>=k`，`return k`
- 经验证循环一定会退出，无需其他循环条件

## DAY13: 2025/9/6

### $1.$ [leetcode-714](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/)
#### 思路
- 类似 买入股票的最佳时机 Ⅱ
- 变更：`dp[0] = max(dp[0], dp[1]+price[i]-fee)`
- 只需多考虑 *卖出股票时* 需要付费 即可

### $2.$ [leetcode-300](https://leetcode.cn/problems/longest-increasing-subsequence/)
#### 思路：DP 数组含义是重点！
- `dp[i]`: 表示 nums序列 0~i 中 *以 nums[i] 为结尾* 的最长子序列 的长度
- 转移方程：仅当 $nums[i]>nums[j]$ 时，$dp[i]=max(dp[i], dp[j]+1)$
- 遍历顺序：i 从 `0` 到 `nums.size()-1`，j 从 0 到 i
- 返回整个 dp 的最大值
    > 不是返回 `dp[num.size()-1]`

##### [优化](./300_long_increase_subseq.hpp)
- 新开一个数组 arr，`arr[i]` 表示 目前为止 所有长度为 `i` 的递增子序列中 末尾元素的最小值
- 可提高第二层 `j` 遍历效率：`j` 从 `res` 到 `0`，到 `arr[j]<nums[i]` 时立刻停止，并做 *相应更新*
- 初始化：`arr[0]=MIN, arr[1]=nums[0]`, 其余为 `MAX`

### $3.$ [leetcode-3495](https://leetcode.cn/problems/minimum-operations-to-make-array-elements-zero/)
#### 思路
- 实现 `get(n)` 函数：获取 1~n 数字 log4 之和

## DAY14: 2025/9/8

### $1.$ [leetcode-718](https://leetcode.cn/problems/maximum-length-of-repeated-subarray/)
#### 法一：二维 DP
- `dp[i][j]`: 表示 `nums1[i-x+1, i]` 和 `nums2[j-x+1, j]` 子数组重复
- 转移方程：$dp[i][j]=dp[i-1][j-1]+1, if nums1[i]==nums2[j]$
- 遍历顺序：`i`, `j` 均递增
    > 若滚动数组：外层递增，内层递减

#### 法二：滑动窗口
```txt
step1:
1 2 3 2 1
3 2 1 4 7
step2: 
(1) 2 3 2 1
    3 2 1 4 (7)
...
stepk:
(1 2 3 2) 1
          3 (2 1 4 7)
stepk+1:
    1 2 3 2 (1)
(3) 2 1 4 7
stepn:
          1 (2 3 2 1)
(3 2 1 4) 7
```

### $2.$ [leetcode-1143](https://leetcode.cn/problems/longest-common-subsequence/)
- `dp[i][j]`: 表示考虑 `text1[0,i)` 和 `text[0,j)]` 的最长公共子序列

> NOTES：
> 
> 此题 左闭右闭 较难处理，选择 左闭右开
> 
> 子序列可以断开，故考虑 0~i；子数组不能断开，故严格比较 0~i 后 x 个元素
- 转移方程：$dp[i][j]=$
    - $dp[i-1][j-1]+1, if text1[i]==text2[j]$
    - $max(dp[i-1][j], dp[i][j-1]), else$
- 遍历顺序：`i`, `j` 从 `1` 开始递增

## DAY15: 2025/9/10

### $1.$ [leetcode-1035](https://leetcode.cn/problems/uncrossed-lines/)

#### 思路一
- dp[i][j]: 表示考虑 nums1(0~i) 和 nums2(0~j) 的最多不相交线数目

- 状态转移方程：dp[i][j] = 
    - nums1[i]==nums2[j]: dp[i-1][j-1]+1
    - else: max(dp[i-1][j], dp[i][j-1])

- 遍历顺序：i, j 均递增

#### 思路二
$dp$ 找出最长公共子序列长度 $len$，$len(nums1)+len(nums2)-2*len$ 即为答案

### $2.$ [leetcode-583](https://leetcode.cn/problems/delete-operation-for-two-strings/)

#### 思路
- dp[i][j]: 表示使得 word1[0~i] 和 word2[0~j] 相同的最小步数

- 状态转移方程：dp[i][j] = 
    - word1[i] == word2[j]: dp[i-1][j-1]
    - else: min(word1[i], word2[j]) + 1

- 初始化：
    - dp[0][0] = (word1[0]==word2[0]) ? 0 : 2
    - dp[i][0] = (word1[i]==word2[0]) ? dp[i-1][0] : i
    - dp[0][j] = (word1[0]==word2[j]) ? dp[0][j-1] : j

- 遍历顺序：i, j 均递增

## DAY16: 2025/9/11

### $1.$ [leetcode-2327](https://leetcode.cn/problems/number-of-people-aware-of-a-secret)
2025/9/9: 每日一题

#### 法一：DP
- `dp[i]`: 在第 i 天 *第一次* 知道秘密的人数

- 遍历的每一个 i，j 从 i+delay 到 i+forget，`dp[j]+= dp[i]`

- 初始化：`dp[0]=1`

- 遍历顺序：i 递增，j 相应范围递增

#### 法二：模拟 + 双端队列

#### 易错
- `10e9+7` 得到的是 float，无法进行 `%` 运算

- 每一次加法运算 `dp[j] = (dp[i]+dp[j]) % mod`，否则会溢出

## DAY17: 2025/9/13

### $1.$ [leetcode-72](https://leetcode.cn/problems/edit-distance/)

#### 思路：DP（自底向上）
- 存在 `word1` 和 `word2` 两个字符串，通常是 *二维数组*

- `dp[i][j]`: `word1[0~i]` 与 `word2[0~j]` 变为相同所需的最少操作数

- 转移方程：$dp[i][j]=$
    - `if word1[i]==word2[j]`: dp[i-1][j-1]
    - `else`: min(dp[i-1][j], dp[i][j-1], dp[i-1][j-1]) + 1

- 初始化
    - `dp[0][0]` = 0(word1[0]==word2[0]) / 1(!=)
    - `dp[i][0]` = i(word1[i]==word2[0]) / dp[i-1][0]+1(!=)
    - `dp[0][j]` = j(==) / dp[0][j-1]+1(!=)

- 遍历顺序：i, j 递增即可

#### 易错
- 必须初始化！因为 *第一行* 与 *第一列* 情况特殊

- 当不相等时，需要找到 3 者最小值，再 +1
    > `word1[i]` 替换为 `word2[j]`: dp[i-1][j-1] + 1  
    > `word1` 删除 `word1[i]` **或** `word2` 添加 `word1[i]`: dp[i-1][j] + 1
    > 
    > 若只能 *删除/添加* 操作：min(dp[i-1][j], dp[i][j-1])+1

### $2.$ [leetcode-647](https://leetcode.cn/problems/palindromic-substrings/)

#### 方法一：中心扩展 $O(n^2)$

枚举法：
- **枚举** 出所有的子串，然后再判断这些子串是否是回文。$O(n^3)$

- **枚举** 每一个可能的回文中心，然后用两个指针分别向左右两边拓展，当两个指针指向的元素相同的时候就拓展，否则停止拓展。$O(n^2)$

#### 方法二：Manacher 算法 $O(n)$
Manacher Algorithm 是在线性时间内求解最长回文子串的算法

##### 规定 
- `f(i)` 为以 `i` 为中心，回文字符的最大半径

- `R` & `L` 为当前最长回文字符串的最 `右` & `左` 索引

- `center` 为 `L` 到 `R` 最长回文字符串 对应的 中心索引

- `mirror` 为 `i` 关于 `center` 的镜像，即 $mirror+i=2*center-1$

##### 已知 $f(0~i-1)$，求解 $f(i)$
1. 初始化 $f(i)$
    - 若 `i<R`，即 当前位置 在 最长回文子字符串内，`f(i)=min(f(mirror), R-i)`
    - 否则，`f(i)` 初始化为 1

2. 在初始化后的 $f(i)$ 基础上，继续向 2 边扩展，直到不匹配，并更新 `R`, `center` 值

## DAY18: 2025/9/15

### $1.$ [leetcode-516](https://leetcode.cn/problems/longest-palindromic-subsequence/)

#### 思路：区间 DP
- `dp[i][j]`: 表示 `s[i~j]` 中最长回文子序列 长度

- 递推公式：$dp[i][j]=$
    - $dp[i+1][j-1]+2, if s[i]==s[j]$
    
    - $max(dp[i+1][j], dp[i][j-1]), else$
    
    > 注：`dp[i+1][j]` 包含了在 `k` 处满足 $s[k]==s[j]$ 的情况，因此无需第三层 `k` 的循环

- 遍历顺序：$i,j$ 均递增

- 初始化：对角线元素为 $1$，其余元素为 $0$

## DAY19: 2025/9/17

### $1.$ [leetcode-2349](https://leetcode.cn/problems/design-a-number-container-system/)

#### 方法一：unordered_map + priority_queue<geater<>>

##### 数据类型
- `nums = unordered_map<int, int>`: 存储 `(index, num)`

- `heap = unordered_map<int, priority_queue<int, vector<int>, greater<>>>`: 存储 `num, indexs`
##### 操作
- `change` 时直接 `nums[index]=number, heap[number].push(index)`

- `find` 时 `pop heap[number]` 直到 *空* 或 *nums[top]==number*，分类返回结果

## DAY20: 2025/9/20

### $1.$ [leetcode-3508](https://leetcode.cn/problems/implement-router/)

#### 思路：按题意模拟

##### Data
- pack = tuple(source, destination, timestamp)

- queue<pack> que
- set<pack> exist
- unordered_map<int, deque<int>> map

##### Function
- addPacket
    ```cpp
    if(exsit.contains(new_pack)) return false; // 重复则失败

    if(size >= limit) forwardPacket(); // 删除第一个

    que.push(new_pack);
    set.insert(new_pack);
    map[destination].push(timestamp);

    return true
    ```

- forwardPacket()
    ```cpp
    if(empty) return {}; // 为空

    data = front();
    que.pop();
    exsit.erase(data);

    map[data[1]].pop();

    return {data[0], data[1], data[2]};
    ```

- getCount()
    ```cpp
    start = lower_bound(startstamp);
    end = upper_bound(endstamp);

    return start-end;
    ```

##### 注意事项
- 获取 `queue` 队头：`queue.front`

- `unordered_set()` 需要有 `operator==` 和 `hash()` 哈希函数，`int` 等简单类型可以使用，但 `vector<int>`, `pair<int, int>` 等不可使用。
- `set()` 只需 `operator<`，故可用 `set` 替代 `unordered_set`
- `set.contains()` 可判断是否存在元素
- `vector<int>` 不可解包，`tuple`, `pair` 可以解包
- multiset 具有成员函数 `upper_bound(val)` 与 `lower_bound(val)`，可以获取 `<val` 第一个元素 和 `>=val` 第一个元素的 **迭代器**
- $++iterator ≠ iterator+1(error!)$
- queue 可以 **二分查找**
    ```cpp
    auto pos1 = lower_bound(sameDestQue[destination].begin(),
                            sameDestQue[destination].end(), startTime);
    auto pos2 = upper_bound(sameDestQue[destination].begin(),
                            sameDestQue[destination].end(), endTime);
    return pos2 - pos1;
    ```

## DAY21: 2025/9/25

### $1.$ [leetcode-166](https://leetcode.cn/problems/fraction-to-recurring-decimal/)

#### 思路
每一次运算后，**分子** 变为 $余数×10$，将 整数结果 添加到 结果的字符串 中

##### 处理循环
使用 `unordered_map` 存储 *整数结果* 和 *当前位置*

若 *当前整数结果* 已经在 `set` 中，则发生循环，在对应位置添加 `(`，并在最后添加 `)`

#### 注意事项
此题有很多 **细节**！！！

- 添加 `(` 的位置是 `set[res]`，使用 `ans.insert(pos, 1, '(')`

- 处理整除：可一次性整除时，最后无需 添加 `.`
- 处理输入负数：分子分母异号时 `ans+'-'`，分子分母全变为整数，进行后续操作
> 注意：分子负数，分母负数

- `to_string(int)` 可直接将 **数字** 变成 **相应数字** 的字符串，不是 ASCII码
- 初始的位置 `pos` 直接赋值 `ans.size()`，而不是固定的数值
> 需要考虑负数，第一次的结果长度等

- 输入两个 `int` 值，但后续计算需要使用 `long long`，因此需要先转化为 `ll`。此外，计算结果 `res` 也必须 `ll`

## DAY22: 2025/9/27

### $1.$ [leetcode-812](https://leetcode.cn/problems/largest-triangle-area)
#### 注意事项
三角形面积公式

$$\frac12 \times (x1-x2, y1-y2) \times (x1-x3, y1-x3)$$
或
$$
\frac12 \times
\begin{vmatrix}
\begin{vmatrix}
x1 & y1 & 1 \\
x2 & y2 & 1 \\
x3 & y3 & 1
\end{vmatrix}
\end{vmatrix}
$$

注意 `x_max` 被修改后，再次使用已经不是原来的 `x_max`

### $2.$ [leetcode=587](https://leetcode.cn/problems/erect-the-fence)

#### 思路：凸包问题

## DAY23: 2025/9/29

### $1.$ [leetcode-1039](https://leetcode.cn/problems/minimum-score-triangulation-of-polygon/)

#### 思路：区间DP / 记忆化搜索
- `dp[i][j]`: 考虑连续的 $i, i+1, ..., j$ 点所围成的凸边形 三角形剖分的 *最低得分*

- 状态转移方程：$dp[i][j] = dp[i][k] + dp[k][j] + values[i]*values[k]*values[j]$
  > - 注意：此题 区间DP 采用 $i,k + k,j$，中间没有 $+1$  
  > - 若有 +1，则默认 k+1 到 j 必须有一条边；因此缺少了很多情况  
  > - 而紧密连接时则不需要，此时的 $2$ 个最小分支 构成的的才是正确答案
- 初始化：
    - $l=0$ $or$ $l=1$，`dp[i][j] = 0`;
    - else: `dp[i][j] = INT_MAX`

#### 总结
- DP 是优化 *搜索* 和 *回溯* 的方法，需要考虑是否可以 DP

- 数据 $n \leq 50$ 时，考虑 $O(n^3)$ 算法 或 暴力枚举，而 区间DP 恰好符合

## DAY24: 2025/10/6

### $1.$ [leetcode-778](https://leetcode.cn/problems/swim-in-rising-water)
#### 法一：二分答案 $+$ BFS
由于答案具有 **单调性**，即若 $x$ 存在道路，则 $x+1$ 也存在道路，需要寻找最小的 $x$，可使用 *二分答案* 法

##### 步骤
- 从区间 $[0, n*n-1]$ 开始，区间表示能存在道路的所有可能

- 对于每一个区间的 $mid$，执行 $BFS$ 检查是否存在 *道路*
- 若存在，更新 $right=mid$；若不存在，更新 $left=mid+1$
- 直到 $left==right$，即找到最小可能的答案

#### 法二：并查集
- 模拟 *下雨* 过程，开始设置所有坐标的 *根* 为 $(-1,-1)$

- 每个 $t$ 时刻设置 $t$ 对应坐标 $(x,y)$ 的 *根* 为 $(x,y)$，并检查四周是否存在非负的坐标，若存在，则合并
- $t+1$ 直到 $find(n-1, n-1)=(0,0)$，即存在道路

#### 法三：dijkstra 算法
- 将矩阵模拟成有向图

- $edge\{(x_1, y_1), (x_2, y_2)\}=grid[x_2][y_2]$
- 更新时不是 $dis+edge$，而是直接 $edge$
- 模拟算法即可

## DAY25: 2025/10/7

### $1.$ [leetcode-1488](https://leetcode.cn/problems/avoid-flood-in-the-city)

#### 思路：贪心 $+$ 二分搜索
- $rains[i]==0$ 即*下雨*：`set.insert(i)`

- $map.count(ranis[i])==1$ 即 *该湖泊已满*：找到 `set` 中 **最小的** 大于 $i$ 的日期 $data$。若找不到，则返回空数组
    - 在 `set` 中删除该值，表示该天不能抽干其他湖泊

    - 设置 $ans[data]=rains[i]$，表示该天抽干了 $rains[i]$ 号湖泊
    - $map[ranis[i]]=i$，表示 $rains[i]$ 号湖泊最近一次满的日期是 $i$
- $map.count(rains[i])==0$ 即 *该湖泊为空*：设置 $map[rains[i]]=i$ 即可

#### 数据结构 $+$ 操作
- `unordered_map<int, int`: 用于记录某个湖泊 最近一次满 的日期
    - `map.count(ranis[i])`: 查找是否满

    - `map[ranis[i]]=i`: 重新赋值

- `set`: 有序集合，用于储存可以抽干湖泊的天
    - `set.lower_bound(map[rains[i]])`: 二分查找 目标日期

    - `set.erase(data)`: 删除日期，表示已经使用过

- `vector<int>`: 用于储存答案

#### 错误 $+$ 注意
- 必须选择 **大于 湖泊最近满日期的 可抽干的日期**

- 用过的抽干日期，必须 **删除**
- 下雨后，需要更新 **湖泊最近满日期**
- 使用 $set$ 有序数组，使用二分查找，优化时间复杂度

### $2.$ [leetcode-2300](https://leetcode.cn/problems/successful-pairs-of-spells-and-potions/)

#### 用法
- `vector` 的 二分查找
    - `lower_bound(vec.begin(), vec.end(), num)`: 返回 $≥ num$ 的第一个元素的 *迭代器*
    
    - `upper_bound(vec.begin(), vec.end(), num)`: 返回 $> num$ 的第一个元素的 *迭代器*

- 已知 `vector` 迭代器，求索引位置：`it-vec.begin()`
    > `vector` 的迭代器可直接加减运算  
    > `vec.end()-it` 可获取 **包括 it 的** vec 之后的元素个数
- 计算 **不符合要求的** *最大的值* 的公式：$(success+i-1)/i-1$
    - 可整除时 $(success+i-1)/i$ 返回 **符合要求的** *最小值*，$-1$ 后变为所求
    
    - 不可整除时 $(success)/i$ 即是目标，先 $(suc+i-1)/i$ 使值 $+1$，最后再 $-1$ 得到目标