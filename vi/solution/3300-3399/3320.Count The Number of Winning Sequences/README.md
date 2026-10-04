---
comments: true
difficulty: Hard
rating: 2153
source: Weekly Contest 419 Q3
tags:
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [3320. Count The Number of Winning Sequences](https://leetcode.com/problems/count-the-number-of-winning-sequences)

[中文文档](/solution/3300-3399/3320.Count%20The%20Number%20of%20Winning%20Sequences/README.md)

## Mô tả

<!-- description:start -->

<p>Alice và Bob đang chơi một trò chơi chiến đấu giả tưởng gồm <code>n</code> lượt, trong đó mỗi lượt họ triệu hồi một trong ba sinh vật phép thuật: Rồng Lửa, Rắn Nước hoặc Người Đá Đất. Trong mỗi lượt, hai người chơi <strong>đồng thời</strong> triệu hồi sinh vật của mình và được cộng điểm như sau:</p>

<ul>
    <li>Nếu một người chơi triệu hồi Rồng Lửa còn người kia triệu hồi Người Đá Đất, người triệu hồi <strong>Rồng Lửa</strong> được cộng một điểm.</li>
    <li>Nếu một người chơi triệu hồi Rắn Nước còn người kia triệu hồi Rồng Lửa, người triệu hồi <strong>Rắn Nước</strong> được cộng một điểm.</li>
    <li>Nếu một người chơi triệu hồi Người Đá Đất còn người kia triệu hồi Rắn Nước, người triệu hồi <strong>Người Đá Đất</strong> được cộng một điểm.</li>
    <li>Nếu cả hai người chơi triệu hồi cùng một sinh vật, không ai được cộng điểm.</li>
</ul>

<p>Cho một chuỗi <code>s</code> gồm <code>n</code> ký tự <code>&#39;F&#39;</code>, <code>&#39;W&#39;</code> và <code>&#39;E&#39;</code>, biểu diễn dãy sinh vật Alice sẽ triệu hồi trong mỗi lượt:</p>

<ul>
    <li>Nếu <code>s[i] == &#39;F&#39;</code>, Alice triệu hồi Rồng Lửa.</li>
    <li>Nếu <code>s[i] == &#39;W&#39;</code>, Alice triệu hồi Rắn Nước.</li>
    <li>Nếu <code>s[i] == &#39;E&#39;</code>, Alice triệu hồi Người Đá Đất.</li>
</ul>

<p>Dãy nước đi của Bob chưa biết, nhưng đảm bảo Bob sẽ không triệu hồi cùng một sinh vật trong hai lượt liên tiếp. Bob <em>đánh bại</em> Alice nếu tổng số điểm Bob nhận được sau <code>n</code> lượt <strong>lớn hơn nghiêm ngặt</strong> số điểm Alice nhận được.</p>

<p>Hãy trả về số dãy nước đi phân biệt mà Bob có thể sử dụng để đánh bại Alice.</p>

<p>Vì đáp án có thể rất lớn, hãy trả về đáp án <strong>chia lấy dư</strong> cho <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;FFF&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Bob có thể đánh bại Alice bằng một trong các dãy nước đi sau: <code>&quot;WFW&quot;</code>, <code>&quot;FWF&quot;</code> hoặc <code>&quot;WEW&quot;</code>. Lưu ý rằng các dãy thắng khác như <code>&quot;WWE&quot;</code> hoặc <code>&quot;EWW&quot;</code> không hợp lệ vì Bob không thể thực hiện cùng một nước đi hai lần liên tiếp.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;FWEFW&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">18</span></p>

<p><strong>Giải thích:</strong></p>

<p><w>Bob có thể đánh bại Alice bằng một trong các dãy nước đi sau: <code>&quot;FWFWF&quot;</code>, <code>&quot;FWFWE&quot;</code>, <code>&quot;FWEFE&quot;</code>, <code>&quot;FWEWE&quot;</code>, <code>&quot;FEFWF&quot;</code>, <code>&quot;FEFWE&quot;</code>, <code>&quot;FEFEW&quot;</code>, <code>&quot;FEWFE&quot;</code>, <code>&quot;WFEFE&quot;</code>, <code>&quot;WFEWE&quot;</code>, <code>&quot;WEFWF&quot;</code>, <code>&quot;WEFWE&quot;</code>, <code>&quot;WEFEF&quot;</code>, <code>&quot;WEFEW&quot;</code>, <code>&quot;WEWFW&quot;</code>, <code>&quot;WEWFE&quot;</code>, <code>&quot;EWFWE&quot;</code> hoặc <code>&quot;EWEWE&quot;</code>.</w></p>
</div>

<p>&nbsp;</p>
<p><strong>Các ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= s.length &lt;= 1000</code></li>
    <li><code>s[i]</code> là một trong các ký tự <code>&#39;F&#39;</code>, <code>&#39;W&#39;</code> hoặc <code>&#39;E&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm có ghi nhớ

<!-- thinking:start -->

> **Tư duy**
>
> Với $|s| \le 1000$ và ba sinh vật trong mỗi lượt (khác với lượt trước), việc liệt kê các dãy là không thể. Trạng thái gồm lượt hiện tại, chênh lệch điểm và sinh vật cuối cùng của Bob.
>
> Nếu số lượt còn lại không đủ để vượt qua Alice, trạng thái này có giá trị $0$. Chênh lệch có thể âm; Python có thể dùng trực tiếp làm chỉ số, còn các ngôn ngữ khác dịch chỉ số thêm $n$.
>
> $\textit{dfs}(i,j,k)$ thử từng sinh vật khác $k$, cập nhật chênh lệch dựa trên kết quả của lượt đó, rồi cộng các kết quả theo modulo $10^9+7$.

<!-- thinking:end -->

Ta xây dựng hàm $\textit{dfs}(i, j, k)$, trong đó $i$ biểu diễn việc bắt đầu từ ký tự thứ $i$ của chuỗi $s$, $j$ biểu diễn chênh lệch điểm hiện tại giữa $\textit{Alice}$ và $\textit{Bob}$, còn $k$ biểu diễn sinh vật cuối cùng mà $\textit{Bob}$ đã triệu hồi. Hàm này tính số dãy nước đi mà $\textit{Bob}$ có thể thực hiện để đánh bại $\textit{Alice}$.

Đáp án là $\textit{dfs}(0, 0, -1)$, trong đó $-1$ cho biết $\textit{Bob}$ chưa triệu hồi sinh vật nào. Trong các ngôn ngữ khác Python, vì chênh lệch điểm có thể âm, ta có thể cộng $n$ vào chênh lệch điểm để đảm bảo nó không âm.

Quy trình tính hàm $\textit{dfs}(i, j, k)$ như sau:

- Nếu $n - i \leq j$, số lượt còn lại không đủ để $\textit{Bob}$ vượt qua điểm của $\textit{Alice}$, nên trả về $0$.
- Nếu $i \geq n$, tất cả các lượt đã kết thúc. Nếu điểm của $\textit{Bob}$ nhỏ hơn $0$, trả về $1$; ngược lại, trả về $0$.
- Nếu không, ta liệt kê các sinh vật mà $\textit{Bob}$ có thể triệu hồi trong lượt này. Nếu sinh vật được triệu hồi trong lượt này giống với sinh vật được triệu hồi ở lượt trước, $\textit{Bob}$ không thể thắng lượt này, nên ta bỏ qua. Nếu không, ta đệ quy tính $\textit{dfs}(i + 1, j + \textit{calc}(d[s[i]], l), l)$, trong đó $\textit{calc}(x, y)$ biểu diễn kết quả giữa $x$ và $y$, còn $d$ là ánh xạ các ký tự sang $\textit{012}$. Ta cộng tất cả kết quả và lấy modulo $10^9 + 7$.

Độ phức tạp thời gian là $O(n^2 \times k^2)$, trong đó $n$ là độ dài chuỗi $s$, còn $k$ là kích thước của tập ký tự. Độ phức tạp không gian là $O(n^2 \times k)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countWinningSequences(self, s: str) -> int:
        def calc(x: int, y: int) -> int:
            if x == y:
                return 0
            if x < y:
                return 1 if x == 0 and y == 2 else -1
            return -1 if x == 2 and y == 0 else 1

        @cache
        def dfs(i: int, j: int, k: int) -> int:
            if len(s) - i <= j:
                return 0
            if i >= len(s):
                return int(j < 0)
            res = 0
            for l in range(3):
                if l == k:
                    continue
                res = (res + dfs(i + 1, j + calc(d[s[i]], l), l)) % mod
            return res

        mod = 10**9 + 7
        d = {"F": 0, "W": 1, "E": 2}
        ans = dfs(0, 0, -1)
        dfs.cache_clear()
        return ans
```

#### Java

```java
class Solution {
    private int n;
    private char[] s;
    private int[] d = new int[26];
    private Integer[][][] f;
    private final int mod = (int) 1e9 + 7;

    public int countWinningSequences(String s) {
        d['W' - 'A'] = 1;
        d['E' - 'A'] = 2;
        this.s = s.toCharArray();
        n = this.s.length;
        f = new Integer[n][n + n + 1][4];
        return dfs(0, n, 3);
    }

    private int dfs(int i, int j, int k) {
        if (n - i <= j - n) {
            return 0;
        }
        if (i >= n) {
            return j - n < 0 ? 1 : 0;
        }
        if (f[i][j][k] != null) {
            return f[i][j][k];
        }

        int ans = 0;
        for (int l = 0; l < 3; ++l) {
            if (l == k) {
                continue;
            }
            ans = (ans + dfs(i + 1, j + calc(d[s[i] - 'A'], l), l)) % mod;
        }
        return f[i][j][k] = ans;
    }

    private int calc(int x, int y) {
        if (x == y) {
            return 0;
        }
        if (x < y) {
            return x == 0 && y == 2 ? 1 : -1;
        }
        return x == 2 && y == 0 ? -1 : 1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countWinningSequences(string s) {
        int n = s.size();
        int d[26]{};
        d['W' - 'A'] = 1;
        d['E' - 'A'] = 2;
        int f[n][n + n + 1][4];
        memset(f, -1, sizeof(f));
        auto calc = [](int x, int y) -> int {
            if (x == y) {
                return 0;
            }
            if (x < y) {
                return x == 0 && y == 2 ? 1 : -1;
            }
            return x == 2 && y == 0 ? -1 : 1;
        };
        const int mod = 1e9 + 7;
        auto dfs = [&](this auto&& dfs, int i, int j, int k) -> int {
            if (n - i <= j - n) {
                return 0;
            }
            if (i >= n) {
                return j - n < 0 ? 1 : 0;
            }
            if (f[i][j][k] != -1) {
                return f[i][j][k];
            }
            int ans = 0;
            for (int l = 0; l < 3; ++l) {
                if (l == k) {
                    continue;
                }
                ans = (ans + dfs(i + 1, j + calc(d[s[i] - 'A'], l), l)) % mod;
            }
            return f[i][j][k] = ans;
        };
        return dfs(0, n, 3);
    }
};
```

#### Go

```go
func countWinningSequences(s string) int {
    const mod int = 1e9 + 7
    d := [26]int{}
    d['W'-'A'] = 1
    d['E'-'A'] = 2
    n := len(s)
    f := make([][][4]int, n)
    for i := range f {
        f[i] = make([][4]int, n+n+1)
        for j := range f[i] {
            for k := range f[i][j] {
                f[i][j][k] = -1
            }
        }
    }
    calc := func(x, y int) int {
        if x == y {
            return 0
        }
        if x < y {
            if x == 0 && y == 2 {
                return 1
            }
            return -1
        }
        if x == 2 && y == 0 {
            return -1
        }
        return 1
    }
    var dfs func(int, int, int) int
    dfs = func(i, j, k int) int {
        if n-i <= j-n {
            return 0
        }
        if i >= n {
            if j-n < 0 {
                return 1
            }
            return 0
        }
        if v := f[i][j][k]; v != -1 {
            return v
        }
        ans := 0
        for l := 0; l < 3; l++ {
            if l == k {
                continue
            }
            ans = (ans + dfs(i+1, j+calc(d[s[i]-'A'], l), l)) % mod
        }
        f[i][j][k] = ans
        return ans
    }
    return dfs(0, n, 3)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
