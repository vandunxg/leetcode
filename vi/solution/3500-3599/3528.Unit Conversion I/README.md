---
comments: true
difficulty: Medium
rating: 1579
source: Biweekly Contest 155 Q2
tags:
    - Depth-First Search
    - Breadth-First Search
    - Graph
---

<!-- problem:start -->

# [3528. Unit Conversion I](https://leetcode.com/problems/unit-conversion-i)

[中文文档](/solution/3500-3599/3528.Unit%20Conversion%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>n</code> loại đơn vị được đánh số từ <code>0</code> đến <code>n - 1</code>. Bạn được cung cấp một mảng số nguyên 2D <code>conversions</code> có độ dài <code>n - 1</code>, trong đó <code>conversions[i] = [sourceUnit<sub>i</sub>, targetUnit<sub>i</sub>, conversionFactor<sub>i</sub>]</code>. Điều này cho biết một đơn vị loại <code>sourceUnit<sub>i</sub></code> tương đương với <code>conversionFactor<sub>i</sub></code> đơn vị loại <code>targetUnit<sub>i</sub></code>.</p>

<p>Hãy trả về một mảng <code>baseUnitConversion</code> có độ dài <code>n</code>, trong đó <code>baseUnitConversion[i]</code> là số đơn vị loại <code>i</code> tương đương với một đơn vị loại 0. Vì đáp án có thể lớn, hãy trả về mỗi <code>baseUnitConversion[i]</code> <strong>theo modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">conversions = [[0,1,2],[1,2,3]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[1,2,6]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Chuyển đổi một đơn vị loại 0 thành 2 đơn vị loại 1 bằng <code>conversions[0]</code>.</li>
    <li>Chuyển đổi một đơn vị loại 0 thành 6 đơn vị loại 2 bằng <code>conversions[0]</code>, sau đó là <code>conversions[1]</code>.</li>
</ul>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3500-3599/3528.Unit%20Conversion%20I/images/example1.png" style="width: 545px; height: 118px;" /></div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">conversions = [[0,1,2],[0,2,3],[1,3,4],[1,4,5],[2,5,2],[4,6,3],[5,7,4]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[1,2,3,8,10,6,30,24]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Chuyển đổi một đơn vị loại 0 thành 2 đơn vị loại 1 bằng <code>conversions[0]</code>.</li>
    <li>Chuyển đổi một đơn vị loại 0 thành 3 đơn vị loại 2 bằng <code>conversions[1]</code>.</li>
    <li>Chuyển đổi một đơn vị loại 0 thành 8 đơn vị loại 3 bằng <code>conversions[0]</code>, sau đó là <code>conversions[2]</code>.</li>
    <li>Chuyển đổi một đơn vị loại 0 thành 10 đơn vị loại 4 bằng <code>conversions[0]</code>, sau đó là <code>conversions[3]</code>.</li>
    <li>Chuyển đổi một đơn vị loại 0 thành 6 đơn vị loại 5 bằng <code>conversions[1]</code>, sau đó là <code>conversions[4]</code>.</li>
    <li>Chuyển đổi một đơn vị loại 0 thành 30 đơn vị loại 6 bằng <code>conversions[0]</code>, <code>conversions[3]</code>, sau đó là <code>conversions[5]</code>.</li>
    <li>Chuyển đổi một đơn vị loại 0 thành 24 đơn vị loại 7 bằng <code>conversions[1]</code>, <code>conversions[4]</code>, sau đó là <code>conversions[6]</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>2 &lt;= n &lt;= 10<sup>5</sup></code></li>
    <li><code>conversions.length == n - 1</code></li>
    <li><code>0 &lt;= sourceUnit<sub>i</sub>, targetUnit<sub>i</sub> &lt; n</code></li>
    <li><code>1 &lt;= conversionFactor<sub>i</sub> &lt;= 10<sup>9</sup></code></li>
    <li>Đảm bảo rằng đơn vị 0 có thể được chuyển đổi thành mọi đơn vị khác thông qua một tổ hợp <strong>duy nhất</strong> các phép chuyển đổi mà không sử dụng bất kỳ phép chuyển đổi nào theo chiều ngược lại.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS

<!-- thinking:start -->

> **Tư duy**
>
> Có $n-1$ phép chuyển đổi và một đường đi duy nhất từ $0$, nên đồ thị là một cây có gốc tại $0$. Ta cần xác định có bao nhiêu đơn vị loại $i$ tương đương với một đơn vị loại $0$.
>
> Lưu các hệ số nhân trong adjacency list và thực hiện DFS từ gốc, nhân modulo $10^9+7$ vào $\textit{ans}[i]$.

<!-- thinking:end -->

Vì đề bài đảm bảo rằng đơn vị 0 có thể được chuyển đổi thành mọi đơn vị khác thông qua một đường đi duy nhất, ta có thể dùng Depth-First Search (DFS) để duyệt qua tất cả các quan hệ chuyển đổi đơn vị. Ngoài ra, vì độ dài của mảng $\textit{conversions}$ là $n - 1$, biểu diễn $n - 1$ quan hệ chuyển đổi, ta có thể xem các quan hệ chuyển đổi đơn vị như một cây, trong đó nút gốc là đơn vị 0 và các nút còn lại là các đơn vị khác.

Ta có thể dùng adjacency list $g$ để biểu diễn các quan hệ chuyển đổi đơn vị, trong đó $g[i]$ biểu diễn các đơn vị mà đơn vị $i$ có thể chuyển đổi thành và các hệ số chuyển đổi tương ứng.

Sau đó, ta bắt đầu DFS từ nút gốc $0$, tức là gọi hàm $\textit{dfs}(s, \textit{mul})$, trong đó $s$ là đơn vị hiện tại và $\textit{mul}$ là hệ số chuyển đổi từ đơn vị $0$ đến đơn vị $s$. Ban đầu, $s = 0$, $\textit{mul} = 1$. Trong mỗi lần đệ quy, ta lưu hệ số chuyển đổi $\textit{mul}$ của đơn vị hiện tại $s$ vào mảng kết quả, sau đó duyệt qua tất cả các đơn vị kề $t$ của đơn vị hiện tại $s$, rồi gọi đệ quy $\textit{dfs}(t, \textit{mul} \times w \mod (10^9 + 7))$, trong đó $w$ là hệ số chuyển đổi từ đơn vị $s$ sang đơn vị $t$.

Cuối cùng, ta trả về mảng kết quả.

Độ phức tạp là $O(n)$, và độ phức tạp không gian là $O(n)$, trong đó $n$ là số loại đơn vị.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def baseUnitConversions(self, conversions: List[List[int]]) -> List[int]:
        def dfs(s: int, mul: int) -> None:
            ans[s] = mul
            for t, w in g[s]:
                dfs(t, mul * w % mod)

        mod = 10**9 + 7
        n = len(conversions) + 1
        g = [[] for _ in range(n)]
        for s, t, w in conversions:
            g[s].append((t, w))
        ans = [0] * n
        dfs(0, 1)
        return ans
```

#### Java

```java
class Solution {
    private final int mod = (int) 1e9 + 7;
    private List<int[]>[] g;
    private int[] ans;
    private int n;

    public int[] baseUnitConversions(int[][] conversions) {
        n = conversions.length + 1;
        g = new List[n];
        Arrays.setAll(g, k -> new ArrayList<>());
        ans = new int[n];
        for (var e : conversions) {
            g[e[0]].add(new int[] {e[1], e[2]});
        }
        dfs(0, 1);
        return ans;
    }

    private void dfs(int s, long mul) {
        ans[s] = (int) mul;
        for (var e : g[s]) {
            dfs(e[0], mul * e[1] % mod);
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> baseUnitConversions(vector<vector<int>>& conversions) {
        const int mod = 1e9 + 7;
        int n = conversions.size() + 1;
        vector<vector<pair<int, int>>> g(n);
        vector<int> ans(n);
        for (const auto& e : conversions) {
            g[e[0]].push_back({e[1], e[2]});
        }
        auto dfs = [&](this auto&& dfs, int s, long long mul) -> void {
            ans[s] = mul;
            for (auto [t, w] : g[s]) {
                dfs(t, mul * w % mod);
            }
        };
        dfs(0, 1);
        return ans;
    }
};
```

#### Go

```go
func baseUnitConversions(conversions [][]int) []int {
    const mod = int(1e9 + 7)
    n := len(conversions) + 1

    g := make([][]struct{ t, w int }, n)
    for _, e := range conversions {
        s, t, w := e[0], e[1], e[2]
        g[s] = append(g[s], struct{ t, w int }{t, w})
    }

    ans := make([]int, n)

    var dfs func(s int, mul int)
    dfs = func(s int, mul int) {
        ans[s] = mul
        for _, e := range g[s] {
            dfs(e.t, mul*e.w%mod)
        }
    }

    dfs(0, 1)
    return ans
}
```

#### TypeScript

```ts
function baseUnitConversions(conversions: number[][]): number[] {
    const mod = BigInt(1e9 + 7);
    const n = conversions.length + 1;
    const g: { t: number; w: number }[][] = Array.from({ length: n }, () => []);
    for (const [s, t, w] of conversions) {
        g[s].push({ t, w });
    }
    const ans: number[] = Array(n).fill(0);
    const dfs = (s: number, mul: number) => {
        ans[s] = mul;
        for (const { t, w } of g[s]) {
            dfs(t, Number((BigInt(mul) * BigInt(w)) % mod));
        }
    };
    dfs(0, 1);
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
