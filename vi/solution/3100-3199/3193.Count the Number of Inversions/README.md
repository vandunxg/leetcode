---
comments: true
difficulty: Hard
rating: 2266
source: Biweekly Contest 133 Q4
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [3193. Count the Number of Inversions](https://leetcode.com/problems/count-the-number-of-inversions)

[中文文档](/solution/3100-3199/3193.Count%20the%20Number%20of%20Inversions/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên <code>n</code> và một mảng 2D <code>requirements</code>, trong đó <code>requirements[i] = [end<sub>i</sub>, cnt<sub>i</sub>]</code> biểu diễn chỉ số kết thúc và số lượng <strong>nghịch thế</strong> của mỗi ràng buộc.</p>

<p>Một cặp chỉ số <code>(i, j)</code> trong một mảng số nguyên <code>nums</code> được gọi là một <strong>nghịch thế</strong> nếu:</p>

<ul>
    <li><code>i &lt; j</code> và <code>nums[i] &gt; nums[j]</code></li>
</ul>

<p>Trả về số lượng <span data-keyword="permutation">hoán vị</span> <code>perm</code> của <code>[0, 1, 2, ..., n - 1]</code> sao cho với <strong>mọi</strong> <code>requirements[i]</code>, <code>perm[0..end<sub>i</sub>]</code> có chính xác <code>cnt<sub>i</sub></code> nghịch thế.</p>

<p>Vì đáp án có thể rất lớn, hãy trả về đáp án <strong>theo modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, requirements = [[2,2],[0,0]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Hai hoán vị thỏa mãn là:</p>

<ul>
    <li><code>[2, 0, 1]</code>

    <ul>
        <li>Tiền tố <code>[2, 0, 1]</code> có các nghịch thế <code>(0, 1)</code> và <code>(0, 2)</code>.</li>
        <li>Tiền tố <code>[2]</code> có 0 nghịch thế.</li>
    </ul>
    </li>
    <li><code>[1, 2, 0]</code>
    <ul>
        <li>Tiền tố <code>[1, 2, 0]</code> có các nghịch thế <code>(0, 2)</code> và <code>(1, 2)</code>.</li>
        <li>Tiền tố <code>[1]</code> có 0 nghịch thế.</li>
    </ul>
    </li>

</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, requirements = [[2,2],[1,1],[0,0]]</span></p>

<p><strong>Đầu ra:</strong> 1</p>

<p><strong>Giải thích:</strong></p>

<p>Hoán vị duy nhất thỏa mãn là <code>[2, 0, 1]</code>:</p>

<ul>
    <li>Tiền tố <code>[2, 0, 1]</code> có các nghịch thế <code>(0, 1)</code> và <code>(0, 2)</code>.</li>
    <li>Tiền tố <code>[2, 0]</code> có một nghịch thế <code>(0, 1)</code>.</li>
    <li>Tiền tố <code>[2]</code> có 0 nghịch thế.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 2, requirements = [[0,0],[1,0]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Hoán vị duy nhất thỏa mãn là <code>[0, 1]</code>:</p>

<ul>
    <li>Tiền tố <code>[0]</code> có 0 nghịch thế.</li>
    <li>Tiền tố <code>[0, 1]</code> không có nghịch thế.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>2 &lt;= n &lt;= 300</code></li>
    <li><code>1 &lt;= requirements.length &lt;= n</code></li>
    <li><code>requirements[i] = [end<sub>i</sub>, cnt<sub>i</sub>]</code></li>
    <li><code>0 &lt;= end<sub>i</sub> &lt;= n - 1</code></li>
    <li><code>0 &lt;= cnt<sub>i</sub> &lt;= 400</code></li>
    <li>Input được tạo sao cho tồn tại ít nhất một <code>i</code> thỏa mãn <code>end<sub>i</sub> == n - 1</code>.</li>
    <li>Input được tạo sao cho mọi <code>end<sub>i</sub></code> đều khác nhau.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Đếm các hoán vị có số nghịch thế của tiền tố khớp với các ràng buộc đã cho. Khi đặt giá trị $i$, ta có thể tạo thêm $0..i$ nghịch thế, nên DP theo dõi có bao nhiêu số đã được đặt và chúng có bao nhiêu nghịch thế.
>
> Một ràng buộc cố định một tiền tố; các lớp khác vẫn duyệt qua $j$. Vì $m\le 400$, vòng lặp ba tầng vẫn đủ nhanh.
>
> Gọi $f[i][j]$ là số cách điền $[0..i]$ với $j$ nghịch thế. Nếu có ràng buộc, chỉ tính cột đó; nếu không, cộng $f[i-1][j-k]$ với $k\le\min(i,j)$. Tiền tố đầu tiên phải có số nghịch thế bằng không.

<!-- thinking:end -->

Ta định nghĩa $f[i][j]$ là số hoán vị của $[0..i]$ có $j$ nghịch thế. Xét mối quan hệ giữa số $a_i$ ở chỉ số $i$ và $i$ số trước đó. Nếu $a_i$ nhỏ hơn $k$ số trước đó, thì mỗi số trong $k$ số này tạo thành một cặp nghịch thế với $a_i$, đóng góp $k$ nghịch thế. Vì vậy, ta có thể suy ra công thức chuyển trạng thái:

$$
f[i][j] = \sum_{k=0}^{\min(i, j)} f[i-1][j-k]
$$

Vì bài toán yêu cầu số nghịch thế trong $[0..\textit{end}_i]$ phải bằng $\textit{cnt}_i$, khi tính với $i = \textit{end}_i$, ta chỉ cần tính $f[i][\textit{cnt}_i]$. Các giá trị còn lại của $f[i][..]$ sẽ bằng $0$.

Độ phức tạp thời gian là $O(n \times m \times \min(n, m))$, và độ phức tạp không gian là $O(n \times m)$. Trong đó, $m$ là số nghịch thế lớn nhất và trong bài toán này, $m \le 400$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberOfPermutations(self, n: int, requirements: List[List[int]]) -> int:
        req = [-1] * n
        for end, cnt in requirements:
            req[end] = cnt
        if req[0] > 0:
            return 0
        req[0] = 0
        mod = 10**9 + 7
        m = max(req)
        f = [[0] * (m + 1) for _ in range(n)]
        f[0][0] = 1
        for i in range(1, n):
            l, r = 0, m
            if req[i] >= 0:
                l = r = req[i]
            for j in range(l, r + 1):
                for k in range(min(i, j) + 1):
                    f[i][j] = (f[i][j] + f[i - 1][j - k]) % mod
        return f[n - 1][req[n - 1]]
```

#### Java

```java
class Solution {
    public int numberOfPermutations(int n, int[][] requirements) {
        int[] req = new int[n];
        Arrays.fill(req, -1);
        int m = 0;
        for (var r : requirements) {
            req[r[0]] = r[1];
            m = Math.max(m, r[1]);
        }
        if (req[0] > 0) {
            return 0;
        }
        req[0] = 0;
        final int mod = (int) 1e9 + 7;
        int[][] f = new int[n][m + 1];
        f[0][0] = 1;
        for (int i = 1; i < n; ++i) {
            int l = 0, r = m;
            if (req[i] >= 0) {
                l = r = req[i];
            }
            for (int j = l; j <= r; ++j) {
                for (int k = 0; k <= Math.min(i, j); ++k) {
                    f[i][j] = (f[i][j] + f[i - 1][j - k]) % mod;
                }
            }
        }
        return f[n - 1][req[n - 1]];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numberOfPermutations(int n, vector<vector<int>>& requirements) {
        vector<int> req(n, -1);
        int m = 0;
        for (const auto& r : requirements) {
            req[r[0]] = r[1];
            m = max(m, r[1]);
        }
        if (req[0] > 0) {
            return 0;
        }
        req[0] = 0;
        const int mod = 1e9 + 7;
        vector<vector<int>> f(n, vector<int>(m + 1, 0));
        f[0][0] = 1;
        for (int i = 1; i < n; ++i) {
            int l = 0, r = m;
            if (req[i] >= 0) {
                l = r = req[i];
            }
            for (int j = l; j <= r; ++j) {
                for (int k = 0; k <= min(i, j); ++k) {
                    f[i][j] = (f[i][j] + f[i - 1][j - k]) % mod;
                }
            }
        }
        return f[n - 1][req[n - 1]];
    }
};
```

#### Go

```go
func numberOfPermutations(n int, requirements [][]int) int {
    req := make([]int, n)
    for i := range req {
        req[i] = -1
    }
    for _, r := range requirements {
        req[r[0]] = r[1]
    }
    if req[0] > 0 {
        return 0
    }
    req[0] = 0
    m := slices.Max(req)
    const mod = int(1e9 + 7)
    f := make([][]int, n)
    for i := range f {
        f[i] = make([]int, m+1)
    }
    f[0][0] = 1
    for i := 1; i < n; i++ {
        l, r := 0, m
        if req[i] >= 0 {
            l, r = req[i], req[i]
        }
        for j := l; j <= r; j++ {
            for k := 0; k <= min(i, j); k++ {
                f[i][j] = (f[i][j] + f[i-1][j-k]) % mod
            }
        }
    }
    return f[n-1][req[n-1]]
}
```

#### TypeScript

```ts
function numberOfPermutations(n: number, requirements: number[][]): number {
    const req: number[] = Array(n).fill(-1);
    for (const [end, cnt] of requirements) {
        req[end] = cnt;
    }
    if (req[0] > 0) {
        return 0;
    }
    req[0] = 0;
    const m = Math.max(...req);
    const mod = 1e9 + 7;
    const f = Array.from({ length: n }, () => Array(m + 1).fill(0));
    f[0][0] = 1;
    for (let i = 1; i < n; ++i) {
        let [l, r] = [0, m];
        if (req[i] >= 0) {
            l = r = req[i];
        }
        for (let j = l; j <= r; ++j) {
            for (let k = 0; k <= Math.min(i, j); ++k) {
                f[i][j] = (f[i][j] + f[i - 1][j - k]) % mod;
            }
        }
    }
    return f[n - 1][req[n - 1]];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
