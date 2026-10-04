---
comments: true
difficulty: Hard
rating: 2628
source: Biweekly Contest 142 Q4
tags:
    - String
    - Dynamic Programming
    - Prefix Sum
---

<!-- problem:start -->

# [3333. Find the Original Typed String II](https://leetcode.com/problems/find-the-original-typed-string-ii)

[Tài liệu tiếng Trung](/solution/3300-3399/3333.Find%20the%20Original%20Typed%20String%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Alice đang cố gắng gõ một chuỗi cụ thể trên máy tính. Tuy nhiên, cô ấy khá vụng về và <strong>có thể</strong> nhấn giữ một phím quá lâu, khiến một ký tự được gõ <strong>nhiều</strong> lần.</p>

<p>Bạn được cho một chuỗi <code>word</code>, đại diện cho kết quả <strong>cuối cùng</strong> hiển thị trên màn hình của Alice. Bạn cũng được cho một số nguyên <code>k</code> <strong>dương</strong>.</p>

<p>Hãy trả về tổng số chuỗi gốc <em>có thể</em> là chuỗi Alice <em>đã định</em> gõ, nếu cô ấy định gõ một chuỗi có độ dài <strong>ít nhất</strong> <code>k</code>.</p>

<p>Vì đáp án có thể rất lớn, hãy trả về đáp án <strong>theo modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">word = &quot;aabbccdd&quot;, k = 7</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các chuỗi có thể là: <code>&quot;aabbccdd&quot;</code>, <code>&quot;aabbccd&quot;</code>, <code>&quot;aabbcdd&quot;</code>, <code>&quot;aabccdd&quot;</code> và <code>&quot;abbccdd&quot;</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">word = &quot;aabbccdd&quot;, k = 8</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chuỗi duy nhất có thể là <code>&quot;aabbccdd&quot;</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">word = &quot;aaabbb&quot;, k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">8</span></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= word.length &lt;= 5 * 10<sup>5</sup></code></li>
    <li><code>word</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
    <li><code>1 &lt;= k &lt;= 2000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động + Tổng tiền tố

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi nhóm phải giữ lại ít nhất một ký tự và tổng độ dài phải ít nhất là $k$. Với $n \le 5 \times 10^5$ và $k \le 2000$, ta biểu diễn “độ dài ít nhất $k$” bằng số cách không bị giới hạn $a$ trừ đi số cách có độ dài nhỏ hơn $k$.
>
> Bắt buộc chọn một ký tự từ mỗi nhóm, nên $a$ là tích độ dài các nhóm; phần sức chứa còn lại được đưa vào $\textit{nums}$ và $k$ được giảm theo số nhóm. Nếu $k < 1$, mọi lựa chọn đều đã thỏa mãn giới hạn.
>
> $f[i][j]$ là số cách sử dụng $j$ lượt chọn thêm trong $i$ nhóm đầu tiên. Chuyển trạng thái là một tổng trên đoạn, vì vậy tổng tiền tố giúp đưa độ phức tạp về $O(k)$. $b$ là hàng cuối bên dưới $k$, nên đáp án là $a-b$.

<!-- thinking:end -->

Với điều kiện độ dài ít nhất là $k$, ta có thể chia bài toán thành hai phần:

- Không giới hạn độ dài: với mỗi nhóm các ký tự liên tiếp giống nhau, ta có thể chọn từ $1$ đến độ dài của nhóm. Gọi số cách là $a$.
- Với độ dài nhỏ hơn $k$, gọi số cách là $b$.

Vậy đáp án cuối cùng là $a - b$.

Ta có thể nhóm các ký tự liên tiếp giống nhau trong chuỗi $\textit{word}$. Vì phải chọn ít nhất một ký tự từ mỗi nhóm, nếu một nhóm còn nhiều hơn $0$ ký tự có thể chọn, ta thêm số đó vào mảng $\textit{nums}$. Sau khi chọn trước một ký tự từ mỗi nhóm, ta cập nhật số ký tự cần chọn còn lại $k$.

Nếu $k < 1$, điều đó có nghĩa là sau khi chọn một ký tự từ mỗi nhóm, yêu cầu về độ dài ít nhất $k$ đã được thỏa mãn, nên đáp án là $a$.

Ngược lại, ta cần tính giá trị của $b$. Ta sử dụng mảng hai chiều $\textit{f}$, trong đó $\textit{f}[i][j]$ biểu diễn số cách chọn $j$ ký tự từ $i$ nhóm đầu tiên. Ban đầu, $\textit{f}[0][0] = 1$, nghĩa là có $1$ cách chọn $0$ ký tự từ $0$ nhóm. Khi đó $b = \sum_{j=0}^{k-1} \text{f}[m][j]$, trong đó $m$ là độ dài của $\textit{nums}$. Đáp án là $a - b$.

Xét công thức chuyển của $\textit{f}[i][j]$. Với nhóm ký tự thứ $i$, giả sử độ dài còn lại của nhóm là $x$. Với mỗi $j$, ta có thể liệt kê số ký tự $l$ được chọn từ nhóm này, trong đó $l \in [0, \min(x, j)]$. Khi đó, $\textit{f}[i][j]$ có thể được chuyển từ $\textit{f}[i-1][j-l]$. Ta có thể sử dụng tổng tiền tố để tối ưu chuyển trạng thái này.

Độ phức tạp thời gian là $O(n + k^2)$ và độ phức tạp không gian là $O(k^2)$, trong đó $n$ là độ dài của chuỗi $\textit{word}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def possibleStringCount(self, word: str, k: int) -> int:
        mod = 10**9 + 7
        nums = []
        ans = 1
        cur = 0
        for i, c in enumerate(word):
            cur += 1
            if i == len(word) - 1 or c != word[i + 1]:
                if cur > 1:
                    if k > 0:
                        nums.append(cur - 1)
                    ans = ans * cur % mod
                cur = 0
                k -= 1
        if k < 1:
            return ans
        m = len(nums)
        f = [[0] * k for _ in range(m + 1)]
        f[0][0] = 1
        for i, x in enumerate(nums, 1):
            s = list(accumulate(f[i - 1], initial=0))
            for j in range(k):
                f[i][j] = (s[j + 1] - s[j - min(x, j)] + mod) % mod
        return (ans - sum(f[m][j] for j in range(k))) % mod
```

#### Java

```java
class Solution {
    public int possibleStringCount(String word, int k) {
        final int mod = (int) 1e9 + 7;
        List<Integer> nums = new ArrayList<>();
        long ans = 1;
        int cur = 0;
        int n = word.length();

        for (int i = 0; i < n; i++) {
            cur++;
            if (i == n - 1 || word.charAt(i) != word.charAt(i + 1)) {
                if (cur > 1) {
                    if (k > 0) {
                        nums.add(cur - 1);
                    }
                    ans = ans * cur % mod;
                }
                cur = 0;
                k--;
            }
        }

        if (k < 1) {
            return (int) ans;
        }

        int m = nums.size();
        int[][] f = new int[m + 1][k];
        f[0][0] = 1;

        for (int i = 1; i <= m; i++) {
            int x = nums.get(i - 1);
            long[] s = new long[k + 1];
            for (int j = 0; j < k; j++) {
                s[j + 1] = (s[j] + f[i - 1][j]) % mod;
            }
            for (int j = 0; j < k; j++) {
                int l = Math.max(0, j - x);
                f[i][j] = (int) ((s[j + 1] - s[l] + mod) % mod);
            }
        }

        long sum = 0;
        for (int j = 0; j < k; j++) {
            sum = (sum + f[m][j]) % mod;
        }

        return (int) ((ans - sum + mod) % mod);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int possibleStringCount(string word, int k) {
        const int mod = 1e9 + 7;
        vector<int> nums;
        long long ans = 1;
        int cur = 0;
        int n = word.size();

        for (int i = 0; i < n; ++i) {
            cur++;
            if (i == n - 1 || word[i] != word[i + 1]) {
                if (cur > 1) {
                    if (k > 0) {
                        nums.push_back(cur - 1);
                    }
                    ans = ans * cur % mod;
                }
                cur = 0;
                k--;
            }
        }

        if (k < 1) {
            return ans;
        }

        int m = nums.size();
        vector<vector<int>> f(m + 1, vector<int>(k, 0));
        f[0][0] = 1;

        for (int i = 1; i <= m; ++i) {
            int x = nums[i - 1];
            vector<long long> s(k + 1, 0);
            for (int j = 0; j < k; ++j) {
                s[j + 1] = (s[j] + f[i - 1][j]) % mod;
            }
            for (int j = 0; j < k; ++j) {
                int l = max(0, j - x);
                f[i][j] = (s[j + 1] - s[l] + mod) % mod;
            }
        }

        long long sum = 0;
        for (int j = 0; j < k; ++j) {
            sum = (sum + f[m][j]) % mod;
        }

        return (ans - sum + mod) % mod;
    }
};
```

#### Go

```go
func possibleStringCount(word string, k int) int {
    const mod = 1_000_000_007
    nums := []int{}
    ans := 1
    cur := 0
    n := len(word)

    for i := 0; i < n; i++ {
        cur++
        if i == n-1 || word[i] != word[i+1] {
            if cur > 1 {
                if k > 0 {
                    nums = append(nums, cur-1)
                }
                ans = ans * cur % mod
            }
            cur = 0
            k--
        }
    }

    if k < 1 {
        return ans
    }

    m := len(nums)
    f := make([][]int, m+1)
    for i := range f {
        f[i] = make([]int, k)
    }
    f[0][0] = 1

    for i := 1; i <= m; i++ {
        x := nums[i-1]
        s := make([]int, k+1)
        for j := 0; j < k; j++ {
            s[j+1] = (s[j] + f[i-1][j]) % mod
        }
        for j := 0; j < k; j++ {
            l := j - x
            if l < 0 {
                l = 0
            }
            f[i][j] = (s[j+1] - s[l] + mod) % mod
        }
    }

    sum := 0
    for j := 0; j < k; j++ {
        sum = (sum + f[m][j]) % mod
    }

    return (ans - sum + mod) % mod
}
```

#### TypeScript

```ts
function possibleStringCount(word: string, k: number): number {
    const mod = 1_000_000_007;
    const nums: number[] = [];
    let ans = 1;
    let cur = 0;
    const n = word.length;

    for (let i = 0; i < n; i++) {
        cur++;
        if (i === n - 1 || word[i] !== word[i + 1]) {
            if (cur > 1) {
                if (k > 0) {
                    nums.push(cur - 1);
                }
                ans = (ans * cur) % mod;
            }
            cur = 0;
            k--;
        }
    }

    if (k < 1) {
        return ans;
    }

    const m = nums.length;
    const f: number[][] = Array.from({ length: m + 1 }, () => Array(k).fill(0));
    f[0][0] = 1;

    for (let i = 1; i <= m; i++) {
        const x = nums[i - 1];
        const s: number[] = Array(k + 1).fill(0);
        for (let j = 0; j < k; j++) {
            s[j + 1] = (s[j] + f[i - 1][j]) % mod;
        }
        for (let j = 0; j < k; j++) {
            const l = Math.max(0, j - x);
            f[i][j] = (s[j + 1] - s[l] + mod) % mod;
        }
    }

    let sum = 0;
    for (let j = 0; j < k; j++) {
        sum = (sum + f[m][j]) % mod;
    }

    return (ans - sum + mod) % mod;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
