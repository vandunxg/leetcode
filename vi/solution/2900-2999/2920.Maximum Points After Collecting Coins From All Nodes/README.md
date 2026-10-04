---
comments: true
difficulty: Hard
rating: 2350
source: Weekly Contest 369 Q4
tags:
    - Bit Manipulation
    - Tree
    - Depth-First Search
    - Memoization
    - Array
    - Dynamic Programming
    - Tree DP
---

<!-- problem:start -->

# [2920. Maximum Points After Collecting Coins From All Nodes](https://leetcode.com/problems/maximum-points-after-collecting-coins-from-all-nodes)

[中文文档](/solution/2900-2999/2920.Maximum%20Points%20After%20Collecting%20Coins%20From%20All%20Nodes/README.md)

## Mô tả

<!-- description:start -->

<p>Có một cây vô hướng với gốc là đỉnh <code>0</code>, gồm <code>n</code> node được đánh số từ <code>0</code> đến <code>n - 1</code>. Cho một mảng số nguyên 2 chiều <strong>integer</strong> <code>edges</code> có độ dài <code>n - 1</code>, trong đó <code>edges[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> cho biết có một cạnh nối giữa các node <code>a<sub>i</sub></code> và <code>b<sub>i</sub></code> trong cây. Bạn cũng được cho một mảng <strong>0-indexed</strong> <code>coins</code> có kích thước <code>n</code>, trong đó <code>coins[i]</code> là số coin tại đỉnh <code>i</code>, và một số nguyên <code>k</code>.</p>

<p>Bắt đầu từ gốc, bạn phải thu thập tất cả coin sao cho chỉ có thể thu thập coin tại một node sau khi đã thu thập coin của tất cả tổ tiên của node đó.</p>

<p>Có thể thu thập coin tại <code>node<sub>i</sub></code> theo một trong các cách sau:</p>

<ul>
	<li>Thu thập toàn bộ coin, nhận được <code>coins[i] - k</code> điểm. Nếu <code>coins[i] - k</code> là số âm, bạn sẽ mất <code>abs(coins[i] - k)</code> điểm.</li>
	<li>Thu thập toàn bộ coin, nhận được <code>floor(coins[i] / 2)</code> điểm. Nếu sử dụng cách này, với mọi <code>node<sub>j</sub></code> thuộc cây con của <code>node<sub>i</sub></code>, <code>coins[j]</code> sẽ được giảm thành <code>floor(coins[j] / 2)</code>.</li>
</ul>

<p>Trả về <em><strong>số điểm tối đa</strong> bạn có thể nhận được sau khi thu thập coin từ <strong>tất cả</strong> node trong cây.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2900-2999/2920.Maximum%20Points%20After%20Collecting%20Coins%20From%20All%20Nodes/images/ex1-copy.png" style="width: 60px; height: 316px; padding: 10px; background: rgb(255, 255, 255); border-radius: 0.5rem;" />
<pre>
<strong>Đầu vào:</strong> edges = [[0,1],[1,2],[2,3]], coins = [10,10,3,3], k = 5
<strong>Đầu ra:</strong> 11
<strong>Giải thích:</strong>
Thu thập toàn bộ coin từ node 0 bằng cách thứ nhất. Tổng số điểm = 10 - 5 = 5.
Thu thập toàn bộ coin từ node 1 bằng cách thứ nhất. Tổng số điểm = 5 + (10 - 5) = 10.
Thu thập toàn bộ coin từ node 2 bằng cách thứ hai, khi đó số coin còn lại tại node 3 sẽ là floor(3 / 2) = 1. Tổng số điểm = 10 + floor(3 / 2) = 11.
Thu thập toàn bộ coin từ node 3 bằng cách thứ hai. Tổng số điểm = 11 + floor(1 / 2) = 11.
Có thể chứng minh rằng số điểm tối đa có thể nhận được sau khi thu thập coin từ tất cả node là 11.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<strong class="example"> <img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2900-2999/2920.Maximum%20Points%20After%20Collecting%20Coins%20From%20All%20Nodes/images/ex2.png" style="width: 140px; height: 147px; padding: 10px; background: #fff; border-radius: .5rem;" /></strong>

<pre>
<strong>Đầu vào:</strong> edges = [[0,1],[0,2]], coins = [8,4,4], k = 0
<strong>Đầu ra:</strong> 16
<strong>Giải thích:</strong>
Tất cả node đều được thu thập coin bằng cách thứ nhất. Do đó, tổng số điểm = (8 - 0) + (4 - 0) + (4 - 0) = 16.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == coins.length</code></li>
	<li><code>2 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code><font face="monospace">0 &lt;= coins[i] &lt;= 10<sup>4</sup></font></code></li>
	<li><code>edges.length == n - 1</code></li>
	<li><code><font face="monospace">0 &lt;= edges[i][0], edges[i][1] &lt; n</font></code></li>
	<li><code><font face="monospace">0 &lt;= k &lt;= 10<sup>4</sup></font></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm có ghi nhớ

<!-- thinking:start -->

> **Tư duy**
>
> Tại mỗi node, ta có thể trừ $k$ hoặc dịch phải mọi coin còn lại. Phép dịch áp dụng cho cây con chưa xử lý, và coin có giá trị không quá $10^4$, nên khoảng $14$ lần dịch là đưa chúng về 0; số lần dịch chỉ là một trạng thái phụ rất nhỏ.
>
> $dfs(i,fa,j)$ là điểm tốt nhất tại $i$ sau $j$ lần dịch: chọn $(coins[i] \gg j)-k$ rồi đệ quy với $j$, hoặc chọn $coins[i] \gg (j+1)$ rồi đệ quy với $j+1$ khi $j<14$. DFS từ gốc kết hợp ghi nhớ sẽ cho đáp án.

<!-- thinking:end -->

Trước hết, ta xây dựng một graph $g$ dựa trên các cạnh đã cho trong đề bài, trong đó $g[i]$ biểu diễn tất cả node kề với node $i$. Sau đó, ta có thể dùng phương pháp tìm kiếm có ghi nhớ để giải bài toán này.

Ta định nghĩa hàm $dfs(i, fa, j)$, biểu diễn trạng thái hiện tại là node $i$, node cha là $fa$, số coin tại node hiện tại cần được dịch phải $j$ bit, và điểm tối đa có thể đạt được.

Quá trình thực thi hàm $dfs(i, fa, j)$ như sau:

Nếu dùng cách thứ nhất để thu thập coin tại node hiện tại, điểm của node hiện tại là $(coins[i] >> j) - k$. Sau đó, ta duyệt qua tất cả node kề $c$ của node hiện tại. Nếu $c$ khác $fa$, ta cộng kết quả của $dfs(c, i, j)$ vào điểm của node hiện tại.

Nếu dùng cách thứ hai để thu thập coin tại node hiện tại, điểm của node hiện tại là $coins[i] >> (j + 1)$. Sau đó, ta duyệt qua tất cả node kề $c$ của node hiện tại. Nếu $c$ khác $fa$, ta cộng kết quả của $dfs(c, i, j + 1)$ vào điểm của node hiện tại. Lưu ý rằng vì giá trị lớn nhất của $coins[i]$ trong đề bài là $10^4$, ta chỉ cần dịch phải nhiều nhất $14$ bit để giá trị của $coins[i] >> (j + 1)$ bằng $0$.

Cuối cùng, ta trả về điểm lớn nhất có thể đạt được khi sử dụng hai cách tại node hiện tại.

Để tránh tính toán lặp lại, ta sử dụng phương pháp tìm kiếm có ghi nhớ và lưu kết quả của $dfs(i, fa, j)$ vào $f[i][j]$, trong đó $f[i][j]$ biểu diễn trạng thái hiện tại là node $i$, node cha là $fa$, số coin tại node hiện tại cần được dịch phải $j$ bit, và điểm tối đa có thể đạt được.

Độ phức tạp thời gian là $O(n \times \log M)$, độ phức tạp không gian là $O(n \times \log M)$. Trong đó, $M$ là giá trị lớn nhất của $coins[i]$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumPoints(self, edges: List[List[int]], coins: List[int], k: int) -> int:
        @cache
        def dfs(i: int, fa: int, j: int) -> int:
            a = (coins[i] >> j) - k
            b = coins[i] >> (j + 1)
            for c in g[i]:
                if c != fa:
                    a += dfs(c, i, j)
                    if j < 14:
                        b += dfs(c, i, j + 1)
            return max(a, b)

        n = len(coins)
        g = [[] for _ in range(n)]
        for a, b in edges:
            g[a].append(b)
            g[b].append(a)
        ans = dfs(0, -1, 0)
        dfs.cache_clear()
        return ans
```

#### Java

```java
class Solution {
    private int k;
    private int[] coins;
    private Integer[][] f;
    private List<Integer>[] g;

    public int maximumPoints(int[][] edges, int[] coins, int k) {
        this.k = k;
        this.coins = coins;
        int n = coins.length;
        f = new Integer[n][15];
        g = new List[n];
        Arrays.setAll(g, i -> new ArrayList<>());
        for (var e : edges) {
            int a = e[0], b = e[1];
            g[a].add(b);
            g[b].add(a);
        }
        return dfs(0, -1, 0);
    }

    private int dfs(int i, int fa, int j) {
        if (f[i][j] != null) {
            return f[i][j];
        }
        int a = (coins[i] >> j) - k;
        int b = coins[i] >> (j + 1);
        for (int c : g[i]) {
            if (c != fa) {
                a += dfs(c, i, j);
                if (j < 14) {
                    b += dfs(c, i, j + 1);
                }
            }
        }
        return f[i][j] = Math.max(a, b);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximumPoints(vector<vector<int>>& edges, vector<int>& coins, int k) {
        int n = coins.size();
        int f[n][15];
        memset(f, -1, sizeof(f));
        vector<int> g[n];
        for (auto& e : edges) {
            int a = e[0], b = e[1];
            g[a].emplace_back(b);
            g[b].emplace_back(a);
        }
        auto dfs = [&](this auto&& dfs, int i, int fa, int j) -> int {
            if (f[i][j] != -1) {
                return f[i][j];
            }
            int a = (coins[i] >> j) - k;
            int b = coins[i] >> (j + 1);
            for (int c : g[i]) {
                if (c != fa) {
                    a += dfs(c, i, j);
                    if (j < 14) {
                        b += dfs(c, i, j + 1);
                    }
                }
            }
            return f[i][j] = max(a, b);
        };
        return dfs(0, -1, 0);
    }
};
```

#### Go

```go
func maximumPoints(edges [][]int, coins []int, k int) int {
	n := len(coins)
	f := make([][]int, n)
	for i := range f {
		f[i] = make([]int, 15)
		for j := range f[i] {
			f[i][j] = -1
		}
	}
	g := make([][]int, n)
	for _, e := range edges {
		a, b := e[0], e[1]
		g[a] = append(g[a], b)
		g[b] = append(g[b], a)
	}
	var dfs func(int, int, int) int
	dfs = func(i, fa, j int) int {
		if f[i][j] != -1 {
			return f[i][j]
		}
		a := (coins[i] >> j) - k
		b := coins[i] >> (j + 1)
		for _, c := range g[i] {
			if c != fa {
				a += dfs(c, i, j)
				if j < 14 {
					b += dfs(c, i, j+1)
				}
			}
		}
		f[i][j] = max(a, b)
		return f[i][j]
	}
	return dfs(0, -1, 0)
}
```

#### TypeScript

```ts
function maximumPoints(edges: number[][], coins: number[], k: number): number {
    const n = coins.length;
    const f: number[][] = Array.from({ length: n }, () => Array(15).fill(-1));
    const g: number[][] = Array.from({ length: n }, () => []);
    for (const [a, b] of edges) {
        g[a].push(b);
        g[b].push(a);
    }
    const dfs = (i: number, fa: number, j: number): number => {
        if (f[i][j] !== -1) {
            return f[i][j];
        }
        let a = (coins[i] >> j) - k;
        let b = coins[i] >> (j + 1);
        for (const c of g[i]) {
            if (c !== fa) {
                a += dfs(c, i, j);
                if (j < 14) {
                    b += dfs(c, i, j + 1);
                }
            }
        }
        return (f[i][j] = Math.max(a, b));
    };
    return dfs(0, -1, 0);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
