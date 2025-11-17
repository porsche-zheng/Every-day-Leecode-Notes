# Sunday

Date: 2025/11/16

Duration: 20min

## Problem 1 — 
- Difficulty: Medium
- Time spent: 20m

### Approach
- 找出所有连续全 `1` 子串
- 每个子串，都会对最终结果 `sum` 贡献 `(n+1)*n/2` 中情况，其中 `n` 为子串长度，也即 `1` 的个数
- 结合 `mod` 得到 `sum = (sum+(n+1)*n/2 mod m) mod m` 的公式

### Complexity
- Time: $O(n)$
- Space: $O(1)$

### Code
```cpp
const int mod = 10e8+7;

class Solution {
public:
    int numSub(string s) {
        long long sum=0;
        long long cnt = 0;
        for(char ch : s) {
            if(ch=='1') {
                ++cnt;
            }
            else if(cnt) {
                sum = (sum+ (cnt+1)*cnt/2 % mod ) % mod;
                cnt = 0;
            }
        }
        if(cnt) {
            sum = (sum+ (cnt+1)*cnt/2 % mod ) % mod;
        }
        
        return sum;
    }
};
```

### Note & Mistake
- `long long = int*int` 也可能溢出，运算数中必须至少一个是 `long long`
- 注意在循环结束后再次计算，*清除* `cnt`。即末尾全是 `1`，这些 `1` 需要循环外计算
- 注意！`10e8+7` 才是 $10^9+7$，`10eN` = $10 \times 10^N$！
    - `1e9` 也可以表示 $10^9$。一般地，`aeb`=$a \times 10^b$，但注意科学计数法中 $|a|∈[0, 9)$
- 但 `10e8` 是 `double` 类型，`10...0` 是 `int` 或 `long long` 类型
    - 若 `num` 是 `int` 或 `long long` 类型，不能 `num / 10e8`
    - 但可以 `num / 10...0`