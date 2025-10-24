# Monday

Date: 2025/10/20

Duration: 5m

### Problem 1 — [leetcode-2011](https://leetcode.cn/problems/final-value-of-variable-after-performing-operations)
- Status: Solved
- Difficulty: Easy
- Language: C++

#### Approach
按规则遍历即可

#### Code
```cpp
class Solution {
public:
    int finalValueAfterOperations(vector<string>& operations) {
        int x = 0;
        for(string& str : operations) {
            if(str == "X++" || str == "++X") {
                ++x;
            }
            else {
                --x;
            }
        }
        return x;
    }
};
```

# Thursday

Date: 2025/10/23

### Problem 1 — [leetcode-3461](https://leetcode.cn/problems/check-if-digits-are-equal-in-string-after-operations-i)

- Status: Solved
- Difficulty: Easy
- Language: C++

### Approach
暴力模拟

#### Code
```cpp
class Solution {
public:
    bool hasSameDigits(string s) {
        while(s.size() != 2) {
            string temp = "";
            for(int i=0; i<s.size()-1; ++i) {
                int d0 = s[i]-'0';
                int d1 = s[i+1]-'0';
                temp += char((d0+d1)%10 + '0');
            }
            s = temp;
            // cout << s << endl;
        }
        if(s[0] == s[1]) return true;
        else return false;
    }
};
```

#### Mistake
- 不要忘记取模 `%10`
- `string += ...` 用于在 `string` 后添加 `string` 或 `char`


# Friday

Date: 2025/10/24

### Problem 1 — [2048](https://leetcode.cn/problems/next-greater-numerically-balanced-number)

- Status: Solved
- Difficulty: Medium
- Language: C++

#### Approach
暴力枚举 + 判断

#### Code
```cpp
class Solution {
    bool is_balance(long long num) {
        vector<int> cnt(10, 0);
        while(num) {
            int d = num % 10;
            ++cnt[d];
            if(cnt[d]>d) return false;
            num /= 10;
        }

        for(int i=0; i<10; ++i) {
            if(cnt[i] !=0 && cnt[i] != i) return false;
        }

        return true;
    }
public:
    int nextBeautifulNumber(int n) {
        long long num = n+1;
        while(num<=1224444) {
            if(is_balance(num)) break;
            ++num;
        }
        return num;
    }
};
```

#### Mistake
- 最大值 $122444$
- `is_balance()` 初始化时 `cnt` 用 `vector` 初始化为全 $0$ 或 `int[10]` 并循环赋值
- `int[10]` 具有不确定性，有时初始化全为 $0$，有时出错
- 测试时正确，提交后也会出错


